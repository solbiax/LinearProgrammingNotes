---
tags:
  - math/optimization/linear-programming
  - math/theorem
next: "[[LPP Polyhedra In Standard Form]]"
prev: "[[LPP Polyhedra And Their Vertices]]"
up: "[[Linear Programming Master Note]]"
---
# Fundamental Theorem Of Linear Programming
We will confirm the intuition that as long as a linear programming problem has an optimal solution, and as long as the feasible set has at least one extreme point, then we can always find an optimal solution within the set of extreme points. We will also discuss the necessary and sufficient conditions for a polyhedron to have at least one extreme point.
These results will culminate in what is known as the fundamental theorem of linear programming, which defines all the possible outcomes of a linear programming problem.
## Existence of extreme points
We write the necessary and sufficient conditions for a polyhedron to have at least one extreme point:

> [!theorem] Existence of extreme points
> For a matrix $A \in \mathbb{R}^{m \times n}$  consider a nonempty [[LPP Polyhedra And Their Vertices#^polyhedron-definition|polyhedron]]
> ![[LPP Polyhedra And Their Vertices#^polyhedron-formula]]
> Then, the following are equivalent:
> 1) $P$ has at least one [[LPP Polyhedra And Their Vertices#^extreme-point-definition|extreme point]];
> 2) $P$ does not contain a line;
> 3) There exists $n$ vectors out of the family $a_{1},\dots,a_{m}$ which are linearly independent.

^existence-of-vertices

> [!proof]-
> ***($2 \Rightarrow 1$)*** Let $x \in P$ and let $I$ be the set of active constraints at $x$. If $n$ of the active constraints are linearly independent, then $x$ is a basic feasible solution, [[LPP Polyhedra And Their Vertices#^corner-definition-equivalence|therefore]] an extreme point. 
> Otherwise, let us consider a vector $d \neq 0$ orthogonal to $<a_{i} \; | \; i \in I>$ and the line $x + \lambda d$. We note that all the active constraints of $x$ remain active along this line, since $a_{i}^{T} (x + \lambda d) =  a_{i}^{T}x$. 
> By hypothesis, the line is not contained in $P$, hence there must be a point on the line where one of the constraints (not in $I$) is violated. So, by continuity, there must be a point $x + \lambda^{*}d$ where a new constraint $j$ becomes active. We claim that $a_{j}$ is linearly independent from the previously active $a_{i}$. Indeed, if it weren't then $a_{j} = \sum \beta_{i} a_{i}$ and therefore $a_{j}^{T}d = 0$, meaning that $a_{j}^{T}(x+\lambda d)$ is constant along the line, which is absurd, because this would mean that $j$ was active at $x$.
> We have thus found a point $x+\lambda^{*}d$ that has one more active linearly independent constraint, and by induction we can eventually reach a point that has $n$ active constraints and so is an extreme point.
> 
> ***($1 \Rightarrow 3$)*** If $x$ is an extreme point $x$, then it's also a [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic solution]], therefore there exists $n$ linearly independent vectors $a_{i}$. 
>
> ***($3 \Rightarrow 2$)*** W.L.O.G. let $a_{1},\dots,a_{n}$ be linearly independent. For any line $x + <d>$ with $x \in P$ we know that there must be a vector $a_{i}$ non orthogonal to $d$ (otherwise $d$ would be linearly independent from $a_{1},\dots,a_{n}$). 
> For $\lambda \to \pm \infty$ we would have $a_{i}^{T}(x + \lambda d) \to \pm \text{sgn}(a_{i}^{T}d) \cdot\infty$,  hence the constraint will eventually be violated along the line, thus $P$ does not contain $x + <d>$. 
> `\end{proof}`

> [!corollary]
> Every nonempty bounded polyhedron, or polyhedron in [[LPP In Standard Form#^d3d2e7|standard form]] has at least one basic feasible solution

^c85132

>[!proof]-
>Every bounded polyhedron cannot contain a line, so by the previous theorem we conclude that there is at least one basic feasible solution.
>Every polyhedron in standard form has the constraints $x_{i} \geq 0$ to which the basis $e_{1},\dots,e_{n}$ is associated. By the previous theorem there must be at least one basic feasible solution.
>`\end{proof}`

## Optimality Of Extreme Points
With the next theorem we show that for linear programming problems that admit an optimal solution, we can always find an extreme point which is also optimal (if the polyhedron has extreme points):

> [!theorem] Optimality Of Extreme Points
> Consider a [[LPP Definition#^07d883|linear programming problem]] over a [[LPP Polyhedra And Their Vertices#^polyhedron-definition|polyhedron]] $P$. Suppose that $P$ has at least one [[LPP Polyhedra And Their Vertices#^extreme-point-definition|extreme point]] and that there exists an optimal solution. Then, there exists an optimal solution which is an extreme point of $P$.

> [!proof]-
> Let $Q$ be the set of all optimal solutions, and let $v:= c^{T}x^{*}$ be the optimal value. We can always write the polyhedron $P$ in general form:
> ![[LPP Polyhedra And Their Vertices#^polyhedron-formula]]
> We note that $Q = \{ x \in \mathbb{R}^{n} \; | \; Ax \geq b, \; c^{T}x = v\}$ can also be written  as a polyhedron and that $Q \subseteq P$. Since $P$ [[#^existence-of-vertices|does not contain any lines]], then $Q$ doesn't either, hence $Q$ must have an extreme point.
> Let $y^{*}$ be an extreme point of $Q$, we show that $y^{*}$ it's also an extreme point of $P$. By contradiction, suppose that:
> $$y^{*} = \lambda w + (1-\lambda)z \quad z,w \in P-\{ y^{*} \} \quad \lambda \in[0,1] $$
> since $c^{T}y^{*} = \lambda c^{T}w + (1-\lambda)c^{T}z \geq \lambda c^{T}y^{*} + (1-\lambda)c^{T}y^{*} = c^{T}y^{*}$  it must follow that also $w$ and $z$ are optimal, so $z,w \in Q$. But this contradicts the fact that $y^{*}$ is an extreme point of $Q$, absurd. Therefore we conclude that $y^{*}$ is a corner of $P$ which is also optimal, since it belongs to $Q$.
> `\end{proof}` 

The next result is stronger than the above theorem: we show that if $P$ has at least one extreme point and the cost function is finite over the feasible set, then the optimal solution exists and can be found in one of the corners (a priori this is not obvious! think of $\inf_{x > 0}1 / x$, which has a finite infimum but no optimum).

To show the result we will need the following definition:
> [!definition]
> Let $P$ be a [[LPP Polyhedra And Their Vertices#^polyhedron-definition|polyhedron]]. An element $x \in P$ has **rank $k$** if we can find $k$, but not more than $k$, linearly independent constraints that are active at $x$.

By definition, a point is a basic feasible solution ([[LPP Polyhedra And Their Vertices#^corner-definition-equivalence|hence]] an extreme point) if and only if it is a point of rank $n$. 
We are now ready to prove the following:

> [!theorem]
> Consider a LPP over a [[LPP Polyhedra And Their Vertices#^polyhedron-definition|polyhedron]] $P$.  If $P$ has at least one  [[LPP Polyhedra And Their Vertices#^extreme-point-definition|extreme point]] then either the optimal cost is $-\infty$, or there exists an extreme point which is optimal.

^6d6d23

> [!proof]-
> Either $\inf_{x \in P} c^{T}x$ is $-\infty$, or it's finite. In the case it's finite, we want to show that there is a corner that has an optimal value. To do that we will show that for any non extreme point of the feasible set, i.e. for any point of rank $k<n$, we can move inside of $P$ towards a point of higher rank without increasing the value of the cost function, eventually reaching a vertex of the polyhedron. This would prove that the infimum over $P$ is the same as the infimum over the extreme points of $P$, and [[LPP Polyhedra And Their Vertices#^56c105|since they are finite]] this would prove that there is an extreme point which is optimal.
> 
> Let $P = \{ x \in \mathbb{R}^{n} \; | \; Ax \geq b \}$ and consider some $x \in P$ of rank $k < n$ with active constraints $I$.  We want to show that there exists some $y \in P$ which has greater rank and satisfies $c^{T}y \leq c^{T}x$.  By definition of rank, we know that  $<a_{i} \; | \; i \in I>$ is a proper subspace of $\mathbb{R}^{n}$, hence we can find $d \in \mathbb{R}^{n}$ orthogonal to every $a_{i}$, and, by possibly taking the negative of $d$, we can assume that $c^{T}d \leq 0$. There are two cases:
> ***($c^{T}d < 0$)*** If $c^{T}d <0$ then we consider the half-line $x+\lambda d$ with $\lambda \geq 0$. Since $a_{i}^{T}(x+\lambda d) = a_{i}^{T}x = b_{i}$, all constraints in $I$ remain active along this line. 
> If the entire half line was contained in $P$, then the optimal cost would be $-\infty$, so the line eventually violates another constraint $j \not\in I$. By continuity there must exist a $\lambda^{*}$ such that $y:=x+\lambda^{*} \in P$ that also has $j$ as an active constraint. We were able to find a point of higher rank with lower value  $c^{T}y = c^{T}(x+\lambda^{*}d)<c^{T}x$.
> ***($c^{T}d=0$)*** Suppose that $c^{T}d=0$. We consider the line $x+\lambda d$. Since $P$ has at least an extreme point, [[#^existence-of-vertices|then it contains no lines]], so the line must eventually exit $P$ on a point in the polyhedron $y = x+\lambda^{*}d$, which, for reasons similar to the previous case, has a greater rank than $x$ and a cost equal to $x$.
> 
> In either case we were able to find $y$ of higher rank and lower value of the cost function. Together with the arguments above, this concludes the proof.
> `\end{proof}`

As a natural consequence of all the previous results, we can state the fundamental theorem of linear programming:

> [!corollary] Fundamental Theorem Of Linear Programming
> For a a [[LPP Definition#^07d883|linear programming problem]],  exactly one of the following holds:
> 1) The problem is infeasible;
> 2) The problem is unbounded;
> 3) The problem has an optimal solution.
>
> Furthermore, if the problem has an optimal solution and its feasible set has extreme points, then the an optimum can be found in one of the extreme points.

^537f75

> [!proof]-
> If the problem is infeasible  or unbounded there is nothing to prove. In the case the problem is feasible and bounded, we [[LPP In Standard Form#^1d57d7|have seen]] that the LPP can be rewritten in an equivalent [[LPP In Standard Form#^d3d2e7|standard form]].  According to  [[#^c85132]], every feasible problem in standard form has at least one extreme point in their feasible set, therefore, by [[#^6d6d23]], the problem has an optimal solution.
> `\end{proof}`