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

The global frame is important because the orthonormal vectors defining the local and cursor frames (to be explained later) are expressed relative to it.

## Local Reference Frames

Each surface \\( i \\) has a local reference frame \\( \mathbf{L}_i \\) whose origin lies at the vertex of the surface. Its coordinate axes are denoted \\( x_i^{\prime} \\), \\( y_i^{\prime} \\), and \\( z_i^{\prime} \\). For flat surfaces, I set the \\( z_i^{\prime} \\) axis perpendicular to the surface.

![The local reference frames of the two mirrors.](/images/sequential-layout-local-reference-frames.png)

Notice that the \\( x^{\prime} \\) axes flip directions when going from mirror 1 to mirror 2. This is done to preserve the right-handedness of the reference frames. More about this will be explained in the next section.

# Sequential System Models

Ray tracing programs for optical design are often divided into two categories: sequential and nonsequential. In sequential ray tracers, rays are traced from one surface to another in the sequence for which they are defined. This means that a ray could pass right through a surface if it is not the next surface in the model sequence.

Nonsequential ray tracers do not take account of the order in which surfaces are defined. Rays are fired into the world and the intersect whatever the closest object is on their path. Illumination optics often use nonsequential ray tracing, as do rendering engines for cinema.

My ray tracer is a sequential ray tracer because sequential ray tracing is easier to implement and can be applied to nearly all the use cases that I encounter in the lab.

## 3D Layouts of Sequential Surfaces

One possibility to layout sequential surfaces in 3D is to specify the coordinates and orientations of each surface relative to the global frame. This is how one adds surfaces in 3D in the open source Python library [Optiland](https://github.com/HarrisonKramer/optiland), for example. In practice, I found that I need to have a piece of paper by my side to work out the positions of each surface independently. This option provides maximum flexibility in surface placement.

The other possibility that I considered is to leverage the fact that the surfaces are an ordered sequence, and position them in 3D space along the optical axis. The axis can reflect from reflecting surfaces using the law of reflection. Furthermore, any tilt or decenter could be specified relative to this axis. I ultimately chose this solution because I felt that it better matches my mental model of sequential optical systems.

## The Cursor

I created the idea of the cursor to position sequential surfaces in 3D space. A cursor has a 3D position, \\( \mathbf{r} \left( s \right) \\) that is parameterized over the track length \\( s \\). \\( s \\) is negative for the object surface, \\( s = 0 \\) at the first non-object surface, and achieves its greatest value at the final image plane.

In addition, the cursor has a reference frame attached to it that I denote \\( \mathbf{C} \left( s \right) \\). The axes of the cursor frame are \\( r \\), \\( u \\)  and \\( f \\), which stand for right, up, and forward, respectively. This nearly matches the [FRU](https://dev.epicgames.com/documentation/en-us/uefn/forwardrightup-coordinate-system-in-unreal-editor-for-fortnite) coordinate system in game engines such as Unreal, except I take the forward direction to represent the optical axis because I would say that this convention is universal in optical design.

![The cursor frames at three different positions along the optical axis.](/images/sequential-layout-cursor-frames.png)

Above I show the cursor frame at three different positions along the optical axis \\( s_1 < 0 < s_2 < s_3 \\). Refracting surfaces will not change the orientation of the cursor frame, but reflecting surfaces will. Reorientation of the 

Finally, when \\( s \\) is exactly equal to a reflecting surface position, I take the orientation of the cursor frame to be the one **before** reflection. An infinitesimal distance later, the frame reorients by reflecting about the surface normal at the vertex of the surface in its local frame.

# Rotations between Frames

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



[^1]: G. H. Spencer and M. V. R. K. Murty, "General Ray-Tracing Procedure," J. Opt. Soc. Am. 52, 672-678 (1962). [https://doi.org/10.1364/JOSA.52.000672](https://doi.org/10.1364/JOSA.52.000672).
[^2]: Ray/surface intersections with spherical surfaces can be found analytically using the quadratic equation with only minor caveats considering stability issues due to floating point arithmetic. This would likely be faster than using Newton-Raphson. However, a general system contains both spherical and non-spherical surfaces, and I was concerned that checking each surface type would result in a performance hit due to branch prediction failures by the processor. I could probably have found a way around this by deciding ahead of time which algorithm to use to determine the intersection for each surface before entering the main ray tracing loop, but during initial development I decided to just use Newton-Raphson for everything because doing so resulted in very simple code. (Thanks to Andy York for telling me about the numerical instabilities when using the quadratic equation. See Chapter 7 here: [https://www.realtimerendering.com/raytracinggems/rtg/index.html](https://www.realtimerendering.com/raytracinggems/rtg/index.html).)
[^3]: The object plane is the flat surface perpendicular to the optical axis in which the object lies. It is always at surface index 0 in my convention.
