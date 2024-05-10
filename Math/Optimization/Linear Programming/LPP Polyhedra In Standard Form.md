---
tags:
  - math/optimization/linear-programming
next: "[[LPP Degeneracy]]"
prev: "[[LPP Fundamental Theorem Of Linear Programming]]"
up: "[[Linear Programming Master Note]]"
---
# Polyhedra In Standard Form
## Motivations
[[LPP Fundamental Theorem Of Linear Programming#^537f75|Previously]], we have established the importance of [[LPP Polyhedra And Their Vertices#^bd7b2a|vertices]] in linear programming. We have seen that when they exist, the search for the optimum can be restricted to the extreme points of the polyhedra, and [[LPP Fundamental Theorem Of Linear Programming#^c85132|since every polyhedra in standard form  admits at least a vertex]], it's easy to understand why the standard form plays a crucial role in the development of the simplex method. 
We will now focus on the properties of vertices for polyhedra in standard form.
## The Full Row Rank Assumption

^0b2975

We will assume throughout this note that the standard form problem:
![[LPP In Standard Form#^linear-program-standard-form]]
has a matrix $A \in \mathbb{R}^{m \times n}$ with full row rank, i.e. $\text{rank}(A) = m \leq n$. This can be done with no loss of generality, because linearly dependent rows can either:
- Impose an impossible constraint, such as $x_1 = 1$ and $2x_{1} = 1$;
- Be redundant.

We prove this formally:

> [!theorem]
> Let $P = \{ x \in \mathbb{R}^{n} \; | \; Ax = b, \; x \geq 0 \}$ be a nonempty polyhedron, where $A \in \mathbb{R}^{m \times n}$ is a matrix with rows $a_{1},\dots,a_{m}$.
> Suppose that $\text{rank}(A) = k <m$, then for any choice of linearly independent rows $a_{i_{1}}, \dots,a_{i_{k}}$ we have:
> $$
> Q := \{ x \in \mathbb{R}^{n} \; | \; a_{i_{1}}^{T}x = b_{i_{1}},\dots,
> a_{i_{k}}^{T}x = b_{i_{k}}, \; x\geq 0 \} = P
> $$

^5647cd

> [!proof]-
> By rearranging the rows of $A$ we can assume without loss of generalization that $i_{1}=1, \dots, i_{k}=k$. 
> Clearly $P \subseteq Q$, since any element of $P$ automatically satisfies the constraints of $Q$. 
> Viceversa, for any $x \in Q$ we just need to prove that $a_{i}^{T}x =b_{i}$ for all $i>k$. By hypothesis, $a_{1},\dots,a_{k}$ form a basis of the row space, hence we can rewrite $a_{i}$ as their linear combination: 
> $$
> a_{i} = \sum_{j=1}^{k}\lambda_{i,j}a_{j}  
> $$
> For any $y \in P \neq \emptyset$ we can conclude that:
> $$b_{i} = a_{i}^{T}y =  \sum_{j=1}^{k} \lambda_{i,j}a_{j}^{T}y = \sum_{j=1}^{k}\lambda_{i,j}b_{j} = \sum_{j=1}^{k} \lambda_{i,j}a_{j}^{T}x = a_{i}^{T}x $$
> therefore $a_{i}^{T}x = b_{i}$ and $x$ belongs to $P$ as well.
> `\end{proof}`
## Constructing Basic Solutions
> [!theorem] Characterization Of Basic Solutions
> Consider a polyhedron in standard form:
> $$P = \{ x \in \mathbb{R}^{n} \; | \; Ax = b, x\geq 0 \}$$
> where $A \in \mathbb{R}^{m \times n}$ has full row rank $m$. Then $x \in \mathbb{R}^{n}$ is a [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic solution]] if and only if the following hold:
> 1) $Ax = b$;
> 2) There exist indices $B_{1},\dots,B_{m}$ such that:
>    - The columns $A_{B_{1}}, \dots, A_{B_{m}}$ are linearly independent;
>    - If $i \not\in \{ B_{1},\dots,B_{m} \}$ then $x_{i} = 0$.

^bc0820

> [!proof]-
> ***($\Leftarrow$)*** Let $x$ respect (1) and (2) in the theorem. We want to prove that the active constraints $Ax = b$ and $x_{i} = 0$ for $i \neq B_{1},\dots,B_{m}$ are linearly independent. For any $x$ that respects both we have that:
> $$b = Ax = \sum_{i=1}^{n}A_{i}x_{i} = \sum_{i=1}^{m}A_{B_{i}} x_{B_{i}}$$
> Since the columns $A_{B_{i}}$ are linearly independent the coefficients $x_{B_{i}}$ are uniquely determined, thus the system $Ax=b$, $x_{i} = 0$ has a unique solution. But according to [[Rouche-Capelli Theorem|Rouché-Capelli theorem]] this is true if and only if the constraints are linearly independent, hence $x$ is a basic solution.
> 
> ***($\Rightarrow$)***  Assume that $x$ is a basic solution. Let $x_{B_{1}},\dots,x_{B_{k}}$ be the components of $x$ that are nonzero. Since $x$ is a basic solution the system of equations formed by the active constraints has a unique solution. But:
> $$\begin{cases}
 x_{i} = 0 \quad \forall i \neq B_{j} \\
Ax = \sum_{i=1}^{n} A_{i}x_{i} = b\\
\end{cases} 
\quad \text{ is unique only if} \quad \sum_{i=1}^{k} A_{B_{j}}x_{B_{j}} = b \quad \text{is unique} $$
and the linear combination being uniquely determined means that the columns $A_{B_{i}}$ are linearly independent. Since the matrix $A$ has row rank $m$, there must be $m$ linearly independent columns, so we can complete the previous "base" by adding $A_{B_{k+1}},\dots,A_{B_{m}}$ other columns. Thus we have found the indices $B_{1},\dots,B_{m}$ that respect the theorem hypothesis.
`\end{proof}`

Thanks to the previous characterization we have found a procedure to construct basic solutions:

> [!algorithm] Procedure for constructing basic solutions
> 1) Choose $m$ linearly independent columns $A_{B_{1}}, \dots, A_{B_{m}}$;
> 2) Let $x_{i} = 0$ for all $i \not\in \{ B_{1},\dots,B_{m} \}$
> 3) Solve the system $Ax = b$ for the unknowns $x_{B_{1}},\dots, x_{B_{m}}$

If a basic solution constructed according to this procedure is nonnegative, then it's also a basic feasible solution. Conversely, since every basic feasible solution is a basic solution, it can be obtained from this procedure.
### Useful Terminology For Basic Solutions
If $x$ is a basic solution, the variables $x_{B_{1}},\dots,x_{B_{m}}$ are called **basic variables**, while the remaining variable are called **nonbasic**. The columns $A_{B_{1}},\dots,A_{B_{m}}$ are called **basic columns** (note that they form a basis of $\mathbb{R}^{m}$).
We will say that two bases $(B_{i})_{i}$ and $(\tilde{B}_{i})_{i}$ are distinct, or different, if they involve different set of basic indices, i.e. if $\{ B_{i} \}_{i} \neq \{ \tilde{B}_{i} \}_{i}$ (the order of the indices does not matter).
By arranging the $m$ basic columns next to each other, we obtain a matrix $B \in \mathbb{R}^{m\times m}$ known as **basis matrix**. Note that this matrix is invertible, since the basic columns are linearly independent.

$$
B := \begin{pmatrix}
\mid & \mid & & \mid  \\
A_{B_{1}} & A_{B_{2}} & \dots & A_{B_{m}} \\
\mid & \mid & & \mid
\end{pmatrix}
$$
^basis-matrix-formula

Similarly, we define the vector $x_{B}$ as:
$$
x_{B} := \begin{pmatrix}
x_{B_{1}} \\
\vdots \\
x_{B_{m}}
\end{pmatrix}
$$
Since the nonbasic variables are all zero, we have that $Ax = Bx_{B}$, hence the basic variables are determined by solving the equation $Bx_{B} = b$, whose unique solution is given by:
$$
x_{B} = B^{-1}b
$$
We summarise all the previous terminology in the following definition:

> [!definition]+ Basis, Basic Variables And Basis Matrix
> For a full row rank matrix $A \in \mathbb{R}^{m\times n}$ consider a LPP in standard form:
> ![[LPP In Standard Form#^linear-program-standard-form]]
> A set of indices $B_{1},\dots,B_{m}$ such that the columns $A_{B_{1}},\dots, A_{B_{m}}$ are linearly independent is called a **basis**. The **basis matrix** $B$ is defined as:
> ![[#^basis-matrix-formula]]
> A basis determines a [[LPP Polyhedra And Their Vertices#^basic-solution-definition|basic solution]] $x$ by imposing:
> - $x_{i} = 0$ for all **nonbasic variables** $i \not\in \{ B_{1},\dots,B_{m} \}$ 
> - $x_{B} = (x_{B_{1}},\dots, x_{B_{m}}) = B^{-1}b$ for the **basic variables**.

^12f1aa

> [!example]-
> Let the constraint $Ax = b$ be of the form:
> $$
> \begin{pmatrix}
> 1 & 1 & 2 & 1 & 0 & 0 & 0 \\
> 0 & 1 & 6 & 0 & 1 & 0 & 0  \\
> 1 & 0 & 0 & 0 & 0 & 1 & 0 \\
> 0 & 1 & 0 & 0 & 0 & 0 & 1
> \end{pmatrix} x = 
> \begin{pmatrix}
> 8 \\
> 12 \\
> 4 \\
> 6
> \end{pmatrix}
> $$
> we choose $(B_{1},\dots,B_{4}) = (4,5,6,7)$ as our basis. Note that the basic columns $A_{4}, \dots, A_{7} = e_{1},\dots,e_{4}$  are linearly independent and that the corresponding basis matrix is $B = \mathbb{1}_{4}$. This means that our basic solution, obtained by solving $Bx_{B} = (8,12,4,6)$ is simply $x = (0,0,0,8,12,4,6)$, which is nonnegative, hence a basic feasible solution as well.
> Another basis is obtained by choosing the columns $A_{3},A_{5},A_{6},A_{7}$. The corresponding basic solution is $x = (0,0,4,0,-12,4,6)$, which is not feasible because $x_{5} = -12 < 0$.

### Intuition On Basic Solutions
[[LPP In Standard Form#^82f69c|Recall]] the interpretation of the standard form constraints as a requirement to synthesize the vector $b$ using a nonnegative amount of the resource vectors $A_{i}$. In a basic solution, we use only $m$ out of the $n$ available resource vectors. 
Intuitively it makes sense that the optimal solution is obtained by exploiting the lowest cost resource vectors sufficient to synthesize $b$. If we were to use another redundant (linearly dependent) and more costly resource vector, it can be argued that its contribution to the synthesis of $b$ can be replaced by using a cheaper linear combination of the other resource vectors of the basis.

## Adjacent Basic Solutions And Adjacent Bases

^814764

We recall the definition of adjacent basic solutions:
![[LPP Polyhedra And Their Vertices#^fb166a]]

For standard form problems, the $m$ constraints $Ax=b$ are always active, while the constraints $x_{i} \geq 0$ that are active are determined by the $n-m$ indices $i \not\in \{ B_{1},\dots,B_{m} \}$, hence the following definition:

> [!definition] Adjacent Bases
> Two bases of a standard form problem are adjacent if they share all but one basic column.

^d64bc2

## Correspondence Of Bases And Basic Solutions
Basic solutions must correspond to different bases, because a basis uniquely determines a basic solution. However, two different bases may lead to the same basic solution. This may happen if $x_{B}$ has a zero component (for $b=0$ this is always the case, and every basis leads to the same basic solution $x = 0$).
This phenomenon has some important algorithmic implications, and is closely related to degeneracy.