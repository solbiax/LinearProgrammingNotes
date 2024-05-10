---
tags:
  - "#math/optimization/linear-programming"
  - math/definition
next: "[[LPP Simplex Method Optimality Conditions]]"
prev: "[[LPP Polyhedra In Standard Form]]"
up: "[[Linear Programming Master Note]]"
---

# Degeneracy Of Basic Solutions
## Definition
At a [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic solution]] we must have $n$ linearly independent active constraints. This allows for the possibility that the number of active constraints is greater than $n$. In this case, we say that we have a *degenerate* basic solution:

> [!definition] Degenerate Basic Solution
> A [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic solution]] $x \in \mathbb{R}^{n}$ is said to be degenerate if more than $n$ of the constraints are active at $x$

^ee501b

It turns out that the presence of degeneracy can strongly affect the behaviour of algorithms. 

> [!example]
> ![[lpp_degenerate_solutions.excalidraw|700]]
> In two dimensions, a degenerate basic solution is at the intersection of three or more lines; in three dimensions, a degenerate basic solution is at the intersection of four or more planes

## Degeneracy In Standard Form Polyhedra
For a polyhedron in [[LPP In Standard Form#^d3d2e7|standard form]] (with full row rank constraint matrix $A$) $m$ equality constraints ($Ax = b$) are always active. Having more than $n$ active constraints means that we have more than $n-m$ variables at zero level:

> [!definition] Degeneracy For Standard Form Polyhedra
> Consider $P = \{ x \in \mathbb{R}^{n} \; | \; Ax = b, x\geq 0 \}$  with $A \in \mathbb{R}^{m \times n}$ such that $\text{rank}(A)= m$.  A basic solution $x$ is  **degenerate** if more than $n-m$ of its components are zero.

^bd5d3e

> [!example]-
> Consider the polyhedron $P := \{ x \in \mathbb{R}^{7} \; | \; Ax=b, x\geq 0 \}$ where: 
> $$
> A = \begin{pmatrix}
> 1 & 1 & 2 & 1 & 0 & 0 & 0 \\
> 0 & 1 & 6 & 0 & 1 & 0 & 0  \\
> 1 & 0 & 0 & 0 & 0 & 1 & 0 \\
> 0 & 1 & 0 & 0 & 0 & 0 & 1
> \end{pmatrix}
> \qquad  
> b = \begin{pmatrix}
> 8 \\
> 12 \\
> 4 \\
> 6
> \end{pmatrix}
> $$
> Consider the basis given by the linearly independent columns $A_{1},A_{2},A_{3},A_{7}$. By setting $x_{4}, x_{5}, x_{6}$ to zero and by solving $Ax = b$ we obtain $x = (4,0,2,0,0,0,6)$. This is a degenerate basic feasible solution, because one of the basic variables $x_{2}$ is $0$. We can subsitute $2$ with $4$ choosing the basis $A_{1},A_{4},A_{3},A_{7}$ and obtain the same basic solution $x = (4,0,2,0,0,0,6)$

Degeneracy happens when the same basic solution could be obtained by choosing different combinations of linearly independent constraints. The picture below shows this concepts quite well: a degenerate solution can be obtained by choosing one of the three intersecting lines:

![[lpp_removing_degeneracy.excalidraw|600]]

If the matrix $A$ was randomly generated, this would almost surely (i.e. with probability 1) never happen. However, most linear programming problems have a special nonrandom structure, so degeneracy is something that does appear. ^3a2828

Unlike nondegenerate solutions that are uniquely determined by their basis, degenerate solutions in general can be obtained by choosing other bases. 

## Degeneracy Is Not Purely Geometrical
Degeneracy of basic feasible solutions is not, in general, a geometric property, but rather it may depend on the particular representation of a polyhedron.

> [!example] Degeneracy is not geometrical
> Consider the following polyhedron in standard form:
> $$P := \{ (x_{1}, x_{2}, x_{3}) \; | \; x_{1}-x_{2} = 0,\; x_1 + x_{2} + 2x_{3} = 2, \;  x\geq 0\}$$
> the associated matrix is:
> $$
> A = \begin{pmatrix}
> 1 & -1 & 0 \\
> 1 & 1 & 2
> \end{pmatrix}
> $$
> We can choose the basis $A_{2}, A_{3}$ and obtain the degenerate solution $(0,0,1)$, however, the same polyhedron can be described using a nonstandard form:
> $$ P = \{ (x_{1},x_{2}, x_{3}) \; | \; x_1 - x_{2} = 0, \; x_{1} = x_{2} + 2 x_{3} = 2 \; x_{1} \geq 0, \; x_{3} \geq 0 \}$$
> The vector $(0,0,1)$ is now a nondegenerate basic feasible solution, because there are only three active constraints.

However, it can be shown that if a basic feasible solution is degenerate under one particular standard form representation, then it is degenerate under every standard form representation of the same polyhedron.