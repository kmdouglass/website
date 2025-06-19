<!--
.. title: Why is Camera Read Noise Gaussian Distributed?
.. slug: why-is-camera-read-noise-gaussian-distributed
.. date: 2025-06-19 10:40:12 UTC+02:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: true
-->

As a microscopist I work with very weak light signals, often just tens of photons per camera pixel. As a result, the images I record are noisy[^1]. To a good approximation, the value of a pixel is a sum of two random variables describing two different physical processes:

1. photon shot noise, which is described by a Poisson probability mass function, and
2. camera read noise, which is described by a Gaussian probability density function.

Read noise has units of electrons, which must be discrete. So why is it modeled as a continuous probability density function[^2]?



[^1]: I wrote a blog post about this a while back: https://kmdouglass.github.io/posts/modeling-noise-for-image-simulations/
[^2]: See Janesick, Photon Transfer, page 34
