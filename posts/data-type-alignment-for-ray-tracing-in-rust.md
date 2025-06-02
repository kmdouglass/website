<!--
.. title: Data Type Alignment for Ray Tracing in Rust
.. slug: data-type-alignment-for-ray-tracing-in-rust
.. date: 2025-02-24 08:40:00 UTC+01:00
.. tags: ray tracing, rust
.. category: programming
.. link: 
.. description: I explore data type size, alignment, and padding for ray tracing in Rust.
.. type: text
-->

I would like to clean up my 3D ray trace routines for my [Rust-based optical design library](https://www.github.com/kmdouglass/cherry). The proof of concept (PoC) is finished and I now I need to make the code easier to modify to better support the features that I want to add on the frontend. I suspect that I might be able to make some performance gains as well during refactoring. Towards this end, I want to take a look at my ray data type from the perspective of making it CPU cache friendly.

One of the current obstacles to adding more features to the GUI (for example color selection for different ray bundles) is how I handle individual rays. For the PoC it was fastest to add two additional fields to each ray to track where they come from and whether they are terminated:

```rust
struct Vec3 {
    e: [f64; 3],
}

struct Ray {
    pos: Vec3,
    dir: Vec3,
    terminated: bool,
    field_id: usize,
}
```

A ray is just two, 3-element arrays of floats that specify the coordinates of a point on the ray and its direction cosines. I have additionally included a boolean flag to indicate whether the ray has terminated, i.e. gone out-of-bounds of the system or failed to converge during calculation of the intersection point with a surface.

A ray fan is a collection of rays and is specified by a 3-tuple of wavelength, axis, and field; `field_id` really should not belong to an individual Ray because it can be stored along with the set of all rays for the current ray fan. I probably added it because it was the easiest thing to do at the time to get the application working.

# A deeper look into the Ray struct

## Size of a ray

Let's first look to see how much space the Ray struct occupies.

```rust
use std::mem;

#[derive(Debug)]
struct Vec3 {
    e: [f64; 3],
}

#[derive(Debug)]
struct Ray {
    pos: Vec3,
    dir: Vec3,
    terminated: bool,
    field_id: usize,
}

fn main() {
    println!("Size of ray: {:?}", mem::size_of::<Ray>());
}
```

The `Ray` struct occupies 64 bytes in memory. Does this make sense?

The sizes of the individual fields are:

| Field | Size, bytes |
|-------|-------------|
| pos | 24 |
| dir | 24 |
| terminated | 1 |
| field_id | 8* |

`pos` and `dir` are each 24 bytes because they are each composed of three 64-bit floats and 8 bits = 1 byte. `terminated` is only one byte because it is a boolean. `field_id` is a [usize](https://doc.rust-lang.org/std/primitive.usize.html), which means that it depends on the compilation target. On 64-bit targets, such as x86_64, it is 64 bits = 8 bytes in size.

Adding the sizes in the above table gives 57 bytes, not 64 bytes as was output from the example code. Why is this?

## Alignment and padding

[Alignment](https://en.wikipedia.org/wiki/Data_structure_alignment) refers to the layout of a data type in memory and how it is accessed. CPUs read memory in chunks that are equal in size to the [word size](https://en.wikipedia.org/wiki/Word_(computer_architecture)). Misaligned data is inefficient to access because the CPU requires more cycles than is necessary to fetch the data.

Natural alignment refers to the most efficient alignment of a data type for CPU access. To achieve natural alignment, a compiler can introduce padding between fields of a struct so that the memory address of a field or datatype is a multiple of the field's/data type's alignment.

As an example of misalignment, consider a 4-byte integer and that starts at memory address 5. The CPU has 32-bit memory words. To read the data, the CPU must:

1. read bytes 4-7,
2. read bytes 8-11,
3. and combine the relevant parts of both reads to get the 4 bytes, i.e. bytes 5 - 8.

Notice that we must specify the memory word size to determine whether a data type is misaligned.

Here is an important question: **why can't the CPU just start reading from memory address 5?** The answer, as far as I can tell, is that it just can't. This is not how the CPU, RAM, and memory bus are wired.

## Alignment in Rust

[Alignment in Rust](https://doc.rust-lang.org/reference/type-layout.html#size-and-alignment) is defined as follows:

> The alignment of a value specifies what addresses are valid to store the value at. A value of alignment n must only be stored at an address that is a multiple of n.

[The Rust compiler only guarantees the following](https://doc.rust-lang.org/reference/type-layout.html#the-rust-representation) when it comes to padding fields in structs:
 
> 1. The fields are properly aligned.
> 2. The fields do not overlap.
> 3. The alignment of the type is at least the maximum alignment of its fields.

So for my `Ray` data type, its alignment is 8 because the maximum alignment of its fields is 8 bytes. (`pos` and `dir` are composed of 8-byte floating point numbers). The addresses of its fields are:

```rust
fn main() {
    let ray = Ray {
        pos: Vec3 { e: [0.0, 0.0, 0.0] },
        dir: Vec3 { e: [0.0, 0.0, 1.0] },
        terminated: false,
        field_id: 0,
    };
    
    unsafe {
        println!("Address of ray.pos: {:p}", ptr::addr_of!(ray.pos));
        println!("Address of ray.dir: {:p}", ptr::addr_of!(ray.dir));
        println!("Address of ray.terminated: {:p}", ptr::addr_of!(ray.terminated));
        println!("Address of ray.field_id: {:p}", ptr::addr_of!(ray.field_id));
    }
}
```

I got the following results, which will vary from system-to-system and probably run-to-run:

```console
Address of ray.pos: 0x7fff076c6b50
Address of ray.dir: 0x7fff076c6b68
Address of ray.terminated: 0x7fff076c6b88
Address of ray.field_id: 0x7fff076c6b80
```

So the `pos` field comes first at address `0x6b50` (omitting the most significant hexadecimal digits). Then, 24 bytes later, comes `dir` at address `0x6b68`. Note that the difference is hexadecimal 0x18, which is decimal 16 + 8 = 24! So `pos` really occupies 24 bytes like we previously calculated.

Next comes `field_id` and not `terminated`. It is `0x6b80 - 0x6b68 = 0x0018`, or 24 bytes after `dir` like before. So far we have no padding, but the compiler did swap the order of the fields. Finally, `terminated` is 8 bytes after `field_id` because `field_id` is 8-byte aligned. This means that the Rust compiler must have placed 7 bytes of padding after the `terminated` field.

# What makes a good data type?

As I mentioned, I already know that `field_id` shouldn't belong to the ray for reasons related to data access by the programmer. So the reason for removing it from the `Ray` struct is not related to performance. But what about the `terminated` bool? Well, in this case, it's resulting in 7 extra bytes of padding for each ray!

```rust
#[derive(Debug)]
struct Vec3 {
    e: [f64; 3],
}

#[derive(Debug)]
struct Ray {
    pos: Vec3,
    dir: Vec3,
    terminated: bool,
}

fn main() {
    let ray = Ray {
        pos: Vec3 { e: [0.0, 0.0, 0.0] },
        dir: Vec3 { e: [0.0, 0.0, 1.0] },
        terminated: false,
    };

    println!("Size of ray: {:?}", mem::size_of::<Ray>());
}
```

This program prints `Size of ray : 56`, but 24 + 24 + 1 = 49. In both versions we waste 7 bytes.

## Fitting a Ray into the CPU cache

Do I have a good reason to remove `terminated` from the `Ray` struct because it wastes space? Consider the following:

We want as many `Ray` instances as possible to fit within a CPU cache line if we want to maximize performance. (Note that I'm not saying that we necessarily want to maximize performance because that comes with tradeoffs.) Each CPU core on my AMD Ryzen 7 has a 64 kB L1 cache with 64 byte cache lines. This means that I can fit only 1 of the current version of `Ray` into each cache line for a total of 64 kB / 64 bytes = 1024 rays maximum in the L1 cache of each core. If I remove `field_size` and `terminated`, then the size of a ray becomes 48 bytes. Unfortunately, this means that only one `Ray` instance fits in a cache line, just as before with a 64 byte `Ray`.

But, if I also reduce my precision to 32-bit floats, then the size of a `Ray` becomes 6 * 4 = 24 bytes and I have doubled the number of rays that fit in L1 cache.

Now what if I reduced the precision but kept `terminated`? Then I get 6 * 4 + 8 = 32 bytes per Ray and I still have 2 rays per cache line.

I conclude that there is no reason to remove `terminated` for performance reasons. Reducing my floating point precision would produce a more noticeable effect on the cache locality of the `Ray` data type.

# Does all of this matter?

My Ryzen 7 laptop can trace about 600 rays through 3 surfaces in 380 microseconds with Firefox, Slack, and Outlook running. At this point, I doubt that crafting my data types for cache friendliness is going to offer a significant payoff. Creating data types that are easy to work with is likely more important.

I do think, however, that it's important to understand these concepts. If I do need to tune the performance in the future, then I know where to look.
