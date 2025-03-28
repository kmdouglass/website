<!--
.. title: A Very Brief Summary of Fresnel and Fraunhofer Diffraction Integrals
.. slug: a-very-brief-summary-of-fresnel-and-fraunhofer-diffraction-integrals
.. date: 2025-03-28 09:06:03 UTC+01:00
.. tags: diffraction, Fresnel, Fraunhofer
.. category: optics
.. link: 
.. description: I summarize the knowledge that I find essential to understanding the Fresnel and Fraunhofer diffraction integrals.
.. type: text
.. has_math: true
-->

Fourier Optics is complicated, and though I have internalized its concepts over the years, I often still need to review the specifics of its mathematical models. Unfortunately, my go-to resource for this, Goodman's Fourier Optics[^1], tends to disperse information across chapters and homework problems. This makes quick review difficult.

Here I condense what I think are the essentials of Fresnel and Fraunhofer diffraction into one blog post.

# Starting Point: the Huygens-Fresnel Principle

Ignore Chapter 3 of Goodman; it's largely irrelevant for practical work. The Huygens-Fresnel principle itself is a good intuitive model to start with.

## The Model

An opaque screen with a clear aperture \\( \Sigma \\)  is located in the \\( z = 0 \\) plane with transverse coordinates \\( \left( \xi , \eta \right ) \\). It is illuminated by a complex-valued scalar field \\( U \left( \xi, \eta \right) \\). Let \\( \vec{r_0} = \left( \xi, \eta, 0 \right) \\) be a point in the plane of the aperture and \\( \vec{r_1} = \left( x, y, z \right) \\) be a point in the observation plane. The Huygens-Fresnel Principle provides the following formula for the diffracted field \\( U \left( x, y \right) \\) in the plane \\( z \\):

$$ U \left( x, y; z \right) = \frac{z}{j \lambda} \iint_{\Sigma} U \left( \xi , \eta \right) \frac{\exp \left( j k r_{01} \right)}{r_{01}^2} \, d\xi d\eta $$

with the distance \\( r_{01}^2 = \left( x - \xi \right)^2 + \left( y - \eta \right)^2 + z^2 \\).

- We assumed an obliquity factor \\( cos \, \theta = z / r_{01}\\). The choice of obliquity factor depends on the boundary conditions discussed in Chapter 3, but again this isn't terribly important for practical work.
- The integral is a sum over secondary spherical wavelets emitted by each point in the aperture and weighted by the incident field and the obliquity factor.
- The factor \\( 1 / j \\) means that each secondary wavelet from a point \\( \left( \xi, \eta \right) \\) is 90 degrees out-of-phase with the incident field at that point.

### Approximations used in the Huygens-Fresnel Principle

1. The electromagnetic field can be approximated as a complex-valued scalar field.
2. \\( r_{01} \gg \lambda \\), or the observation screen is many multiples of the wavelength away from the aperture.

# The Fresnel Diffraction Integral

## The Fresnel Approximation

Rewrite \\( r_{01} \\) as:

$$ r_{01} = z \sqrt{ 1 + \frac{\left( x - \xi \right)^2 + \left( y - \eta \right)^2}{z^2} } $$

Apply the binomial approximation:

$$ r_{01} \approx z + \frac{\left( x - \xi \right)^2 + \left( y - \eta \right)^2}{2z} $$

In the Huygens-Fresnel diffraction integral, replace:

1. \\(r_{01}^2 \\) in the denominator with \\( z^2 \\)
2. \\(r_{01}\\) in the argument of the exponential with \\( z + \frac{\left( x - \xi \right)^2 + \left( y - \eta \right)^2}{2z} \\)

### The Diffraction Integral: Form 1

Perform the substitutions for \\( r_{01} \\) into the Huygens-Fresnel formula that were mentioned above to get the first form of the Fresnel diffraction integral:

$$ U \left( x, y; z \right) = \frac{ e^{jkz} }{j \lambda z} \iint_{-\infty}^{\infty} U \left( \xi , \eta \right) \exp \left\\{ \frac{jk}{2z} \left[ \left( x - \xi \right)^2 + \left( y - \eta \right)^2 \right] \right\\}  \,d\xi \,d\eta $$

- It is space invariant, i.e. it depends only on the differences in coordinates \\( \left( x - \xi \right) \\) and \\( \left( y - \eta \right) \\).
- It represents a convolution of the input field with the kernel \\( h \left( x, y \right) = \frac{e^{j k z}}{j \lambda z} \exp \left[ \frac{j k}{2 z} \left( x^2 + y^2 \right) \right] \\).

### The Diffraction Integral: Form 2

Expand the squared quantities inside the parantheses of Form 1 to get the second from of the integral:

$$ U \left( x, y; z \right) = \frac{ e^{jkz} }{j \lambda z} e^{\frac{j k}{2 z} \left( x^2 + y^2 \right)} \iint_{-\infty}^{\infty} \left[ U \left( \xi , \eta \right) e^{\frac{j k}{2 z} \left( \xi^2 + \eta^2 \right)} \right] e^{-j \frac{2 \pi }{\lambda z} \left( x \xi + y \eta \right) }  \,d\xi \,d\eta $$

- It is proportional to the Fourier transform of the product of the incident field and a parabolic phase curvature \\( e^{\frac{j k}{2 z} \left( \xi^2 + \eta^2 \right)} \\).

# Phasor Conventions

Section 4.2.1 of Goodman is an interesting practical aside about how to identify whether a spherical or parabolic wavefront is converging or diverging based on the sign of its phasor. It is useful for solving the important homework problem 4.16 which concerns the diffraction pattern from an aperture that is illuminated by a converging spherical wave.

Unfortunately, Figure 4.2 does not align well with its description in the text about negative z-values, and it's not clear how the interpretations change for point sources not at \\( z = 0 \\). I address this below.

- Let the point of convergence (or center of divergence) of a spherical wave sit on the z-axis at \\( z = Z \\).
- The phasor describing the time-dependent part of the field in Goodman's notation is \\( e^{-j 2 \pi f t} \\).
- If we move away from the center of the wave such that \\( z - Z \\) is positive and we encounter wavefronts emitted earlier in time, then \\( t \\) is decreasing and the argument to the phasor is increasing. The wave is therefore diverging if the argument is positive.
- If we move away from the center of the wave  such that \\( z - Z \\) is negative and we encounter wavefronts emitted earlier in time, then \\( t \\) is decreasing and the argument to the phasor is increasing. However, a negative \\( z - Z \\) makes the phasor negative again so that it is in fact decreasing. The wave is therefore diverging if the argument is negative.
- Likewise for converging waves.

To summarize:

<table border="1">
  <thead>
    <tr>
      <th>Phasor</th>
      <th> \( \left( z - Z \right) \) positive </th>
      <th> \( \left( z - Z \right) \) negative </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>\( e^{ j k r} \)</td>
      <td>diverging</td>
      <td>converging</td>
    </tr>
    <tr>
      <td>\( e^{ -j k r} \)</td>
      <td>converging</td>
      <td>diverging</td>
    </tr>
  </tbody>
</table>

# The Fraunhofer Diffraction Integral

## The Fraunhofer Approximation

Assume we are so far from the screen that the quadratic phasor inside the diffraction integral is effectively flat. This means: 

$$ z \gg \frac{k \left( \xi^2 + \eta^2 \right)_{\text{max}}}{2} $$

## The Diffraction Integral

Applying the approximation above allows us to drop the quadratic phasor inside the Fresnel diffraction integral because it is effectively 1:

$$ U \left( x, y; z \right) = \frac{ e^{jkz} }{j \lambda z} e^{\frac{j k}{2 z} \left( x^2 + y^2 \right)} \iint_{-\infty}^{\infty} U \left( \xi , \eta \right) e^{-j \frac{2 \pi }{\lambda z} \left( x \xi + y \eta \right) }  \,d\xi \,d\eta $$

- Apart from the phase term that depends on \\( z \\), this expression represents a Fourier transform of the incident field.
- It appears to break spatial invariance because we no longer depend on differences of coordinates, e.g. \\( x - \xi \\). However, we can still use the Fresnel transfer function (the Fourier transform of the Fresnel convolution kernel) as the transfer function for Fraunhofer diffraction because if the Fraunhofer approximation is valid, then so is the Fresnel approximation.

[^1]: Goodman, Joseph W. Introduction to Fourier optics. Roberts and Company publishers (2005). ISBN 978-0974707723.
