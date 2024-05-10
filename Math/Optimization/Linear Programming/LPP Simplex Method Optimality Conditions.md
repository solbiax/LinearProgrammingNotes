---
tags:
  - math/optimization/linear-programming
next: "[[LPP The Simplex Method Framework]]"
prev: "[[LPP Degeneracy]]"
up: "[[Linear Programming Master Note]]"
---

## Introduction
[[LPP Fundamental Theorem Of Linear Programming#^537f75| By the fundamental theorem of linear programming]] we know that a feasible bounded LPP always has an optimal solution that can be found in one of the [[LPP Polyhedra And Their Vertices#The Corners Of A Polyhedron|vertices]] of the feasible set. In particular, we know that [[LPP Fundamental Theorem Of Linear Programming#^c85132|every feasible LPP in standard problem has at least one vertex]]. 
The simplex method is based on these facts, and searches for an optimal solution by moving from one basic feasible solution to another, along the edges of the feasible set, always in a cost reducing direction.
Eventually, a basic feasible solution is reached at which none of the available edges leads to a cost reduction; such a basic feasible solution is optimal.
In this note we, describe the sufficient and "necessary" conditions for a [[LPP Polyhedra In Standard Form#^12f1aa|basis]] associated to a basic feasible solution to be optimal.

Throughout this note we shall consider the standard form problem:
![[LPP In Standard Form#^linear-program-standard-form]]
where, [[LPP Polyhedra In Standard Form#The Full Row Rank Assumption| without loss of generality]], we shall assume that $A \in \mathbb{R}^{m \times n}$ has linearly independent rows.

# Optimality Conditions For A Basis
## Feasible Cost Reducing Directions
### Feasible Direction Definition
Many optimization algorithms work as follows: starting from a feasible solution, they search its neighbourhood to find a nearby feasible solution with lower cost. If no nearby feasible solution leads to a cost improvement, the algorithm terminates and we have a *locally optimal solution*.
Since LPPs are convex optimization problems, [[Local Optimality In Convex Optimization#^727d82|local optimality implies global optimality]], therefore by finding a local optimum we have solved the LPP. 
In this section we concentrate on the problem of searching for a direction $d\in\mathbb{R}^{n}$ of cost decrease in a neighbourhood of a given basic feasible solution. Clearly, we should only consider directions $d$ such that we are able to remain inside the feasible region. This leads to the following definition:

> [!definition] Feasible Direction
> Given a feasible region $P \subseteq \mathbb{R}^{n}$ and a point $x \in P$, a vector $d \in \mathbb{R}^{n}$ is said to be a **feasible direction** at $x$, if there exists $\theta > 0$ such that $x+\theta d \in P$. 

^24a7e6

### Finding Feasible Directions Along The Polyhedron Edges

^911911

Let $x$ be a [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic feasible solution]] to the standard form problem, and let **$B_{1}, \dots, B_{m}$** be the indices of the [[LPP Polyhedra In Standard Form#^12f1aa|basic variables]], and **$B = [A_{B_{1}}, \dots, A_{B_{m}}]$** the corresponding basis matrix. In particular, we have $x_{i} = 0$ for every nonbasic variable, while the vector $x_{B}$ of basic variables is given by 
$$
x_{B} = B^{-1}b
$$

==We want to move from one basic feasible solution to another==. To do this we consider moving away from $x$ to a new vector $x+\theta d$ by selecting a nonbasic variable $x_{j}$ and increasing it to a positive value $\theta$ while keeping the remaining nonbasic variables at zero. Geometrically, this means moving along the [[LPP Polyhedra In Standard Form#Adjacent Basic Solutions And Adjacent Bases|edge]] of the polyhedron.
Algebraically **$d$ has $d_{j} = 1$ and $d_{i} = 0$ for all basic variables $i \neq j$**. We want to determine the basic variables $d_{B}$  of $d$ so that $x+\theta d$ is feasible. This necessarily imposes the condition $A(x+\theta d) = b$, but since $Ax = b$ this must mean that $Ad = 0$, hence:
$$
0 = Ad = A_{j} + \sum_{i = 1}^{m}A_{B_{i}}d_{B_{i}} = A_{j} + Bd_{b}
$$
and since $B$ is invertible, we obtain:

$$
d_{B} = -B^{-1}A_{j}
$$

**The direction $d$** that we have just constructed will be refereed to as **the $j$th basic direction**. 

> [!definition] $j$th basic direction
> For a [[LPP In Standard Form#^linear-program-standard-form|LPP in standard form]] with constraint matrix $A \in \mathbb{R}^{m\times n}$, let $x$ be a basic feasible solution with [[LPP Polyhedra In Standard Form#^12f1aa|basis]] $B_{1},\dots,B_{m}$ and basis matrix $B \in\mathbb{R}^{m \times m}$.
> For any $j \not\in \{ B_{1},\dots,B_{m} \}$ the $j$th basic direction $d \in\mathbb{R}^{n}$ is the vector defined as follows:
>$$
>d_{i} := \begin{cases}
>1 \quad \text{for} \; i=j \\
>0 \quad \text{for} \; i \not\in \{ B_{1},\dots,B_{m}, j \} \\
>d_{B_{k}} \quad \text{for} \; i = B_{k}
>\end{cases}
>\qquad
>\text{where} \quad 
>d_{B} = -B^{-1}A_{j}
>$$

^fbcf42


So far we have guaranteed that the equality constraints are respected, what about the nonnegativity constraints? All non basic variables stay at zero level, except for $x_{j}$ that gets increased.  Regarding basic variables we have two cases:
1) If $x$ is a [[LPP Degeneracy#^bd5d3e|nondegenerate]] basic feasible solution then $x_{B} > 0$ and for $\theta$ small enough $x_{B} + \theta d_{B} \geq 0$, so feasibility is maintained;
2) If x is a degenerate basic feasible solution then $d$ is not a feasible direction whenever for one of the zero basic variables $x_{B_{i}}$ we have $d_{B_{i}}<0$.

> [!proposition]
> Let $x$ be a [[LPP Degeneracy#^bd5d3e|nondegenerate]] basic feasible solution. Then the $j$th [[#^fbcf42|basic direction]] $d$ is always a [[#^24a7e6|feasible direction]].

^eb4930

> [!proof]-
> It follows directly from the [[#Finding Feasible Directions Along The Polyhedron Edges|above]] arguments.
> `\end{proof}`

> [!example]- Graphical Representation Of The jth Feasible Direction
> ![[lpp_basic_directions_example.excalidraw|500]]
As [[LPP Graphical Representation And Intuitions#Geometrical Meaning Of Standard Form Problems|previously discussed]], we can visualize the feasible set of a LPP in standard form by standing on the affine set defined by $Ax = b$. In this example $n = 5$ and $n-m = 2$.
The basic feasible solution $E$ is nondegenerate because it is exactly defined by the intersection of 2 edges, $x_{1} = 0$ and $x_{3} = 0$. Moving along the edge of the polyhedron, say from $E$ to $F$, means increasing the value of $x_{1}$ while keeping $x_{3}$ to zero. This is the $1$st basic direction in $E$. As expected by [[#^eb4930]], this IS a feasible direction.
Now, let us consider $F$ as a starting point. $F$ is degenerate because $3$ variables ($x_{3}, x_{4}, x_{5}$) are zero. Let's consider the basis $B_{1},B_{2} = 3,5$ and let us pick the $3$rd basic direction in $E$. This is equivalent to moving along the edge $FG$. Note that this direction is NOT a feasible direction, as it takes us outside of the feasible set.
### The Reduced Cost Of The $j$th Basic Direction
If $d$ is the $j$th basic direction, then the rate $c^{T}d$ is the cost change along the direction $d$. Since $d_{i} = 0$ for all nonbasic variables $i\neq j$, then $c^{T}d$ can be rewritten as:
$$c^{T}d = c_{j}+c_{B}^{T}d_{B} = c_{j}-c^{T}_{B}B^{-1}A_{j} =: \bar{c}_{j}$$
The quantity $\bar{c}_{j}$ is very important, and it's known as *reduced cost*. In the reduced cost, the term $c_{j}$ is the cost per unit increase in the variable $x_{j}$, while the term $-c_{B}^{T}B^{-1}A_{j}$ is the cost of the compensating change in the basic variables necessitated by the constraint $Ax = b$.

For the sake of convenience, if we were to include $j = B_{k}$ in the definition of reduced cost, then we would have $B^{-1}[A_{B_1},\dots,A_{B_{m}}] = \mathbb{1}_{m}$, so $B^{-1}A_{j} = e_{k}$ and:
$$
\bar{c}_{j} = c_{B_{k}} - c_{B}^{T}B^{-1}A_{B_{k}} = c_{B_{k}} - c_{B}^{T}e_{k} = c_{B_{k}} - c_{B_{k}} = 0
$$
so, by the same definition, *the reduced cost at every basic variable is zero*.

> [!definition] Reduced Cost $\bar{c}_{j}$
> For a [[LPP In Standard Form#^linear-program-standard-form|LPP in standard form]] with constraint matrix $A \in \mathbb{R}^{m\times n}$ and cost vector $c \in \mathbb{R}^{n}$, let $x$ be a basic feasible solution with [[LPP Polyhedra In Standard Form#^12f1aa|basis]] $B_{1},\dots,B_{m}$ and basis matrix $B \in\mathbb{R}^{m \times m}$.
> For any $j=1,\dots,n$ the **$j$th reduced cost $\bar{c}_{j}$** of the variable $x_{j}$ is defined as:
> $$
> \bar{c}_{j} := c_{j}-c_{B}^{T}B^{-1}A_{j}
>$$
> The vector $\bar{c} := (\bar{c}_{1},\dots,\bar{c}_{n})$ is called the **vector of reduced costs**.
> For nonbasic variables $j \not\in \{ B_{1},\dots,B_{m} \}$ the reduced cost $\bar{c}_{j}$ is, by construction, the rate of cost change along the $j$th [[#^fbcf42|basic direction]] $d$, i.e. the value of $c^{T}d$.

^3cc9c2

> [!example]-
> Consider the linear programming problem:
> $$
> \begin{array}{}
> \min_{x \in \mathbb{R}^{4}} &  c^{T}x \\ \\
> \text{s.t.} & x_{1} & + x_{2} & +x_{3} &+ x_{4} & = 2 \\
> & 2x_{1} & & +3x_{3} & +4x_{4} & = 2 \\
> & x  \geq 0
> \end{array}
> $$
> we have that $A_{1}=(1,2)$ and $A_{2} = (1,0)$ are linearly independent, so we can choose $x_{1}$ and $x_{2}$ as our basic variables. The corresponding basis matrix is:
> $$
> B = 
> \begin{pmatrix}
> 1 & 1 \\
> 2 & 0
> \end{pmatrix}
> $$
> By solving $Bx_{B} = (2,2)$ we obtain  $x_{B} = (x_{1},x_{2}) = (1,1)$, so our basic solution is $x = (1,1,0,0)$, which is also feasible. We can construct the third basic direction $d$ by increasing the nonbasic variable $x_{3}$, so $d_{3} = 1$ and $d_{4} = 0$. The basic variables $d_{B}$ are obtained by trying to maintain feasibility, i.e. by imposing $Ad = Bd_{B} +A_{3} = 0$, so we find:
> $$
> \begin{pmatrix}
> d_{1} \\
> d_{2}
> \end{pmatrix}
> = \begin{pmatrix}
> d_{B_{1}} \\
> d_{B_{2}}
> \end{pmatrix}
> = 
> -B^{-1} A_{3} =
> - \begin{pmatrix}
> 0 & 1 / 2 \\
> 1 & -1 / 2
> \end{pmatrix}
> \begin{pmatrix}
> 1 \\
> 3
> \end{pmatrix} = 
> \begin{pmatrix}
> - 3 / 2 \\
> 1 / 2
> \end{pmatrix}
> $$
> The cost of moving along this basic direction is $c^{T}d = -\frac{3}{2} c_{1} + \frac{1}{2} c_{2} + c_{3}$, which is the same as the reduced cost of the variable $x_{3}$.
## Optimal Basis
> [!theorem] Characterization Of Optimal Basic Solutions
> For a [[LPP In Standard Form#^linear-program-standard-form|LPP in standard form]]  consider a basic feasible solution $x$ with a [[LPP Polyhedra In Standard Form#^12f1aa|basis matrix]] $B$, and let $\bar{c}$ be the corresponding [[#^1abbda|vector of reduced costs]]. The following holds:
> 1) If $\bar{c}\geq 0$ then $x$ is optimal;
> 2) if $x$ is optimal and [[LPP Degeneracy#^bd5d3e|nondegenerate]], then $\bar{c} \geq 0$.

^35d8cb

> [!proof]-
> ***($1$)*** Assume that $\bar{c} \geq 0$. For any feasible solution $y$ let $d := y-x$.  To prove $x$ optimality we will prove that $c^{T}x \leq c^{T}y$ , which is equivalent to showing that $c^{T}d \geq 0$.
 > Feasibility implies that $Ad = Ay - Ax = b-b =  0$.  Let $N$ be the indices of nonbasic variables. The previous equation can be rewritten as:
 > $$
 > Ad = Bd_{B} + \sum_{i \in N} A_{i} d_{i} = 0
 > $$
 > Since $B$ is invertible we obtain:
 > $$
 > d_{B} = - \sum_{i \in N} B^{-1} A_{i} d_{i}
 > $$
 > and
 > $$
 > c^{T}d =  \left(\sum_{i \in N}c_{i} d_{i}\right) - c^{T}_{B}d_{B} = \sum_{i \in N}(c_{i}-c_{B}^{T}B^{-1}A_{i})d_{i} = \sum_{i \in N}\bar{c}_{i}d_{i}
 > $$
 > For any $i \in N$ we must have $x_{i} = 0$, so $d_{i} = y_{i} \geq 0$, therefore concluding that $c^{T}d = \sum_{i \in N}\bar{c}_{i}d_{i} \geq 0$.  
 > 
 > ***($2$)***  Let $x$ be a nondegenerate basic feasible solution. Assume that $\bar{c}_{j} <0$ for some $j$ : we will show that $x$ is not optimal. Since $\bar{c}_{j} = 0$ for all basic variables $j$, then $j$ must be a nonbasic variable. By construction, $\bar{c}_{j}$ is the rate of cost change along the $j$th basic direction $d$. Since $x$ is nondegenerate, the $j$th basic direction is a direction of feasible cost decrease, hence, by picking $x+\theta d$ with $\theta>0$ small enough, we find a feasible solution whose cost is less than $x$.
 > `\end{proof}`

The previous theorem tells us that, for a basis $B_{1},\dots,B_{m}$, we obtain an optimal solution as long as the associated basic solution is feasible, i.e. if $x_{B}\geq 0$, and if the associated reduced cost is nonnegative. This justifies the following definition:

> [!definition] Optimal Basis 
> A [[LPP Polyhedra In Standard Form#^12f1aa|basis matrix]] $B$ is said to be **optimal** if:
> 1) $x_{B} = B^{-1}b \geq 0$
> 2) $\bar{c} =  c - c_{B}B^{-1}A \geq 0$

^b193cd

*Having an optimal basis means that we have found the optimum*. ==If a basic solution is nondegenerate==, then the previous theorem tells us that ==the basis being optimal is also a necessary condition for optimality==. 
On the other hand, ==degenerate basic solutions may be optimal even if their basis is not optimal==, i.e. they may be optimal but have a negative component in the reduced cost vector.