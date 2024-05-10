---
tags:
  - math/theorem
  - math/geometry
  - math/linear-algebra
---

> [!theorem] Rouché-Capelli Theorem
> A system of $m$ linear equations in $n$ variables with coefficients in a field $\mathbb{K}$ in the form:
>  $$\begin{eqnarray}
> & a_{1,1}x_{1}  + & \dots + a_{1,n} x_{n} = b_{1} \\
> & & \dots \\
> & a_{m,1}x_{1} + & \dots + a_{m,n} x_{n} = b_{m} \\
\end{eqnarray}$$
has a solution if and only if its coefficient matrix $A$ and its augmented matrix $[A|b]$ have the same rank.
> Given any solution $x \in \mathbb{K}^{n}$ to the linear system, all other solutions are given by $x + \ker(A)$,  which means they form an affine subspace of dimension $n- \text{rank}(A)$.
> In particular If $\text{rank}(A) = n$ the linear system always has a unique solution.

> [!proof]+
> Let $A_{i}$ be the columns of the system's coefficient matrix $A$. Solving a linear system is equivalent to finding a vector $x \in \mathbb{K}^{n}$ such that $Ax = \sum_{i}^{n} x_{i} A_{i} = b$. This means that a solution exists if and only if the vectors $A_{1},\dots, A_{n}, b$ are linearly dependent. In particular, since $b$ can be written as a linear combination of the vectors $A_{i}$ it can be taken out of the generator set and the span would remain the same, hence there is a solution iff $\text{rank}(A) =\text{rank}([A | b])$.
> Let's assume now that $x$ is a solution of the linear system. Then, for any $w \in \ker (A)$ we have $A(x+w) = Ax + Aw = b + 0 = b$ , which means that $x + w$ is also a solution. Viceversa, let $y \in \mathbb{K}^{n}$ be a solution, then $A(y-x) = Ay - Ax = b-b=0$, hence $(y-x) \in \ker (A)$, so $y = x + (y-x) \in x+\ker (A)$.
> `\end{proof}`  