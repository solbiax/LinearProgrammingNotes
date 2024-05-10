---
tags:
  - math/optimization/linear-programming
next: "[[LPP Simplex Method Finding An Initial Solution]]"
prev: "[[LPP Simplex Method Implementations]]"
up: "[[Linear Programming Master Note]]"
---
# Anticycling Rules
We have seen that the [[LPP The Simplex Method Framework#^f73df0|simplex method]] always [[LPP The Simplex Method Framework#^a3d33b|terminates]] if there are no [[LPP Degeneracy#^bd5d3e|degenerate]] basic solutions. Otherwise, there are cases where the simplex method may cycle forever on the same basic solutions.
In this note, we discuss *anticycling pivoting rules* under which the simplex method is guaranteed to terminate.
In the case of degenerate solutions, pivoting rules have the crucial role of deciding which variable exits the basis, so to avoid cycling.

## Lexicographic Pivoting Rule
### Definition
We present the lexicographic pivoting rule. Historically, this rule was derived by analyzing the behaviour of the simplex method after doing a small perturbation of the vector $b$, in order to transform the original degenerate problem into a nondegenerate one ([[LPP Degeneracy#^3a2828|degeneracy is nonexistent for random problems]]).

> [!definition] Lexicographic Order
> A vector $u \in \mathbb{R}^{n}$ is said to be **lexicographically larger** (or **smaller**) than another vector $v \in \mathbb{R}^{n}$, if $u \neq v$ and the first nonzero component of $u-v$ is positive (or negative, respectively). Symbolically, we write:
> $$
> u \overset{L}{>}v \quad \text{or} \quad u \overset{L}{<}v
> $$

> [!example]-
> For example:
> $$
> (0, 2, 3, 0) \overset{L}{>} (0,2,1,4)
> $$

Normally, to choose the variable that exits the basis, we compute for all positive $u_{i}$ the $i$th ratio $x_{B_{i}} / u_{i}$ and pick the $i$th row with the smallest ratio. When some ratios coincide, (for example when the basic solution is degenerate, and we have multiple zero values), the lexicographical pivoting rule breaks the tie by proceeding with the same criteria for the next column, until it breaks the tie. Effectively, this means picking the smallest row (divided by $u_{i}$) according to the lexicographical order.

> [!definition] Lexicographic Pivoting Rule
> 1) Choose an enterin column $A_{j}$ arbitrarly as long as $\bar{c}_{j}<0$. Let $u = B^{-1}A_{j}$ be the $j$th column of the [[LPP Simplex Method Implementations#The Full Tableau Implementation|tableau]];
> 2) For each $i$ wit $u_{i} > 0$, divide the $i$th row of the tableau (including the zeroth column) by $u_{i}$ and choose the lexicographically smallest row $\ell$. The $\ell$th basic variable $x_{B_{\ell}}$ exits the basis.

> [!example]-
>  Consider the following tableau where we omit the zeroth row:
> $$
> \begin{array}{|c | c c c c|}
> \hline
> 1 & 0 & 5 & 3 & \dots \\
> 2 & 4 & 6 & -1 & \dots\\
> 3 & 0 & 7 & 9 & \dots \\
> \hline
> \end{array}
> $$
> Suppose that the pivot column is the third one. There is a tie in trying to determine the exiting variable because $x_{B_{1}} / u_{1} = 1 / 3 = 3 / 9 = x_{B_{3}} / u_{3}$. We divide the first and third rows of the tableau by $u_{1}$ and $u_{3}$ respectively:
> 
> $$
> \begin{array}{|c | c c c c|}
> \hline
> 1 / 3 & 0 & 5 / 3 & 1 & \dots \\
> * & * & * & * & \dots\\
> 1 / 3 & 0 & 7 / 9 & 1 & \dots \\
> \hline
> \end{array}
> $$
> Since $7 / 9 < 5 / 3$, the third row is lexicographically the smallest, and is therefore chosen as a pivot row (in other words, $3$ exits the basis).

The lexicographic pivoting rule always leads to a unique choice for the exiting variable. *Indeed*, If that wasn't the case, two of the rows in the tableau would be proportional, hence linearly dependent. But this is absurd, because this would mean that $m = \text{rank}_{row}(A) = \text{rank}_{row}(B^{-1}A) < m$.
### The Lexicographic Rule Is An Anticycling Rule
The following theorem states that, under the assumption that the nonzeroth rows are lexicographically positive, the lexicographical rule is an anticycling rule. Note that since $x_{B} \geq 0$ and $B^{-1}A_{B_{i}} = e_{i}$, the lexicographic positivity can be achieved by simply renaming the variables so that the basic variables are the first $m$ ones.

> [!theorem]
> Suppose that the simplex algorithm starts with all the rows in the simplex tableau, other than the zeroth row, lexicographically positive. Suppose that the lexicographic pivoting rule is followed. Then:
> 1) Every row of the simplex tableau, other than the zeroth row, remains lexicographically positive;
> 2) The zeroth row strictly increases lexicographically at each iteration;
> 3) The simplex method terminates after a finite number of iterations.

> [!proof]-
> ***($1$)*** Suppose that $x_{j}$ enters the basis and that the pivot row is the $\ell$th row. According to the lexicographic pivoting rule, we have $u_{\ell}>0$. In particular, for each $u_{i} > 0$ with $i \neq \ell$ it holds that:
> $$
> \frac{(\ell \text{th row})}{u_{\ell}} \overset{L}{<} \frac{(i\text{th row})}{u_{i}}
> \implies (\text{updated} \; i\text{th row}) = 
> (i\text{th row}) - \frac{u_{i}}{u_{\ell}}(\ell \text{th row}) \overset{L}{>}0
> $$
> So, these $i$th rows remain lexicographically positive after the tableau update. For $i = \ell$ the tableau update is simply computed by diving the $\ell$th row by $u_{\ell}>0$, so that row remains positive as well.
> Finally for $u_{i}\leq0$ in order to set $u_{i}$ to zero we need to add a positive multiple of the pivot row, so those rows remain lexicographically positive.
> 
> ***($2$)*** At the beginning of an iteration, the reduced cost in the pivot column is negative. In order to make it zero, we need to add a postive multiple of the pivot row. Since the pivot row is lexicographically positive, the zeroth row increases lexicographically.
> 
> ***($3$)*** Since the zeroth row increases lexicographically at each iteration, we cannot have the same values twice for that row. Since the zeroth row is determined completely by the current basis, this means that no basis is ever revisited, and since the number of basis is finite, the simplex method must terminate.
> `\end{proof}`

### Some implementation Notes
The lexicographic rule is straightforward in terms of the full tableau. It can also be used with the revised simplex method, as long as $B^{-1}$ is explicitly stored. In sophisticated implementations of the revised simplex method, $B^{-1}$ is never computed explicitly, so the lexicographic rule is not suitable.

## Bland's Rule
The **smallest subscript pivoting rule**, also known as **Bland's rule**, is as follow:

> [!definition] Bland's Rule
> 1) Find the smallest $j$ for which the reduced cost $\bar{c}_{j}$ is negative and have the column $A_{j}$ enter the basis;
> 2) Out of all variables $x_{i}$ that are tied in the test for choosing an exiting variable, pick the one with the smallest value of $i$

Under this pivoting rule it is known that cycling never occurs and the simplex method is guaranteed to terminate after a finite number of iterations.
Bland's rule is compatible with an implementation of the revised simplex method in which the reduced costs of the nonbasic variables are computed one at a time.