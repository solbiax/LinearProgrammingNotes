---
tags:
  - math/optimization/linear-programming/simplex-method
next: 
prev: "[[LPP Simplex Method Computational Complexity]]"
up: "[[Linear Programming Master Note]]"
---
# The Column Geometry Of The Simplex Method
The [[LPP The Simplex Method Framework#Iteration Of The Simplex Method|simplex method]] was conceived by George B. Dantzig in 1947. During its development, Dantzig was skeptical of the method's practical efficiency. Indeed, progressing from vertex to vertex might not seem like the fastest strategy to solve linear programming problems, given its more indirect approach of navigating along the polyhedron edges. A direct approach, that would "jump" towards the optimum, would seem to be a more obvious choice for an efficient algorithm. Nevertheless, the simplex method has repeatedly proven its effectiveness, often surpassing expectations in real-world applications.
Following his initial developments, Dantzig introduced *the column geometry* perspective, which provided insights into the simplex method's practical efficiency.

In this note we will discuss the column geometry of the simplex method, which also explains the real meaning behind terms such as "pivot" and even the very name of the method itself!
## The Column Geometry LPP
Throughout this note we shall consider the following [[LPP Definition#^07d883|linear programming problem]]:

$$
\begin{flalign}{}
\min_{x \in \mathbb{R}^{n}} \quad & c^{T}x \\
\text{s.t.} \quad & Ax = b \\
& e^{T}x = \sum_{i = 1}^{n}x_{i} = 1 \\
& x \geq 0
\end{flalign}
$$

^182ebd

where $A \in \mathbb{R}^{m \times n}$ and $b \in \mathbb{R}^{m}$. This problem is identical to the [[LPP In Standard Form#^d3d2e7|LPP in standard form]], but with the added **convexity constraint** $e^{T}x = 1$. We can consider [[#^182ebd]] without much loss of generality, because every problem with a bounded feasible set can be brought into this form:

> [!proposition]
> Any [[LPP In Standard Form#^d3d2e7|LPP in standard form]], with a bounded feasible set $P$, can be reformulated into an equivalent problem in the form of [[#^182ebd]] 

> [!proof]-
> Since $P$ is bounded there must exist $U \in \mathbb{R}^{n}$ such that $x_{i} \leq U$. Let us define the slack variable $s = nU - \sum_{i}x_{i}$. It easily follows that $s \geq 0$ and that $s + \sum_{i}x_{i} = nU$. This means that the original problem remains the same if we add $s$ with the constraint $s \geq 0$ and $s + \sum_{i}x_{i} = nU$. By dividing every involved quantity by $nU$ we obtain an equivalent problem in the form of [[#^182ebd]]
> `\end{proof}`

## The Column Geometry Representation
By introducing the auxiliary variable $z = c^{T}x$ we can see problem [[#^182ebd]] as trying to  minimize $z$ while synthesizing the vector $b$ using a convex combination of the columns $A_{i}$:
$$
x_{1}\begin{pmatrix}
A_{1} \\
c_{1}
\end{pmatrix} +
x_{2}\begin{pmatrix}
A_{2} \\
c_{2}
\end{pmatrix}+ \dots +
x_{n} \begin{pmatrix}
A_{n} \\
c_{n}
\end{pmatrix} = 
\begin{pmatrix}
b \\
z
\end{pmatrix}
$$
The column geometry can be used to give a natural description of the above equation. It is built as follows:
1) We view $A_{i}$ and $b$ as points in the "horizontal plane" $\mathbb{R}^{m}$;
2) We view the vertical axis as the one-dimensional space associated to $c_{i}$ and $z$:
3) Each point in the column lives in the  "3"-dimensional space created by the horizontal plane and vertical axis. Every point $(A_{i}, c_{i})$ can be drawn in this special space, which gives rise to the column geometry representation.

![[lpp_column_geometry.excalidraw|760]]

Our objective is to construct $(b,z)$ with a convex combination of $(A_{i}, c_{i})$, such that $z$ is as small as possible.
Note that the vectors of the form $(b,z )$ lie on a vertical line, known as the **requirement line**.
If the requirement line does not intersect the convex hull of $(A_{i},c_{i})$ then the problem is infeasible. Otherwise the optimal solution corresponds to the lowest point in the intersection. 

![[convex_hull_column_geometry.excalidraw|450]]

As shown in the above image, the optimal value can always be found in the intersection between the requirement line and one of the faces of the polytope defined by the convex hull of $(A_{i},c_{i})$ (we will see why later!). Each face of the polytope is an $m$-dimensional simplex:

> [!definition] Simplex
> The convex hull of $k+1$ affinely independent vectors in $\mathbb{R}^{n}$ is called a **$k$-dimensional simplex**

## Basic Feasible Solutions And Pivots In The Column Geometry 
Since problem [[#^182ebd]] has $m+1$ equality constraints we need $m+1$ linearly independent columns $(A_{i},1)$ in order to form a [[LPP Polyhedra In Standard Form#^12f1aa|basis]] of a basic solution. For a given basis $B_{1},\dots,B_{m+1}$ we define the points $(A_{B_{i}}, c_{B_{i}})$ as the **basic points**. All the remaining points $(A_{i},c_{i})$ are called **nonbasic points**.
It's easy to show that the basic points are affinely independent, hence their convex hull is an $m$-dimensional simplex, which we call the **basic simplex**.  In particular, by [[LPP Fundamental Theorem Of Linear Programming#^537f75|the fundamental theorem of linear programming]] there's always an optimal vertex for bounded polyhedra, so the requirement line will have an optimal intersection with one of the basic simplexes. 

Let the requirement line intersect the $m$-dimensional basic simplex at $(b,z)$. In other words:
$$
\begin{pmatrix}
b \\ z
\end{pmatrix}
= x_{B_{1}}\begin{pmatrix}
A_{B_{1}} \\ c_{B_{1}}
\end{pmatrix} + \dots +
x_{B_{m+1}}\begin{pmatrix}
A_{B_{m+1}} \\ c_{B_{m+1}}
\end{pmatrix}
$$
The basic feasible solution is simply given by the components $x_{i}$ used in the convex combinations that generates $(b,z)$.

When we pivot from one base to another, an index, say $B_{i}$, exits the basis and another index $j$ enters it. In terms of the column geometry this means changing from one basic simplex to another that differs only by one basic point:

![[lpp_pivot_in_column_geometry.excalidraw|800]]

Physically, we can image the basic simplex as anchored to its basic points. Changing basis means grasping the exiting basic point and moving it towards the entering basic point. In doing so, the basic simplex will *pivot*  on its anchor, and stretch down to the lower position. The anchor is the $(m-1)$-dimensional simplex defined by the other basic points that remain in the basis.
## Reduced Costs And Exiting Variable In The Column Geometry
When pivoting from one column to another, the new basic simplex will intercept the requirement line at a lower value if and only if the new basic point is below the plan that passes through the old basic simplex; we refer to that plane as the **dual plane**.  This is due to the fact that the vertical distance from the dual plane to a point $(A_{j}, c_{j})$ is equal to the [[LPP Simplex Method Optimality Conditions#^3cc9c2|reduced cost]] $c_{j}$. 

> [!proposition]
> The vertical distance from the dual plane to a point $(A_{j}, c_{j})$ is equal to the reduced cost $\bar{c}_{j}$ of the variable $x_{j}$.

> [!proof]-
> To compute the vertical distance from the point $(A_{j},c_{j})$ to the dual plane, we need to compute the intersection between the vertical line $\begin{pmatrix}A_{j} \\ c_{j}\end{pmatrix} + <\begin{pmatrix} \mathbb{0} \\ 1 \end{pmatrix}>$ and the dual plane:
> $$
> \begin{pmatrix}
> A_{B_{1}} \\ c_{B_{1}} 
> \end{pmatrix}
> + 
> <\begin{pmatrix}
> A_{B_{i}} \\ c_{B_{i}} 
> \end{pmatrix}-
> \begin{pmatrix}
> A_{B_{1}} \\ c_{B_{1}} 
> \end{pmatrix}
> >_{i = 2,\dots,m+1}
> $$
> Finding such an intersection is equivalent to finding the distance value $d \in \mathbb{R}^{n}$ such that:
> $$
> \begin{pmatrix}
> A_{j} \\ c_{j}-d 
> \end{pmatrix} = 
> \lambda_{1}\begin{pmatrix}
> A_{B_{1}} \\ c_{B_{1}} 
> \end{pmatrix} + \dots +
> \lambda_{m+1} \begin{pmatrix}
> A_{B_{m+1}} \\ c_{B_{m+1}} 
> \end{pmatrix}
> $$
> for some $\lambda_{i} \in \mathbb{R}$ with $\lambda_{1} = 1-\sum_{i = 2}^{m+1}\lambda_{i}$ . Let $B$ be the [[LPP Polyhedra In Standard Form#^12f1aa|basis matrix]], then the above equation implies that:
> $$
> A_{j} = \sum_{i = 1}^{m+1} \lambda_{i}A_{B_{i}} = B\lambda \implies \lambda = B^{-1}A_{j}
> $$
> hence we must have:
> $$
> d = c_{j} - \sum_{i = 1}^{m+1}\lambda_{i}c_{B_{i}} = c_{j} - c_{B}^{T}\lambda = c_{j} - c^{T}_{B}B^{-1}A_{j} = \bar{c}_{j}
> $$
> `\end{proof}`

Once we have chosen a variable to enter the basis, the simplex method determines which variable exits the basis. We can determine the exiting variable geometrically using the column geometry.
The entering basic point, together with the previous basic points, forms a $(m+1)$-simplex, which faces are the $m$-simplexes formed by every possible choice of these points.
The requirement line will intersect this $(m+1)$-simplex at a bottom and a top face. The bottom face is the next basic simplex, and the exiting variable is the basic point not contained in it.

![[lpp_column_geometry_exiting_index.excalidraw|650]]

## Why The Simplex Method Is So Goddamn Fast
The dynamics of the column geometry give significant insights on the practical efficiency of the simplex method. The column geometry tells us that finding an optimal basis is mostly a matter of choosing the basic points with the lowest heights. As long as the distribution of the points $(A_{i},c_{i})$ is not "degenerate", we can intuitively grasp that this can be done, most of the times, just with $m$ pivots. This is why the typical complexity of the method is $\mathcal{O}(m)$

![[lpp_column_geometry_efficiency.excalidraw]]

The above examples showcases why the simplex method may require a rather small number of pivots, even when the number of decision variables is large.