# 复杂度类别

[中文](./the_definitions_cn.md) | [English](./the_definitions.md)

- [复杂度类别](#复杂度类别)
  - [验证](#验证)
  - [NP 的正式定义（但用直观方式表达）](#np-的正式定义但用直观方式表达)
  - [一些 NP 问题的例子](#一些-np-问题的例子)
    - [Independet Set Problem 独立集问题](#independet-set-problem-独立集问题)
    - [SAT Problem and 3-SAT Problem](#sat-problem-and-3-sat-problem)
    - [Clique Problem](#clique-problem)
    - [Vertex Cover](#vertex-cover)
    - [Hamiltonian Cycle（决策版本）](#hamiltonian-cycle决策版本)
  - [什么是 NP-hard (NP难)](#什么是-np-hard-np难)
  - [什么是 NP-complete (NP完全)](#什么是-np-complete-np完全)
  - [P vs NP：计算机科学最重要的问题](#p-vs-np计算机科学最重要的问题)
  - [如何证明一个问题属于NP](#如何证明一个问题属于np)
    - [例子一](#例子一)
    - [例子二](#例子二)



这一章，我们先去搞清楚这些名词的定义：NP、NP-hard、NP-complete、co-NP、P vs NP。

在我接触这些知识之前，我一直觉得这些词语很高级很深奥，今天就让我们来一起探索。

**重温：P问题是 能在poly-time复杂度内被解决的问题的集合。**

## 验证

**一个问题是否属于 NP 问题，我们并不关心这个问题是否难解，而是关心这个问题的验证！**

NP 的核心是：

**给你一个 candidate 解，你能否在多项式时间“验证”它是否正确。**

这个就是重点中的重点，我们并不关心你是否好解。


## NP 的正式定义（但用直观方式表达）

NP = Nondeterministic Polynomial-time。

> 正式定义：一个问题属于 NP，当且仅当
每个 YES 实例都有一个多项式长度的 certificate，并且该 certificate 可以被 poly-time 验证。

## 一些 NP 问题的例子

### Independet Set Problem 独立集问题

> Def. In a graph $G = (V, E)$, we say that a set of nodes $S \sube E$ is a independent set if no two nodes in $S$ are joind by an edge.

即：一个图G的点子集，两两之间没有边。

- 如果解决这个问题，是 NP 完全的（后续会深入讲解，现在我们先不关心）。
- 但是如果有人给你一个点集合 (candidate with k vertices), 我是可以在 poly-time 内验证这个集合是否为 IS。

所以 Independent Set Problem 属于 NP。

### SAT Problem and 3-SAT Problem

Clause (子句) 的定义: 
> Given n boolean variables $x_1, x_2, ..., x_n$, a clause is a disjunction of terms $t_1 \vee t_2 \vee ... \vee t_l$, where $t_i \in \{x_1, ..., x_n, \overline{x_1}, ..., \overline{x_n}\}$

其实就是一堆0或1取or的变量。

SAT Problem 的定义:
> Given clauses $C_1, ..., C_k$, if there is an assignment that can satisfied all the clauses: $C_1 \wedge C_2, ..., \wedge  C_k$.

其实问，能否找到一个赋值方法，让所有子句都满足。

显然，这个问题也可以在 poly-time 时间内完成验证。


**因此：NP = 所有能在多项式时间“验证 YES 实例”的问题集合。**

求解可能难，但验证必须快。

### Clique Problem

> Problem $\operatorname{CLIQUE}(G,k)$ asks whether, given a graph G, does G contain a k-clique? A k-clique is defined to be a set of k vertices such that each pair of these vertices shares an edge.

给一个图，能否找到k个节点，这k个节点是两两相连的。

- certificate = k个点的集合
- certifier = 检查所有点对是否相连 → O(k²)

### Vertex Cover

在一个图里，能否找到一系列点，覆盖所有的边。一条边被覆盖：即两端其中有一个点属于 Vertex Cover 中。

- certificate = 选定顶点集合
- certifier = 检查是否覆盖所有边 → O(m)

### Hamiltonian Cycle（决策版本）

在图G种找一条环路径，遍历每一个点一次。

- certificate = 一个排列（路径）
- certifier = 检查是否访问每个点一次且形成环 → O(n)

## 什么是 NP-hard (NP难)

NP-hard = “至少和 NP 中最难的问题一样难”。

更正式地：

> NP-hard 问题是所有 NP 问题都可以 poly-time 规约到它的那些问题。


- NP-hard 不要求在 NP 里（因为它可能无法验证）
- 最著名的 NP-hard 非 NP 问题是： 停机问题（Halting Problem） → 不可验证，不可求解

> The Halting Problem is one of the most fundamental results in theoretical computer science. It asks: \
> Given a program P and an input x, will P(x) eventually halt, or will it run forever? \
> Alan Turing proved in 1936 that there is no algorithm that can always answer this question correctly for all possible programs and inputs.

因此：

NP-complete $\sube$ NP-hard

NP-hard 是一个更大的集合。

关于如何证明一个问题是否属于NP的完整严谨证明过程，我放到本章末尾: [如何证明一个问题属于NP](#如何证明一个问题属于np)


## 什么是 NP-complete (NP完全)

NP-complete = NP $\cap$ NP-hard。

换句话说：

一个问题 X 是 NP-complete，当且仅当：
1.	X ∈ NP
2.	NP 中所有问题都可以 poly-time 规约到 X

这些问题是 NP 中最难的“代表性问题”。

常见 NP-complete 的问题：
1. SAT
2. 3SAT
3. CLIQUE
4. Independent Set
5. Vertex Cover
6. Hamiltonian Cycle
7. TSP（decision）

它们之间可以互相规约，因此本质上难度相同。

![](../assets/np.png)

## P vs NP：计算机科学最重要的问题

通过上面的内容，可以知道：
- P = 可以 poly-time 求解
- NP = 可以 poly-time 验证

显然：

$P \sube NP$

但是否：

P = NP ?

目前未知。

> [!important]
> 目前结论：P 是否等于 NP 是未知的！

但大多数计算机科学家都相信：

P ≠ NP

如果 P = NP，会发生什么？
- 所有 NP-complete 问题都能 poly-time 求解
- 密码学（RSA/ECC）直接崩溃
- 机器学习和优化会出现飞跃
- 搜索 / 调度 / TSP 等问题都能瞬间求解

但目前，这个问题仍然未决，是千禧年七大难题之一。

## 如何证明一个问题属于NP

- 第一步：列出 candidate: 即候选人集合（需要验证的东西的集合，通过例子就很容易明白了）
- 第二步：证明验证过程是 poly-time 完成的。

### 例子一

The Sushi Express wants to implement a new feature, "order batching." The menu consists of a set of disired food items $F$ and set of combos $c_1, ..., c_n$ where each combo is a set of food items along with a price - we represent combo $c_i = (\{f_i^1, f_i^2, ..., f_i^k\}, p_i)$, where each $f_i^j$ is a food item in the combo. If a student orders $c_i$, they receive each of the food items, and they pay the cost $p_i$. In the new order batching system, the students would input a list of food items that they want, and the order batching system should find the cheapest set of combos, which would include at least one of each desired food item. Write the decision version of this problem, and show via reduction that this decision problem is NP Hard.

这是一道我的作业题，当然，这里先不去证明 HP-Hard, 我们先去证明问题的Decision Version属于 NP。

**Define the Decision Version**

We define the decision version of the Sushi Batching problem as follows:

**Input:**

- A set of food items $F$
- A collection of combos $C_1, ..., C_n$, where $C_i = (S_i, p_i), S_i \subseteq F$.
- A set of required items $R \subseteq F$
- A budget $B\in\mathbb{Z}^+$.

**Question:** 

Does there exists a subset of combos $I \subseteq \{1, ...,n\}$ such that:

1. $\displaystyle \bigcup_{i\in I} S_i \supseteq R$ (Unite to cover all the food you want), and
2. $\displaystyle \sum_{i\in I} p_i \le B$

**Showing the Problem is in NP**

A certificate can be a subset of combo indices $I \sube \{1,...,n\}$. The certifier checks the following in polynomial time: 

1. Compute $\sum_{i\in I}p_i$ and verify it is $\le B$
2. Compute the union $\bigcup_{i\in I} S_i$ and verify that it contains every element of R.
3. Verify that I has no duplicate indices.

These checks require only scanning sets and summing numbers, all of which take polynomial time. **Thus the problem has an efficient certification and is in NP.**

### 例子二

Problem $\operatorname{CLIQUE}(G,k)$ asks whether, given a graph G, does G contain a k-clique? A k-clique is defined to be a set of k vertices such that each pair of these vertices shares an edge. Show via reduction that CLIQUE is NP-complete.

同样，先证明问题是属于 NP 的。

**Showing the Problem is in NP**

**Certificate:**

A certificate can be a subset of vertices $S \sube V$ with $|S|=k$.

**Certifier:**

Given $(G,k)$ and the certificate $S$, the certifier:

1. Checks that $|S|=k$
2. For every pair $(u,v)$ in $S$, checks whether $(u,v)\in E$

There are at most $k / 2$ pairs, so this verification takes polynomial time in the size of the input graph. Thus CLIQUE has an efficient certifiction and is in NP. 

看到这里应该证明的套路已经没问题了，其实证明一个问题属于 NP 是非常简单的，就直接去说这个问题可以在 poly-time 内认证即可。