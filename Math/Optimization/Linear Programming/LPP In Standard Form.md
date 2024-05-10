---
tags:
  - math/optimization/linear-programming
  - math/definition
next: "[[Notable Classes Of Nonlinear LPP]]"
prev: "[[LPP Definition]]"
up: "[[Linear Programming Master Note]]"
abstract: definition of the standard form for linear programs
---
# The Standard Form
## Motivations
We can convert all linear programming problems into an equivalent structure referred to as the standard form.
This standard form proves to be highly beneficial in crafting the majority of theoretical frameworks and numerical methodologies employed to address linear programming problems.
## Transforming Linear Programs Into Standard Form
Consider a linear program in [[LPP Definition#^linear-program-general-form|general form]]:

![[LPP Definition#^linear-program-general-form]]

This problem can be transformed into an equivalent problem in **standard form**:

$$
\begin{align}
\min_{x \in \mathbb{R}^n}  \quad  & c^T x \\
\text{s.t.}  \quad & Ax = b \\
& x \geq 0
\end{align}
$$
^linear-program-standard-form

Indeed, we just need to perform the *following two steps*: ^979f31
- *Elimination of free variables*:  given a [[LPP Definition#^fe000b|free variable]] $x_j$ we replace it by introducing two new variables $x_j^+$ and $x_j^-$ such that:   $$\begin{align}
& x_{j} = x_{j}^+ - x_{j}^- \\
& x_j^+, x_j^- \geq 0
\end{align}
$$
- *Elimination of inequality constraints*: Given an inequality constraint $a_i^T x \geq b_i$ we can introduce a **slack variable** $s_i$ and rewrite the constraint as:
  $$
  \begin{align}
  a^T x - s_{i} & = b_{i} \\
s_{i} & \geq 0
\end{align}
   $$

> [!definition] Standard Form
> A linear programming problem of the form :
> ![[#^linear-program-standard-form]]
> is said to be in **standard form**

^d3d2e7

> [!proposition]
> Every linear program can be rewritten into an equivalent linear program in standard form

^1d57d7

> [!proof]-
> [[LPP Definition#^6ccfd9|Without loss of generalization]]  we can assume the linear program to be in general form. Let us consider the problem in standard form constructed using the [[#^979f31|previous derivation]].
>  It suffices to show that every feasible solution of a linear program in general form can be transformed into a feasible solution of the same cost in standard form, and, viceversa, that every feasible solution of the standard form can be transformed into a solution with the same cost in general form.
>  
> ($\Rightarrow$) Given a feasible solution $x$ we can create a new solution $\hat{x}$ for the standard form by setting the free variables $\hat{x}_j^+$ and $\hat{x}_j^-$ as the positive and negative part of $x_j$, while leaving the other nonfree variables the same. Then it's easy to see that $c^T x = \hat{c}^T \hat{x}$ and that $\hat{x}$ is a feasible solution
> 
> ($\Leftarrow$) Similarly, given a feasible solution $\hat{x}$ in standard form we can create a new solution $x$ by setting $x_j = \hat{x}_{j}^+ - \hat{x}_{j}^-$ for the free variables and $x_j = \hat{x}_{j}$ for all the other nonfree variables. It can be trivially checked that the cost of $x$ is the same and that constraints are respected.
> `\end{proof}`

## Interpretation of Problems In Standard Form

^82f69c

Let $A_1,...,A_n$ be the columns of $A$. The constraint $Ax = b$ can be rewritten as:

$$
\sum_{i}^n x_{i }A_{i}= b
$$

In other words, we are trying to synthesize the target vector $b$ by using a non-negative amount of $x_i$ of each resource vector $A_i$ while minimizing the cost $\sum_{i}^n c_{i} x_{i}$

> [!example] The diet problem
> Suppose we want to create a diet using $n$ different foods trying to reach for $m$ different nutrients an exact target amount $b_i$, while minimizing the quantity of calories. 
> Each food $j = 1,...,n$ has a specific amount of nutrients $A_{j} =(a_{1,j},\dots, a_{m,j})$ and calories $c_j$ per unit. 
> 
> | Nutrients\ Foods | Food 1    | $\dots$  | Food $n$  |
> | ---------------- | --------- | -------- | --------- |
> | nutrient 1       | $a_{1,1}$ | $\dots$  | $a_{1,n}$ |
> | $\vdots$         | $\vdots$  | $\ddots$ | $\vdots$  |
> | nutrient $m$     | $a_{m,1}$ | $\dots$  | $a_{m,n}$ |
> 
> Finding an optimal diet is equivalent to solving a linear program in standard form:
> ![[#^linear-program-standard-form]]
> If instead of reaching an exact amount of nutrients we were trying to reach a minimum amount, then the constraint $Ax = b$ would be rewritten as $Ax \geq b$ and the problem would be in general form.