---
tags:
  - "#math/optimization/convex-optimization"
  - "#math/convex"
---

> [!theorem]
> Let us consider a convex optimization problem:
> $$
> \min_{x \in P} f(x)
> $$
> where $f : P \to \mathbb{R}$ is a convex function and $P \subseteq \mathbb{R}^n$ is a convex set. If $x^{*}$ is a local optimum, then $x^{*}$ is a global optimum as well.

^727d82

> [!proof]+
> Let $x^{*}$ be a local optimum, then by definition there exists a ball $B_{x^{*}}(\epsilon)$ centred in $x^{*}$ such that for any $y \in P \cap B_{x^{*}}(\epsilon)$ we have $f(x^{*}) \leq f(y)$. For any other point $z \in P$ we have that the line $[x,z] \subseteq P$ intersects $B_{x^{*}}(\epsilon)$ at a point $y = \lambda x^{*} + (1-\lambda)z$. By convexity of $f$ we also have:
> $$
> f(x^{*}) \leq f(y) \leq \lambda f(x^{*}) + (1-\lambda)f(z)
> \implies
> (1-\lambda)f(x^{*}) \leq (1-\lambda)f(z)
> \implies
> f(x^{*}) \leq f(z)
> $$
> so we can conclude that $x^{*}$ is also a global optimum.
> `\end{proof}`