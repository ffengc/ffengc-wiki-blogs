# 旅行商问题与汉密尔顿环

[中文](./tsp_hc_cn.md) | [English](./tsp_hc.md)

这一章引入两个在计算复杂性理论中极其经典的问题：
- Hamiltonian Cycle（汉密尔顿环，HC）
- Traveling Salesman Problem（旅行商问题，TSP）

它们有三个重要意义：
1. 都是NP-Complete / NP-Hard 的代表问题
2.	都非常“直观”，但本质上极难
3.	是后续近似算法（Approximation Algorithms）的理论基础和起点。

- [旅行商问题与汉密尔顿环](#旅行商问题与汉密尔顿环)
  - [Hamiltonian Cycle（汉密尔顿环）](#hamiltonian-cycle汉密尔顿环)
    - [问题定义](#问题定义)
    - [HC问题的判定版本（Decision Version）](#hc问题的判定版本decision-version)
    - [证明 Hamiltonian Cycle ∈ NP](#证明-hamiltonian-cycle--np)
    - [Hamiltonian Cycle 是 NP-Complete](#hamiltonian-cycle-是-np-complete)
  - [Traveling Salesman Problem（TSP）](#traveling-salesman-problemtsp)
    - [问题定义（优化版 vs 判定版）](#问题定义优化版-vs-判定版)
    - [证明 TSP ∈ NP](#证明-tsp--np)
    - [TSP 是 NP-Complete](#tsp-是-np-complete)
  - [Metric TSP 与近似算法](#metric-tsp-与近似算法)
    - [Triangle Inequality](#triangle-inequality)
    - [基于 MST 的 2-Approximation](#基于-mst-的-2-approximation)
  - [为什么 General TSP 没有常数近似](#为什么-general-tsp-没有常数近似)


## Hamiltonian Cycle（汉密尔顿环）

### 问题定义

给定一个无向图 $G = (V, E)$。Hamiltonian Cycle（HC） 指的是：一个简单环，恰好访问每一个顶点一次，并最终回到起点。

### HC问题的判定版本（Decision Version）

我们关注的是判定问题：Hamiltonian Cycle Problem

**给定图 G，是否存在一个 Hamiltonian Cycle？**

**这是一个标准的 YES / NO 问题，为后续 NP 完全性讨论做准备。**

### 证明 Hamiltonian Cycle ∈ NP

这个问题还是很简单的，证明属于NP一直都是不难的。

Certificate: 如果图中存在一个 Hamiltonian Cycle，那么 certificate 可以是一个顶点序列 $v_1, v_2, \dots, v_n$

Certifier: 检查如下内容。
1.	序列是否包含所有顶点，且无重复
2.	对任意相邻 $(v_i, v_{i+1})$，是否存在边
3.	是否存在边 $(v_n, v_1)$

这些检查都可以在 poly-time 内完成。

### Hamiltonian Cycle 是 NP-Complete

这是一个核心结论，这部分的构造是比较复杂的。

我们选择用 Vertex Cover 问题进行规约。

$\text{Vertex Cover} ≤ₚ \text{Hamiltonian Cycle}$


Vertex Cover:
> 给定图 $G=(V,E)$ 和整数 $k$，是否存在大小 $≤ k$ 的点集，使得每条边至少有一个端点被选中？

规约的核心思想是：
- 把“选点覆盖边”的选择
- 转换为“Hamiltonian Cycle 必须以某种方式穿过 gadget”

换句话说：是否存在 Hamiltonian Cycle ⇔ 是否存在合法的 Vertex Cover。

570课件中使用了一组复杂的 gadget，这里不逐条照抄，而解释为什么这样设计是有效的。

|![](../assets/hc1.png)|![](../assets/hc2.png)|
|---|---|

直觉可以总结为三点：
1. 每条边对应一个结构: HC 经过这个结构的方式，等价于“选择了哪一个端点来 cover 这条边”
2.	选择顶点是受限的: 通过 selector vertices，强制最多只能选择 k 个“cover 决策”
3.	Hamiltonian Cycle 必须覆盖所有 gadget: 不能跳过任何一个 gadget，否则无法形成完整环

因此：
- 如果存在大小 ≤ k 的 vertex cover → 可以构造 Hamiltonian Cycle
- 如果存在 Hamiltonian Cycle → 可反推出一个 vertex cover

**结论：Hamiltonian Cycle 是 NP-Complete**

## Traveling Salesman Problem（TSP）

### 问题定义（优化版 vs 判定版）

优化版 TSP：

找到一条访问每个城市一次、并返回起点的最短路径。

但 NP 完全性讨论只关心判定版：

TSP（Decision Version）
给定带权完全图和一个阈值 D，是否存在一条 tour，其总权重 ≤ D？


### 证明 TSP ∈ NP

Certificate：一个城市排列（tour）

Certifier：
- 检查是否访问每个城市一次
- 计算总路径长度是否 ≤ D

所有步骤都可在多项式时间完成。

因此：TSP ∈ NP

这里都是很简单的。

### TSP 是 NP-Complete

570课件中使用的是一个非常经典且优雅的构造。

给定图 $G=(V,E)$，构造一个完全图 $G'$：
- 若 $(u,v) \in E$，则 $w(u,v) = 1$
- 若 $(u,v) \notin E$，则 $w(u,v) = 2$

并设定阈值：$D = |V|$

**此时：**

如果原图存在 Hamiltonian Cycle → 该环在新图中总代价 = |V| → TSP 回答 YES

如果不存在 Hamiltonian Cycle → 任意 tour 至少包含一条权重为 2 的边 → 总权重 > |V| → TSP 回答 NO

**因此：**

Hamiltonian Cycle ≤ₚ TSP

所以 TSP 是 NP-Complete

## Metric TSP 与近似算法

### Triangle Inequality

Metric TSP 要求边权满足三角不等式：

$w(u,v) \le w(u,x) + w(x,v)$

这使得问题结构发生了根本变化。

![](../assets/metric_tsp.png)

### 基于 MST 的 2-Approximation

课件中的算法思路：
1. 计算最小生成树（MST）
2.	对 MST 做 preorder traversal
3.	利用 triangle inequality shortcut 重复节点

**结论：Metric TSP 存在 2-approximation 算法**


## 为什么 General TSP 没有常数近似

关键结论是：

**如果 General TSP 存在常数近似算法 ⇒ 可以在多项式时间内解决 Hamiltonian Cycle ⇒ 推出 P = NP**

因此：

**General TSP 不存在任何常数因子的近似算法（除非 P=NP）**

**而这正是 Metric TSP 与 General TSP 的本质区别。**