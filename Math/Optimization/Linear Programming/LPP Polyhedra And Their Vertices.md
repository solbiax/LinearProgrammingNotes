---
tags:
  - "#math/optimization/linear-programming"
  - "#math/definition"
next: "[[LPP Fundamental Theorem Of Linear Programming]]"
prev: "[[LPP Graphical Representation And Intuitions]]"
up: "[[Linear Programming Master Note]]"
---

# The Polyhedron
## Motivations: Geometric View Of LPPs
[[LPP Definition#^6ccfd9|As seen before]], any feasible set of a LPP can be described using inequality constraints of the form $Ax \geq b$. Any set described like that is commonly known as a polyhedron:

$$
P := \{x \in \mathbb{R}^{n} \; | \; Ax \geq b\}
$$
^polyhedron-formula

![[lpp_polyhedron_set.excalidraw|400]]


Having a solid grasp on the geometrical properties of polyhedrons is important to get a good intuition on LPPs and develop their theory. Indeed, we will see that the optima of LPPs can always be found in the corners of the polyhedron, and that a polyhedron contains at least a corner if and only if it does not contain a line.
All of these ideas are at the core of the simplex method used to solve linear programs.

While for the sake of computations the algebraic view is essential, the geometric framework is more natural, and understanding the interplay between both views significantly enriches the subject. 

## Polyhedron Definition
> [!definition] Polyhedron
> A **polyhedron** is any set $P \subseteq \mathbb{R}^{n}$ that can be written as:
> ![[#^polyhedron-formula]]
>  where $A \in \mathbb{R}^{m \times n}$ and $b \in \mathbb{R}^{m}$
^polyhedron-definition

Polyhedrons can be seen as the intersection of *halfspaces*:

> [!definition]
> Let $a \in \mathbb{R}^{n}$ be a nonzero vector and $b \in \mathbb{R}$ a scalar. Then:
> - The set $\{ x \in \mathbb{R}^{n} \; | \; a^{T}x = b \}$ is called a **hyperplane**
> - The set $\{ x \in \mathbb{R}^{n} \; | \; a^{T}x \geq b \}$ is called a **halfspace**

> [!proposition]
> Every [[#^polyhedron-definition|polyhedron]] is a convex set

> [!proof]-
> Since polyhedrons are the finite intersection of halfspaces, and the intersection of convex sets is still a convex set it suffices to prove that halfspaces are convex. For any $\lambda \in [0,1]$,  $a^{T}x \geq b$ and $a^{T}y \geq b$  it holds that:
> $$\lambda a^{T}x \geq \lambda b \quad \text{and} \quad (1-\lambda)a^{T}y \geq (1-\lambda)b 
> \; \implies a^{T}[\lambda x + (1-\lambda)y]\geq \lambda b + (1-\lambda)b = b$$
> therefore $\lambda x +(1 - \lambda)y$ also belongs to the halfspace, proving that halfspaces are convex.
> `\end{proof}`


# The Corners Of A Polyhedron

^bd7b2a

[[LPP Graphical Representation And Intuitions#^ffaa94|We observed]] that an optimal solution to a linear programming problem tends to occur at a "corner" of the polyhedron. There are three different ways of defining a "corner". Two of them are geometric, and one of them is algebraic. We will show that they are all equivalent to each other.

## Geometric Definition: Extreme Points
Visually, we can define a "corner" of a polyhedron as any point of $P$ that is not in its interior or edges. Such points "do not belong strictly" to any segment contained in $P$. Therefore we propose the following geometric definition of corners: 

> [!definition] Extreme Point
> Let $P$ be a polyhedron. A vector $x \in P$ is an **extreme point** of $P$ if:
> $$x \neq \lambda y + (1-\lambda)z \quad \forall \; y,z \in P-\{ x \} \; \text{and} \; \lambda \in [0,1]$$
> In other words, $x$ is an extreme point of $P$ if it is not the strict convex combination of any other two points of $P$.
^extreme-point-definition

![[lpp_extreme_point_definition.excalidraw|600]]


## Geometric Definition:  Vertices

An alternative geometric definition defines a corner of a polyhedron $P$ as the unique optimal solution to some linear programming problem with feasible set $P$:

> [!definition] Vertex
> Let $P$ be a polyhedron. A vector $x \in P$ is a **vertex** of $P$ if there exists some $c \in \mathbb{R}^{n}$ such that:
> $$c^{T}x<c^{T}y \quad \forall \; y \in P-\{ x \}$$
^vertex-definition

The basic intuition behind this definition is that $x$ is a corner (a vertex) of $P$ if and only if we can find an hyperplane that meets $P$ only at the point $x$.

![[lpp_vertex_example.excalidraw|600]]

## Algebraic Definition: Basic Feasible Solution
### Active Constraints
An important concept in constrained optimization is the one of active constraints. In the characterization of a local minimum $x^{*}$, a continuous constraint $g(x) \geq 0$ is relevant only if $g(x^{*})=0$, i.e. if the constraint is *active* at $x^{*}$.  *Indeed*, If $g$ was inactive, we could always find a neighbourhood of $x^{*}$ where $g(x) > 0$, i.e. a neighboruhood where the constraint is always respected. This means that whether $g$ was present or not, $x^{*}$ would still be a local minimum. 

> [!definition] Active Constraints
> In constrained optimization any constraint $i$ in the form:
> $$g_{i}(x) \geq 0 \quad \text{or} \quad g_{i}(x) \leq 0 \quad \text{or} \quad g_{i}(x) = 0$$
> is said to be **active** (or binding) at $x^{*}$ if $g_{i}(x^{*}) = 0$. 
> In the case of linear optimization a constraint is active if: 
> $$a_{i}^{T}x^{*} = b_{i}$$

### Basic Solution Definition
By [[Rouche-Capelli Theorem|Rouché-Capelli's theorem]] we know that $k$ independent linear equations define an affine subspace of solutions of dimension $n-k$.
Intuitively, we can see corners of a polyhedron as places where all the active constraints restrict the polyhedron so that there is only one solution (the corner), and hence there must be  $k =n$ active constraints.
This is the main idea behind the definition of basic feasible solution:

> [!definition] Basic Solution And Basic Feasible Solution
> Consider a polyhedron $P$ defined by linear equality and inequality constraints. Let $x^{*} \in \mathbb{R}^{n}$, then:
> 1) The vector $x^{*}$ is a **basic solution** if:
> 	- All equality constraints are active;
> 	- Out of all the constraints that are active at $x^{*}$, there are $n$ of them that are linearly independent.
> 2) If $x^{*}$ is a basic solution that satisfies all of the constraints, we say that it is a **basic feasible solution**
^basic-solution-definition

Note that if the number of constraints $m$ is less than $n$ there are no basic solutions, hence there are no corners.

> [!example]
> ![[lpp_basic_feasible_solution.excalidraw|400]]
> Let $P \subseteq \mathbb{R}^{3}$ be defined by the constraints:
> $$\begin{eqnarray}
> x_{1} + x_{2} + x_{3} = 1 \\
> x_{1}, x_{2}, x_{3} \geq 0
> \end{eqnarray}
>$$
>There are three constraints that are active at each of one of the points $A,B,C,D$, while only two are active at point $E$.
>In this example $A,B,C$ are basic feasible solutions. $D$ is not a basic solution because it fails the equality constraint $x_{1} + x_{2} + x_{3} = 1$. $E$ is feasible but not basic, because there are only 2 active linearly independent constraints.

> [!remark] Basic Solutions Are Not Geometrical
> Considering the previous example, if the equality constraint $x_{1} + x_{2} + x_{3} = 1$ were to be replaced by  $x_{1} + x_{2} + x_{3} \leq 1$ and $x_{1} + x_{2} + x_{3} \geq 1$ then $D$ would be a basic solution, even though the geometry of the polyhedron is the same. In this sense, basic solutions are NOT a geometric property of the polyhedron, as they depend on the algebraic representation of $P$.
## Equivalence Of  Definitions Of Corner
> [!theorem]
> Let $P$ be  a nonempty polyhedron and let $x^{*}\in P$. Then, the following are equivalent:
> 1) $x^{*}$ is a [[#^vertex-definition|vertex]];
> 2) $x^{*}$ is an [[#^extreme-point-definition|extreme point]];
> 3) $x^{*}$ is a [[#^basic-solution-definition|basic feasible solution]].

^corner-definition-equivalence

> [!proof]-
> ***($1 \implies 2$)***  By [[#^vertex-definition|definition of vertex]] there exists some $c \in \mathbb{R}^{n}$ such that $c^{T}x < c^{T}y$ for all $y \in P$. By contradiction, if $x^{*}$ wasn't an [[#^extreme-point-definition|extreme point]] there would be $y,z \in P-\{ x \}$ and $\lambda \in [0,1]$ such that $x = \lambda y + (1-\lambda)z$. But then we would have:
> $$c^{T}x =  \lambda c^{T} y + (1-\lambda)c^{T} z > \lambda c^{T}x + (1-\lambda)c^{T}x = c^{T}x$$
> hence $c^{T}x > c^{T}x$, which is obviously absurd.
> 
> ***($2 \implies 3$)*** We proceed by proving $\neg 3 \implies \neg 2$. Suppose that $x^{*}$ is not a [[#^basic-solution-definition|basic feasible solution]]. The set $I$ of all active constraints then is such that the span $<a_{i} \; | \; i \in I>$ is a proper subspace of $\mathbb{R}^{n}$. In particular, there exists $d \in \mathbb{R}^{n}$ such that $a_{i}^{T}d = 0$ for every $i \in I$. 
> For $\epsilon>0$ consider $y := x^{*}+\epsilon d$ and $z:= x^{*}-\epsilon d$. Then, for $\epsilon$ small enough, all inactive constraints are respected due to continuity, while for all the active constraints we have:
> $$a_{i}^{T}(x^{*} \pm \epsilon d) = a_{i}^{T}x^{*} = b_{i}$$
> hence $y,z$ belong to $P$. This proves that $x^{*}$ is not an extreme point because  $x^{*} = (y + z) /2$  with $y,z \in P-\{ x \}$ 
>
> ***($3 \implies 1$)*** Let $x^{*}$ be a basic feasible solution and let $I$ be the set of all active constraints. By defining $c:= \sum_{i \in I} a_{i}$ we have for any $y \in P-\{ x^{*} \}$ that:
> $$c^{T}y = \sum_{i \in I} a_{i}^{T}y \geq \sum_{i \in I} b_{i} = c^{T}x^{*}$$
> in particular, the equality holds iff $a_{i}^{T}y = b_{i}$ for all $i \in I$. But since there are $n$ independent vectors $a_{i}$, by [[Rouche-Capelli Theorem|Rouché-Capelli's theorem]] this can only hold for one point, which by definition is $x^{*}$. Therefore $c^{T}y > c^{T}x^{*}$, concluding that $x^{*}$ is a vertex.
> `\end{proof}`

> [!remark] Basic Feasible Solutions ARE Geometrical
> Since the concept of basic feasible solution is equivalent to the geometrically defined concept of vertex (and extreme point) we can conclude that, unlike basic solutions, they don't depend on the algebraic representation of $P$. 

> [!corollary] Corners Of Polyhedra Are Finite
> For any polyhedron there can be only a finite number of corners. In particular for $m$ linear constraints in $\mathbb{R}^{n}$ there can be a maximum of $m \choose{n}$ corners.

^56c105

> [!proof]-
> There can only be as many basic feasible solutions as there are basic solutions. Basic solutions are given by $n$ linearly independent active constraints, so they correspond to the choice of a set of $n$ constraints out of $m$ available constraints, therefore the number of basic solutions is bounded above by $m \choose{n}$.
> `\end{proof}`

Although finite, the number of basic feasible solutions can be very large. For example, the unit cube $P = \{ x \in \mathbb{R}^{n} \; | \; 0\leq x_{i} \leq 1 \quad i=1,\dots,n \}$ is defined by $2n$ constraints but has $2^{n}$ basic feasible solutions (you can count them as the binary choice between $\{ 0,1 \}$ for each $i$).

## Adjacent Basic Solutions And Edges
Given two vertices on the polyhedron, if they share $n-1$ active independent constraints, then they are on the same affine line, or more properly *edge*, at the border of the polyhedron. 
The definition follows:

> [!definition] Adjacent Solutions And Edges
> Two distinct basic solutions are said to be **adjacent** if we can find $n-1$ linearly independent constraints that are active at both of them.
> If two adjacent basic solutions are also feasible, then the line segment that joins them is called an **edge** of the feasible set.

^fb166a

we will see that the simplex method works by travelling along the edges of the feasible set trying to diminish the value of the cost function $c^{T}x$ at each iteration.