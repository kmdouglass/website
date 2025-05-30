<!--
.. title: Rotation Matrices for 3D Ray Tracing
.. slug: rotation-matrices-for-3d-ray-tracing
.. date: 2025-05-30 10:36:25 UTC+02:00
.. tags: ray tracing, optics
.. category: programming
.. link: 
.. description: 
.. type: text
.. has_math: true
-->

I am working on a new feature in my [ray tracer](https://github.com/kmdouglass/cherry) that will allow users to lay out sequential optical systems in 3D. This is forcing me to think carefully about 3D rigid body transformations in a level of detail that I have never before considered.

In this post I walk through the mathematics for modeling a pair of flat mirrors that are oriented at different angles. Strictly speaking, the layout can be represented more easily in 2D, but I will treat the problem as if it were the more general 3D case. Emphasis will be placed on specifying rotations in an intuitive manner, which will mean rotations about the optical axis, rather than about a fixed axis in a global reference frame.

# The Problem

The ray tracing problem that I will consider is depicted as follows:

![A system of two flat mirrors whose optical axis forms the figure Z.](/images/sequential-layout-problem-statement.png)

The system consists of two flat mirrors whose optical axis forms a "figure Z." The normal of the first mirror is at 30 degrees to the axis, and likewise for the second. The optical axis emerges from the second mirror parallel to the first.

The questions are:

1. How do I construct the system without requiring the user to specify the absolute coordinates of the mirror surfaces?
1. How do I represent the local coordinate reference frames for each mirror surface?
1. How do I handle rotations of the reference frames after reflection?

## Ray Tracing Review

As a quick review, the ray tracing algorithm that I implemented was described by Spencer and Murty[^1]. It is loosely follows this pseudo-code:

```
for each surface in system:
    for each ray in ray bundle:
        1. transform the ray coordinates by rotating the local reference frame of the surface
        2. find the ray/surface intersection point
        3. propagate the ray to the intersection point
        4. perform bounds checking against the surface
        5. redirect the ray according to the laws of refraction or reflection
        6. transform the ray coordinates by rotating the reference frame back to the global one
```

Rotations are performed using 3x3 rotation matrices. Ray/surface intersections are found numerically using the Newton-Raphson method, even for spherical surfaces[^2]. I computed the expressions for the surface sag and normal vectors for conics and flat surfaces by hand and hard-coded them as functions of the intersection point in the local surface reference frame to avoid having to compute them on-the-fly.

Looking at the ray trace algorithm, I see three things that are relevant to this discussion:

1. There are both global and local reference frames
1. Surfaces are iterated over sequentially
1. There are rotations, one at the beginning of each loop iteration and one at the end

Let's explore each one individually, starting with the global and local reference frames.

# Reference Frames

I will use only right-handed reference frames where positive rotations are in the counterclockwise direction.

## The Global Reference Frame

The global reference frame \\( \mathbf{G} \\) remains fixed. Sometimes it's called the world frame. I denote the coordinate axes of the global frame using \\( x \\), \\( y \\), and \\( z \\).

![The global reference frame.](/images/sequential-layout-global-reference-frame.png)

By convention, I put its origin at the first non-object surface; this would be at the first mirror in the system of two mirrors I described above[^3]. I also establish the convention that the optical axis between the object and the first surface is parallel to the global z-axis.

The global frame is important because the following **active** rotation matrices are defined relative to its axes:

$$\begin{eqnarray}
R_x \left( \theta \right) = \left(
  \begin{array}{cccc}
    1 & 0 & 0 \\\\
    0 & \cos \theta & - \sin \theta \\\\
    0 & \sin \theta & \cos \theta
  \end{array}
\right)
\end{eqnarray}$$

$$\begin{eqnarray}
R_y \left( \theta \right) = \left(
  \begin{array}{cccc}
    \cos \theta & 0 & \sin \theta \\\\
    0 & 1 & 0 \\\\
    - \sin \theta & 0 & \cos \theta
  \end{array}
\right)
\end{eqnarray}$$

$$\begin{eqnarray}
R_z \left( \theta \right) = \left(
  \begin{array}{cccc}
    \cos \theta & - \sin \theta & 0 \\\\
    \sin \theta & \cos \theta & 0 \\\\
    0 & 0 & 1
  \end{array}
\right)
\end{eqnarray}$$

These rotations are active in the sense that their application to a single vector would result in the vector rotating within the global frame, i.e. the frame would remain fixed.

## Local Reference Frames

Each surface \\( i \\) has a local reference frame \\( \mathbf{L}_i \\) whose origin lies at the vertex of the surface. Its coordinate axes are denoted \\( x_i^{\prime} \\), \\( y_i^{\prime} \\), and \\( z_i^{\prime} \\). For flat surfaces, I set the \\( z_i^{\prime} \\) axis perpendicular to the surface.

[^1]: G. H. Spencer and M. V. R. K. Murty, "General Ray-Tracing Procedure," J. Opt. Soc. Am. 52, 672-678 (1962). [https://doi.org/10.1364/JOSA.52.000672](https://doi.org/10.1364/JOSA.52.000672).
[^2]: Ray/surface intersections with spherical surfaces can be found analytically using the quadratic equation with only minor caveats considering stability issues due to floating point arithmetic. This would likely be faster than using Newton-Raphson. However, a general system contains both spherical and non-spherical surfaces, and I was concerned that checking each surface type would result in a performance hit due to branch prediction failures by the processor. I could probably have found a way around this by deciding ahead of time which algorithm to use to determine the intersection for each surface before entering the main ray tracing loop, but during initial development I decided to just use Newton-Raphson for everything because doing so resulted in very simple code. (Thanks to Andy York for telling me about the numerical instabilities when using the quadratic equation. See Chapter 7 here: [https://www.realtimerendering.com/raytracinggems/rtg/index.html](https://www.realtimerendering.com/raytracinggems/rtg/index.html).)
[^3]: The object plane is the flat surface perpendicular to the optical axis in which the object lies. It is always at surface index 0 in my convention.
