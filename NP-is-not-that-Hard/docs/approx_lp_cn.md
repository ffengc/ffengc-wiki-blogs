# 近似算法与线性规划

[中文](./approx_lp_cn.md) | [English](./approx_lp.md)

- [近似算法与线性规划](#近似算法与线性规划)
  - [为什么需要近似算法](#为什么需要近似算法)
  - [什么是近似算法（Approximation Algorithm）](#什么是近似算法approximation-algorithm)
  - [近似比（Approximation Ratio）](#近似比approximation-ratio)
  - [负载均衡问题（Load Balancing Problem）](#负载均衡问题load-balancing-problem)
    - [Greedy Balancing（贪心负载均衡）](#greedy-balancing贪心负载均衡)
    - [改进的贪心：LPT（1.5-Approximation）](#改进的贪心lpt15-approximation)
  - [Vertex Cover 的 2-Approximation 算法](#vertex-cover-的-2-approximation-算法)
  - [为什么 VC 的近似算法不能直接用于 Independent Set？](#为什么-vc-的近似算法不能直接用于-independent-set)
  - [Set Cover 与 Vertex Cover 的近似关系](#set-cover-与-vertex-cover-的近似关系)
  - [Max-3SAT 的近似算法](#max-3sat-的近似算法)
  - [用线性规划（LP）设计近似算法](#用线性规划lp设计近似算法)
    - [加权 Vertex Cover 的 ILP 表达](#加权-vertex-cover-的-ilp-表达)
    - [LP Relaxation（关键技巧）](#lp-relaxation关键技巧)
    - [从 LP 解构造近似解](#从-lp-解构造近似解)
  - [LP、IP 与复杂度类别的关系](#lpip-与复杂度类别的关系)


## 为什么需要近似算法

在前几章中，我们已经看到：
- 很多经典优化问题（如 Vertex Cover、TSP、Independent Set）是 NP-hard
- 这意味着：在合理时间内精确求最优解几乎不可能

于是，我们退一步，问一个现实问题：

如果我不追求最优解，是否可以快速求一个“差不多”的解？

这个就是近似解。

## 什么是近似算法（Approximation Algorithm）

定义（非形式化）：

> 近似算法是一个**多项式时间算法**， \
> 它输出的解不一定是最优解，但保证和最优解之间的“差距”有上界。

我们通常用 近似比（approximation ratio） 来衡量算法质量。

## 近似比（Approximation Ratio）

对于一个最小化问题：

$\text{Approximation Ratio} = \frac{\text{算法解}}{\text{最优解}}$

如果一个算法保证：

$\text{算法解} \le \rho \cdot \text{OPT}$

那么称它是一个 **ρ-approximation algorithm**。

## 负载均衡问题（Load Balancing Problem）

在570课程的 Lecture 13 里用来引入近似算法的第一个例子。

问题描述
- 有 $m$ 台 处理能力相同的机器
- 有 $n$ 个作业，第 $i$ 个作业需要时间 $t_i$
- 每个作业不能拆分
- 目标：最小化最大机器负载


### Greedy Balancing（贪心负载均衡）

**算法：**
1.	按给定顺序处理作业
2.	每次把当前作业分配给当前负载最小的机器

这个其实是一个很简单的贪心思想。

**近似比分析（核心思想）**

设：
- $T^*$：最优解的最大负载
- $T$：算法产生的最大负载

**结论：**
$T \le 2T^*$

所以这个贪心算法是一个是一个 2-approximation 算法

直觉可以告诉我们：
- 最后一个被放入“最重机器”的作业，大小 $≤ T^*$
- 放入前，该机器负载 $≤ T^*$
- 所以最终 $≤ 2T^*$

⸻

### 改进的贪心：LPT（1.5-Approximation）

改进思路：

**先把作业按处理时间从大到小排序，再用相同的贪心策略。**

这是著名的 LPT（Longest Processing Time First）。

结论

Lecture 13 给出证明：

$T \le 1.5 \cdot T^*$

因此：这是一个 1.5-approximation 算法

**这说明：简单排序 + 贪心，就能显著提升近似质量。**

## Vertex Cover 的 2-Approximation 算法

回到我们非常熟悉的 Vertex Cover。

算法（非常重要）

```
S = ∅
while S 还不是 vertex cover:
    选一条未覆盖的边 (u, v)
    把 u 和 v 都加入 S
```


这个算法就是 2-Approximation，为什么：
- 每次选一条边 $(u, v)$
- 最优解至少要选 $u$ 或 $v$ 其中一个
- 我们选了两个

因此：$|S| \le 2 \cdot |OPT|$

这是一个经典的 2-approximation 算法。

## 为什么 VC 的近似算法不能直接用于 Independent Set？

虽然：

$\text{Independent Set} \leftrightarrow \text{Vertex Cover}$

但近似并不保结构。

**结论：规约 ≠ 近似可传递**

## Set Cover 与 Vertex Cover 的近似关系

- Vertex Cover ≤ₚ Set Cover
- 近似是可以从 Set Cover 传回 Vertex Cover 的

原因：
- Vertex Cover 是 Set Cover 的一个特殊情况
- 解的规模完全一致

**结论：Set Cover 的 ρ-approx ⇒ Vertex Cover 的 ρ-approx**

## Max-3SAT 的近似算法

问题：给一堆长度为 3 的子句，最大化被满足的子句数。

这是一个极其简单的 1/2-Approximation

算法：
1. 把所有变量设为 TRUE
2. 如果 ≥ 50% 子句满足，结束
3. 否则全部设为 FALSE

**结论：** 这是一个 1/2-approximation

课程上还说明：更复杂的方法可以做到 8/9-approximation，但不展开。

## 用线性规划（LP）设计近似算法

### 加权 Vertex Cover 的 ILP 表达

定义变量：

$
x_i =
\begin{cases}
1 & i \in S \\
0 & i \notin S
\end{cases}$

目标函数：

$\min \sum_i w_i x_i$

约束：

$x_i + x_j \ge 1 \quad \forall (i,j) \in E$

这是一个 整数线性规划（ILP）。

### LP Relaxation（关键技巧）

把：

$x_i \in \{0,1\}$

放松成：

$x_i \in [0,1]$

得到一个 线性规划（LP），可以多项式时间求解。

### 从 LP 解构造近似解

定义：

$S = \{ i \mid x_i \ge 1/2 \}$

正确性:
- 对任意边 $(i,j)$，必有 $x_i + x_j \ge 1$
- 不可能两个都 $< 1/2$
- 所以 $S$ 覆盖所有边

近似比分析:

$w(S) \le 2 \cdot w(OPT)$

LP Relaxation + Round 得到 2-Approximation

## LP、IP 与复杂度类别的关系

Lecture 13 中的总结图非常关键：
- LP 曾被认为是 NP-intermediate
- 1979 年证明 LP 可在多项式时间求解
- Simplex 最坏情况指数级
- 但实践中通常表现很好