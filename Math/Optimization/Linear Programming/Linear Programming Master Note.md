---
tags:
  - math/optimization/linear-programming
  - master-note
---
# Linear Programming
Notes on linear programming, mostly inspired by "*Introduction To Linear Optimization*" by D. Bertsimas and J.N Tsitsiklis.

1) ## Introduction
	1) [[LPP Definition]]: definition of the linear programming problem and of the general form
	2) [[LPP In Standard Form]]: definition of the standard form for linear programs
	3) [[Notable Classes Of Nonlinear LPP]]: modelling techniques to transform some nonlinear problems into linear programs
	4) [[LPP Examples]]: some concrete examples and applications of linear programming
	5) [[LPP Graphical Representation And Intuitions]]: some graphical representations of the feasible set of LPPs and intuitions on the optimal solutions and possible outcomes of a LPP
2) ## The Geometry Of Linear Programming
	1) [[LPP Polyhedra And Their Vertices]]: definition of polyhedra as the feasible set of LPPs. Equivalent definitions of corners of polyhedra as vertices, extreme points and basic feasible solutions.
	2) [[LPP Fundamental Theorem Of Linear Programming]]: existence and optimality of polyhedra vertices for LPPs and the fundamental theorem of linear programming
	3) [[LPP Polyhedra In Standard Form]]: construction of basic solutions for standard form polyhedra. Definition of basis, basic variables and basis matrix
	4) [[LPP Degeneracy]]: definition of degeneracy for basic solutions both in general and standard form
3) ## The Simplex Method
     1) [[LPP Simplex Method Optimality Conditions]]: definition of the $j$th basic direction and of the reduced cost. Optimality conditions for a basis
	 2) [[LPP The Simplex Method Framework]]: derivation and description of a general iteration of the simplex method. Proof that the simplex method terminates
	 3) [[LPP Simplex Method Implementations]]: derivation and description of the naive implementation of the simplex method, the revised simplex method, and the full tableau implementation
	 4) [[LPP Simplex Method Anticycling Rules]]: pivot selection rules that guarantee termination of the simplex method in the degenerate case
	 5) [[LPP Simplex Method Finding An Initial Solution]]: how to start the simplex method. The two-phase simplex method and the big-$M$ method
	 6) [[LPP Simplex Method Computational Complexity]]: The worst, average and practical computational complexity of the simplex method
	 7) [[LPP Simplex Method And The Column Geometry]]: the column geometry of the simplex method and insights on the  inner workings of the algorithm