# Complexity Categories

[Chinese](./the_definitions_cn.md) | [English](./the_definitions.md)

- [Complexity Categories](#complexity-categories)
  - [Verification](#verification)
  - [Formal Definition of NP (but expressed intuitively)](#formal-definition-of-np-but-expressed-intuitively)
  - [Examples of Some NP Problems](#examples-of-some-np-problems)
    - [Independet Set Problem](#independet-set-problem)
    - [SAT Problem and 3-SAT Problem](#sat-problem-and-3-sat-problem)
    - [Clique Problem](#clique-problem)
    - [Vertex Cover](#vertex-cover)
    - [Hamiltonian Cycle (Decision Version)](#hamiltonian-cycle-decision-version)
  - [What is NP-hard?](#what-is-np-hard)
  - [What is NP-complete?](#what-is-np-complete)
  - [P vs NP: The Most Important Problem in Computer Science](#p-vs-np-the-most-important-problem-in-computer-science)
  - [How to prove a problem is NP](#how-to-prove-a-problem-is-np)
    - [Example 1](#example-1)
    - [Example 2](#example-2)

In this chapter, we'll first clarify the definitions of these terms: NP, NP-hard, NP-complete, co-NP, P vs NP.

Before encountering this knowledge, I always thought these terms were very advanced and profound. Today, let's explore them together.

**Review:** P problems are the set of problems that can be solved in poly-time complexity.

## Verification

**Whether a problem belongs to NP is not about whether the problem is difficult to solve, but about verifying it!** **
**The core of NP is:**

**Given a candidate solution, can you "verify" its correctness in polynomial time?**

This is the key point; we don't care whether it's easy to solve.

## Formal Definition of NP (but expressed intuitively)

NP = Nondeterministic Polynomial-time.

> Formal definition: A problem is NP if and only if

Every YES instance has a polynomial-length certificate, and this certificate can be verified in poly-time.

## Examples of Some NP Problems

### Independet Set Problem

> Def. In a graph $G = (V, E)$, we say that a set of nodes $S \sube E$ is an independent set if no two nodes in $S$ are joined by an edge.

That is: a subset of nodes in a graph G where no two nodes are joined by an edge.

- Solving this problem is NP-complete (we will discuss this in more detail later; we won't concern ourselves with that now).

- However, if someone gives you a set of points (candidates with k vertices), I can verify whether this set is IS in polytime.

Therefore, the Independent Set Problem is NP-complete.

### SAT Problem and 3-SAT Problem

Definition of a Clause:

> Given n boolean variables $x_1, x_2, ..., x_n$, a clause is a disjunction of terms $t_1 \vee t_2 \vee ... \vee t_l$, where $t_i \in \{x_1, ..., x_n, \overline{x_1}, ..., \overline{x_n}\}$

Essentially, it's just a bunch of variables that are ORed with 0 or 1.

The SAT Problem is defined as follows:

Given clauses $C_1, ..., C_k$, if there is an assignment that can satisfy all the clauses: $C_1 \wedge C_2, ..., \wedge C_k$.

In essence, it asks whether it's possible to find an assignment that satisfies all clauses.

Clearly, this problem can also be verified in poly-time.

**Therefore: NP = the set of all problems that can "verify YES instances" in polynomial time.**

Solving may be difficult, but verification must be fast.

### Clique Problem

Problem $\operatorname{CLIQUE}(G,k)$ asks whether, given a graph G, does G contain a k-clique? A k-clique is defined to be a set of k vertices such that each pair of these vertices shares an edge.

Given a graph, can we find k nodes that are connected in pairs?

- certificate = a set of k vertices

- certifier = check if all pairs of vertices are connected → O(k²)

### Vertex Cover

In a graph, can we find a set of vertices that covers all edges? An edge is covered if one of its two ends belongs to the Vertex Cover.

- certificate = selected set of vertices

- certifier = check if all edges are covered → O(m)

### Hamiltonian Cycle (Decision Version)

Find a cyclic path in graph G, traversing each vertex once.

- certificate = a permutation (path)

- certifier = check if each vertex is visited once and a cycle is formed → O(n)

## What is NP-hard?

NP-hard = "At least as hard as the hardest problem in NP".

More formally:

> NP-hard problems are those problems to which all NP problems can be poly-time reduced.

- NP-hard does not require being in NP (because it may be unverifiable).

- The most famous NP-hard non-NP problem is: The Halting Problem → Unverifiable, unsolvable.

> The Halting Problem is one of the most fundamental results in theoretical computer science. It asks:

> Given a program P and an input x, will P(x) eventually halt, or will it run forever?

> Alan Turing proved in 1936 that there is no algorithm that can always answer this question correctly for all possible programs and inputs.

Therefore:

NP-complete $\sube$ NP-hard

NP-hard is a larger set.

The complete and rigorous proof of whether a problem belongs to NP is at the end of this chapter: [How to Prove a Problem Belongs to NP](#How to Prove a Problem Belongs to np)

## What is NP-complete?

NP-complete = NP $\cap$ NP-hard.

In other words:

A problem X is NP-complete if and only if:

1. X ∈ NP

2. All problems in NP can be poly-time reduced to X

These problems are the most difficult "representative problems" in NP.

Common NP-complete problems:

1. SAT

2. 3SAT

3. CLIQUE

4. Independent Set

5. Vertex Cover

6. Hamiltonian Cycle

7. TSP (decision)

They are mutually reducible, therefore essentially of equal difficulty.

![](../assets/np.png)

## P vs NP: The Most Important Problem in Computer Science

From the above, we know:

- P = can be poly-time solved

- NP = can be poly-time verified

Obviously:

$P \sube NP$

But is:

P = NP?

Currently unknown.

[!important]

Current conclusion: Whether P equals NP is unknown!

However, most computer scientists believe:

P ≠ NP

What would happen if P = NP?

- All NP-complete problems could be solved polytime.

- Cryptography (RSA/ECC) would collapse.

- Machine learning and optimization would experience a leap forward.

Search/scheduling/TSP problems could be solved instantaneously.

But currently, this problem remains unsolved and is one of the Seven Millennium Problems.

## How to prove a problem is NP

- Step 1: List the candidates: i.e., the set of candidates (the set of things to be verified, easily understood through examples).

- Step 2: Prove that the verification process is completed polytime.

### Example 1

The Sushi Express wants to implement a new feature, "order batching." The menu consists of a set of disired food items $F$ and set of combos $c_1, ..., c_n$ where each combo is a set of food items along with a price - we represent combo $c_i = (\{f_i^1, f_i^2, ..., f_i^k\}, p_i)$, where each $f_i^j$ is a food item in the combo. If a student orders $c_i$, they receive each of the food items, and they pay the cost $p_i$. In the new order batching system, the students would input a list of food items that they want, and the order batching system should find the cheapest set of combos, which would include at least one of each desired food item. Write the decision version of this problem, and show via reduction that this decision problem is NP Hard.

This is a homework question for me. Of course, I won’t prove it here. HP-Hard, let's first prove that the Decision Version of the problem is NP.

**Define the Decision Version**

We define the decision version of the Sushi Batching problem as follows:

**Input:**

- A set of food items $F$

- A collection of combos $C_1, ..., C_n$, where $C_i = (S_i, p_i), S_i \subseteq F$.

- A set of required items $R \subseteq F$
- A budget $B\in\mathbb{Z}^+$.

**Question:**

Does there exist a subset of combos $I \subseteq \{1, ...,n\}$ such that:

1. $\displaystyle \bigcup_{i\in I} S_i \supseteq R$ (Unite to cover all the food you want), and
2. $\displaystyle \sum_{i\in I} p_i \le B$

**Showing the Problem is in NP**

A certificate can be a subset of combo indices $I \sube \{1,...,n\}$. The certifier checks the following in polynomial time:

1. Compute $\sum_{i\in I}p_i$ and verify it is $\le B$
2. Compute the union $\bigcup_{i\in I} S_i$ and verify that it contains every element of R.
3. Verify that I have no duplicate indices.

These checks require only scanning sets and summing numbers, all of which take polynomial time. **Thus the problem has an efficient certification and is in NP.**

### Example 2

Problem $\operatorname{CLIQUE}(G,k)$ asks whether, given a graph G, does G contain a k-clique? A k-clique is defined to be a set of k vertices such that each pair of these vertices shares an edge. Show via reduction that CLIQUE is NP-complete.

Again, first prove that the problem is NP.

**Showing the Problem is NP**

**Certificate:**

A certificate can be a subset of vertices $S \sube V$ with $|S|=k$.

**Certifier:**

Given $(G,k)$ and the certificate $S$, the certifier is:

1. Checks that $|S|=k$

2. For every pair $(u,v)$ in $S$, checks whether $(u,v)\in E$

There are at most $k / 2$ pairs, so this verification takes polynomial time in the size of the input graph. Thus, CLIQUE has an efficient certificate and is NP.

The proof should be straightforward by now. Proving a problem is NP is very simple; just state that the problem can be verified in poly-time.