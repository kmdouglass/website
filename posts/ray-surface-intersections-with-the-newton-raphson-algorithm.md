<!--
.. title: Ray-Surface Intersections with the Newton-Raphson Algorithm
.. slug: ray-surface-intersections-with-the-newton-raphson-algorithm
.. date: 2026-03-23 08:26:02 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: true
-->

A few weeks ago I restarted work on [Cherry](https://kmdouglass.github.io/cherry/), my sequential ray tracer, by porting the GUI from Javascript/React to pure WASM with [egui](https://github.com/emilk/egui). I am very happy with the results. It's much easier to add features with egui, and I have no regrets about giving up the DOM in the web application. I was never very good at web developement, and I always felt that React has too much unseen magic happening behind the scenes.

Having gotten the frontend work out of the way, I turned my attention back to adding features to Cherry. One of the applications that interests me in the lab is scan lenses, i.e. lenses that translate an angular deviation of a laser beam into a lateral displacement. These lenses are designed so that their scanning plane is as flat as possible over a large field of view. They often must work across multiple wavelengths and at large field angles.

As a starting point, I put the [f-theta scan lens example from Optiland](https://optiland.readthedocs.io/en/latest/gallery/specialized_lenses/f_theta_lens.html), which itself comes from [Milton Laikin's Lens Design, 4th ed.](https://www.routledge.com/Lens-Design/Laikin/p/book/9780849382789), into Cherry and began varying the incident field angle. It did not take long before problems in the ray tracing routine appeared.

![](/images/newton-raphson-ray-trace-error.png)

At a field angle of exactly 5.6 degrees, the marginal ray from the Fraunhofer C line ( \\( \lambda = 0.6563 \, \mu m\\) ) reflects backwards off the first lens surface and intersects the origin. It propagates correctly for field angles of 5.5 degrees and 5.7 degrees, which to me suggests that there is a numerical accident that happens at exactly this value. Furthermore, the spot diagram shows ray-surface intersections across all wavelengths in the image plane disappearing and reappearing randomly as the field angle increases, with the overall number of ray trace errors increasing with field angle. Small angles do not seem to have the problem, and indeed I had not yet tried examples with highly curved surfaces such as this f-theta lens.

As it turns out, I was very naïve about the robustness of the Newton-Raphson root finding algorithm for computing ray-surface intersections.

# Debugging Ray-Surface Intersections

## Running Traces through Algorithms

In practice there are a lot of ray-surface intersections to compute; tracing 1000 rays through 8 surfaces, for example, yields 8000 intersections. This means the algorithm loops 8000 individual times. When only a subset of these fail, it pays to have good debugging tooling in place to identify the state of the algorithm during a failure.

For this I turned to the excellent [tracing](https://github.com/tokio-rs/tracing) crate, which has become something of a *de facto* standard for logging in Rust. The primary abstractions in tracing are events and spans. Events are the most straightforward to understand because they are the same thing as log messages in other languages.

tracing's documentation provides a good, high-level explanation of spans[^1]:

> Unlike a log line that represents a moment in time, a span represents a period of time with a beginning and an end. When a program begins executing in a context or performing a unit of work, it enters that context’s span, and when it stops executing in that context, it exits the span.

The value in using spans is that you can attach data to them, forming a context. Every event that is emitted during a span is associated with this data, regardless of where the event was emitted. In pseudocode, my ray tracing algorithm roughly works like this:

```
for field in fields.iter():
  for (surface_id, surface) in surfaces.iter().enumerate():
    for (ray_id, ray) in rays.iter().enumerate():
        // coordinate system transformations
        
        intersect(ray, surface)

        // ray transformations
        // coordinate system transformations
```

When an intersection failure occurs, I'd like to know the `ray_id`, but I'd also like to know the state of variables inside the `intersection` method. Using spans, I do not have to thread `ray_id` inside the `intersect` method to attach it to log messages. Instead, I create a span at the top of the inner-most loop that contains ray_id in its context. Then, any events inside the `intersection` method will be associated to that context. I can then filter by, say, `ray_id=42` to see all events that happened for that ray inside `intersect()`.

```
for field in fields.iter():
  for (surface_id, surface) in surfaces.iter().enumerate():
    for (ray_id, ray) in rays.iter().enumerate():
        let _span = trace_span!("trace_ray", ray_id, surface_id).entered();  // <-- Span begins
        // coordinate system transformations                                        |
                                                                                    |
        intersect(ray, surface)                                                     |
                                                                                    |
        // ray transformations                                                      |
        // coordinate system transformations                                        |
        // <----------------------------------------------------------------------- Span ends
```

## A Failing Test Case

I recreated the same lens in an integration test with a single wavelength at \\( \lambda = 0.5876 \\) and an off-axis tangential ray fan consisting of 9 rays and incident at 20 degrees. I simplified the test case to reduce the total number of errors, which in turn allowed me to better isolate problems. There were two notable types of errors. In the first, I saw NaNs appear in some of the values manipulated by the Newton-Raphson algorithm:

```console
  2026-03-26T07:49:32.552835Z ERROR cherry_rs::views::ray_trace_3d::rays: Ray intersection did not converge, ctr: 999, s: NaN, residual: NaN
    at cherry-rs/src/views/ray_trace_3d/rays.rs:97

  2026-03-26T07:49:32.552896Z ERROR cherry_rs::views::ray_trace_3d::trace: Ray terminated due to intersection failure, ray_id: 8, surface_id: 2, reason: Ray intersection did not converge
    at cherry-rs/src/views/ray_trace_3d/trace.rs:57
```

In the second type of error, the ray-surface intersection simply did not converge.

```console
  2026-03-26T07:49:32.553113Z ERROR cherry_rs::views::ray_trace_3d::rays: Ray intersection did not converge, ctr: 999, s: 0.339233626291856, residual: -2.220446049250313e-16
    at cherry-rs/src/views/ray_trace_3d/rays.rs:97

  2026-03-26T07:49:32.553139Z ERROR cherry_rs::views::ray_trace_3d::trace: Ray terminated due to intersection failure, ray_id: 3, surface_id: 3, reason: Ray intersection did not converge
    at cherry-rs/src/views/ray_trace_3d/trace.rs:57
```

I am going to spend the rest of this post analyzing the cause of these errors.

# The Newton-Raphson Algorithm

At this point while writing the post, I began to feel like I needed a refresher in the Newton-Raphson algorithm. It has been about two years since I first implemented it and quite frankly I have forgotten a lot of the details. So I'm going to circle back to the basics to better prepare myself to fix this thing.

## Root Finding

The Newton-Raphson algorithm is a well-known numerical routine for finding the roots (a.k.a. zeros) of a function. I think it's best illustrated by way of example. I found one at <https://atozmath.com/example/CONM/Bisection.aspx?q=nr&q1=E1> that involves finding the single zero of the function \\( f(x) = x^3 - x - 1 \\). The function is plotted below:

![](/images/newton_raphson_example_function.png)

The algorithm is derived as follows: assume we want to find the root of a function \\( f(x) \\). Choose a starting point \\( x_0 \\) close to the root and find the slope of the line tangent to the curve of the function at this point. Extend this line to \\( x_1 \\), the x-intercept where \\( y = 0 \\). The expression for the slope is

$$ f'(x_0) = \frac{\Delta y}{ \Delta x} = \frac{f(x_0) - f(x_1)}{x_0 - x_1} = \frac{f(x_0) - 0}{x_0 - x_1}. $$

Below you can see what this construction looks like using \\( x_0 = 1.5 \\).

![](/images/newton_raphson_construction.png)

Solving this expression for \\( x_ 1 \\) gives

$$ x_1 = x_0 - \frac{f(x_0)}{f'(x_0)}. $$

Repeat the process using \\( x_1 \\) as the new starting point:

$$ x_2 = x_1 - \frac{f(x_1)}{f'(x_1)}. $$

Stop iterating whenever the difference between \\( f(x_n) \\) and 0 is less than some tolerance. If all goes well, the algorithm will quickly converge to the root.

Here are the first six iterations of the algorithm for finding the root of \\( f(x) = x^3 - x - 1 \\) when starting at \\( x_0 = 1.5 \\).

![](/images/newton_raphson_convergence.png)

After 6 steps, the algorithm has identified the root \\( x = 1.324718 \\) with a precision better than \\( 10^{-6} \\).

## Oscillations

Now of course I deliberately diverted your attention away from the important point that you need to choose the starting point such that it is already close to the root. Here's what happens when I choose a starting point close the local maximum at -0.5:

![](/images/newton_raphson_divergence.png)

The algorithm struggles to converge because the initial tangent line is nearly horizontal. This results in the next guess being very far off target and ultimately the algorithm oscillates irregularly around the starting point. However, at step 12, it happens to land just to the right of the local minimum at \\( x = 0.7425 \\), which sends the next guess far to the right of all local extrema.

![](/images/newton_raphson_near_local_minimum.png)

From this point, the algorithm can simply descend downhill, where by step 19 it has found the root with a tolerance better than \\( 10^{-6} \\).

[If the starting point is close to the root, the Newton-Raphson algorithm has quadratic convergence.](https://archive.nptel.ac.in/content/storage2/courses/122104019/numerical-analysis/Rathish-kumar/ratish-1/f3node7.html) This means that the error \\( \epsilon_{n+1} \\) in step \\( n+ 1 \\) is proportional to the square of the error at the previous step, \\( \epsilon_n^2 \\). But if the starting point is not sufficiently close, then the algorithm can display quite erratic behavior.

All of this is to say that the choice of starting point is of great importance.

[^1]: Spans remind me a lot of [Sentry](https://sentry.io), which I used to perform tracing on distributed code bases when I worked for a photogrammetry company doing image processing on the Cloud.
