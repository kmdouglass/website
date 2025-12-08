<!--
.. title: Literature Review: Clarification and Unification of the Obliquity Factor in Diffraction and Scattering Theories
.. slug: literature-review-clarification-and-unification-of-the-obliquity-factor-in-diffraction-and-scattering-theories
.. date: 2025-12-08 15:10:53 UTC+01:00
.. tags: microscopy, optics
.. category: literature review 
.. link: 
.. description: What does it mean for something be thin?
.. type: text
.. has_math: true
-->

### Citation

[Yijun Bao and Thomas K. Gaylord, "Clarification and Unification of the Obliquity Factor in Diffraction and Scattering Theories: Discussion,"  J. Opt. Soc. Am. A 34, 1738-1745 (2017)](https://doi.org/10.1364/JOSAA.34.001738)

### Abstract

> Two-dimensional (2D) and three-dimensional (3D) diffraction theories form the underlying basis of quantitative phase imaging. This paper reviews how 2D and 3D diffraction theories are developed based on thin and thick object requirements. However, some previously reported work has mixed 2D and 3D theories. This discussion shows that it is possible to enable consistent mixed use of 2D and 3D theories by applying appropriate obliquity factor (OF) modifications. The discussion is concluded with an overall unifying representation for the usage of the OF modifications in 2D and 3D diffraction theories as applied to both thin and thick objects.

# Reason for this Review

I often notice that articles concerning quantitative phase imaging (QPI) are unclear about what is meant by 2D and 3D objects. The article helps to clarify this point.

# Summary of the Paper

The paper addresses the problem of when to use 2D and 3D diffraction theories in forward models of image formation in a microscope. I will refer to this problem as the choice of dimensionality. In addition, the authors highlight the choice of whether an object may be treated as a "thick" object or a "thin" object, which is not the same as the choice of dimensionality. I call this the choice of object model.

Ultimately, a decision matrix is constructed in which the dimensionality and object model serve as inputs. The output is the form of the obliquity factor, a term found in all diffraction integrals relating to the Huygens-Fresnel principle and that is used to prevent backward energy flow of the wave field[^1]. The correct forms of the obliquity factor (OF) are then used to allow the application of 2D diffraction theory to thick objects (the so-called Type-1 OF modification) and 3D diffraction theory to thin objects (the Type-2 OF modification).

The unifying theory is meant to address a problem of consistency in the authors' previous work, but I think that it is important on a more fundamental level.

# Diffraction Theory

## 2D Diffraction Theory

2D diffraction theory follows from the well-known developments of Huygens, Fresnel, Kirchoff, Rayleigh, and Sommerfeld. The integral equation for 2D diffraction is:

$$ u ( \vec{x}, z ) = \frac{1}{j \lambda }\int u_{inc} ( \vec{x}', 0) t ( \vec{x}' ) \frac{e^{ j k \sqrt{ (\vec{x} - \vec{x}' ) + z^2 } } }{\sqrt{ (\vec{x} - \vec{x}' ) + z^2 }} K ( \vec{x}, z; \vec{x} ') \, d \vec{x}' . $$

In words, the above expression determines the field \\( u \\) at transverse coordinate \\( \vec{x} \\) and axial coordinate \\( z \\) due to an incident field \\( u_{inc} \\) on a 2D complex transmission screen \\( t \\) at \\(z = 0 \\). \\( K \\) is the obliquity factor, takes the form \\( cos(\theta) \\) in the first Rayleigh-Sommerfeld diffraction integral, \\( \theta \\) being the angle between the normal to the screen and the line from the origin to the point of observation.

This expression is equivalent to Eq. 3-41 of Goodman[^2].

### Assumption of the 2D Theory

The most important assumption in 2D diffraction theory is that the object is thin. I think the authors give a somewhat unsatisfactory definition of "thin," stating:

> A thin object usually means that the light exits the object approximately at the same transveral coordinate as it enters the object, or the transversal deviation of light can be neglected.

For this definition they cite Goodman[^2]. I think they are specifically referring to this passage in Chapter 5, section 1:

> With reference to Appendix B, a lens is said to be a thin lens if a ray entering at coordinates (x, y) on one face exits at approximately the same coordinates on the opposite face, i.e. if there is negligible translation of a ray within the lens. Thus a thin lens simply delays an incident wavefront by an amount proportional to the thickness of the lens at each point.

I find the defintion unsatisfactory partly because Goodman is specifically talking about rays and lenses, whereas Bao and Gaylord are talking about waves and inhomogeneous media. At the end of this post I provide a more detailed critique and a possible solution.

Mathematically, this assumption allows us to write the refractive index difference due to the screen as

$$ \Delta n ( \vec{x}, z) = \phi ( \vec{x} ) \delta ( z ) / k $$

where \\( \delta \\) is the Dirac delta function. The delta function is ultimately the source of the difficulties in reconciling the 2D and 3D theories.

## 3D Diffraction Theory

By "3D diffraction theory," Bao and Gaylord are referring to what I normally think of as "scattering." In particular, under the first Born approximation:

$$ u ( \vec{r} ) = u_{inc} ( \vec{r} ) + \int u_{inc} ( \vec{r}' ) F ( \vec{ r }' ) G ( \vec{r}, \vec{r}' ) \, d \vec{r}' .$$

This expression states that the total field diffracted (i.e. scattered) by a 3D scattering potental \\( F \\) is the sum of the incident field and an integral over source terms \\( u_{inc} ( \vec{r}') F ( \vec{r}' ) \\) multiplied by the Greens function \\( G \\).

### Assumptions of the 3D Theory

In deriving the integral expression above, Born and Wolf[^3] note in chapter 13 that the gradient of the object's refractive index must be small so that the electric field components can be decoupled, thereby reducing the vector theory to a scalar one. Additionally, the first Born approximation requires that the refractive index contrast of the object be small.

The authors rightly point out that these assumptions are in contradiction with what constitutes a thin object. As a result of the delta function in the expression for \\( \Delta n \\) of a thin object, the refractive index gradient is huge and 3D diffraction theory should not apply. More specifically, the scalar approximation to the vector Helmholtz equation should be invalid, and it is this approximation that leads to the integral equation of potential scattering.

The authors further state that another consequence of a thin object is that the *values* of the refractive index become large, which violates the assumption of the first Born approximation. Here I am less certain of their argument, and I think the reason again is due to the nature of the delta function. I think that their argument is this: the refractive index values are large because the delta function technically has an infinite value. But I usually think that the delta function by itself is physically meaningless unless integrated over, which is why I am less certain of the strength of this argument.

# An Inconsistency Arises

When applied to a thin object, the two different theories lead to nearly identical expressions for the diffracted field. They differ in that the expression from the 2D theory has an obliquity factor whereas that from the 3D theory does not. The authors point out that this near similarity was discussed in Chapter 1, section 8 of Cowley [^1], a book originally from the 1970's. The authors clarify that the inconsistency arises from "dissimilar object requirements." I think another way to say this is that the assumptions of the 3D theory are violated when applied to a thin object. We will later see that the assumptions are not violated. Rather, the discrepancy comes from incorrectly accounting for the phase accumulated by light incident at an angle to the object.

# Conversions between 2D and 3D Theories

## Application of 2D theory to 3D objects

In an illuminating (pun intended) discussion the authors proceed to demonstrate that 3D diffraction theory actually emerges from the 2D theory by applying multislice theory. In this theory, a 3D object is divided into many, infinitesimally thin slices and the final field is the sum over the 2D diffracted fields from each slice.

The authors state that, **for each slice, both the thin object requirement and the small refractive index contrast requirements are satisfied**, which allows for the 2D and 3D theories to be linked. There is an extremely subtle point here because in the previous section the authors stated that an infinitesimally thin slice violates the assumption of small refractive index constrat *values*. But this is probably because the phase shift of the slice is finite but not infinitesimally thin. Here, by making the slices of the 3D object extremely thin, we are also inducing an infinitesimal phase shift in each slice. An infinitesimal phase shift is consistent with the small refractive index contrasts required by the first Born approximation.

Thus the 3D theory can be in a sense rescued by proper and repeated application of the 2D theory.

Now, a proper accounting of the phase shift of each slice must include a division by the obliquity factor:

$$ d \phi ( \vec{x}', z' ) = \frac{ k \Delta n (\vec{x}', z' ) dz' } {K (\vec{x}, z; \vec{x}', z' )} .$$

The logic behind this assertion is that illumination of the slice at an angle results in a larger distance covered in traversing the slice relative to normal illumination. In the words of the authors:

> The division of the OF enlarges the effective phase, because the light path length in the slice is \\( dz' / K \\) for off-axis light.

Combining this with the 2D integral theory results in a cancellation of the obliquity factors and a recovery of the 3D integral expression for the diffracted field.

The authors call this modification to the differential phase imparted by each slice a **type I OF modification**.

## Application of 3D theory to 2D objects

3D theory cannot be applied to 2D objects in an attempt to recover 2D diffraction theory because of the "dissimilar object requirements." By "cannot" the authors really mean "can" because in just a few sentences they circumvent the perceived difficulty by modifying the Greens function in the 3D theory by multiplying by the obliquity factor. This leads to what the authors call a **type II OF modification** which appears never to have been published before in the literature.

## Discussion on the Two Types OF Modification

Section 3C is probably the most important section of the paper. In it, the authors discuss the nature of the proposed OF modifications and why they are justified. By the term "nature," I mean whether they are physically motivated or just mathematical conveniences.

The application of 2D theory to 3D objects is discussed first. The corresponding OF modification to the phase of each slice is physically motivated by "the longer propagation distances of the off-axis rays." The statement at the beginning of the paragraph "A thick object must satisfy [ \\( | n ( \vec{r} ) -1 | \ll 1 \\) ], which is the requirement of 3D diffraction theory," is a bit puzzling not because of what it asserts (it's correct), but because it seems to imply that a proper application of 2D theory can recover this requirement without having to assume it. But by asserting that we can arrive at the total diffracted field by summing contributions from each infinitesimal slice, we are assuming exactly the first Born approximation. So it is not surprising that we should recover the 3D diffraction theory from multislice modeling *when the slices are independent of one another* because we made the first Born approximation in assuming the independence of each slice.

Perhaps the authors' intent was just to point out the physical basis of the OF modification to the phase. In this case, I completely agree with the modification and its physical origin.

Next we arrive at a paragraph about why 2D theory cannot be derived from 3D theory. Here the authors state that the phase induced by thin 2D object of thickness \\( l \\) is \\( \phi \approx k l \Delta n \\). Since both \\( l \\) and \\( \Delta n \\) are small, their product is very small and therefore the object has no appreciable effect on the phase. They next claim that the object cannot satisfy the first Born approximation, which is a bit confusing because the discussion in section 2C seemed to suggest that the problem comes from refractive index values that are too large, not too small.

To be honest, I don't quite follow the logic here, but I again agree with the conclusion. The type II OF modification of the Greens function is just a mathematical convenience that produces the correct results when applying 3D theory to thin objects.

# What about the Depth of Field?

Overall, this paper has helped me immensely in understanding the subtleties in the different ways to model diffraction and scattering from transparent objects. In particular, I have a much clearer understanding now on how to properly carry out forward modeling of image formation in high numerical aperture (NA) microscopes.

There is however one significant oversight, which is the definition of what it means for an object to be thin. I find their definition unsatisfactory for the following reason.

Consider a flat piece of glass in air and a ray of light incident upon it. If thin means that "light exits the object approximately at the same tranversal coordinate as it enters the object," then a physically thick piece of glass can be just as "thin" under a small angle of incidence as a physically thin piece of glass under a large angle of incidence. It's the optical path length that matters, not the physical extent of the object.

This is not so much an objection to their definition, but rather to the word "thin" itself. "Thin" alludes to physical extent, but we must remind ourselves that it is really optical extent. But consider next a ray of light that is normally incident upon the glass. Now it doesn't matter how physically thick or thin the glass is; it always exits at the same tranverse coordinate. Is every object under illumination at normal incidence a thin object?

These examples indicate that any defintion of "thin" might have to include the angle of the illumination.

Carrying this example further, if we continuously reduce the NA of the system, then we will measure light that is confined to smaller and smaller angles to the axis. But then we will approach the situation described above where even physically thick objects appear thin under illumination at angles close to normal. Since decreasing the NA increases the depth of field, I suspect that a more satisfactory definition of "thin" will ultimately depend on the depth of field as well.

My hypothesis is that an object's "thinness" really depends on two things:

1. The ratio of its physical extent to the depth of field
2. The refractive index contrast of the object

We need both of the above quantities to be small for an object to be thin.

# Conclusion

In conclusion, I really love this paper. It made me critically examine aspects of QPI that I think are taken for granted by clarifying a common point of confusion. Though I might sound critical in this post, it's only because I wish to see the authors' arguments further improved to the point where no ambiguity remains.

[^1]: Cowley, John Maxwell. Diffraction physics. Elsevier, 1995.
[^2]: Goodman, Joseph W. Introduction to Fourier optics. Roberts and Company publishers, 2005.
[^3]: Born, Max, and Emil Wolf. Principles of optics: electromagnetic theory of propagation, interference and diffraction of light. Elsevier, 2013.
