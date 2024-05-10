---
tags:
  - math/optimization/linear-programming
next: "[[LPP Simplex Method Implementations]]"
prev: "[[LPP Simplex Method Optimality Conditions]]"
up: "[[Linear Programming Master Note]]"
---
# The Simplex Method Framework
We develop in this note the general theoretical framework behind every practical implementation of the simplex method. As [[LPP Simplex Method Optimality Conditions|previously discussed]], the simplex method works by moving along the edges of the feasible set, going from a [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic feasible solution]] to the next one, always trying to reduce the cost function.

We shall consider the following [[LPP In Standard Form#^d3d2e7|linear programming problem in standard form]]:
![[LPP In Standard Form#^linear-program-standard-form]]
where $A \in \mathbb{R}^{m \times n}$ has, [[LPP Polyhedra In Standard Form#^0b2975|without loss of generality]], full row rank $m$. ==We shall also assume that every [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic solution]] is [[LPP Degeneracy#^bd5d3e|nondegenerate]]==. This assumption will be relaxed later. 
## Development Of The Simplex Method
Suppose that we are at a basic feasible solution $x$, and that we have computed the [[LPP Simplex Method Optimality Conditions#^3cc9c2|reduced costs]] $\bar{c}_{j}$ of the nonbasic variables. If the [[LPP Simplex Method Optimality Conditions#^b193cd|basis is optimal]], i.e. if $\bar{c}_{j} \geq 0$, then $x$ is optimal and we stop.
Otherwise there exist a nonbasic variable $j$ for which $c_{j}<0$. Let $d$ be the $j$th [[LPP Simplex Method Optimality Conditions#^fbcf42|basic direction]]. Since $x$ is nondegenerate, [[LPP Simplex Method Optimality Conditions#^eb4930|then]]  $d$ is a [[LPP Simplex Method Optimality Conditions#^24a7e6|feasible direction]] of cost decrease. 
While moving along this direction $d$, $x_{j}$ becomes positive and all other nonbasic variables remain at zero. We describe this situation by saying that $x_{j}$ **enters** or is **brought into the basis**.
Moving along $d$ from $x$ means considering points of the form $x+\theta d$, where $\theta > 0$. Since costs decrease along $d$, it is desirable to move as far as possible, i.e. to choose $\theta$ as big as possible while maintaining feasibility. We denote such optimal $\theta$ with $\theta^{*}$:

$$
\theta^{*} := \max \{ \theta \geq 0 \; | \; x + \theta d \in P \}
$$
The resulting cost change is $\theta^{*}c^{T}d = \theta^{*} \bar{c}_{j}$.

We now derive a formula for $\theta^{*}$. Since $Ad = 0$, we observe that $x + \theta d$ can become infeasible only if one of its components becomes negative. There are two cases:

*(case 1)* If $d \geq 0$ then $x + \theta d \geq 0$ for all $\theta \geq 0$, so $x+\theta d$ is always feasible and $\theta^{*} = + \infty$, meaning that the problem is unbounded;

*(case 2)* If $d_{i} < 0$ for some $i$, then the constraint $x_{i}+ \theta d_{i} \geq 0$ is equivalent to $\theta \leq - x_{i} / d_{i}$. This must be satisfied by every $i$ with $d_{i} < 0$, so the optimal $\theta^{*}$ is given by:

$$
\theta^{*} = \min_{\{ i | \; d_{i}<0 \}} \left(- \frac{x_{i}}{d_{i}} \right)
$$
Recall that every nonbasic variable, except $j$, has $d_{i} = 0$, so the serach for $\theta^{*}$ can be restricted only to the basic variables:

$$
\theta^{*} = \min_{d_{B_{i}} < 0} \left( - \frac{x_{B_{i}}}{d_{B_{i}}}\right)
$$

^698770

Since $x$ is nondegenerate $x_{B} > 0$, so we must have $\theta^{*} > 0$.

Assume we're in case 2. Once we have found $\theta^{*}$, we move to the new feasible solution $y = x + \theta^{*} d$. We have $y_{j} = \theta^{*} > 0$. Let $\ell$ be a minimizing index for equation [[#^698770]], i.e.:

$$
\theta^{*} = - \frac{x_{B_{\ell}}}{d_{B_{\ell}}}
$$

^dfa58d

Then we must have: 
$$
y_{\ell} = x_{B_{\ell}}+ \theta^{*}d_{B_{\ell}} = 0
$$
In particular, $x_{B_{\ell}}$ has become zero, or, in other words, **came out of the basis** and became a basic variable; whereas  $x_{j}$ became positive and replaced $x_{B_{\ell}}$ in the basis. Accordingly, we update the old basis matrix $B$ by replacing the column $A_{B_{\ell}}$ with the column$A_{j}$:
$$
\bar{B} = \begin{pmatrix}
| & & | & | & | & & | \\
A_{B_{1}} & \dots & A_{B_{\ell-1}} & A_{j} & A_{B_{\ell + 1}} & \dots & A_{B_{m}} \\
| & & | & | & | & & |
\end{pmatrix}
$$
and equivalently, we replace the basis indices $\{ B_{1},\dots,B_{m} \}$ with $\{ \bar{B}_{1},\dots,\bar{B}_{m} \}$ where:
$$
\bar{B}_{i} = \begin{cases}
B_{i} \quad & \text{for} \; i \neq \ell \\
j \quad & \text{for} \; i = \ell
\end{cases}
$$
> [!theorem]
> 1) The columns $\{  A_{B_{i}}\; | \; i \neq \ell \}$ and $A_{j}$ are linearly independent, therefore $\bar{B}$ is a basis matrix;
> 2) The vector $y = x + \theta^{*}d$ is a basic feasible solution associated with the basis matrix $\bar{B}$.

^d7531f

> [!proof]-
> ***(1)*** By contradiction, if they were linearly dependent then we could write $A_{j}$ as a linear combination of the other $A_{B_{i}}$:
> $$
> A_{j} = \sum_{i \neq \ell} \lambda_{i}A_{\bar{B_{i}}} \implies d_{B} = B^{-1}A_{j} = \sum_{i \neq \ell} \lambda_{i} B^{-1}A_{\bar{B}_{i}} = \sum_{i \neq \ell} \lambda_{i} e_{i}
> $$
> But by [[#^dfa58d|construction]] of $\ell$ and $\theta^{*}$ $d_{B_{\ell}} \neq 0$ , so $d_{B}$ cannot be a linear combination of $e_{i}$ with $i \neq \ell$ , absurd.
>
> ***(2)*** We have $y \geq 0$ and $Ay = b$ so $y$ is a feasible solution. For $i \neq \bar{B}_{k}$ we also have $y_{i} = 0$, and the columns $A_{\bar{B}_{i}}$ are linearly independent. From the [[LPP Polyhedra In Standard Form#^bc0820| characterization of basic solution]] it follows that $y$ is a basic feasible solution with basis matrix $\bar{B}$.
> `\end{proof}`

The new basic feasible solution $x + \theta^{*}d$ is distinct from $x$ and its cost is strictly smaller. 
## Iteration Of The Simplex Method

^0dd161

We can summarize a typical iteration of the simplex method, also known as a **pivot**.  It is convenient to define a vector $u = (u_{1},\dots,u_{m})$ by letting:
$$
u := -d_{B} = B^{-1}A_{j}
$$
where $A_{j}$ is the column that enters the basis.

> [!algorithm] An Iteration Of The Simplex Method
> 1) We start from a [[LPP Polyhedra In Standard Form#^12f1aa|basis]] $B_{1},\dots,B_{m}$ associated to a basic feasible solution $x$
> 2) We compute the [[LPP Simplex Method Optimality Conditions#^3cc9c2|reduced costs]] $\bar{c}_{j} = c_{j}-c^{T}_{B}B^{-1}A_{j}$. If $\bar{c} \geq 0$, then the basis is optimal and we stop, otherwise we choose an index $j$ for which $\bar{c}_{j}<0$ to enter the basis.
> 3) We compute $u = B^{-1}A_{j}$ . If $u \leq 0$ , then $\theta^{*} = +\infty$  and the optimal cost is $-\infty$ and we terminate the algorithm. Otherwise we move to step 4
> 4) We compute the quantity:
>    $$
>  \theta^{*} = \min_{u_{i}>0} \frac{x_{B_{i}}}{u_{i}} = \frac{x_{B_{\ell}}}{u_{\ell}}
>   $$
> 5) We form a new basis by replacing $A_{B_{\ell}}$ with $A_{j}$. The new basic feasible solution $y$ will have $y_j = \theta^{*}$ and $y_{B_{i}} = x_{B_{i}} - \theta^{*}u_{i}$ for all $i \neq \ell$. The remaining components are all zero. 
> 

^f73df0

The simplex method is initialized with an arbitrary basic feasible solution, which, for feasible standard form problems, is guaranteed to exist. The following theorem states that, in the nondegenerate case, the simplex method will terminate after a finite number of iterations.

> [!theorem] The Simplex Method Terminates
> For a [[LPP In Standard Form#^d3d2e7|LPP in standard form]], assume that the feasible region $P$ is nonempty and that every basic feasible solution is [[LPP Degeneracy#^ee501b|nondegenerate]]. Then, the simplex method terminates after a finite number of iterations with the following two possibilities:
> 1) We have an optimal basis $B$ and an associated basic feasible solution which is optimal;
> 2) We have found a vector $d$ satifsying $Ad = 0$, $d \geq 0$ such that $c^{T}d < 0$, and the optimal cost is $-\infty$.

^a3d33b

> [!proof]-
> If the algorithm terminates due to the stopping criterion in point 2, then the [[LPP Simplex Method Optimality Conditions#^35d8cb|optimality conditions]] have been met, and we are in case 1 of the theorem. 
> If the algorithm terminates because of step 3, then we have discovered a basic direction $d$ for which $x+\theta d \in P$ for all $\theta>0$. Since $c^{T}d = \bar{c}_{j} < 0$,  for $\theta \to +\infty$ we have that the cost goes to $-\infty$, so the problem is unbounded (case 2 of the theorem). 
> To conclude the proof we just need to prove that the algorithm terminates. At each iteration the algorithm moves toward a different basic feasible solution with strictly less cost than the previous one, so no basic feasible solution can be visited twice. Since [[LPP Polyhedra And Their Vertices#^56c105|there is a finite number of basic feasible solutions]], the algorithm must eventually terminate.
> `\end{proof}`

Note that this theorem provides an independent proof of the [[LPP Fundamental Theorem Of Linear Programming#^537f75|fundamental theorem of linear programming]] for nondegenerate standard form problems. While the proof given here might appear more elementary, its extension to the degenerate case is not as simple.
## Relaxing The Nondegeneracy Assumption
Suppose now that the simplex method is used in the presence of degeneracy. Then, the following new possibilities may be encountered:
1) If $x$ is degenerate, $\theta^{*}$ could be zero, in which case it's impossible to move along the edge and the new solution $y$ is identical to $x$. This happens if some basic variable $x_{B_{\ell}}$ is equal to zero and the corresponding component $d_{B_{\ell}}$ is negative. We can still define a new basis by replacing $A_{B_{\ell}}$, and [[#^d7531f]] is still valid for this new basis;
2) Even if $\theta^{*}$ is positive, it may happen that more than of the original basic variables becomes zero at the new point $x + \theta^{*}d$. Since only of them exits the basis, the new basic feasible solution is degenerate, even if $x$ was not.

The basis changes of case 1 are not necessarily in vain. A sequence of such basis changes may lead to the eventual discovery of a cost reducing feasible direction. On the other hand, since the cost is not strictly decreasing anymore, we could be lead back to a previous basis, in which case the algorithm may loop indefinitely. This phenomenon is called **cycling**.
By judiciously choosing the variables that enter or exit the basis it is possible to avoid cycling. Those pivoting rules are known as **anticycling rules**. Under them,  we can extend [[#^a3d33b]] to the degenerate case.

> [!example]- Basis Changes On The Same Basic Solution
> ![[lpp_degenerate_base_change.excalidraw|500]]
> We visualize a problem in standard form with $n-m = 2$ by standing on the plane defined by $Ax = b$.
> In this example, $x$ is degenerate. If $x_{4}$ and $x_{5}$ are chosen as nonbasic variables (and so the basis is given by $x_{1},x_{2},x_{3},x_{6}$), then the two corresponding basic directions are  the vectors $g$ and $f$. For either of these two, we have $\theta^{*} = 0$.
> However, if we perform a change of basis, with $x_{4}$ entering and $x_{6}$ exiting, the new nonbasic variables are $x_{5}$ and $x_{6}$, and the two basic directions are $h$ and $-g$. In particular, we can now follow direction $h$ to reach a new basic feasible solution $y$ with lower cost.

## Pivot Selection
The [[#^f73df0|simplex algorithm]]  we described before has certain degrees of freedom:
1) In step 2, we are free tho choose any $j$ whose reduced cost $\bar{c}_{j}$ is negative;
2) In step 5, there may be multiple indices $\ell$ that attain the minimum in the definition of $\theta^{*}$ (in that case, the new solution is degenerate), but we are free to choose any of them to exit the basis.

Rules for making such choices are called **pivoting rules**.

*Regarding the choice of the entering column*, the following rules are some natural candidates:
1) Choose a column $A_{j}$ with $\bar{c}_{j}<0$ whose reduced cost is the most negative. This  rule chooses the direction that has the fastest rate of change. This choice does not necessarily lead to the biggest cost decrease, since that depends also on how far we move along the chosen direction;
2) Choose $j$ so that the corresponding cost decrease $\theta^{*}|\bar{c}_{j}|$ is largest. This rule offers the possibility of reaching optimality after a smaller number of iterations, but its computational burden at each iteration is larger. Empirical evidence suggests that overall the running time does not improve.

For large problems, even the rule that chooses the most negative $\bar{c}_{j}$ can be computationally expensive. In practice simpler rules are sometimes used, such as the **smallest subscript rule**, that chooses the smallest $j$ for which $\bar{c}_{j}$ is negative.
Other criteria that improve the overall running time are the **Devex**, and the **steepest edge rule**. Finally, there are methods based on **candidate lists** whereby one examines the reduced costs of nonbasic variables by picking them from a prioritized list. There are many ways of maintaining such prioritized lists.

*Regarding the choice of exiting column*, the simplest option is the smallest subscript rule. It turns out that by following the smallest subscript rule for both the entering and the exiting column, cycling can be avoided.