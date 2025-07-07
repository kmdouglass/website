<!--
.. title: Completing the Square and the Normal Form of Quadrics
.. slug: completing-the-square-and-normal-form-quadrics
.. date: 2025-07-07 08:42:35 UTC+02:00
.. tags: algebraic geometry, algebra, mathematics, ray tracing
.. category: optics
.. link: 
.. description: I explain how to convert the expression for the sag of an optical surface into the normal form of a quadric equation.
.. type: text
.. has_math: true
-->

I am working on rendering cross section views of optical systems for my ray tracer. The problem is one of finding the intersection curve between a plane (the cutting plane) and a [quadric surface](https://en.wikipedia.org/wiki/Quadric) which represents an interface between two media with different refractive indexes. Quadric surfaces are important primitives for modeling optical interfaces because they represent common surface types in optics, such as spheroids and paraboloids. A pair of quadrics, or a quadric and a plane, models a common lens.

In 3D, the implicit surface equation for a quadric is

$$
A x^2 + B y^2 + C z^2 + D x y + E y z + F x z + G x + H y + I z + J = 0
$$

Any quadric can be reduced to a so-called [normal form](https://en.wikipedia.org/wiki/Quadric#Euclidean_space) that identifies its class, i.e. ellipsoid, hyperbolic paraboloid, etc. Except for paraboloids, none of the normal form equations contain linear terms in \\( x \\), \\( y \\), or \\( z \\).

A quadric of revolution occurs when two or more of the parameters of the the quadric's normal form are equal, such as \\( x^2 / R^2 + y^2 / R^2 + z^2 / R^2 = 1 \\), which is the equation for a spheroid with radius parameter \\( R \\)[^1]. Quadrics of revolution are the surface types most-often encountered in optics[^2].

The surface sag of a quadric surface is a very important quantity for ray tracing. The sag of a quadric is usually given in terms of the [conic constant](https://en.wikipedia.org/wiki/Conic_constant), \\( K \\). One obtains the sag by solving the following quadric equation for \\( z \\):

$$
x^2 + y^2 - 2 R z + ( K + 1 ) z^2 = 0
$$

Here, \\( R \\) is the radius of curvature of the surface at its apex, \\( x = y = z = 0 \\).

At this point I asked myself how I could rewrite the above expression in its normal form, and for a while I was unable to do it. After a bit of searching on the internet, I eventually realized that the solution involves [completing the square](https://en.wikipedia.org/wiki/Completing_the_square), a topic that was not given much attention during my high school education. After this exercise, I realize now that the purpose of completing the square is to essentially **move any linear terms of a quadratic equation into squared parantheses**. This allows one to then remove the linear terms entirely by applying a suitable transformation, leaving only quadratic and constant terms.

# Converting the Quadric to its Normal Form

The conversion of the above equation proceeds as follows. We first factor out \\( ( K + 1 ) \\) from the terms involving \\( z \\).

$$
x^2 + y^2 + ( K + 1 ) \left[ z^2 - \frac{ 2 R z }{ K + 1 }\right] = 0
$$

Next, we "add zero" to the term inside the square brackets by adding \\( [ 2 R / 2 ( K + 1 ) ]^2 - [ 2 R / 2 ( K + 1 ) ]^2 = [ R / ( K + 1 ) ]^2 - [ R / ( K + 1 ) ]^2 \\):

$$
x^2 + y^2 + ( K + 1 ) \left[ z^2 - \frac{ 2 R z }{ K + 1 } + \left( \frac{ R }{ K + 1 } \right)^2 - \left( \frac{ R }{ K + 1 } \right)^2 \right] = 0
$$

We can understand this a bit more generally by considering the expression \\( z^2 - a z \\). Here I need to add and subtract \\( ( a / 2 )^2 \\). The reason is that now we can rewrite the first three terms inside the square brackets as a squared binomial:

$$
x^2 + y^2 + ( K + 1 ) \left[ \left( z - \frac{ R }{ K + 1 } \right)^2 - \left( \frac{ R } { K + 1 } \right)^2 \right] = 0
$$

These last two steps complete the square. To place the equation into its normal form, I apply the Euclidean transformation \\( z' = z - \frac{ R }{ K + 1 } \\) and carry through the \\( K + 1 \\).

$$
x^2 + y^2 + ( K + 1 ) z^{ \prime 2 } - R^2 / ( K + 1 ) = 0
$$

The above equation is *almost* a normal form expression for a quadric. To finish the job, I would need to substiute in a specific value for the conic constant and divide through so that the constant is either -1, 0, or 1.

[^1]: The coefficients of each term need not be equal in general.
[^2]: A cylindrical lens does actually contain a quadric, but rather would consist of at least one toroidal surface. These are less common than lenses with spherical profiles, however.
