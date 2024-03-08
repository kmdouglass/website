<!--
.. title: Fusion 360 Core Concepts
.. slug: fusion-360-core-concepts
.. date: 2024-03-08 15:16:22 UTC+01:00
.. tags: cad
.. category: optics
.. link: 
.. description: Core concepts in Fusion 360
.. type: text
-->

I decided recently to learn Fusion 360 to help with some custom optomechanical designs that I need in the lab. The following are my notes about its core concepts.

# Assemblies

An assembly is a group of parts in one design file.

In CAD, there are two ways to create assemblies:

  1. **Bottom-up**
    1. Create parts
    2. Add parts to the assembly
  2. **Top-down** (used by Fusion 360)
    1. Start with an assembly
    2. Add parts to it

# Bodies vs. components

## Bodies

A body is a 3D shape used to add or remove components.

There are two core types:

1. Solid bodies
2. Surface bodies (denoted by a yellow face)

Other types include T-Splines (created in the Form environment) used to create freeform shapes, and mesh bodies.

**Bodies must be of the same type to interact with one another.**

## Components

A component is a part or "container" used within an assembly.

Components can contain

- bodies
- construction planes
- sketches
- canvases
- origin planes
- other components (a.k.a. **subassemblies**)

# Joints

Joints are how components are forced to stay together.

# Guidelines

1. Always start an assembly with a new component
2. Always rename components and bodies right after creation

# References

1. [Bodies vs Components](https://www.youtube.com/watch?v=TzG2deElWqI&t=0s)
