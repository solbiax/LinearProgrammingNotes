---
tags:
  - math/optimization/linear-programming
  - math/modelling
next: "[[LPP Examples]]"
prev: "[[LPP In Standard Form]]"
up: "[[Linear Programming Master Note]]"
abstract: modelling techniques to transform some nonlinear problems into linear programs
---

# Linear Programs With Piecewise Linear Convex Objective functions
There is an important class of optimization problems with a nonlinear objective function that can be cast as linear programming problems.
This class of problems can be used to approximate any optimization problem with a convex objective function and linear constraints.
## Definition

Let $c_1,\dots, c_{m} \in \mathbb{R}^n$ be vectors and $d_1, \dots, d_{m} \in \mathbb{R}$ be scalars. Consider the function:

$$
f(x) := \max_{i=1,\dots,m} c_{i}^T x + d_{i}
$$

^2720a9

Such a function is convex since it's the maximum of convex functions (in particular,  affine functions). A function in the form of $f$ is called a **piecewise linear convex function**. A notable example of piecewise linear convex function is $f(x) = |x| = \max \{ x,-x\}$

> [!definition] Piecewise Linear Convex Functions
> A function $f : \mathbb{R}^n \to \mathbb{R}$ is said to be a **piecewise linear convex function** if it can be written as:
> ![[#^2720a9]] ^jz7eJmiZ

Piecewise linear convex function can be used to approximate any convex objective function:

![[piecewise-linear-approximation.excalidraw|700]]

## Linear Programs Using Piecewise Linear Convex Functions
Consider $f(x)$ defined as in [[#^2720a9]]. Note that $\max_{i} (c_{i}^T + d_{i})$ is equal to the smallest number $z$ that satisfies $z \geq c_{i}^T x + d_{i}$. For this reason, the optimization problem:

$$
\begin{align}
\min_{x} \quad & f(x) \\
\text{s.t.}\quad & Ax \geq b
\end{align}
$$
is equivalent to the following linear programming problem:

$$
\begin{align}
\min_{x,z} \quad & z \\
\text{s.t.} \quad & z\geq c_{i}^T x + d_{i} \\
& Ax \geq b
\end{align}
$$
Similarly, if we are given a constraint of the form $f(x) \leq b_{i}$ such a constraint can be rewritten as:
$$
c_{i}^Tx + d_{i} \leq b_{i} \quad i = 1,\dots,m
$$

# Problems Involving Absolute Values
Let $c_i \geq 0$ and consider a problem of the form:

$$
\begin{align}
\min_{x} \quad & \sum_{i}^n c_{i}|x_{i}| \\
\text{s.t.} \quad & Ax \geq b \\
\end{align}
$$

^5e9e35

It's easy to see that the cost function is a piecewise linear convex function, however writing it as such is a bit involved, and a more direct route is preferable.
We notice that $|x_{i}|$ is the smallest number $z_i$ that satisfies $z_{i} \geq x_{i}$ and $z_i \geq -x_i$, hence we can reformulate the problem into a linear program:

$$
\begin{align}
\min_{x,z} \quad & \sum_{i}^n c_{i} z_{i} \\
\text{s.t.} \quad & Ax \geq b \\
& z_{i} \geq x_{i} \quad & i = 1,\dots,n\\
& z_{i} \geq -x_{i} \quad & i = 1,\dots,n\\
\end{align}
$$

We remark that $c_i$ *being nonnegative is an essential assumption for this reformulation*.

An alternative method would consist in decomposing $x_{i}$ into its negative and positive parts $x_i^+, x_i^-$:

$$
\begin{align}
\min_{x^+, x^-} \quad & \sum_{i}^n c_{i}(x_{i}^+ + x_{i}^-) \\
\text{s.t.} \quad & Ax^+ - Ax^- \geq b \\
& x^+, x^- \geq 0
\end{align}
$$

^2bf3d9

The relations $x_i = x_i^+ - x_i^+$ and $x_i^+, x_i^- \geq 0$  *are not enough to guarantee* that $|x_i| = x_i^+ + x_i^-$ (for example $x := -3 = 1-4$ but $|x| \neq 1+4$), so the validity of the formulation is not entirely obvious.

> [!proposition]
> Problem [[#^5e9e35]] is equivalent to problem [[#^2bf3d9]]

>[!proof]-
> ($\tilde{\Rightarrow}$) Let $x$ be a feasible solution of problem [[#^5e9e35]], then by defining $x^+,x^-$ as the positive and negative parts of $x$ it easily follows that $(x^+,x^-)$ is also a feasible solution of problem [[#^2bf3d9]] with the same cost.
> 
> ($\tilde{\Leftarrow}$) Let $(x^+,x^-)$ be a feasible solution of problem [[#^2bf3d9]] and let $x := x^+ - x^-$. It's easy to see that $x$ is also a feasible solution for problem [[#^5e9e35]].
> To conclude the proof we just need to show that the cost of $x$ is smaller than the cost of  $(x^+,x^-)$. Indeed, in that case we would conclude, using the first part of the proof, that both problems have the same optimum which can be "translated" from one problem to the other .
> For every $i$ we have that :
> $$|x_{i}| = |x_{i}^+ - x_{i}^-| \leq |x_{i}^+| + |x_{i}^-| = x_{i}^+ + x_{i}^- \; \Rightarrow c_{i}|x_{i}| \leq c_{i}(x_{i}^+ + x_{i}^-)$$
> therefore:
> $$\sum_{i}^n c_{i}|x_{i}| \leq \sum_{i}^n c_{i}(x_{i}^+ + x_{i}^-)$$
> which concludes the proof.
> `\end{proof}`

> [!example]- Data Fitting: $L_{1}$ Linear Regression
> We are given $m$ data points $(a_{i},b_{i})$ where $a_{i} \in \mathbb{R}^{n}$ and $b_{i} \in \mathbb{R}$. We want to build a linear model that predicts the value of $b$ given a vector $a$. Every linear model can be parametrized by a vector $x \in \mathbb{R}^{n}$  as:
> $$
> f_{x}(a) = a^{T}x
> $$
> A good linear model would ideally interpolate the data points $(a_{i},b_{i})$ as much as possible. Mathematically, this means reducing the quantities $|b_{i} - a^{T}x|$, also known as *residuals* or *prediction errors* .
> There are many ways one could define the problem of "reducing the prediction errors". One possible definition would be minimizing the *largest residual*:
> $$
> \max_{i = 1,\dots,m} |b_{i} - a_{i}^{T}x|
> $$
> This problem can be formulated as a linear programming problem:
> $$
> \begin{align} 
>  \min_{(x,z) \in \mathbb{R}^{n+1}} \quad & z  \\ 
> \text{s.t.} \qquad & b_{i} - a^{T}_{i}x \leq z & i = 1,\dots,m \\
> & -(b_{i} - a^{T}_{i}x )  \leq z & i = 1,\dots ,m
>  \end{align}
> $$
> 
> Alternatively, we could minimize the sum of the prediction errors:
> $$
> \sum_{i = 1}^{m} |b_{i} - a^{T}_{i}x|
> $$
> which can also be formulated as a linear programming problem:
> $$
> \begin{align} 
>  \min_{(x,z) \in \mathbb{R}^{n+m}} \quad &  z_{1} + \dots + z_{m} \\ 
> \text{s.t.} \quad &  b_{i} - a^{T}_{i}x \leq z_{i} & i = 1, \dots, m \\
> & -(b_{i} - a^{T}_{i}x) \leq z_{i} & i = 1, \dots, m
>  \end{align}
> $$
> This formulation is also known as least absolute deviation linear regression. Typically, the quadratic cost criterion $\sum_{i}(b_{i} - a^{T}_{i}x)^{2}$ is preferred, because it's way easier to solve and has some favourable statistical properties. However, the least absolute deviation can prove useful when one needs a more robust linear regression against outliers in the data set.
