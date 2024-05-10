---
tags:
  - "#math/optimization/linear-programming/simplex-method"
next: "[[LPP Simplex Method And The Column Geometry]]"
prev: "[[LPP Simplex Method Finding An Initial Solution]]"
up: "[[Linear Programming Master Note]]"
---

# Computational Complexity Of The Simplex Method
For a [[LPP In Standard Form#^d3d2e7|LPP in standard form]] of the form:
![[LPP In Standard Form#^linear-program-standard-form]]
with $A \in \mathbb{R}^{m\times n}$,  we have seen that the [[LPP Simplex Method Implementations#The Full Tableau Algorithm Complexity|algorithmic complexity]] of each iteration of the [[LPP The Simplex Method Framework#^f73df0|simplex method]] is, in the worst case, $\mathcal{O}(mn)$. In this note we will discuss how many iterations are necessary to terminate the simplex method.
## Worst Case Complexity
The number of [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic feasible solutions]] can be exponential in the number of variables $n$. In principle, with an "unlucky" pivoting rule, we may need to explore almost all the basic feasible solutions of the polyhedron, meaning that *the computational complexity is exponential*. It turns out that, unfortunately, that is indeed the case: we can define a family of problems and a pivoting rule for which an exponential number of pivots is required.

Consider the unit cube in $\mathbb{R}^{n}$:
$$
P := \{ x \in \mathbb{R}^{n} \; | \; 0 \leq x_{i} \leq 1 \}
$$
The cube has $2^{n}$ vertices, each determined by the choice between the active constraint $x_{i} = 0$ and $x_{i} = 1$ for each $i = 1,\dots,n$.  For the unit cube there exists a path that travels along the edges visiting each vertex exactly once; we call such a path a **spanning path**:

![[Cube Spanning Path.excalidraw|500]]

Recursively, a spanning path can be found by using the following algorithm:

```pseudo
 \begin{algorithm}
     \caption{$n$-Dimensional Unit Cube Spanning Path}
     \begin{algorithmic}
       \Procedure{VisitJFace}{$x, j$}
         \If{$j = 0$}
           \State Visit $x$ and terminate the procedure
         \EndIf
 		\State \Call{VisitJFace}{x, j-1}
 		\State set $x_j = 1-x_j$
 		\State \Call{VisitJFace}{x, j-1}
       \EndProcedure
 	  \State \\
 	  \State Set $x = (0,\dots, 0)$
 	  \State \Call{VisitJFace}{x, n}
       \end{algorithmic}
     \end{algorithm}
 ```

^a07f1f

If we were able to find a cost function such that the cost strictly decreases along the spanning path, then we would have proven that the simplex method has an exponential complexity, because we could pivot along the spanning path and visit each node of the unit cube, performing $2^{n}-1$ iterations.
This can be done. *Indeed*, Consider the cost function:
$$
f(x) = -x_{n}
$$
The basic idea is to skew the unit cube a little so that $f$ strictly decreases along the spanning path. This can be achieved by choosing some $\epsilon \in (0, 1 / 2)$ and by perturbing the constraints as follows:
$$ 
\begin{align}
\epsilon \leq & x_{1} \leq 1 \\
\epsilon x_{i-1} \leq &x_{i} \leq 1- \epsilon x_{i-1} \qquad i = 2,\dots,n
\end{align}
$$
> [!algorithm]- Code For Skewed Spanning Path
> The Python code below shows how a visit on this skewed unit cube is performed and highlights how the cost is strictly decreasing along the spanning path
> ```Python
> def skewed_bfs(x: list[int]):
>     eps = 1 / 4
>     y = []
>     y.append(1 if x[0] else eps)
>     for j, xj in enumerate(x[1:], start=1):
>          y.append(1-eps*y[j-1] if xj else eps * y[j-1])
>     return y
> 
> 
> def visit_unit_cube(n: int):
>     def visit_j_dimensional_face(x: list[int], j: int):
>         if j == -1:
>             y = skewed_bfs(x)
>             print(f"{x=}  -->  {y} ---> has cost {-y[-1]}")
>             return
>         visit_j_dimensional_face(x, j - 1)
>         x[j] = int(not x[j])
>         visit_j_dimensional_face(x, j - 1)
> 
>     visit_j_dimensional_face([0] * n, j=n - 1)
> 
> visit_unit_cube(3)
> ```

> [!theorem] The Simplex Method Has Exponential Complexity
> For $\epsilon \in(0,1 / 2)$, consider the linear programming problem:
> $$
> \begin{align}
> \min_{x \in \mathbb{R}^{n}} \quad &  -x_{n} \\
> \text{s.t.} \quad & \epsilon \leq x_{1} \leq 1 \\
> & \epsilon x_{i-1} \leq x_{i} \leq 1 - \epsilon x_{i-1} \quad i = 2,\dots,n
> \end{align}
> $$
> Then the following statements hold:
> 1) The feasible set has $2^{n}$ vertices;
> 2) There exists a sequence of adjacent vertices such that the cost is strictly decreasing;
> 3) There exists a pivoting rule under which the simplex method requires $2^{n}-1$ pivots in order to terminate.

> [!proof]-
> ***(1)*** It's easy to verify that $n$ linearly independent active constraints can be found by, and only by, choosing  between the lower or upper constraint for each $i$. This means that each basic feasible solution is obtained by choosing between $2$ elements $n$ times , hence there is a total of $2 ^{n}$ vertices.
> *(2)* We prove this point by induction, reasoning on the [[#^a07f1f|construction of the spanning path]] algorithm that we've seen before. In order to visit this particular polyhedron, the algorithm needs to be slightly modified in order to respect the constraints, but the idea is basically the same. Given a unit cube vertex $y$, we can build a vertex $x$ of the skewed polyhedron by iteratively defining $x_{j}$ as:
> $$
> x_{1} = \begin{cases}
> \epsilon & \text{if} \; y_{1} = 0 \\
> 1 & \text{if} \; y_{1} = 1
> \end{cases}
> \qquad
> x_{j} = \begin{cases}
> \epsilon x_{j-1}  &\text{if} \; y_{j} = 0 \\
> 1 - \epsilon x_{j-1} & \text{if} \; y_{j} = 1
> \end{cases}
> $$
> Let's call the recursive algorithm that performs this kind of visit `SkewVisit(x,n)`.  By induction we want to prove the following statement:
> 
> > [!proposition]
> > `SkewVisit(x,n)` performs  a total of $2^{n}$ visits to adjacent vertices. Moreover, if the algorithm starts from the minimum $x$ associated to $y = 0$, then the visit is such that the quantity $x_{n}$ strictly increases. Similarly, if the algorithm starts from the maximum value $x$ associated to $y = (0,\dots,0,1)$ then the visit is such that the quantity $y_{n}$ strictly decreases.
> 
> > [!proof]
> > *(Base case $n = 1$)* In the base case the statement is trivially proven just by testing the two cases $y = 0$ and $y = 1$.
> > *(Inductive step $n \Rightarrow n+1$)* Let's say we're starting from the point associated to $y = 0$, then we have $x_{n+1} = \epsilon x_{n}$ and, by induction hypothesis,  the algorithm performs $2^{n}$ adjacent visits on the remaining $(x_{1},\dots,x_{n})$ so that the value of $x_{n}$ increases, which implies that $x_{n+1} = \epsilon x_{n}$ also increases. 
> > The algorithm then sets $y_{n+1} = 1$ implying that $x_{n+1} = 1 - \epsilon x_{n}$. This new point is clearly adjacent to the previous. We need to prove that  this $x_{n+1}$  is strictly larger than $\epsilon x_{n}$, this follows trivially from the fact that $\epsilon \in(0,1 / 2)$, indeed:
> > $$
> > x_{n} \in [0,1] \; \text{and} \; \epsilon < \frac{1}{2} \implies 2 \epsilon x_{n} < 1 \implies 1 - \epsilon x_{n} > \epsilon x_{n}
> > $$
> > Finally, by induction hypothesis, the recursive visit on $(x_{1},\dots, x_{n)}$ starts on the optimal point due to the previous `SkewVisit`, in particular, $x_{n}$ is strictly decreasing, hence the quantity $x_{n+1} = 1 - \epsilon x_{n}$ is strictly increasing. In total the algorithm has performed $2^{n} + 2^{n} = 2^{n+1}$ visits to adjacent vertices with an increasing value of $x_{n+1}$, so two of the statements have been proven. The statement on the visit performed starting from the optimum is proven in a similar fashion `\end{proof}`
> 
> Using the previous proposition we can conclude that there exists a path of adjacent vertices for which the cost $-x_{n}$ is strictly decreasing.
> 
> *(3)* Since the cost along the spanning path is strictly decreasing, we can pivot along the path edges, performing a total of $2^{n}-1$ iterations of the simplex method.
> `\end{proof}`

### Worst Cases Are (Currently) Built For Each Pivoting Rule
The previously demonstrated worst-case complexity specifically applies to a certain pivoting rule, not universally. Interestingly, with some rules, one can solve the aforementioned linear programming problem in a single pivot.
For many widely-employed pivoting rules, examples exist showing an exponential worst-case complexity. Yet, for each of these instances, there exists an alternate pivoting rule without exponential complexity. The question of whether exponential complexity applies across all pivoting rules, or if there is one with polynomial time, still remains an open issue. This question is what motivates the *Hirsch conjecture*.

## Hirsch Conjecture
> [!definition] Diameter Of Polyhedra
> For a polyhedron $P$ let $V$ be the set of vertices of $P$.  For two vertices $x,y \in B$ we define the distance $d(x,y)$ as the minimum amount of edges that need to be traversed in order to reach $y$ starting from $x$.
> The diameter of $P$ is defined as
> $$
> D(P) := \max_{x,y \in V} d(x,y)
> $$
> Finally, we define $\Delta(n,m)$ as the maximum of $D(P)$ over all bounded polyhedra in $\mathbb{R}^{n}$ defined by $m$ inequality constraints. Similarly, we define $\Delta_{u}(n,m)$ for unbounded Polyhedra.

To construct an example of exponential worst-case complexity applicable to every pivoting rule, one could devise a LPP that initiates at point $x$ and aims in the direction of an optimal $y$ where the distance $d(x,y)$ is exponential. This distance would represent the minimum number of pivots any rule requires. If the growth of both $\Delta(n,m)$ and $\Delta_{u}(n,m)$ were exponential, then a polynomial-time pivoting rule would not exist.
The practical success of the simplex method has led to the conjecture that $\Delta(n,m)$ and $\Delta_{u}(n,m)$ do not grow exponentially fast:

> [!conjecture] Hirsch Conjecture
> $\Delta(n,m) \leq m -n$

Despite their significance, relevant upper and lower bounds are yet to be found for $\Delta$ and $\Delta_{u}$.
It is known that Hirsch conjecture is false both for bounded and unbounded polyhedra, in particular:
$$
\Delta_{u}(n,m) \geq m-n +  \left\lfloor  \frac{n}{5}  \right\rfloor 
$$
Regarding upper bounds, it has been established that the worst-case diameter grows slower than exponentially. Nevertheless, the upper bound that was found is faster than any polynomial:
$$
\Delta(n,m) \leq \Delta_{u}(n,m) \leq m^{1 + \log_{2}n} = (2n)^{\log_{2}m}
$$
## Practical And Average Case Complexity
Despite the worst-case-complexity analysis suggesting otherwise, the simplex method has been proven to be very efficient in practice, *typically taking only $\mathcal{O}(m)$ iterations* to find an optimal solution.
For this reason, quite a bit of research has tried to understand the average behaviour of the algorithm, in order to explain its practical success. There is no natural definition of the term "average" for linear programming problems, but one of the most convincing results comes from the concept of *smoothed analysis*.

The key to the simplex method's success lies in the fact that the worst-case scenarios are highly specific and delicately chosen examples. This means that the conditions needed for the algorithm to show exponential complexity have to be exactingly arranged. Even the slightest perturbation can significantly reduce the complexity to a more typical case.
This is the main idea behind the concept of **smoothed analysis**: we consider the worst case computed on the average complexity of a randomly perturbed problem. Typically, the perturbation to the problem is in the form of Gaussian noise. Under specific pivoting rules (such as the *shadow vertex pivot rule*) it can be shown that the complexity is polynomial.