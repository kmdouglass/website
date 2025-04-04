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

# Solution to Homework Problem 4.16

Problem 4.16 is important because it is a basis for the development of the frequency analysis of image-forming systems in later chapters of Goodman.

The purpose of 4.16 is to show that the diffraction pattern of an aperture that is illuminated by a spherical converging wave in the Fresnel regime is the Fraunhofer diffraction pattern of the aperture.

## Part a: Quadratic phase approximation to the incident wave

Let \\( z = 0 \\) be the plane of the aperture and \\( z = Z \\) be the observation plane. Additionally, let \\( \left( \xi, \eta \right) \\) represent the coordinates in the plane of the aperture, and \\( \left( x, y \right) \\) the coordinates in the observation plane. The spherical wave that illuminates the aperture is convering to a point \\( \vec{r}_P = Y \hat{ \jmath} + Z \hat{k} \\) in the observation plane.

To find a quadratic phase approximation for the incident wave, start with its representation as a time-harmonic spherical wave of amplitude \\( A \\):

$$ U \left( x, y, z \right) = A \frac{e^{j k |\vec{r} - \vec{r}_P|}}{|\vec{r} - \vec{r}_P|} $$

Note that \\( \vec{r} - \vec{r}_P = x \hat{\imath} + \left( y - Y \right) \hat{\jmath} + \left( z - Z \right) \hat{k} \\). Its magnitude is

$$\begin{eqnarray}
| \vec{r} - \vec{r}_P | &=& \sqrt{x^2 + \left( y - Y \right)^2 + \left( z - Z \right)^2} \\\\
&=& \left( z - Z \right) \sqrt{1 + \frac{x^2 + \left( y - Y \right)^2}{\left( z - Z \right)^2} } \\\\
&\approx& \left( z - Z \right) + \frac{ x^2 + \left( y - Y \right)^2 }{2 \left( z - Z \right)}
\end{eqnarray}$$

At first glance, there's a problem here because allowing \\( \left( z - Z \right) \\) to be negative will result in a negative value for the magnitude of the vector \\( \left( \hat{r} - \hat{r}_P \right) \\). However, if we use the above table for selecting \\( e^{j k r} \\) as the phasor for a converging wave when \\( \left( z - Z \right) \\) is negative, then we will have the correct sign of the argument to the phasor. We do however need to take the absolute value of the \\( z - Z \\) term in the denominator of the expression of the spherical wave.

Replacing the distance in the phasor's argument with the two lowest order terms in the binomial expansion and the lowest order term in the denominator:

$$ U \left( x, y, z \right) \approx A \frac{e^{j k \left(z - Z \right)} e^{j k \left[ x^2 + \left( y - Y \right)^2 \right] / 2 \left(z - Z \right) }}{\left|z - Z \right|} $$

In the \\( z = 0 \\) plane, this becomes:

$$ U \left( x, y; z = 0 \right) \approx A \left(x, y \right) \frac{e^{-j k Z} e^{-j k \left[ x^2 + \left( y - Y \right)^2 \right] / 2 Z }}{Z} $$

I moved the finite extent of the aperture into a new function for the amplitude \\( A \\) above. This function is zero outside the aperture and a constant \\( A \\) inside it.

## Part b: Diffraction pattern at the point \\( P \\)

Use the second form of the Fresnel diffraction integral to compute the diffraction pattern at \\( P \\):

$$\begin{eqnarray}
U \left( x = 0, y = Y, z = Z \right) &=& \frac{ e^{jkZ} }{j \lambda Z} e^{\frac{j k Y^2}{2 Z}} \iint_{-\infty}^{\infty} \left[ U \left( \xi , \eta ; z = 0 \right) e^{\frac{j k}{2 Z} \left( \xi^2 + \eta^2 \right)} \right] e^{-j \frac{2 \pi }{\lambda Z} y \eta }  \,d\xi \,d\eta \\\\
&\approx& \frac{ e^{jkZ} e^{-jkZ} }{j \lambda Z^2} e^{\frac{j k Y^2}{2 Z}} \iint_{-\infty}^{\infty} A \left(\xi, \eta \right) \left[ e^{-\frac{j k}{2Z} \left[ \xi^2 + \left( \eta - Y \right\)^2 \right]} e^{\frac{j k}{2 Z} \left( \xi^2 + \eta^2 \right)} \right] e^{-j \frac{2 \pi }{\lambda Z} y \eta }  \,d\xi \,d\eta \\\\
&\approx& \frac{1}{j \lambda Z^2} e^{\frac{j k Y^2}{2 Z} } \iint_{-\infty}^{\infty} A \left(\xi, \eta \right) \left[ e^{-\frac{j k}{2Z} \left( \xi^2 + \eta^2 - 2 \eta Y + Y^2 \right)} e^{\frac{j k}{2 Z} \left( \xi^2 + \eta^2 \right)} \right] e^{-j \frac{2 \pi }{\lambda Z} y \eta }  \,d\xi \,d\eta \\\\
&\approx& \frac{1}{j \lambda Z^2} \iint_{-\infty}^{\infty} A \left(\xi, \eta \right) e^{\frac{j k \eta Y}{Z}} e^{-j \frac{2 \pi}{\lambda Z} y \eta }  \,d\xi \,d\eta \\\\
&\approx& \frac{1}{j \lambda Z^2} \iint_{-\infty}^{\infty} A \left(\xi, \eta \right) e^{-j \frac{2 \pi }{\lambda Z} \left(\eta - Y \right) }  \,d\xi \,d\eta
\end{eqnarray}$$

The final expression above is proportional to the Fraunhofer diffraction pattern of the aperture. The reason that the Fraunhofer diffraction pattern appears as the result is that the converging spherical wavefronts exactly cancel the diverging quadratic phase term inside the Fresnel diffraction formula, leaving a simple Fourier transform of the aperture as a result.

[^1]: Goodman, Joseph W. Introduction to Fourier optics. Roberts and Company publishers (2005). ISBN 978-0974707723.
