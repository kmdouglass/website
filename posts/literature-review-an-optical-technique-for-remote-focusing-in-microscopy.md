<!--
.. title: Literature Review: An Optical Technique for Remote Focusing in Microscopy
.. slug: literature-review-an-optical-technique-for-remote-focusing-in-microscopy
.. date: 2024-05-22 13:43:39 UTC+02:00
.. tags: microscopy, optics
.. category: literature review
.. link: 
.. description: 
.. type: text
-->

### Citation

[E.J. Botcherby, R. Juškaitis, M.J. Booth, T. Wilson, "An optical technique for remote focusing in microscopy," Optics Communications, Volume 281, Issue 4, 2008, Pages 880-887](https://doi.org/10.1016/j.optcom.2007.10.007)

### Abstract

> We describe the theory of a new method of optical refocusing that is particularly relevant for confocal and multiphoton microscopy systems. This method avoids the spherical aberration that is common to other optical refocusing systems. We show that aberration-free refocusing can be achieved over an axial scan range of 70 μm for a 1.4 NA objective lens. As refocusing is implemented remotely from the specimen, this method enables high axial scan speeds without mechanical interference between the objective lens and the specimen.

# Reasons for this Review

I am interested in this paper for two reasons:

1. Recent advances in light sheet microscopy have made the theory of remote focusing more relevant than in the past.
2. The paper presents a simplified theory of imaging by a high numerical aperture (NA) objective that is useful for understanding image formation in microscopes without resorting to the usual (and more complicated) Richards and Wolf description.

# Problem Addressed by the Paper

The introduction lays out the reasons for this paper in a straightforward manner:

- The primary bottleneck in 3D microscopy is axial scanning of the sample (what the authors call **refocusing**).
- Due to fundamental optics, refocusing a high resolution microscope involves varying the objective/sample distance, i.e. the image plane must remain fixed.
- It would be desirable to develop a **simple** mechanism whereby the objective or sample need not move to achieve refocusing in such microscopes without introducing unwanted aberrations.
    - This is because samples are becoming more complex (think embryos, organoids, etc.).
    - Adaptive optics to fix these aberrations would introduce too much complexity into the setup. (More on this later.)

# Theory of 3D Imaging in Microscopes

The theory of 3D imaging is introduced by first considering a perfect imaging system with an object space refractive index of \\( n_1 \\) and an image space refractive index of \\( n_2 \\). Such a system transforms all the rays emanating from any point in the 3D object space to converge to a single point in the 3D image space. An image formed by such a system is known as a **stigmatic** image. Unfortunately, Maxwell, followed by Born and Wolf, showed that such a system is only possible if the magnification is the same in all directions and with magnitude

$$ \left| M \right| = \frac{n_1}{n_2} $$

This also implies that conjugate rays must have the same angle with respect to the optical axis.

$$ \gamma_2 = \pm \gamma_1 $$

Any system that does not meet these criteria is not a perfect imaging system. However, there exist some conditions whereby the system can create a perfect image if their requirements are satisfied. Under these conditions, a perfect image will be created only for objects of limited extent in the object space. The two conditions that are relevant for microscopy are

1. the sine condition, and
2. the Herschel condition.

Under the sine condition, points in a plane transverse to the optical axis are imaged perfectly onto the image plane; points that lie at some axial distance from the object plane suffer from spherical aberration and their images are not stigmatic. In some sense, the Herschel condition is the opposite: on-axis points are imaged stigmatically regardless of their axial position, but off-axis points suffer from aberrations.

The authors note the important fact that most microscope objectives are designed to satisfy the sine condition. As a result, the image plane must remain fixed so that aberration-free refocusing can only be achieved by varying the sample-objective distance. In the authors' words:

> ...it is possible to see why commercial microscopes, operating under the sine condition refocus by changing the distance between the specimen and objective, as any attempt to detect images away from the optimal image plane will lead to a degradation by spherical aberration.

## Questions

1. Does an ideal imaging system need only produce stigmatic images, or must it also accurately reproduce the relative positions between any pair of points in the image space (up to a proportionality factor)? 
2. What exactly are the defintions of the sine and Herschel conditions? Is it the equations relating the angles of conjugate rays? Is it based on the subset of the object space that is imaged stigmatically? Or, as we'll see in the next section, are they defined by the mapping of ray heights between principal surfaces? The authors present a few attributes of each condition, but I'm not certain which attributes serve as the definitions and which are consequences of their assumptions being true.

# The General Pupil Function

I really liked this section. The authors present a model of a high NA microscope objective that is based on its principal surfaces. They then use a mix of scalar wave theory and ray tracing to explain why the sine condition produces stigmatic images for points near the axis in the focal plane of the objective. I think the value in this model is that it is much more approachable than the electromagnetic Richards and Wolf model for aplanatic systems.

To recall, the principal planes in paraxial optics are used to abstract away the details of a lens system. Refraction effectively occurs at these planes, and the focal length is measured relative to them. In non-paraxial systems, the principal planes actually become curved surfaces. Interestingly, most of the famous optics texts, such as Born and Wolf, are somewhat quiet about this fact, but it can be found in papers such as [Mansuripur, Optics and Photonics News, 9, 56-60 (1998)](https://opg.optica.org/opn/abstract.cfm?uri=opn-9-2-56).

So a high NA objective is modeled as a pair of principal surfaces:

1. The first is a sphere centered on the axis with a radius of curvature equal to the focal distance
2. The second is a plane perpendicular to the axis, and they refer to it as the pupil plane

Another important thing to note is that these surfaces **are not** the usual reference spheres centered about object and image points and located in the entrance/exit pupils. I think the authors are right to use principal surfaces because many modern objectives are object-space telecentric, which places the entrance pupil at infinity. In this case the concept of a reference sphere sitting in the entrance pupil becomes a bit murky and I do not know whether it's applicable.

In any case, the authors compute the path length differences between points in the object space in this system and use the sine and Herschel conditions to map the rays from the object to the image space principal surfaces. (Each condition results in a different mapping.) Under the approximation that the extent of the object is small, the equations for the path length differences demonstrate what was stated in the previous section: that the sine condition leads to spherical aberration for points that do not lie in the focal plane of the objective. In fact, the phase profile of the wave (the authors weave between ray and wave optics) exiting the second principal plane is expanded as:

$$ znk \left[ 1 - \frac{\rho^2 \sin^2 \alpha}{2} + \frac{\rho^4 \sin^4 \alpha}{8} + \cdots \right] $$

For \\( z = 0 \\), i.e. the object is in the focal plane, all the terms disappear and we get a flat exit wave. When \\( z \neq 0 \\):

> Focussing the tube lens is accurately described by the quadratic term, as it operates in the paraxial regime. Unfortunately the higher order terms which represent spherical aberrations cannot be focussed by the tube lens and consequently there is a breakdown of stigmatic imaging for these points.

In other words, **under the sine condition, object points that are outside the focal plane produce curved, non-spherical wavefronts that cannot be focussed to a single point by a tube lens.**

If, however, another lens in a reversed orientation was placed so that the curved wavefront from the objective was input into it, it would form a stigmatic image in its image space. This suggests a method for remote focussing.

## Questions

1. Is the second principal surface flat because the image is formed at infinity by a high NA, infinity-corrected objective? What would its radius of curvature be in a finite conjugate objective?
2. Is the authors' pupil plane coplanar with the objective's exit pupil? Probably not; I think they're referring to the plane in which we find the objective's pupil function, which is somewhat standard (and confusing) nomenclature in microscopy.

# A Technique for Remote Focusing

We arrive now at the crux of the paper. The authors suggest a setup for remote focusing that is free (within limits) of the spherical aberration that is introduced by objectives that satisfy the sine condition. Effectively they image the pupil from one objective onto the other with a 4f system. This ensures that the aberrated wavefront from the first objective is "unaberrated" by the second objective. Then, another microscope images the focal region of the second objective. 3D scanning is achieved by moving the objective of the second microscope (often called O3 in light sheet microscopes).

There are a few important points:

- A 4f system needs to be used between the first (O1) and second (O2) objectives to relay the pupil because it faithfully maps the wavefront without adding any additional phase distortion.
- On a related note, you can't use tube lenses in the 4f system that are not afocal with the objective. These so-called widefield tube lenses do not share a focal plane with the objective. The objective's pupil must be in the front focal plane of the 4f system.
- The "perfect" imaging system of O1/4f system/O2 will have an isotropic magnification of \\( n1 / n2 \\). This satisfies Maxwell's requirement for 3D stigmatic imaging.
- This approach will not work well for objectives that require specific tube lenses for aberration correction. (Sorry Zeiss.)
- You will not lose resolution as long as the second objective has a higher *angular aperture* (not numerical aperture). You can, for example, use a NA 1.4 oil objective for O1 and a NA 0.95 dry objective for O2 because the O2 object space is in air, whereas the O1 object space is in oil with \\( n \approx 1.5 \\). From the definition of numerical aperture, the sine of the limiting angle of O1 must necessarily be smaller than the air objective.

At this point I found it amusing that the authors cited "complexity" as a reason for why their approach is superior to adaptive optics in the introduction of this paper.

## Questions

1. The authors suggest a different approach where a mirror is placed after O2 so that it also serves as O3 and use a beam splitter to direct the light leaving O2 onto a camera. Why don't light sheet microscopes use this setup? Is it because of a loss of photons due to the beam splitter?

# Range of Operation

The equation for the path length difference between points in object space depends on the assumption of small object distances. This assumption places a limit on the range of validity of this approach. To quantify this limit, the authors computed the Strehl ratio of the phase of the wavefront in the pupil. Honestly, the calculations of this section look tedious. In the end, and after "some routine but rather protracted calculations, a simple result emerges." The simple result looks kind of ugly, depending, among other things on the sine to the eigth power of the aperture angle. It looks like the approach is valid for distances of several tens of microns on both sides of the focal plane of O1, which is in fact quite useful for many biological samples.

Ironically, the authors decide at this point that adaptive optics, the approach to remote focusing that is too complex, probably isn't that bad after all. It can be used to extend the range of validity of the authors' approach by correcting the higher order terms that are dropped in the binomial expansion for the optical path difference.

# Summary

The authors go on to experimentally verify the approach in a rather unremarkable experiment of taking z-stacks of beads in two different setups. The PSF in their approach is much less aberrated than a normal widefield microscope over an axial range of about \\( \pm 40 \mu m \\).

Overall I quite like the paper because of its simplified theoretical model and clear explantion of the sine condition. I would argue, though, that the approach is not necessarily less complex than some of the alternatives that they rule out in the introduction. Admittedly, arguments over complexity are usually subjective and this doesn't necessarily mean the paper is of low quality. Given that many light sheet approaches are now based on this method, the paper serves as a good theoretical grounding into why remote focusing works and, in some cases, may be necessary.
