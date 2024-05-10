---
tags:
  - math/optimization/linear-programming
  - math/definition
next: "[[LPP In Standard Form]]"
prev: "[[Linear Programming Master Note]]"
up: "[[Linear Programming Master Note]]"
abstract: definition of the linear programming problem and of the general form
---
# The Linear Programming Problem
## Basic Terminology
In a general **linear programming problem** we are given a **cost vector** $c = (c_1,...,c_n)$ and we seek to minimize a **linear cost function**

$$f(x) := c^T x = \sum_{i=1}^n c_{i}x_{i}$$

over  all $n$-dimensional vectors $x = (x_1, ..., x_n$) subject to a set of linear equality and inequality constraints

$$
\begin{align}
a_{i}^T x \geq b_{i} \qquad & i \in M_{1} \\
a_{i}^T x \leq b_{i} \qquad & i \in M_{2} \\
a_{i}^T x = b_{i} \qquad & i \in M_{3} \\
x_{j} \geq 0 \qquad & j \in N_{1} \\
x_{j} \leq 0 \qquad & j \in N_{2} \\
\end{align}
$$

where $a_{i} \in \mathbb{R}^n$, $b_{i} \in \mathbb{R}$ and $M_1, M_2, M_3, N_{1}, N_{2}$ are finite index sets.

> [!definition] Linear Programming Problem
> A linear programming problem it's any optimization problem in the following form:
> $$\begin{align}
> \min_{x\in \mathbb{R}^n} \quad &\sum_{i}^n c_{i}x_{i} & \\
> \text{s.t.} \quad & a_{i}^T x \geq b_{i} & \quad  i \in M_{1} & \\
> & a_{i}^T x \leq b_{i} & \quad  i \in M_{2} \\
> & a_{i}^T x = b_{i} & \quad  i \in M_{3} \\
> & x_{j} \geq 0 & \quad  j \in N_{1} \\
> & x_{j} \leq 0 & \quad  j \in N_{2} \\
> \end{align}
> $$

^07d883



The variables $x_1,...,x_n$ are known as **decision variables**, and a vector $x$ satisfying all of the constraints is said to be a **feasible solution**. The set of all feasible solutions is called **feasible set** or **feasible region**.
If $j \not\in N_{1} \cup N_{2}$ then we say that $x_j$ is a **free or unrestricted variable**. ^fe000b

The function $f(x) = c^T x$ is called **objective function** or **cost function**. A feasible solution $x^*$ that minimizes the objective function is called an **optimal feasible solution**, or simply an optimal solution.
If for any $K \in \mathbb{R}$ we can find a feasible $x$ such that $f(x) < K$ then we say that the optimal cost is $-\infty$ or that the cost is **unbounded below**.

## General Form Of A Linear Program
### Transforming a Maximization Into a Minimization
A linear program where we are trying to maximize the cost function $c^T x$ can be easily transformed into an equivalent minimization problem by minimizing the cost function $(-c)^T x$ . So, without any loss of generalization, we can consider only linear minimization problems.
### Transforming Every Linear Program Into The General Form

^58dd5b

Every constraint in a linear program can be transformed into an inequality constraint in the form of $a_{i}^T x  \geq 0$.  *Indeed* we have that:

$$ a_{i}^T x \leq b_{i} \iff (-a_{i})^T x \geq -b_{i}$$
$$ a_{i}^T x = b_{i} \iff a_{i}^T x \geq b_{i}\quad \text{and} \quad(-a_{i})^T x \geq -b_{i} $$
The constraints in the form $x_{j} \geq 0$ and $x_{j} \leq 0$ are just a special case where $a_i = e_j = (0,\dots,0,1,0,\dots,0)$.
This suggests that we can rewrite every linear programming problem in  **general form** as:

$$\begin{align}
\min_{x\in \mathbb{R}^n} \quad &  c^Tx \\ \\
\text{s.t.} \quad  & a_{i}^Tx \geq b_{i} \quad \forall \; i = 1,\dots,m
\end{align}
$$
### Compact Matrix notation For General Form
By defining matrix $A \in \mathbb{R}^{m \times n}$ and vector $b \in \mathbb{R}^m$ as:

$$A:= \begin{pmatrix}
- & a_{1}^T & - \\
& \vdots & \\
- & a_{m}^T & - \\
\end{pmatrix}
\qquad
b:= \begin{pmatrix}
b_{1} \\
\vdots \\
b_{m} \\
\end{pmatrix}
$$
we can rewrite the general form in compact notation as:

$$\begin{align}
\min_{x\in \mathbb{R}^n} \quad &  c^Tx \\ \\
\text{s.t.} \quad  & Ax \geq b 
\end{align}
$$
^linear-program-general-form

> [!definition] Linear Program In General Form
> A linear program is written in **general form** if it is expressed as follows:
> ![[#^linear-program-general-form]]

^73494d

> [!proposition]
> Every linear program can be written in general form

^6ccfd9

> [!proof]
> It follows trivially from the [[#^58dd5b|arguments above]]
> `\end{proof}`
