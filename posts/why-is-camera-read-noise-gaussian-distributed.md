<!--
.. title: Why is Camera Read Noise Gaussian Distributed?
.. slug: why-is-camera-read-noise-gaussian-distributed
.. date: 2025-06-19 10:40:12 UTC+02:00
.. tags: cameras, microscopy, statistics
.. category: optics
.. link: 
.. description: 
.. type: text
.. has_math: true
-->

As a microscopist I work with very weak light signals, often just tens of photons per camera pixel. As a result, the images I record are noisy[^1]. To a good approximation, the value of a pixel is a sum of two random variables describing two different physical processes:

1. photon shot noise, which is described by a Poisson probability mass function, and
2. camera read noise, which is described by a Gaussian probability density function.

Read noise has units of electrons, which must be discrete, positive integers. So why is it modeled as a continuous probability density function[^2]?

# The Source(s) of Read Noise

Janesick[^3] defines read noise as "any noise source that is not a function of signal." This means that there is not necessarily one single source of read noise. It is commonly understood that it comes from somewhere in the camera electronics, but "somewhere" need not imply that it is isolated to one location.

The signal from a camera pixel is the number of photoelectrons that were generated inside the pixel. I imagine readout of this signal as a linear path consisting of many steps. The signal might change form along this path, such as going from number of electrons to a voltage. At each step, there is a small probability that some small error is added to (or maybe also removed from?) the signal. The final result is a value that differs randomly from the original signal.

Importantly, I do not think that it matters which physical process each step actually represents; rather there just has to be many of them for this abstraction to be valid.

# Read Noise is Gaussian because of the Central Limit Theorem

The reason for my conclusion that we can ignore the details so long as there are many steps is the following:

We can model the error introduced by each step as a random variable. Let's assume that each step is independent of the others. The result of camera readout is a sum of a large number of independent random variables. And of course the [Central Limit Theorem](https://en.wikipedia.org/wiki/Central_limit_theorem) states that the distribution of the sum of random variables tends towards a normal distribution, i.e. Gaussian, as the number of random variables tends towards infinity. This happens regardless of the distributions of the underlying random variables.

So read noise can appear to be effectively Gaussian so long as there are many steps along the path of conversion from photoelectrons to pixel values and each step has a chance of introducing an error.

## Sums of Discrete Random Variables

I encountered one conceptual difficulty here: the sum of discrete random variables is still discrete. If I have several variables that produce only integers, their sum is still an integer. I cannot get, say, 3.14159 as a result. Does the Gaussian approximation, which is for continuous random variables, still apply in this case?

Let's say that I have a discrete random variable that can assume values of 0 or 1, and the probability that the value is 1 is denoted \\( p \\). This is known as a Bernoulli trial. Now let's say that I have a large number \\( n \\) of Bernoulli trials. But the sum of \\( n \\) Bernoulli trials has a distribution that is binomial, and this is well-known to be approximated as a Gaussian when certain conditions are met, including large \\( n \\)[^4]. So a sum of a large number of discrete random variables can have a probability distribution function that is approximated as a Gaussian. 

**This does not mean that the sum of discrete random variables can take on continuous values.** Rather, the probability associated with any one output value is approximately given by a Gaussian probability density function sampled at the same value[^5].

I think that I struggled with this because I was erroneously concluding that random variables that are approximately described by Gaussian probability distributions should produce samples that can be drawn from a Gaussian probability density function. 3.14159 is a valid sample from a Gaussian probability density function, but not for a random variable whose domain is the set of positive integers.

[^1]: I wrote a blog post about this a while back: [https://kmdouglass.github.io/posts/modeling-noise-for-image-simulations/](https://kmdouglass.github.io/posts/modeling-noise-for-image-simulations/)
[^2]: This is often asserted without justification. See for example Janesick, Photon Transfer, page 34.
[^3]: [https://doi.org/10.1117/3.725073](https://doi.org/10.1117/3.725073)
[^4]: [https://en.wikipedia.org/wiki/Binomial_distribution#Normal_approximation](https://en.wikipedia.org/wiki/Binomial_distribution#Normal_approximation)
[^5]: Technically, you need to apply what is known as a continuity correction to ensure that the set of possible discrete outcomes sample the Gaussian at the positions that produce the best approximation to the true probabilities. This is like choosing to sample the Gaussian at the midpoints between integers instead of at the discrete integer values themselves because the result is closer to the true value. [https://en.wikipedia.org/wiki/Continuity_correction#Binomial](https://en.wikipedia.org/wiki/Continuity_correction#Binomial)
