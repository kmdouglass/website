<!--
.. title: A Very Brief Summary of The Analytic Signal in Fourier Optics
.. slug: a-very-brief-summary-of-the-analytic-signal-in-fourier-optics
.. date: 2025-04-01 15:53:21 UTC+02:00
.. tags: fourier optics, analytic signal, coherence
.. category: optics
.. link: 
.. description: I summarize the analytic signal representations for monochromatic and narrowband polychromatic scalar optical waves.
.. type: text
.. has_math: true
-->

# The Analytic Signal Representation of a Monochromatic Wave

## Monochromatic Scalar Waves

A monochromatic, scalar waveform is described by the expression:

$$ u \left( \mathbf{r}, t\right) = A ( \mathbf{r} ) \cos \left[2 \pi f_0 t + \phi \left( \mathbf{r} \right) \right] $$

- The signal is real-valued
- The signal has a known phase for all \\( t \\)

## The Analytic Signal

An analytic signal is a generalization of a phasor. It is used to represent a real-valued signal as a complex exponential or a sum of complex exponentials. When Goodman[^1] refers to a phasor, he often means the analytic signal. This is made clear in Chapter 6 where he describes the construction of the phasor for a narrowband signal as follows:

1. compute its Fourier transform
2. set the positive frequency components to zero
3. double the amplitudes of the negative frequency components
4. inverse Fourier transform the resulting one-sided spectrum

Strictly speaking, the analytic signal is obtained by setting the negative frequencies to zero and doubling by application of the Hilbert transform. However, many engineering fields have adopted the convention of setting the positive frequencies to zero instead. The results will be the same, except that the direction of power flow will be reversed (if I recall correctly).

The Fourier transform of \\( u \left( \mathbf{r}, t\right) \\) is:

$$ \mathcal{F} \left\\{ u \left( \mathbf{r}, t\right) \right\\} = \frac{A ( \mathbf{r} ) }{2} \left[ e^{j \phi \left( \mathbf{r} \right) } \delta \left( f - f_0 \right) + e^{-j \phi \left( \mathbf{r} \right) } \delta \left( f + f_0 \right) \right] $$

Drop the positive frequency term \\( e^{j \phi \left( \mathbf{r} \right) } \delta \left( f - f_0 \right) \\) and double the result. This produces:

$$ A ( \mathbf{r} ) e^{-j \phi \left( \mathbf{r} \right) } \delta \left( f + f_0 \right) $$

Let \\( U ( \mathbf{r} ) := A ( \mathbf{r} ) e^{-j \phi \left( \mathbf{r} \right) } \\). The inverse Fourier transform of this signal is:

$$ \mathcal{F}^{-1} \left\\{ U ( \mathbf{r} ) \delta \left( f + f_0 \right) \right\\} = U ( \mathbf{r} ) e^{-j 2 \pi f_0 t } $$

We can recover the original field by taking the real part of this expression, which is equivalent to applying Euler's identity and dropping the imaginary part:

$$ u \left( \mathbf{r}, t \right) = \Re \left[ U ( \mathbf{r} ) e^{-j 2 \pi f t} \right] $$

# Polychromatic Scalar Waves

To model a polychromatic wave, we integrate over the analytic signals of each spectral component and take the real part of the result:

$$ u \left( \mathbf{r}, t\right) = \Re \left[ \int_{-\infty}^{\infty} \tilde{U} \left( \mathbf{r}, f \right) e^{-j 2 \pi f t} \,df \right] $$

## The Narrowband Assumption

We get a useful representation to the expression above if we assume that the bandwidth of the signal is much smaller than its center frequency \\( \Delta f \ll f_0 \\):

$$ \int_{-\infty}^{\infty} \tilde{U} \left( \mathbf{r}, f \right) e^{-j 2 \pi f t} \,df = U \left( \mathbf{r}, t \right) e^{-j 2 \pi f_0 t} $$

To better understand the meaning of this assumption, make the substitution \\( \nu = f - f_0 \\) into the expression on the left hand side:

$$\begin{eqnarray}
\int_{-\infty}^{\infty} \tilde{U} \left( \mathbf{r}, f \right) e^{-j 2 \pi f t} \,df &=& \int_{-\infty}^{\infty} \tilde{U} \left( \mathbf{r}, \nu + f0 \right) e^{-j 2 \pi \left( \nu + f_0 \right) t} \,d\nu \\\\
&=& e^{-j 2 \pi f_0 t} \int_{-\infty}^{\infty} \tilde{U} \left( \mathbf{r}, \nu + f0 \right) e^{-j 2 \pi \nu t} \,d\nu
\end{eqnarray}$$

Under the narrowband assumption, the integration in the expression above is constrained to small values around \\( \nu = 0 \\) that are much less than the phasor term that is oscillating at frequency \\( f_0 \\). If we define the following function:

$$ U \left( \mathbf{r}, t \right) := \int_{-\infty}^{\infty} \tilde{U} \left( \mathbf{r}, \nu + f0 \right) e^{-j 2 \pi \nu t} \,d\nu $$ 

then it will vary slowly with respect to the carrier frequency \\( f_0 \\).

As a result, under the assumptions of narrowbandedness, we can interpret the complex function \\( U \left( \mathbf{r}, t \right) \\) as an "envelope" modulating the amplitude of the fast oscillating carrier wave. If the assumption is not valid, then this interpretation fails.

## The Slowly Varying Envelope Assumption

It is instructive to reverse our reasoning and see why a slowly-varying envelope implies a narrowband signal. Compute the Fourier transforms of the narrowband waveform, along with the Fourier transform of the derivative of \\( U \left( \mathbf{r}, t \right) \\).

The Fourier transform of the analytic signal:

$$ \int_{-\infty}^{\infty} \left[ U \left( \mathbf{r}, t \right) e^{-j 2 \pi f_0 t} \right] e^{-j 2 \pi f t} \,dt = \tilde{U} \left( \mathbf{r}, f + f_0 \right) $$

The Fourier transform of the derivative of \\( U \\):

$$\begin{eqnarray}
\int_{-\infty}^{\infty} \frac{d}{dt} \left[ U \left( \mathbf{r}, t \right) \right] e^{-j 2 \pi f t} \,dt &=& j 2 \pi f \tilde{U} \left( \mathbf{r}, f \right)
\end{eqnarray}$$

Now, apply the [slowly varying envelope approximation (SVEA)](https://en.wikipedia.org/wiki/Slowly_varying_envelope_approximation) by asserting that the rate of change of \\( U \\) with respect to time is much less than the value of \\( U \\) multiplied by the center frequency, or \\( \left| \frac{d}{dt} U \left( \mathbf{r}, t\right) \right| \ll \left| 2 \pi f_0 U \left( \mathbf{r, t} \right) \right| \\)

$$\begin{eqnarray}
\left| j 2 \pi f \tilde{U} \left( \mathbf{r}, f \right) \right| &=& \left| \int_{-\infty}^{\infty} \frac{d}{dt} \left[ U \left( \mathbf{r}, t \right) \right] e^{-j 2 \pi f t} \,dt \right| \\\\
&\ll& \left| \int_{-\infty}^{\infty} 2 \pi f_0 U \left( \mathbf{r}, t \right) e^{-j 2 \pi f t} \,dt \right| \\\\
&\ll& 2 \pi f_0 \left| \tilde{U} \left( \mathbf{r}, f \right) \right|
\end{eqnarray}$$

This expression means that the appreciable frequency components of \\( U \left( \mathbf{r} , t \right) \\) are much less than the frequency \\( f_0 \\)[^2]. And when we consider the spectrum of \\( U \left( \mathbf{r} , t \right) \\) centered around \\( f_0 \\), we find that the bandwidth \\( \Delta f \\) is small with respect to \\( f_0 \\).

## Assumptions, not Approximations!

The narrowband and slowly varying envelope assumptions are usually referred to as approximations. This is misleading! The resulting expression for the field is not an approximation at all; instead, under the assumptions of narrowbandedness, we can interpret the complex function \\( U \left( \mathbf{r}, t \right) \\) an "envelope" modulating the amplitude of the fast oscillating carrier wave. If the assumption is not valid, then this interpretation is not correct.

## Narrowband Polychromatic Waves

In summary, narrowband polychromatic waves with a center frequency \\( f_0 \\) are modeled as the product of a fast rotating phasor and slowly varying envelope:

$$ u \left( \mathbf{r}, t \right) = \Re \left[ U \left( \mathbf{r}, t \right) e^{-j 2 \pi f_0 t} \right] $$

The amplitude and the phase of the envelope are the amplitude and phase of the real optical wave.

# Coherence

While the expression for the analytic signal \\( U \left( \mathbf{r}, t \right) \\) as an integral over frequency components appears deterministic, the phase relationships between the spectral components are often unknown and vary randomly in time. As a result, the envelope of the optical wave will vary unpredictably and must be analyzed in terms of its statistical properties.

## Monochromatic Light is Coherent

Since monochromatic light has only one spectral component by definition, it is completely coherent.

- I mean monochromatic in the ideal sense, not like how we sometimes describe lasers.
- Monochromatic waves, like plane waves, cannot exist in real life. The uncertainy principle requires that a monochromatic wave exist for an infinite duration.


[^1]: Goodman, Joseph W. Introduction to Fourier optics. Roberts and Company publishers (2005). ISBN 978-0974707723.
[^2]: [https://physics.stackexchange.com/questions/451239/slowly-varying-envelope-approximation-what-does-it-imply](https://physics.stackexchange.com/questions/451239/slowly-varying-envelope-approximation-what-does-it-imply)
