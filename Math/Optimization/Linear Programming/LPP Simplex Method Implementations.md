---
tags:
  - "#math/optimization/linear-programming"
next: "[[LPP Simplex Method Anticycling Rules]]"
prev: "[[LPP The Simplex Method Framework]]"
up: "[[Linear Programming Master Note]]"
---
# Simplex Method Implementations
## The simplex Method Framework
In order to solve the [[LPP In Standard Form#^d3d2e7|LPP in standard form]]:
![[LPP In Standard Form#^linear-program-standard-form]]
we have developed the general framework of the [[LPP The Simplex Method Framework|simplex method]]. An iteration of the method is as follows:
![[LPP The Simplex Method Framework#^f73df0]]

In this note, we will discuss some ways of carrying out some of its mechanics.
Most of the method complexity is in the computation of $B^{-1}A_{j}$, thus the main difference between implementations lies in the way that the vectors $B^{-1}A_{j}$ are computed.
There is another important aspect of the simplex method that has not been discussed: finding the first basic feasible solution. This is addressed in [[LPP Simplex Method Finding An Initial Solution|another note]].
### Minor Note On Computing Complexity
For the sake of complexity analysis keep in mind that for a matrix $B \in \mathbb{R}^{m \times m }$  and two vector $c,x \in\mathbb{R}^{n}$ the following holds:
1) Computing the inverse or solving $Bx = b$ takes $\mathcal{O}(m^{3})$ operations
2) Computing $Bb$ takes $\mathcal{O}(m^{2})$ operations 
3) The $n$-dimensional scalar product $c^{T}x$ requires $\mathcal{O}(n)$ operations.

Moreover, thanks to the [[LPP Polyhedra In Standard Form#The Full Row Rank Assumption|the full row rank assumption]] on $A \in \mathbb{R}^{m \times n}$, we have $m \leq n$.
# Naive Implementation Of The Simplex Method
## Naive Implementation Algorithm
Starting from a basic feasible solution, the most straightforward implementation of the method consists in forming the basis matrix $B$ and computing $p^{T} = c^{T}_{B}B^{-1}$, by solving the linear system $p^{T}B = c_{B}$. 
This vector $p$ is called the **vector of simplex multipliers** associated with the basis $B$.
The reduced cost $\bar{c}_{j} = c_{j} -c_{B}^{T}B^{-1}A_{j}$ is then obtained using the formula:
$$
\bar{c}_{j} = c_{j} - p^{T}A_{j}
$$
Once a column $A_{j}$ is selected to enter the basis, we solve the linear system $Bu = A_{j}$ in order to determine the vector $u$. We finally determine $\theta^{*}$ and the variable that will exit the basis, and construct a new basic feasible solution.

## Algorithm Complexity Of The Naive Implementation
To solve the systems $p^{T}B = c^{T}$ and $Bu = A_{j}$ we need $\mathcal{O}(m^{3})$ arithmetic operations. In addition, computing the reduced costs of all variables requires $\mathcal{O}(mn)$ operations because we need to compute the inner product $p^{T}A_{j}$ $(n-m)\leq n$ times in the worst case.
*The total computational effort per iteration is $\mathcal{O}(m^{3}+mn)$*. We will shortly see that alternative implementations require only $\mathcal{O}(m^{2} + mn)$ arithmetic operations.
The naive implementation is rather inefficient in general. However, for certain problems with special structure, the linear systems $p^{T}B = c^{T}_{B}$ and $Bu=A_{j}$ can be solved very quickly (for example when $B$ is a sparse matrix). In such cases, this implementation can be of practical interest.

# Revised Simplex Method
## Revised Simplex Method Algorithm Development
Much of the computational burden in the naive implementation is due to the need for solving two linear systems of equations. In the revised simplex method, the matrix $B^{-1}$ is computed once and strategically updated in an efficient way for each iteration. This way computing $p^{T} = c^{T}_{B}B^{-1}$ and $B^{-1}A_{j}$ is just a matter of performing a simple matrix-vector multiplication.

Let $B = [A_{B_{1}}, \dots, A_{B_{m}}]$ be the basis matrix and let 
$$
\bar{B} = [A_{B_{1}},\dots,A_{B_{\ell -1}}, A_{j}, A_{B_{\ell +1}},\dots,A_{B_{m}}]
$$
be the updated basis matrix  at the end of the iteration. The basic idea of the revised simplex method is that $B^{-1}$ can be exploited to compute $\bar{B}^{-1}$. To do that we will use *elementary row operations* (in a similar way to Gaussian elimination when solving linear systems).
### Elementary Row Operations
> [!definition] Elementary Row Operation
> Given a matrix, the operation of adding a constant multiple of one row to the same or to another row is called an **elementary row operation**.

Applying an elementary row operation where we add to the $i$th row the $j$th row multiplied by $\beta$ is equivalent to left multiplying the matrix by the matrix $Q = \mathbb{1} + D_{i,j}$, where $D_{i,j}$ is a matrix with all entries equal to zero, except for the $(i,j)$ entry which is equal to $\beta$. Observe that $\det (Q)=1$, so $Q$ is invertible.
Applying a sequence of $K$ elementary row operations is the same as left multiplying $Q_{K}Q_{K-1}\cdots Q_{2}Q_{1}$. In particular, if by performing those operations we bring a matrix $B$ to the identity matrix, then $Q_{K} \cdots Q_{1} = B^{-1}$, by definition and uniqueness of the inverse matrix.
### Computing $\bar{B}^{-1}$ Using Elementary Row Operations
Since $B^{-1}B = \mathbb{1}$, we have that $B^{-1}A_{B_{i}}= e_{i}$, hence:
$$
B^{-1}\bar{B} = \begin{pmatrix}
| & & | & | & | & & |\\
e_{1} & \dots & e_{\ell - 1} & u & e_{\ell + 1} & \dots & e_{m} \\
| & & | & | & | & & |
\end{pmatrix}=
\begin{pmatrix}
1 & & u_{1} \\
& \ddots & \vdots \\
& &  u_{\ell} \\
& & \vdots & \ddots \\
& & u_{m} & & 1
\end{pmatrix}
$$

where $u = B^{-1}A_{j}$.  Like in gaussian elimination, we can apply a sequence of elementary row operations that will change the above matrix to the identity matrix:
1) For each $i \neq \ell$, we add the $\ell$th row times $-u_{i} / u_{\ell}$ to the $i$th row (recall that $u_{\ell}>0$). This replaces $u_{i}$ with zero.
2) We divide the $\ell$th row by $u_{\ell}$. This replaces $u_{\ell}$ by one.

This sequence of operations is equivalent to left multiplying $B^{-1}\bar{B}$ by a certain matrix $Q$ such that $QB^{-1}\bar{B} = \mathbb{1}$, hence $Q B^{1} = \bar{B}^{-1}$. This means that if we simultaneously apply the same elementary row operations to $B^{-1}$ we obtain $\bar{B}^{-1}$.
Once we have obtained $\bar{B}^{-1}$ we can compute all the simplex method quantities by performing simple matrix multiplications. This particular way of updating $\bar{B}^{-1}$ and carrying out computations is a particular implementation of the simplex method known as the *revised simplex method*.

> [!example]-
> Let
> $$
> B^{-1} = 
> \begin{pmatrix}
> 1 & 2 & 3 \\
> -2 & 3 & 1 \\
> 4 & -3 & -2
> \end{pmatrix},
> \qquad
> u = 
> \begin{pmatrix}
> -4   \\
> 2 \\
> 2 
> \end{pmatrix}
> $$
> and suppose that $\ell = 3$ is the index leaving the basis. Our objective is to transform $u$ to the unit vector $e_{3} = (0,0,1)$. We achieve this by performing 3 elementary rows operations:
> 1) We multiply the third row by 2 and add it to the first row
> 2) We subtract the third row from the second row
> 3) We divide the third row by 2
> 
> By performing these operations also on $B^{-1}$ we obtain the matrix $\bar{B}^{-1}$:
> $$
> \bar{B}^{-1} = \begin{pmatrix}
> 9 & -4 & -1 \\
> -6 & 6 & 3 \\
> 2 & -1.5 & -1
> \end{pmatrix}
> $$
## Revised Simplex Method Algorithm
An iteration of the **revised simplex method** is performed as follows:

> [!algorithm] Iteration Of The Revised Simplex Method
> 1) We start from a basis with basic columns $A_{B_{1}},\dots,A_{B_{m}}$ and an associated basic feasible solution $x$. We assume to have already computed the inverse $B^{-1}$ of the basis matrix.
> 2) We compute the row vector $p^{T} = c^{T}_{B}B^{-1}$ and then compute the reduced costs $\bar{c}_{j} = c_{j} - p^{T}A_{j}$. If they are all nonnegative, then $x$ is optimal and we stop. Otherwise we choose some $j$ for which $\bar{c}_{j} < 0$.
> 3) We compute $u = B^{-1}A_{j}$. If no component of $u$ is positive, the optimal cost is $-\infty$ and the algorithm terminates.
> 4) If some component of $u$ is positive, we compute:
>    $$
>    \theta^{*} = min_{u_{i} > 0} \frac{x_{B_{i}}}{u_{i}} = \frac{x_{B_{\ell}}}{u_{\ell}}
>    $$
> 5) We form a new basis by replacing $A_{B_{\ell}}$ with $A_{j}$, finding a new feasible solution with $y_{j} = \theta^{*}$ and $y_{B_{i}} = x_{B_{i}}-\theta^{*}u_{i}$ for $i \neq \ell$;
> 6) We form the $m \times (m+1)$ matrix $[B^{-1} | \; u]$, and we perform elementary row operations so to bring the vector $u$ to be equal to the unit vector $e_{\ell}$. The first $m$ colums of the resulting matrix is the inverse of the new basis matrix, i.e. $\bar{B}^{-1}$.

## Revised Simplex Method Algorithm Complexity
The revised simplex method needs to perform:
1) Two matrix vector multiplications $c_{B}^{T}B^{-1}$, $B^{-1}A_{j}$ of complexity $\mathcal{O}(m^{2})$;
2) Update the $B^{-1}$ matrix to $\bar{B}^{-1}$ with $m$ elementary row operations, which has complexity $\mathcal{O}(m^{2})$;
3) $n-m \leq m$ inner products to compute the reduced costs $c_{j}-p^{T}A_{j}$, which has complexity $\mathcal{O}(mn)$.

We conclude that, for each iteration, *the revised simplex method has a worst case complexity of $\mathcal{O}(m^{2} + m n)$*. Since $m \leq n$, the worst case complexity is also $\mathcal{O}(mn)$.
Sometimes the first reduced cost is negative, so we don't really need to compute all the remaining reduced costs, so *the best case computational complexity is of $\mathcal{O}(m^{2})$*. 
The method requires to store the $m\times m$ matrix $B^{-1}$, so *its memory requirements are $\mathcal{O}(m^{2})$*.

## Practical Performance Enhancements
Practical implementations incorporate additional ideas from numerical linear algebra. Many of the following ideas are designed to improve numerical stability, and exploit sparsity. These techniques have a critical effect in practice.

The first idea is related to **reinversion**: at each iteration of the revised simplex method $B^{-1}$ is updated using elementary row operations. Each iteration introduces roundoff or truncation error, for this reason it is necessary to recompute $B^{-1}$ from scratch every once in a while. The efficiency of such reinversion can be enhanced by using suitable data structures and techniques.

Another set of ideas is related to the way that the inverse $B^{-1}$ is represented. Suppose that $B^{-1}$ has been just calculated. Subsequent updates of the matrix to the new inverse $\bar{B}^{-1} = QB^{-1}$ don't need to be carried out explicitly. Indeed, suppose we need to compute $u = \bar{B}^{-1}A_{j}$. Then it's also true that $u = QB^{-1}A_{j}$, and since the matrix $Q$ is just representing the elementary row operations, we can first compute $B^{-1}A_{j}$, and then apply the same elementary row operations to produce $u$.
The same idea can also be used to represent $B^{-1}$ after several iterations, as a product of the initial inverse basis matrix, and several sparse matrices like $Q$.

Finally, we mention the idea of representing $B^{-1}$, subsequent to a reinversion, in terms of sparse triangular matrices with a special (expolitable) structure.

# The Full Tableau Implementation
## Tableau Algorithm Development
We describe the implementation of the simplex method in terms of the so-called *full tableau*. This implementation is one of the easiest to carry out "by (human) hand".
In concept, it is quite similar to the revised simplex method, but instead of maintaining $B^{-1}$ we maintain and update a matrix known as the **simplex tableau**:
$$
\begin{array}{| l | c|}
\hline
-c_{B}^{T}x_{B} & c^{T} - c^{T}_{B}B^{-1}A
\\
\hline 
B^{-1}b&  B^{-1}A  \\
\hline
\end{array} =

\begin{array}{| l | c|}
\hline
-c^{T}x & \bar{c}
\\
\hline 
x_{B} &  B^{-1}A  \\
\hline
\end{array} =

\begin{array}{| l | c  c c|}
\hline
-c_{B}^{T}x_{B} & \bar{c_{1}} & \dots & \bar{c}_{j}
\\
\hline 
x_{B_{1}} &  | & & | \\
\vdots & B^{-1}A_{1} & \dots & B^{-1}A_{n} \\
x_{B_{m}} & | & & | \\
\hline
\end{array}
$$
^full-tableau-table

The top row, referred to as the **zeroth row**, contains in the first column the negative value of the current cost (the minus sign allows for a simpler update), and in the other columns it contains the reduced costs.
The first column $B^{-1}b$ of the simplex tableau, also called the **zeroth column**, contains the values of the basic variables $x_{B}$. 
The column $B^{-1}A_{i}$, also known as the **$i$th column** of the tableau, corresponds to opposite of the $i$th [[LPP Simplex Method Optimality Conditions#^fbcf42|basic direction]] $d_{B}$. 
The column $u = B^{-1}A_{j}$ corresponding to the variable that enters the basis is called the **pivot column**. If the $\ell$th basic variable exits the basis, the $\ell$th row of the tableau is called the **pivot row**.
The element $u_{\ell}$ identified by the pivot column and pivot row is called the **pivot element**. The pivot element $u_{\ell}$ is always positive (unless all the $i$th columns are nonpositive, in which case the [[LPP The Simplex Method Framework#^f73df0|simplex method]] terminates due to step 3).

*First* , we determine *which index $j$ should enter the basis*. We look at the $i$th columns and pick among them any $j$th column that has at least one $\bar{c}_{j}<0$. If none exists, then the basis is optimal and we terminate.

*Second*, we determine the index $B_{\ell}$ that needs to exit the basis. Looking at the pivot column $u$,  for all $u_{i}> 0$ we compute the ratios $x_{B_{i}} / u_{i}$, picking the minimum among them. If every $u_{i} \leq 0$, then the problem is unbounded and we terminate.

*Third*, we need to *update the tableau information by substituting $B^{-1}$ with $\bar{B}^{-1}$ for the $B^{-1}[b \; | A]$ part of the tableau*. Like for the revised simplex method, this update can be achieved by left-multiplying a sequence of elementary row operations that bring the pivot column to be equal to the unit vector $e_{\ell}$. This means adding multiples of the pivot row to the other rows so that the pivot column elements go to zero, except for the pivot element which is set to one. 

*Finally*, we need to update the zeroth row. The rule for updating the zeroth row is identical to the rule used for the other rows of the tableau: add a multiple of the pivot row to the zeroth row so to set the reduced cost of the entering variable to zero.
We now verify that this update rule produces the correct results for the zeroth row. At the beginning, the zeroth row can be written as:
$$
[0 \; | \; c^{T}] - g^{T}[b \; | \; A]
$$
where $g^{T} = c^{T}_{B}B^{-1}$.  Let $j$ be the pivot column and $\ell$ the pivot row. Note that the pivot row is of the form $h^{T}[b|A]$, where $h^{T}$ is the $\ell$th row of $B^{-1}$. This means that after adding a multiple of the pivot row to the zeroth row, the zeroth row is of the form:
$$
[0 \; | \; c^{T}] - p^{T}[b \; | \; A]
$$
for some vector $p$. Our update rule is such that the pivot column entry of the zeroth row becomes zero, that is:
$$
c_{\bar{B}_{\ell}} - p^{T}A_{\bar{B}_{\ell}} = c_{j} - p^{T} A_{j} = 0
$$
For any other row $\bar{B}_{i}$ ($i \neq \ell$) the zeroth row entry of that column is zero, because $B^{-1}A_{B_{i}} = e_{i}$ and $i \neq \ell$. Hence, adding the pivot row to the zeroth row of the tableau has no effect on the basis columns, which are left at zero.
We conclude by noticing that $p$ satisfies $c_{\bar{B}_{i}}-p^{T}A_{\bar{B}_{i}} = 0$, which implies that $c^{T}_{\bar{B}}-p^{T}\bar{B} =0$, so $p^{T} = c^{T}_{B} \bar{B}^{-1}$, hence the updated zeroth row of the tableaus is equal to :
$$
[0 \; | \; c^{T}] - c^{T}_{\bar{B}}\bar{B}^{-1}[b \; | \; A]
$$
which is exactly what we wanted.
## The Full Tableau Simplex Method Algorithm
We summarize the mechanics of the full tableau implementation:

> [!algorithm]
> 1) An iteration starts with the tableau associated with a basis matrix $B$ and the corresponding basic feasible solution $x$: 
>    $$
> 	\begin{array}{| l | c  c c|}
> 	\hline
> 	-c_{B}^{T}x_{B} & \bar{c_{1}} & \dots & \bar{c}_{j}
> 	\\
> 	\hline 
> 	x_{B_{1}} &  | & & | \\
> 	\vdots & B^{-1}A_{1} & \dots & B^{-1}A_{n} \\
> 	x_{B_{m}} & | & & | \\
> 	\hline
> 	\end{array}
>    $$
> 2) We examine the reduced costs in the zeroth row of the tableau. If they are all nonnegative, the current basic feasible solution is optimal, and the algorithm terminates. Otherwise, choose some $j$ for which $\bar{c}_{j}<0$
> 3) We consider the $j$th column of the tableau $u = B^{-1}A_{j}$. If no component of $u$ is positive, the optimal cost is $-\infty$ and the algorithm terminates
> 4) For each $i$ with a positive $u_{i}$, we compute the ratio $x_{B_{i}} / u_{i}$ and pick the index $\ell$ for which that quantity is minimum. The column $A_{B_{\ell}}$ exits the basis and the column $A_{j}$ enters the basis
> 5) We add to each row a constant multiple of the $\ell$th row so that the pivot element $u_{\ell}$ becomes one and all other entries of the $\ell$ pivot column become zero

^5d32ae

> [!example]- Execution of the simplex method using the full tableau
> We consider the problem:
> $$
> \begin{array}{}
> \min_{x \in \mathbb{R}^{3}} & -10 x_{1} & -12x_{2} & -12x_{3}   \\
> \text{s.t.}  & x_{1} & +2x_{2} &  +2x_{3}  & \leq 20 \\
> &  2x_{1} & +x_{2} & +2x_{3} &   \leq 20 \\
> &  2x_{1} & +2x_{2} & +x_{3} &   \leq 20 \\
>  &      x_{1},x_{2},x_{3} \geq 0
> \end{array}
> $$
> We introduce the slack variables $x_{4}, x_{5},x_{6}$ to transform the problem into standard form:
> $$
> \begin{array}{}
> \min_{x \in \mathbb{R}^{6}} & -10 x_{1} & -12x_{2} & -12x_{3}   \\
> \text{s.t.}  & x_{1} & +2x_{2} &  +2x_{3}  & +x_{4}  &  &  &  = 20 \\
> &  2x_{1} & +x_{2} & +2x_{3} &  & +x_{5} &  &   = 20 \\
> &  2x_{1} & +2x_{2} & +x_{3} &   &  & +x_{6} &  = 20 \\
>  &      x_{1},\dots,x_{6} \geq 0
> \end{array}
> $$
> We note that $x = (0,0,0,20,20,20)$ is a basic feasible solution from which we can start the simplex method algorithm. In particular, $B_{1},B_{2},B_{3} = 4,5,6$ and $B = \mathbb{1}_{3}$. We also note that $c_{B} = 0$ so the reduced costs in the zeroth row are $\bar{c} = c$. The initial tableau will be: 
> 
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
> & 0  & -10 & -12  & -12 & 0 & 0 & 0 \\
> \hline
> x_{4} =  & 20 & 1 & 2 & 2 & 1 & 0 & 0 \\
> x_{5} =   & 20 & \color{red}{2^{*}} & 1 & 2 & 0 & 1 & 0 \\
> x_{6} =   & 20 & 2 & 2 & 1 & 0 & 0 & 1 \\
> \hline
> \end{array}
> $$
> Since the first column, associated to the variable $x_{1}$, has a negative reduced cost $\bar{c}=-10$ we can make $1$ enter the basis. Using the pivot column $u = (1,2,2)$ we compute the ratios $(x_{B_{i}} / u_{i})_i =(20,10,10)$ and use the minimum value to pick the pivot row. In this case there is a tie between $i = 2$ and $i = 3$, we break this tie by choosing $\ell = 2$.
> Thus we have determined the pivot element, that we indicate with an asterisk in the above table.
> Using elementary row operations we bring the first column to be equal to $(0,0,1,0)$:
> $$
> \begin{array}{|c c | ll |}
> \hline
> & 100  & 0 & -7  & -2 & 0 & 5 & 0 \\
> \hline
> x_{4} =  & 10 & 0 & 1.5 & \color{red}1^{*} & 1 & -0.5 & 0 \\
> x_{1} =   & 10 & 1 & 0.5 & 1 & 0 & 0.5 & 0 \\
> x_{6} =   & 0 & 0 & 1 & -1 & 0 & -1 & 1 \\
> \hline
> \end{array}
> $$
> We perform another iteration of the simplex method: we pick $3$ to enter the basis, and compute the ratios $(x_{B_{i}} / u_{i}) = (10, 10)$ for $u_{i}>0$. We make $4$ leave the basis, and update the tableau through elementary row operations:
> 
> $$
> \begin{array}{|c c | ll |}
> \hline
> & 120  & 0 & -4  & 0 & 2 & 4 & 0 \\
> \hline
> x_{3} =  & 10 & 0 & 1.5 & 1 & 1 & -0.5 & 0 \\
> x_{1} =   & 0 & 1 & -1 & 0 & -1 & 1 & 0 \\
> x_{6} =   & 10 & 0 & \color{red} 2.5^{*} & 0 & 1 & -1.5 & 1 \\
> \hline
> \end{array}
> $$
> Since the only negative reduced cost is $\bar{c}_{2} = -4$, $i = 2$ enter the basis. We compute the ratios $(x_{B_{i}} / u_{i}) = (10 /1.5, 10 / 2.5)$, and, by picking the minimum, we individuate the $3$rd row as the pivot row, hence $6$ leaves the basis. We update the tableau:
> 
> $$
> \begin{array}{|c c | ll |}
> \hline
> & 136  & 0 & 0  & 0 & 3.6 & 1.6 & 1.6 \\
> \hline
> x_{3} =  & 4 & 0 & 0 & 1 & 0.4 & 0.4 & -0.6 \\
> x_{1} =   & 4 & 1 & 0 & 0 & -0.6 & 0.4 & 0.4 \\
> x_{2} =   & 4 & 0 & 1 & 0 & 0.4 & -0.6 & 0.4 \\
> \hline
> \end{array}
> $$
> Since all the reduced costs are nonnegative, the simplex method terminates concluding that the basis that we have found is optimal. The optimal solution is $x = (4,4,4,0,0,0)$, which in terms of the original problem is $x = (4,4,4)$.

> [!example]- The Simplex Method May Cycle
> We provide an example that shows that the simplex method can indeed cycle and go on forever. We shall use the following pivoting rule:
> 1) The nonbasic variable with the most negative reduced cost $\bar{c}_{j}$ enters the basis;
> 2) Between the variables that can exit the basis, we always pick the one with the smallest subscript.
>
> Starting from the following tableau, we iterate the simplex method over it:
> 
> $$
> \begin{array}{|c c | ll |}
> \hline
> & 3  & -3 / 4 & 20  & -1 / 2 & 6 & 0 & 0 & 0 \\
> \hline
> x_{5} =  & 0 & \color{red} 1 / 4^{*}  & -8 & -1 & 9 & 1 & 0 & 0 \\
> x_{6} =   & 0 & 1 / 2 & -12 & -1 / 2 & 3  & 0 & 1 & 0 \\
> x_{7} =   & 1 & 0 & 0 & 1 & 0 & 0 & 0 & 1\\
> \hline
> \end{array}
> \Rightarrow
> \begin{array}{|c c | ll |}
> \hline
> & 3  & 0 & -4 & -7 / 2 & 33 & 3  & 0 & 0 \\
> \hline
> x_{1} =  & 0 & 1 & -32 & -4 & 36 & 4 & 0 & 0 \\
> x_{6} =   & 0 & 0 & \color{red} 4^{*} & 3 / 2 & -15 & -2 & 1 & 0 \\
> x_{7} =   & 1 & 0 & 0 & 1 & 0 & 0 & 0 & 1\\
> \hline
> \end{array}
> $$
> 
> $$
> \Rightarrow
> \begin{array}{|c c | ll |}
> \hline
> & 3  & 0 & 0 & -2 & 18 & 1 & 1 & 0 \\
> \hline
> x_{1} =  & 0 & 1 & 0 & \color{red}8^{*} & -84 & -12 & 8 & 0 \\
> x_{2} =   & 0 & 0 & 1 & 3 / 8 & -15 / 4 & - 1 / 2 & 1 / 4 & 0 \\
> x_{7} =   & 1 & 0 & 0 & 1 & 0 & 0 & 0 & 1\\
> \hline
> \end{array}
> \Rightarrow
> \begin{array}{|c c | ll |}
> \hline
> & 3  & 1 / 4 & 0 & 0 & -3 & -2 & 3 & 0 \\
> \hline
> x_{3} =  & 0 & 1 / 8 & 0 & 1 & -21 / 2 & -3 / 2 & 1 & 0 \\
> x_{2} =   & 0 & -3 / 64  & 1  & 0 & \color{red} 3 / 16^{*} & 1 / 16 & - 1 / 8 & 0 \\
> x_{7} =   & 1 & -1 / 8 & 0 & 0 & 21 / 2 & 3 / 2 & -1 & 1 \\
> \hline
> \end{array}
> $$
> 
> $$
> \begin{array}{|c c | ll |}
> \hline
> & 3  & -1 / 2 & 16 & 0  & 0 & -1 & 1 & 0 \\
> \hline
> x_{3} =  & 0 & -5 / 2 & 56 & 1 & 0 & \color{red}2^{*}  & -6 & 0\\
> x_{4} =   & 0 & -1 / 4 & 16 / 3 & 0 & 1 & 1 / 3 & -2 / 3 & 0 \\
> x_{7} =   & 1 & 5 / 2 & -56 & 0 & 0 & -2 & 6 & 1\\
> \hline
> \end{array}
> \Rightarrow
> \begin{array}{|c c | ll |}
> \hline
> & 3  & - 7 / 4 & 44 & 1 / 2 & 0 & 0 & -2 & 0\\
> \hline
> x_{5} =  & 0 & -5 / 4 & 28 & 1 / 2 & 0 & 1  & -3 & 0\\
> x_{4} =   & 0 & 1 / 6 & -4 & -1 / 6 & 1 & 0 & \color{red} 1 / 3^{*} & 0 \\
> x_{7} =   & 1 & 0 & 0 & 1 & 0 & 0 & 0 & 1\\
> \hline
> \end{array}
> $$
> $$
> \implies
> \begin{array}{|c c | ll |}
> \hline
> & 3  & -3 / 4 & 20  & -1 / 2 & 6 & 0 & 0 & 0 \\
> \hline
> x_{5} =  & 0 & \color{red} 1 / 4^{*}  & -8 & -1 & 9 & 1 & 0 & 0 \\
> x_{6} =   & 0 & 1 / 2 & -12 & -1 / 2 & 3  & 0 & 1 & 0 \\
> x_{7} =   & 1 & 0 & 0 & 1 & 0 & 0 & 0 & 1\\
> \hline
> \end{array}
> $$
> After six pivots we return to the initial basis, hence the same sequence of pivots will repeat forever.
> Note that cycling happens due to the presence of a degenerate basis that, for each pivot, imposes that we move of an amount $\theta^{*} = 0$ along the basic direction, therefore making us stay in the same basic feasible solution with the same cost.
> In [[LPP Simplex Method Anticycling Rules|another note]] we discuss some anticycling rules to avoid this undesirable behaviour of the algorithm.
## The Full Tableau Algorithm Complexity
The full tableau method requires to perform $m+1$ elementary row operations on a $(m+1)\times(n+1)$ matrix, which has cost $\mathcal{O}(mn)$. We conclude that, for each iteration, *the full tableau method has a time complexity of $\mathcal{O}(mn)$*. 
Similarly, the maintenance of the full tableau implies a *space complexity of $\mathcal{O}(mn)$*.

Here's a comparison between the complexity of the revised and the full tableau simplex method:
$$
\begin{array}{| c | c | c |}
\hline
& \text{Full Tableau} & \text{Revised Simplex Method} \\
\hline
\text{Best-case time} & \mathcal{O}(mn) & \mathcal{O}(m^{2}) \\
\hline
\text{Worst-case time} & \mathcal{O}(mn) & \mathcal{O}(mn) \\
\hline
\text{Memory} & \mathcal{O}(mn) & \mathcal{O}(m^{2}) \\
\hline
\end{array}
$$