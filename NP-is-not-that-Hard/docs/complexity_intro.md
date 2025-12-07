# Introduction to Time Complexity – What Are P Problems?

[中文](./complexity_intro_cn.md) | [English](./complexity_intro.md)

- [Introduction to Time Complexity – What Are P Problems?](#introduction-to-time-complexity--what-are-p-problems)
  - [Basic Concepts in Computational Complexity](#basic-concepts-in-computational-complexity)
    - [Why do we emphasize poly-time?](#why-do-we-emphasize-poly-time)
  - [P Problems](#p-problems)
  - [Pseudo-Polynomial Time](#pseudo-polynomial-time)
  - [Weakly Polynomial Time](#weakly-polynomial-time)
  - [Strongly Polynomial Time](#strongly-polynomial-time)
  - [Final Comparison Table](#final-comparison-table)


## Basic Concepts in Computational Complexity

Before we dive into NP, NP-completeness, and polynomial reductions, we first need a shared language:

**What is “algorithmic complexity”? What is “polynomial time”? Why are some algorithms fast but *not* poly-time?**

We begin with three foundational complexity notions:

- **Polynomial Time**
- **Pseudo-Polynomial Time**
- **Weakly Polynomial Time**

These concepts will appear repeatedly later.

> [!note]
> In later chapters, “polynomial time” may be abbreviated as **poly-time**, pseudo-polynomial as **pseudo-poly-time**, and weakly polynomial as **weakly-poly-time**.  
> These abbreviations are only for convenience and are not formal terminology.

---

### Why do we emphasize poly-time?

In theoretical CS, **poly-time** is universally considered the threshold of efficient computation.

Because:

1. The growth of polynomial-time algorithms (e.g., $ n $, $ n \\log n $, $ n^2 $, $ n^3 $) is **controlled**.
2. Changing computers, languages, and constant factors will not change its *complexity class*.
3. Poly-time problems are **scalable** in real engineering.

Examples:

| Time Complexity | Practical? | Notes |
|----------------|------------|--------|
| $O(n)$ | ✔ | very fast, linear time |
| $O(n \log n)$ | ✔ | sorting-level fast |
| $O(n^2)$ | ✔ | acceptable |
| $O(n^7)$ | ✔ | still poly-time |
| $O(2^n)$ | ✘ | exponential blow-up |
| $O(n\!)$ | ✘ | catastrophic |

Thus, **P problems (Polynomial-Time Problems)** refer to problems solvable within practical running time.

---

## P Problems

A problem belongs to **P** if there exists an algorithm whose running time is:

$
O(n^k)
$

for some constant $k$.

> [!important]
> Note: **the *problem* is in P**, not the algorithm.  
> A problem is in P *because* it has some poly-time algorithm.


**Classic poly-time algorithm examples**


**1. Dijkstra Shortest Path (Non-negative weights)**

- Code: [Graph.hpp](https://github.com/ffengc/CSCI570-Analysis-of-Algorithms-USC/blob/main/Lecture2-BFS-DFS-Graph/Graph.hpp)
- Running time:  
  - $O(m \log n)$ or  
  - $O(n^2)$
- Therefore, **Shortest Path** is a classic P problem.

---

**2. Maximum Flow**

- Notes: [Network Flow](https://github.com/ffengc/CSCI570-Analysis-of-Algorithms-USC/blob/main/Lecture9-Network-Flow/Note.md)

| **Algorithm**                           | **Time**                    | **Type**              |
|------------------------------------------|-----------------------------|------------------------|
| **Ford–Fulkerson**                       | $O(C m)$                  | pseudo-polynomial      |
| **Capacity Scaling**                     | $O(m^2 \log C)$          | weakly polynomial      |
| **Edmonds–Karp**                         | $O(n m^2)$                | strongly polynomial    |
| **Orlin + KRT (improved)**               | $O(n m)$                  | strongly polynomial    |
| **Modern Approximation Methods**         | $\approx O(m \log n + m^{4/3})$ | almost linear |

Thus, **Max Flow** is also a P problem.

---

**3. Minimum Spanning Tree (MST)**

- Code: [Graph.hpp](https://github.com/ffengc/CSCI570-Analysis-of-Algorithms-USC/blob/main/Lecture2-BFS-DFS-Graph/Graph.hpp)
- Kruskal / Prim run in: $O(m \log n)$

Hence **MST** is also a P problem.

These algorithms form the foundation of tractable computation.

---

## Pseudo-Polynomial Time

**One-sentence definition:**

> A pseudo-polynomial algorithm depends on the *numerical value* $V$ of the input, not its *bit-length* $\log V$.

The best example comes from dynamic programming.

---

**Classic Example: 0/1 Knapsack DP**

(570 course notes: [DP Part 1](https://github.com/ffengc/CSCI570-Analysis-of-Algorithms-USC/tree/main/Lecture7-DP-part1))  
(Carl’s tutorial: [01 Knapsack](https://programmercarl.com/背包理论基础01背包-1.html))

**DP complexity:**

$
O(nW)
$

Where:

- $n$ = number of items  
- $W$ = knapsack capacity (**numeric value**)

---

**Why is it *not* polynomial?**

Because $W = 10^9$ only needs 30 bits to represent—but the DP must run *one billion steps*.

Thus it is not poly-time.

---

**A more intuitive explanation**

You asked:

> Different Paths DP is poly-time, but Knapsack DP is not. Why?

My explanation:

- For grid DP (e.g., “Unique Paths”), the size of the DP table grows with the **input size** (the grid).  
- For Knapsack, the DP table height grows with the **numeric value W**, not with input size.  
  A single digit change in W (e.g., W+1) adds another entire DP row.

This disconnect between “input length” and “DP table size” makes it pseudo-poly.

---

**Example table**

| W | bit-length | DP Time | Feasible? |
|---|------------|----------|-----------|
| 1000 | 10 bits | $O(n \cdot 1000)$ | OK |
| $10^9$ | 30 bits | $O(n \cdot 10^9)$ | Not OK |
| $2^{100}$ | 100 bits | $O(n \cdot 2^{100})$ | Impossible |

Pseudo-poly algorithms frequently appear in NP-hard optimization:

- Knapsack  
- Subset Sum  

We'll revisit this when discussing NP-hardness.

---

## Weakly Polynomial Time

A weakly polynomial algorithm lies between true polynomial and pseudo-polynomial.

**One-sentence definition:**

> Weakly polynomial time = polynomial in $n$ and bit-length $\\log V$, but not in $V$ itself.

Thus:

- weak-poly ⇒ in **P**
- pseudo-poly ⇒ not necessarily in P

---

Classic example: **Linear Programming (LP)**

LP solvers (ellipsoid / interior-point) run in: $\operatorname{poly}(n, B)$


Where:

- $n$ = number of variables/constraints  
- $B$ = bit-length of input coefficients  

This is truly polynomial-time and far better than pseudo-poly.

---

## Strongly Polynomial Time

Definition:

$\operatorname{poly}(n)$

Only depends on structural input size.

Examples:

- BFS / DFS  
- Prim / Kruskal  
- Hopcroft–Karp matching  
- Some max-flow algorithms (e.g., Orlin)

Strongly poly-time algorithms are the most robust.

---

## Final Comparison Table

| **Type**                  | **Depends On**       | **Example**       | **In P?**                       |
|---------------------------|-----------------------|--------------------|----------------------------------|
| **Strongly Polynomial**   | $\operatorname{poly}(n)$    | MST, Matching      | ✔ Yes                            |
| **Weakly Polynomial**     | $\operatorname{poly}(n\, \log V)$ | Linear Programming | ✔ Yes                            |
| **Pseudo-Polynomial**     | $\operatorname{poly}(n\, V)$ | Knapsack DP        | ✘ No (unless numbers are small)  |

> [!tip]
> Pseudo-polynomial algorithms may *look* fast, but because their dependence is on $V$ rather than $\log V$, they do not qualify as poly-time in the formal sense.

---