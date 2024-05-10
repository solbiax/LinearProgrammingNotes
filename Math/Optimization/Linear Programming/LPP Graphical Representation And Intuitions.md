---
tags:
  - math/optimization/linear-programming
  - math/intuition
next: "[[LPP Polyhedra And Their Vertices]]"
prev: "[[LPP Examples]]"
up: "[[Linear Programming Master Note]]"
abstract: some graphical representations of the feasible set of LPPs and intuitions on the optimal solutions and possible outcomes of a LPP
---

# Graphical Representation Of LPPs
We shall consider some examples that provide useful geometric insights into the nature of linear programming.
## 2D LP Example
Consider the problem:

$$
\begin{array}{}
\min_{x_{1},x_{2}} & -x_{1} &  -x_{2}  \\
\text{s.t.} \quad & x_{1} & + 2 x_{2} & \leq 3 \\
& 2x_{1} & +  x_{2} & \leq 3 \\
& x_{1},x_{2} \geq 0
\end{array}
$$

Each constraint defines a line through a linear equation.  For Inequality constraints, each of the corresponding lines will divide the space in two half-planes, in one of which the specific constraint is respected. 
We can draw the feasible set by shading the intersection of all the half-planes defined by the constraints, as in the picture below:

![[lpp_feasible_set_example.excalidraw|700]]


Our main goal is to minimize $c^T x$. In graphical terms this means moving in the exact opposite direction of vector $c$.
We may also notice that for any given scalar $z$ the set of points whose cost is equal to $z$ is given by a line perpendicular to $c$, *indeed*, let $x$ and $y$ belong to $\{\ w \; | \; c^Tw = z \}$, then:

$$ c^T x = z = c^Ty \; \Rightarrow c^T (x-y) = 0 \; \Rightarrow (x-y) \perp c$$
It's easy to see that graphically solving the optimization problem is equivalent to sliding the hyperplane (in this case line) perpendicular to $c$ along the opposite direction of $c$ until we stop intersecting the feasible set.
In this example the best we can do is $z = 2$. Note that *the solution is a corner of the feasible set*. ^ffaa94

## All The Possible Outcomes of a LPP
Consider the following linear constraints:
$$
\begin{array}{}
-x_{1} & +x_{2} & \leq 1  \\
x_{1} & & \geq 0 \\
& x_{2} & \geq 0
\end{array}
$$
Under those constraints, we can evaluate what happens to the minimization problem $\min_x c^Tx$  by changing the value of $c$, or by adding additional constraints:
1) For $c = (1,1)$ we have that $x = (0,0)$ is the unique optimal solution;
2) For $c = (1,0)$ there are multiple optimal solutions, namely any vector in the form $x = (0,x_2)$ with $x_2 \in [0,1]$ is optimal. in this case the set of optimal solutions is bounded;
3) For $c=(0,1)$ any vector in the form $x = (x_{1},0)$  with $x_1 \geq 0$ is optimal. In this case the set of solutions is unbounded;
4) For $c = (-1,-1)$ we can always produce a feasible solution with an arbitrary low value $2z \leq 0$ by choosing $x = \left( -z,-z \right)$, so the optimal cost is $-\infty$;
5) If we impose an additional constraint such as $x_{1} + x_{2} \leq -2$  it is evident that no feasible solution exists.

*To summarize* we have the following possibilities:
1) There exists a unique optimal solution;
2) The set of optimal solutions exists and it's bounded;
3) The set of optimal solutions exists and it's unbouded;
4) The optimal cost is $-\infty$;
5) The problem is not feasible (the feasible set is empty).

In principle there is an additional possibility: an optimal solution does not exists even though the problem is feasible and the optimal cost is not $-\infty$. For example this is the case when minimizing $f(x)=\frac{1}{x}$ for $x \geq 0$.
[[LPP Fundamental Theorem Of Linear Programming#^537f75|We will see]] that this possibility never arises in linear programming.

Another thing to note is that in all of our examples, if the problem has an optimal solution, then *one of the optimal solutions can be found among the corners* of the feasible set. This is another general feature of linear programming problems, as long as the feasible set has at least one corner.

## Geometrical Meaning Of Standard Form Problems
There is a way to visualize [[LPP In Standard Form| linear programs in standard form]]:
![[LPP In Standard Form#^linear-program-standard-form]]

Let us assume that the matrix $A \in \mathbb{R}^{m \times n}$ has $m$ linearly independent rows. Then the equation $Ax = b$ constraints the vector $x$ to lie on a $(n-m)$-dimensional affine set.
Standing on that set, ignoring all the other $m$ dimensions orthogonal to it, the feasible set is only constrained by the linear inequality constraints $x_i \geq 0$.

In Particular, if $n-m =2$ then the feasible set can be drawn as a two-dimensional set defined by $n$ linear inequality constraints. For example, in the case of $x_1 + x_2 + x_3 = 1$ and $x_1, x_2, x_3 \geq 0$ the feasible set in 3D space can be projected onto a plane where it's simply a triangle where each edge is defined by one of the constraints $x_i \geq 0$:

![[standard_form_feasible_set.excalidraw|700]]