<!--
.. title: Electrostatics and Mitochondria
.. slug: electrostatics-and-mitochondria
.. date: 2026-03-13 10:04:38 UTC+01:00
.. tags: mitochondria, membrane potential, electrostatics
.. category: biophysics
.. link: 
.. description: I very briefly explain the electrostatic conventions used for discussing the mitochondrial membrane potential.
.. type: text
.. has_math: true
-->

The mitochondrial [membrane potential](https://en.wikipedia.org/wiki/Membrane_potential) is the potential difference across the inner mitochondrial membrane caused by proton pumps in the electron transport chain. Its value is often cited as about \\( -150\, mV \\). I can never remember the directional conventions for this, so I made the following sketch as a reminder:

<figure>
  <img width="75%" src="/images/mito_membrane_potential.png">
</figure>

- **IMM**: Inner mitochondrial membrane
- **IMS**: Intermembrane space
- **OMM**: Outer mitchondrial membrane

If you stick the positive lead of a voltmeter inside the mitochontrial matrix and the negative lead in the intermembrane space, you will measure a voltage of about \\( -150 \, mV \\).

The electric field, which points from positive to negative charge, points from the IMS into the matrix. The definition of potential difference is

$$ \Delta \psi = -\int_C \vec{E} \cdot \, d \vec{\ell} $$

where \\( C \\) is the path of integration. In the absence of magnetic fields this integral is independent of the path and depends only on the value of the potential at its endpoints:

$$ \Delta \psi = - \left| \vec{E} \right| \times (z_{mat}  - z_{ims}) $$

with \\( z_{mat} \\) and \\( z_{ims} \\) positions within the matrix and IMS, respectively. This equation also assumes that the electric field is uniform across the membrane, which is clearly a simplification.

The direction of integration is from \\( z_{ims} \\) to \\( z_{mat} \\). In the figure above the electric field \\( \vec{E} \\) and the path length differential \\( d \vec{\ell} \\) are parallel, so \\( \vec{E} \cdot \, d \vec{\ell} \\) is positive, and the negative sign in the definition results in a negative value for \\( \Delta \psi \\).
