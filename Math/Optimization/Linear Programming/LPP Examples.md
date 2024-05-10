---
tags:
  - math/optimization/linear-programming
  - math/example
  - math/modelling
next: "[[LPP Graphical Representation And Intuitions]]"
prev: "[[Notable Classes Of Nonlinear LPP]]"
up: "[[Linear Programming Master Note]]"
abstract: some concrete examples and applications of linear programming
---
# Examples Of Linear Programming Problems
## Production Problem
A firm produces $n$ different goods using $m$ different materials. Let $b_i$ be the available amount of the $i$-th material. The $j$-th good requires $a_{i,j}$ units of the $i-th$ material and results in a revenue of $c_{j}$ per unit produced.
The firm needs to decide how many units $x_j$ of each good to produce in order to maximize its total revenue. The problem can be written as the following linear program:

$$
\begin{align}
 \max_{x \in \mathbb{R}^n} \quad & c_{1}x_{1} + \dots + c_{n}x_{n} \\
 \text{s.t.} \quad & a_{i,1}x_{1} + \dots + a_{i,n} \leq b_{i} \quad & i=1,\dots,m \\
& x_{j} \geq 0 \quad & j = 1,\dots, n
\end{align}
$$
## Optimal Control Of Linear Systems
Consider the following dynamical system:
$$
\begin{cases}
x_{t+1} = A x_{t} + B u_{t} \\
y_{t} = c^{T}x_{t}
\end{cases}
$$
Where:
- $x_{t} \in \mathbb{R}^{d}$ is the state of the system at time $t$;
- $y_{t} \in \mathbb{R}$ is the system output;
- $u_{t} \in \mathbb{R}^{n}$ is a control vector that we are free to choose, subject to linear constraints of the form $Du_{t} \leq d$.

A dynamical system like this can be used to describe a model of an engine, an electrical circuit or even a model of economic growth. There are many optimal control problems that we can define over such a system.
In one possible problem we want to choose the values of the control variables $u_{0},\dots,u_{T-1}$ to drive the final state $x_{T}$ to a target state $\bar{x}$. In addition, we want to keep the magnitude of the system output as small as possible during the system evolution, hence we wish to minimize:
$$
\max_{t = 1,\dots, T-1} |y_{t}|
$$
This can be formulated as a linear programming problem:
$$
\begin{align} 
 \min_{u,z} \quad & z  \\ 
\text{s.t.} \quad & -z \leq y_{y} \leq z & t = 1,\dots,T-1 \\
& x_{t+1} = Ax_{t} + B u_{t} & t = 0,\dots,T-1 \\
& y_{t} = c^{T}x_{t} & t = 1,\dots, T-1 \\
& D u_{t} \leq d & t = 0,\dots,T-1 \\
& x_{T} = \bar{x} \\
\end{align}
$$

More complex variants of this problem can incorporate [[Notable Classes Of Nonlinear LPP#^jz7eJmiZ|piecewise linear convex cost function]] of the state and of the control, or additional linear constraints on the state vector $x_{t}$.

### Example: Rocket Control
Let $x_{t},v_{t}$ and $a_{t}$ be the position, velocity, and acceleration of a rocket at time $t$. A discrete-time approximation of the rocket movement can be modelled by the linear system:
$$
\begin{cases}
x_{t+1} = x_{t} + v_{t} \\
v_{t+1} = v_{t} + a_{t}
\end{cases}
$$
In this case, the acceleration $a_{t}$ is under our control. We assume that the fuel consumption is proportional to the magnitude of the acceleration $|a_{t}|$.
We want to reach at time $T$ our destination $\bar{x}$ by minimizing the total fuel spent $\sum_{t} |a_{t}|$. At take off and at landing, the velocity of the rocket needs to be low, so we impose $v_{0} = v_{T} = 0$.
This problem can be expressed as a LPP:
$$
\begin{align} 
 \min_{a,z} \quad & \sum_{i = 0}^{T-1} z_{t} \\ 
\text{s.t.} \quad & -a_{i} \leq z_{i} \leq a_{i} & t = 0,\dots,T-1 \\
& x_{t+1} = x_{t} + v_{t} & t = 0,\dots, T-1 \\
& v_{t+1} = v_{t} + a_{t} & t = 0,\dots, T-1 \\
& x_{T} = \bar{x} \\
& v_{T} = 0
 \end{align}
$$

## Scheduling Problem
A hospital wants to make a weekly night shift schedule for its nurses. Each night $j = 1,\dots,7$ of the week, the hospital needs $d_{j}$ nurses. Every nurse works $5$ days in a row on the night shift. The problem is to find the minimal number of nurses the hospital needs to hire.
We denote with $x_{j}$ the number of nurses that start working their week on day $j$. We then have the following problem:
$$
\begin{array}{}
 \min_{x \in \mathbb{N}^{7}} \quad &  x_{1} & +x_{2}& +x_{3}& +x_{4}& +x_{5}& +x_{6} & +x_{7} \\ 
\text{s.t.} \quad & x_{1} & & & +x_{4} & +x_{5} & +x_{6} & +x_{7}  & \geq d_{1}  \\
& x_{1} & +x_{2} &  &  & +x_{5} & +x_{6}  & +x_{7} & \geq d_{2} \\
& x_{1} & +x_{2} & +x_{3} &  & & +x_{6}  & +x_{7} & \geq d_{3} \\
& x_{1} & +x_{2} & +x_{3} & +x_{4}  & &  & +x_{7} & \geq d_{4} \\
& x_{1} & +x_{2} & +x_{3} & +x_{4}  &+x_{5} &  &  & \geq d_{5} \\
& & +x_{2} & +x_{3} & +x_{4}  &+x_{5} & +x_{6} &  & \geq d_{6} \\
& & & +x_{3} & +x_{4}  &+x_{5} & +x_{6} & +x_{7} & \geq d_{7} \\
 \end{array}
$$
This would be a linear programming problem if it weren't for the fact that $x_{j} \in \mathbb{N}$ must be an integer. This is known as an *integer linear programming problem*. In general, integer programming problems are quite difficult to solve, however, we can ignore the integrality constraints by allowing $x \in \mathbb{R}^{7}$.  Thus, we obtain an approximated problem, also known as the *linear programming relaxation* of the original formulation, which can sometimes provide a good enough approximated solution to the original problem by taking the fractional part $\lceil x \rceil$ of $x$.

## Optimizing Flow Over Networks And Transportation Problems
Consider a communication network of $n$ nodes connected by directional links $(i,j) \in \mathcal{A}$, each able to carry up to $u_{i,j}$ bits per second. There is a cost $c_{i,j}$ per bit transmitted along every link $(i,j)$.
Each node $k$ generates data at the rate of $b^{k \ell}$ per second, that needs to be transmitted to node $\ell$.
The problem is to choose paths along which all the data reach their intended destinations, while minimizing the total cost.
By introducing some additional variables,  we can formulate this problem as a linear programming problem. Indeed, let $x_{i,j}^{k,\ell}$ indicate the amount of data with origin $k$ and destination $\ell$ that traverse the link $(i,j)$, and let:
$$
b_{i}^{k,\ell} = \begin{cases}
b^{k,\ell} & \text{if} \; i = k \\
-b^{k,\ell} & \text{if} \; i = \ell \\
0 & \text{otherwise}  \\
\end{cases}
$$
$b_{i}^{k,\ell}$ denotes the amount of data that goes from $k$ to $\ell$ generated by the node $i$. It is zero if $i$ is unrelated to $k$ and $\ell$, negative if the node $i = \ell$, because $\ell$ is the node requiring the data. In short, $b_{i}^{k,\ell}$ is the net inflow at node $i$, from outside the network, of data with origin $k$ and destination $\ell$.
We can formulate the network problem as follows:

$$
\begin{align} 
 \min_{x} \quad & \sum_{(i,j) \in \mathcal{A}}\sum_{k = 1}^{n}\sum_{\ell = 1}^{n} c_{i,j}x_{i,j}^{k,\ell}  \\ 
\text{s.t.} \quad &  \sum_{\{ j \; | (i,j)\in \mathcal{A} \}}x_{i,j}^{k,\ell} - \sum_{\{ j \; | \; (j,i)\in \mathcal{A} \}}x_{j,i}^{k,\ell} = b_{i}^{k,\ell} & i,k,\ell = 1,\dots,n \\
& \sum_{k=1}^{n}\sum_{\ell = 1}^{n}x_{i,j}^{k,\ell} \leq u_{i,j} & (i,j) \in \mathcal{A} \\
& x_{i,j}^{k,\ell} \geq 0 & (i,j) \in \mathcal{A}, \; k,\ell = 1,\dots,n
 \end{align}
$$

Let's break down this formulation:
1) The objective function is simply stating that we need to minimize the total cost of bits transmitted along the network;
2) The first constraint is a *flow conservation constraint*: the amount of data that leaves a node $i$ in order to transport information from $k$ to $\ell$, i.e.:
		$$
		\sum_{\{j \; | \; (i,j) \in \mathcal{A}\}} x_{i,j}^{k, \ell}
		$$
 must be equal to the amount of data that node has generated for that transmission , i.e. $b_{i}^{k,\ell}$, plus the data that has entered in the node $i$ to go from $k$ to $\ell$, i.e.:
    $$
   \sum_{\{ j \; | (j,i) \in \mathcal{A} \}}x_{j,i}^{k,\ell}
   $$
3) The second constraint is ensuring that we don't exceed the total transmission capacity $u_{i,j}$ for each link $(i,j)$;
4) The third constraint is simply enforcing the fact that we don't transmit a nonsensical amount of negative data over the network.

This problem is known as the *multicommodity flow problem*. A similar problem arises when we consider a transportation company that wishes to transport several commodities from their origins to their destination through a network.
Another variant of this problem exists, known as the *minimum cost network flow problem*, in which we don't distinguish between different commodities. Instead, we are given an amount $b_{i}$ of external supply or demand at each node $i$, with the objective of transporting material from the supply nodes to the demand nodes minimizing costs.
The minimum cost problem generally describes also other more specific problems, such as the shortest path problem, the maximum flow problem, and the assignment problem.