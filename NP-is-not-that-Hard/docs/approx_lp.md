# Approximation Algorithms and Linear Programming

[Chinese](./approx_lp_cn.md) | [English](./approx_lp.md)

- [Approximation Algorithms and Linear Programming](#approximation-algorithms-and-linear-programming)
  - [Why Do We Need Approximation Algorithms?](#why-do-we-need-approximation-algorithms)
  - [Approximation Ratio](#approximation-ratio)
  - [Load Balancing Problem](#load-balancing-problem)
    - [Greedy Balancing](#greedy-balancing)
    - [Improved Greedy: LPT (1.5-Approximation)](#improved-greedy-lpt-15-approximation)
  - [2-Approximation Algorithm for Vertex Cover](#2-approximation-algorithm-for-vertex-cover)
  - [Why can't the approximation algorithm for VC be directly used for Independent Set?](#why-cant-the-approximation-algorithm-for-vc-be-directly-used-for-independent-set)
  - [Approximation Relationship between Set Cover and Vertex Cover](#approximation-relationship-between-set-cover-and-vertex-cover)
  - [Approximation Algorithm for Max-3SAT](#approximation-algorithm-for-max-3sat)
    - [ILP Formulation of Weighted Vertex Cover](#ilp-formulation-of-weighted-vertex-cover)
    - [LP Relaxation (Key Technique)](#lp-relaxation-key-technique)
    - [Constructing an Approximate Solution from the LP Solution](#constructing-an-approximate-solution-from-the-lp-solution)
  - [Relationship between LP, IP, and Complexity Classes](#relationship-between-lp-ip-and-complexity-classes)

## Why Do We Need Approximation Algorithms?

In the previous chapters, we have seen that:
- Many classic optimization problems (such as Vertex Cover, TSP, Independent Set) are NP-hard.
- This means that finding an exact optimal solution in a reasonable time is almost impossible.

Therefore, we take a step back and ask a practical question:

If I don't pursue the optimal solution, can I quickly find an "approximately" good solution?

This is where approximation algorithms come in. ## What is an Approximation Algorithm?

Definition (informal):

> An approximation algorithm is a **polynomial-time algorithm** that does not necessarily output the optimal solution, but guarantees an upper bound on the "gap" between its solution and the optimal solution.

We usually use the approximation ratio to measure the quality of the algorithm.

## Approximation Ratio

For a minimization problem:

$\text{Approximation Ratio} = \frac{\text{Algorithm Solution}}{\text{Optimal Solution}}$

If an algorithm guarantees:

$\text{Algorithm Solution} \le \rho \cdot \text{OPT}$

Then it is called a **ρ-approximation algorithm**.

## Load Balancing Problem

This was the first example used to introduce approximation algorithms in Lecture 13 of the 570 course.

Problem Description:
- There are $m$ machines with the same processing capacity.
- There are $n$ jobs, and the $i$-th job requires time $t_i$.
- Each job cannot be split.
- Goal: Minimize the maximum machine load.


### Greedy Balancing

**Algorithm:**
1. Process the jobs in the given order.
2. Assign the current job to the machine with the minimum current load.

This is a very simple greedy approach.

**Approximation Ratio Analysis (Core Idea)**

Let:
- $T^*$: The maximum load of the optimal solution
- $T$: The maximum load produced by the algorithm

**Conclusion:**
$T \le 2T^*$

Therefore, this greedy algorithm is a 2-approximation algorithm.

Intuitively:
- The last job placed on the "heaviest machine" has a size ≤ $T^*$
- Before placing it, the machine's load was ≤ $T^*$
- So the final load is ≤ $2T^*$

---

### Improved Greedy: LPT (1.5-Approximation)

Improved approach:

**Sort the jobs by processing time from largest to smallest, then use the same greedy strategy.**

This is the famous LPT (Longest Processing Time First). Conclusion

Lecture 13 provides the proof:

$T \le 1.5 \cdot T^*$

Therefore: This is a 1.5-approximation algorithm.

**This shows that simple sorting + greedy approach can significantly improve the approximation quality.**

## 2-Approximation Algorithm for Vertex Cover

Let's return to the familiar Vertex Cover problem.

Algorithm (very important)

```
S = ∅
while S is not a vertex cover:
Select an uncovered edge (u, v)
Add both u and v to S
```

This algorithm is a 2-approximation. Why?
- Each time we select an edge (u, v)
- The optimal solution must select at least one of u or v
- We select both

Therefore: $|S| \le 2 \cdot |OPT|$

This is a classic 2-approximation algorithm.

## Why can't the approximation algorithm for VC be directly used for Independent Set?

Although:

Independent Set ↔ Vertex Cover

Approximation does not preserve structure.

**Conclusion: Reduction ≠ Approximation Transitivity**

## Approximation Relationship between Set Cover and Vertex Cover

- Vertex Cover ≤ₚ Set Cover
- Approximation can be transferred from Set Cover back to Vertex Cover

Reason:
- Vertex Cover is a special case of Set Cover
- The size of the solution is exactly the same

**Conclusion: ρ-approx for Set Cover ⇒ ρ-approx for Vertex Cover**

## Approximation Algorithm for Max-3SAT

Problem: Given a set of clauses of length 3, maximize the number of satisfied clauses.

This is an extremely simple 1/2-Approximation

Algorithm:
1. Set all variables to TRUE
2. If ≥ 50% of clauses are satisfied, terminate
3. Otherwise, set all to FALSE

**Conclusion:** This is a 1/2-approximation

The course also mentioned that more complex methods can achieve an 8/9-approximation, but this will not be discussed in detail. ## Designing Approximation Algorithms using Linear Programming (LP)

### ILP Formulation of Weighted Vertex Cover

Define variables:

$
x_i =
\begin{cases}
1 & i \in S \\
0 & i \notin S
\end{cases}$

Objective function:

$\min \sum_i w_i x_i$

Constraints:

$x_i + x_j \ge 1 \quad \forall (i,j) \in E$

This is an Integer Linear Program (ILP).

### LP Relaxation (Key Technique)

Relax:

$x_i \in \{0,1\}$

to:

$x_i \in [0,1]$

This yields a Linear Program (LP), which can be solved in polynomial time.

### Constructing an Approximate Solution from the LP Solution

Define:

$S = \{ i \mid x_i \ge 1/2 \}$

Correctness:
- For any edge $(i,j)$, we must have $x_i + x_j \ge 1$
- It's impossible for both to be $< 1/2$
- Therefore, $S$ covers all edges

Approximation Ratio Analysis:

$w(S) \le 2 \cdot w(OPT)$

LP Relaxation + Rounding yields a 2-Approximation.

## Relationship between LP, IP, and Complexity Classes

The summary diagram in Lecture 13 is crucial:
- LP was once considered NP-intermediate
- In 1979, it was proven that LP can be solved in polynomial time
- Simplex algorithm has exponential worst-case complexity
- But in practice, it usually performs very well