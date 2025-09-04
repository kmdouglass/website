<!--
.. title: The Axis of Symmetry of a Parabola and the Eigenvectors of its Matrix Representation
.. slug: the-axis-of-symmetry-of-a-parabola-and-the-eigenvectors-of-its-matrix-representation
.. date: 2025-09-04 09:20:31 UTC+02:00
.. tags: algebraic geometry, mathematics, computer graphics
.. category: 
.. link: 
.. description: I show that the eigenvectors of the matrix representation of a parabola's quadratic form are parallel to its axis of symmetry.
.. type: text
.. has_math: true
-->

The last few months I have been working on developing parametric representations of conic sections so that any arbitrary conic can be drawn to the computer screen. Besides being a fun intellectual exercise, I expect that the performance of the parametric approach should be quite good relative to any iterative method for drawing implicit representations of arbitrary conic curves because no iteration is required.

The parabola in particular has proven to be trickier than I had anticipated, and it has forced me to revisit some of its basic properties.

# The Setup

Consider the most general form of a conic curve, which is the implicit equation:

$$
Q ( x, y ) = A x^2 + B x y + C y^2 + D x + E y + F = 0
$$

For a parabola, \\( B \\) is dependent on the coefficients \\( A \\) and \\( C \\) via the equation \\( B^2 = 4 A C \\), so that the implicit equation for a parabola is

$$
Q ( x, y ) = (a x + c y)^2 + D x + E y + F = 0
$$

where \\( a^2 = A \\) and \\( c^2 = C \\).

[It can be shown](https://math.stackexchange.com/questions/2042028/axis-of-symmetry-for-a-general-parabola) that the axis of symmetry of the parabola is the line

$$
a x + c y + \frac{a D + c E}{2 \left( a^2 + c^2 \right)} = 0
$$

The matrix of the quadratic form for the parabola is

$$\begin{eqnarray}
A_{33} = 
\left(
  \begin{array}{cc}
    A & B / 2 \\\\
    B / 2 & C
  \end{array}
\right) = \left(
  \begin{array}{cc}
    a^2 & ac \\\\
    ac & c^2
  \end{array}
\right)
\end{eqnarray}$$

\\( A_{33} \\) is singular and has one eigenvalue whose value is 0.

In this post I show that the eigenvector of the matrix of the quadratic form of a parabola with the zero eigenvalue is parallel to the axis of symmetry.

# Determine the Eigenvalues of \\( A_{33} \\)

The characteristic polynomial of the matrix \\( A_{33} \\) is

$$\begin{eqnarray}
  \left( a^2 - \lambda \right) \left( c^2 - \lambda \right) - a^2 c^2 &=& 0 \\\\
  a^2 c^2 - \left( a^2 + c^2 \right) \lambda + \lambda^2 - a^2 c^2 &=& 0 \\\\
  - \left( a^2 + c^2 \right) \lambda + \lambda^2 &=& 0
\end{eqnarray}$$

The solutions to the above equation are \\( \lambda = \\{ 0, \, a^2 + c^2 \\} \\).

# Find the Eigenvector of the Zero Eigenvalue

The system of equations for finding the eigenvectors is

$$\begin{eqnarray}
  \left( a^2 - \lambda \right) x + acy &=& 0 \\\\
  acx + \left( c^2 - \lambda \right) y &=& 0
\end{eqnarray}$$

Solving the first equation for \\( y \\) gives

$$
y = - \left( \frac{a^2 - \lambda}{ac} \right) x
$$

Substitute in \\( \lambda = 0 \\) to find

$$
y = - \left( \frac{a}{c} \right) x
$$

The eigenvector is thus

$$\begin{pmatrix}
1 \\\\
-c / a
\end{pmatrix}$$

Recall from above that the axis of symmetry is

$$
a x + c y + \frac{a D + c E}{2 \left( a^2 + c^2 \right)} = 0
$$

This is a line of slope \\( m = -a / c \\) and is therefore parallel to the eigenvector with the zero eigenvalue.

# The Tangent at the Vertex and the Eigenvector with Non-Zero Eigenvalue

The tangent to the parabola at its vertex is perpendicular to the axis of symmetry and must therefore have a slope of \\( c / a \\)[^1].

The eigenvector with eigenvalue \\( \lambda = a^2 + c^2 \\) of the matrix of the quadratic form is found by substituting this value into the first equation in the system above:

$$\begin{eqnarray}
  \left( a^2 - \lambda \right) x + ac y &=& 0 \\\\
  -c^2 x + ac y &=& 0 
\end{eqnarray}$$

This gives

$$
y = \left( \frac{c}{a} \right) x
$$

which is a line whose slope is the negative reciprocal of the slope of the axis of symmetry. The eigenvector with non-zero eigenvalue is therefore parallel to the tangent at the vertex.

For completeness, the eigenvector with non-zero eigevalue is

$$\begin{pmatrix}
1 \\\\
a / c
\end{pmatrix}$$

[^1]: Perpendicular lines have slopes whose products are equal to -1.
