---
tags:
  - math/optimization/linear-programming
next: "[[LPP Simplex Method Computational Complexity]]"
prev: "[[LPP Simplex Method Anticycling Rules]]"
up: "[[Linear Programming Master Note]]"
---
# Finding An Initial Basic Feasible Solution
Thus far, we have discussed the [[LPP The Simplex Method Framework#Iteration Of The Simplex Method|iterations of the simplex method]]  and we have been able to prove that [[LPP The Simplex Method Framework#^a3d33b|it will always terminate]] as long as we start the algorithm from a [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic feasible solution]] and we use [[LPP Simplex Method Anticycling Rules#Anticycling Rules|an anticycling rule]] .
In this note, we discuss how to find an initial basic feasible solution in order to start the simplex method.

### Special Case: $Ax \leq b$ with $b\geq 0$
Sometimes obtaining the initial basic feasible solution is straightforward. For example, suppose that we are trying to solve the problem:
$$
\begin{align}{}
\min_{x} \quad &  c^{T}x \\
\text{s.t.} \quad &  Ax \leq b \\
& x \geq 0
\end{align}
$$
Where $b \geq 0$.

We can introduce the slack variable $s \geq 0$ and rewrite $Ax \leq b$  as $Ax + s = b$. The vector $(x,s) = (0,b)$ is a basic feasible solution.
In general, however, ==finding an initial basic feasible solution is not easy and requires the solution of an auxiliary linear programming problem==.

## Using Artificial Variables To Find A Basic Feasible Solution
### Auxiliary Problem Definition
Consider the [[LPP In Standard Form#^d3d2e7|LPP in standard form]]:
![[LPP In Standard Form#^linear-program-standard-form]]
Where, without loss of generality, we assume that $b\geq 0$ (*indeed*, if $b_{i}<0$, we could just multiply the $i$th row of $A$ and $b_{i}$ by $-1$ and obtain an equivalent problem).
We introduce a vector $y \in \mathbb{R}^{m}$ of **artificial variables**, and consider the auxiliary problem:

$$
\begin{align}
\min_{x} \quad & y_{1} + \dots + y_{m} \\
\text{s.t.} \quad & Ax + y = b \\
& x \geq 0 \\
& y \geq 0
\end{align}
$$
^auxiliary-problem-def

For this problem, we always have that $(x,y) = (0,b)$ is a basic feasible solution, and its corresponding [[LPP Polyhedra In Standard Form#^12f1aa|basis matrix]] is the identity. This means that we can start and use the simplex method to solve the auxiliary problem.
There are two cases:
1) If the optimal solution $(\bar{x}, \bar{y})$ has $\bar{y} \neq 0$, then the original problem is infeasible. *Indeed*, if the problem was feasible, then for any $x\geq 0$ with $Ax= b$ we would have that $(x,0)$ is a solution with a strictly lower cost than $(\bar{x},\bar{y})$ for the auxiliary problem.
2) If the optimal solution $(\bar{x}, \bar{y})$ has $\bar{y} = 0$  then $\bar{x}$ is a feasible solution of the original problem. If the auxiliary problem terminated with a basis consisting only of columns of $A$, then $\bar{x}$ would be a basic solution as well and we would have accomplished our goal. Otherwise, we need to find a way to drive the artificial variables $y_{i}$ out of the basis
### Driving Artificial Variables Out Of The Basis
Suppose that we have found an optimal solution $(\bar{x},0)$ to the auxiliary problem but some of the artificial variables $y$ are in the final basis.  Let $k<m$ be the number of columns of $A$ that belong to the final basis and, without loss of generality, assume that they are the first indices of the basis $B_{1}, \dots, B_{k}$. We choose $m-k$ additional columns $A_{B_{k+1}},\dots,A_{B_{m}}$ of $A$ to obtain a set of $m$ linearly independent columns. With this basis, all nonbasic components of $\bar{x}$ are at zero level, and it follows that $\bar{x}$ is a basic feasible solution associated with this new basis as well. The artificial variables $y$ can then be dropped.
This procedure is called **driving the artificial variables out of the basis**, and depends on the assumption that $A\in\mathbb{R}^{m\times n}$ has rank $m$.

*In terms of the [[LPP Simplex Method Implementations#The Full Tableau Implementation|full tableau]],* driving the artificial variables out of the basis can be performed as follows:
Let the $\ell$th basic variable be an artificial variable corresponding to $y_{r}$. We look at the tableau and in the $\ell$th row and search for a column $j \in \{ 1,\dots,n \}$ so that the $(\ell,j)$ element is nonzero. 
If we are not able to find it, then the $\ell$th row of $B^{-1}A$ would be zero, which means that the $\ell$th row is linearly dependent and it can be dropped.
Otherwise, Since $B^{-1}A_{B_{i}} = e_{i}$ have the $\ell$th component equal to zero, then $B^{-1}A_{B_{1}},\dots,B^{-1}A_{B_{k}}$ must be linearly independent from $B^{-1} A_{j}$, which means that $A_{j}$ is linearly independent from $A_{B_{1}},\dots, A_{B_{k}}$.
We can bring $A_{j}$ into the basis and have the $\ell$th basic variable exit the basis. We perform a [[LPP Simplex Method Implementations#^5d32ae|tableau update]] in the usual way, with the difference that the pivot element could be negative.
Notice that since $\bar{y}_{r} = 0$, by adding multiples of the $\ell$th rows, the basic variable values remain exactly the same.  This means that after the change of basis we are still at the same basic feasible solution to the auxiliary problem, but we have removed one artificial variable.
We can repeat this process until all artificial variables have been eliminated.
## The Two-Phase Simplex Method
We can now summarize a complete algorithm for linear programming problems in [[LPP In Standard Form#^linear-program-standard-form|standard form]]. Note that the following algorithm also works when $A$ does not have a full row rank.

> [!algorithm] Two-Phase Simplex Method
> **PHASE 1**
>  1) By multiplying some of the constraints by $-1$, we transform the problem so that $b \geq 0$
>  2) We introduce the artificial variables $y_{1},\dots,y_{m}$ and consider the auxiliary problem:
>     ![[#^auxiliary-problem-def]]
>  3) By starting from the basic feasible solution $(0,b)$ we solve the auxiliary problem with the [[LPP The Simplex Method Framework#^f73df0|simplex method]]
>  4) If the optimal cost of the auxiliary problem is positive then the original problem is infeasible and we stop
>  5) Otherwise we drive the artificial variables out of the basis of the optimal solution. If the $\ell$th basic variable is artificial we examine the columns of $B^{-1}A_{j}$ for $j = 1,\dots,n$. If all of them are zero, then the $\ell$th row is a redundant constraint and we can remove it. Otherwise, if the $(\ell,j)$th element is nonzero, we apply a change of basis: $\ell$ exits the basis, while $j$ enters. We repeat this operation until all artificial variables are driven out
>  
>  **PHASE 2**
>  1) Starting with the basis found in phase 1, we compute the reduced costs associated to the basis
>  2) We iterate the simplex method until termination

This two-phase algorithm is a complete method that can handle all possible outcomes of a linear programming problem, as long as [[LPP Simplex Method Anticycling Rules|cycling is avoided.]] The algorithm will always terminate with one of the following possibilities:
1) The problem is infeasible detected in phase 1;
2) The problem is unbounded, detected in phase 2;
3) The problem has an optimal solution, found in phase 2.

## The Big-$M$ Method
The big-$M$ method is an alternative approach that combines the two phases of the two-phase algorithm into a single one. The idea is to consider the auxiliary problem with an alternative cost function:

$$
\begin{align}
\min_{x,y} \quad & c^{T}x +M\sum_{i}^{m}y_{i} \\
\text{s.t.} \quad & Ax + y = b \\
& x,y \geq 0
\end{align}
$$
where $M \gg 0$ is a large constant. Intuitively, as $M \to +\infty$ , in order to obtain an optimal solution the artificial variables need to be driven to zero. If the original problem is feasible and its optimal cost is finite, then for a sufficiently large $M$ that is indeed the case.
In practice, there is no reason for fixing a numerical value for $M$. We can leave $M$ as an undetermined parameter and let the reduced costs be functions of $M$. Whenever $M$ is compared to another number in order to determine if a reduced cost is negative, $M$ will be always treated as being larger.

> [!example]-
> Let us consider the following problem:
> $$
> \begin{array}{}
> \min_{x \in \mathbb{R}^{4}} & x_{1} & +x_{2} & +x_{3}   \\
> \text{s.t.}  & x_{1} & +2x_{2} &  +3x_{3} &  & =3 \\
> & &  4x_{2} & +9x_{3} & & =5 \\
>  &  &  & 3x_{3} & +x_{4}  & = 1 \\
>  & &  &  &  &  x \geq 0
> \end{array}
> $$
> We will use the big-$M$ method in order to solve it. Consider the auxiliary problem:
> $$
> \begin{array}{}
> \min_{x \in \mathbb{R}^{7}} & x_{1} & +x_{2} & +x_{3}  &  & +Mx_{5} & +Mx_{6} & +Mx_{7}  \\
> \text{s.t.}  & x_{1} & +2x_{2} &  +3x_{3} &  & +x_{5} & &   &    =3 \\
> & &  4x_{2} & +9x_{3} &  & &  +x_{6} & &  =5 \\
>  &  &  & 3x_{3} & +x_{4}  &  &  &   +x_{7} & = 1 \\
>  & &  &  &  &  &  &  &    x \geq 0
> \end{array}
> $$
> A basic feasible solution is obtained by setting $x = (0,0,0,0,3,2,1)$ , for which the basis matrix is $B = \mathbb{1}$, so the resulting [[LPP Simplex Method Implementations#^full-tableau-table|full tableau]] is:
> $$
> \begin{array}{| l | c|}
> \hline
> -c_{B}^{T}x_{B} & c^{T} - c^{T}_{B}B^{-1}A
> \\
> \hline 
> x_{B}&  B^{-1}A  \\
> \hline
> \end{array} = 
> 
> \begin{array}{|c c | ll |}
> \hline
> & -9M  & 1-M & 1-6M  & 1-15M& -M & 0 & 0 & 0\\
> \hline
> x_{5} =  & 3 & \color{red}{1^{*}}& 2 & 3 & 0 & 1 & 0 & 0\\
> x_{6} =   & 5 & 0 & 4 & 9 & 0 & 0 & 1 & 0\\
> x_{7} =   & 1 & 0 & 0 & 3 & 1 & 0 & 0 & 1\\
> \hline
> \end{array}
> $$
> for a big value of $M$, the first column has a negative value: $1$ enters the basis and $5$ exits:
> $$
> \begin{array}{|c c | ll |}
> \hline
> & -6M-3  & 0 & -1-4M  & -2-12M& -M & M-1 & 0 & 0\\
> \hline
> x_{1} =  & 3 & 1 & 2 & 3 & 0 & 1 & 0 & 0\\
> x_{6} =   & 5 & 0 & 4 & 9 & 0 & 0 & 1 & 0\\
> x_{7} =   & 1 & 0 & 0 & 3 & \color{red}{1^{*}} & 0 & 0 & 1\\
> \hline
> \end{array}
> $$
> similarly, $4$ enters, and $7$ exits:
> $$
> \begin{array}{|c c | ll |}
> \hline
> & -5M-3  & 0 & -1-4M  & -2-9M& 0 & M-1 & 0 & M\\
> \hline
> x_{1} =  & 3 & 1 & 2 & 3 & 0 & 1 & 0 & 0\\
> x_{6} =   & 5 & 0 & \color{red}{4^{*}} & 9 & 0 & 0 & 1 & 0\\
> x_{4} =   & 1 & 0 & 0 & 3 & 1 & 0 & 0 & 1\\
> \hline
> \end{array}
> $$
> Finally, $2$ enters and $6$ exits:
> $$
> \begin{array}{|c c | ll |}
> \hline
> & -7 / 4  & 0 & 0  & 1 / 4 & 0 & M-1 & M+ 1 / 4 & M\\
> \hline
> x_{1} =  & 1 / 2 & 1 & 0 & -3 / 2 & 0 & 1 & -1 / 2 & 0\\
> x_{2} =   & 5 / 4 & 0 & 1 & 9 / 4 & 0 & 0 & 1 / 4 & 0\\
> x_{4} =   & 1 & 0 & 0 & 3 & 1 & 0 & 0 & 1\\
> \hline
> \end{array}
> $$
> We were able to drive out all the artificial variables out of the basis, so this is our first basic feasible solution to the original problem. Moreover, since all the reduced costs are nonnegative this basis is optimal, so we have found the optimal solution $x = \left( \frac{1}{2}, \frac{5}{4}, 0, 1 \right)$.

> [!theorem]
> For a given [[LPP In Standard Form#^d3d2e7|LPP in standard form]] consider the big-$M$ formulation where $M\gg 0$ is an arbitrarily (nonfixed) large quantity along the execution of the simplex method. Then the following hold:
> 1) If the simplex method terminates with a solution $(x,y) = (x,0)$ then $x$ is an optimal solution to the original problem;
> 2) If the simplex method terminates with a solution $(x,y) \neq (x,0)$ then the original problem is infeasible;
> 3) If the simplex method terminates with the value $-\infty$ then the original problem is either infeasible or its optimal cost is $-\infty$

> [!proof]-
> ***($1$)*** It is trivially proven that $Ax = b$ and $x \geq 0$, so $x$ is a feasible solution of the original problem. For any other feasible solution $\bar{x}$ of the original problem, we have that $(\bar{x},0)$ is feasible as well, and due to the optimality of $(x,0)$, it must hold that $c^{T}x = c^{T}x + M \cdot 0 \leq c^{T}\bar{x} + M \cdot 0 = c^{T}\bar{x}$,  hence $x$ is optimal for the original problem.
>
>***($2$)*** By contradiction, if the original problem had a nonempty feasible region, [[LPP Fundamental Theorem Of Linear Programming#^c85132|then]] there would exist a feasible solution $\bar{x}$. $(\bar{x},0)$ would be feasible for the big-$M$ formulation as well, and it would have cost $c^{T}\bar{x} \ll c^{T}x + M\sum_{i}y_{i}$ for a large enough value of $M$. Since the algorithm execution would remain the same if we used this fixed large value to begin with, by the optimality of the final solution of the simplex method we would also have $c^{T}x +M\sum_{i}y_{i} \leq c^{T}\bar{x}$, which is absurd, hence the original problem is infeasible.
>
> ***(3)*** Let us assume that the simplex method terminates with an unbounded value $-\infty$.  [[LPP The Simplex Method Framework#^f73df0|By the specific termination criteria of the simplex method]], this means that we have found a feasible direction $d = (d_{x}, d_{y}) \geq 0$ with a  negative [[LPP Simplex Method Optimality Conditions#^3cc9c2|reduced cost]]  $\bar{c} = c^{T}d_{x} + \sum_{i}Md_{y_{i}}<0$. Since $M$ is arbitrarily large when performing comparisons in the algorithm execution, it must hold that $d_{y} = 0$, otherwise $\bar{c}\gg 0$. In particular, it holds that $Ad_{x} = 0$ and $d_{x} \geq 0$ with $c^{T}d_{x} <0$. If the original problem has a feasible solution $\bar{x}$, then $\bar{x}+\theta d_{x}$ remains feasible for all $\theta >0$ and its cost strictly decreases to $-\infty$ as $\theta \to +\infty$, so the original problem is unbounded. Otherwise the original problem is infeasible.
`\end{proof}`