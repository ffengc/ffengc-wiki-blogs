
# 多项式规约与 NP 完全性的基础

[中文](./reductions_cn.md) | [English](./reductions.md)

- [多项式规约与 NP 完全性的基础](#多项式规约与-np-完全性的基础)
  - [多项式规约（Polynomial-Time Reduction）](#多项式规约polynomial-time-reduction)
  - [Independent Set v.s. Vertex Cover: 最经典的互相规约](#independent-set-vs-vertex-cover-最经典的互相规约)
    - [Independent Set Problem and Vertex Cover Set Problem](#independent-set-problem-and-vertex-cover-set-problem)
    - [Important FACT](#important-fact)
    - [两者互相规约](#两者互相规约)
  - [Set Cover: Vertex Cover 更一般化的形式](#set-cover-vertex-cover-更一般化的形式)
  - [使用 Gadgets 的规约（从 SAT 到图）](#使用-gadgets-的规约从-sat-到图)
    - [clause（子句）与 literal（文字）](#clause子句与-literal文字)
    - [SAT 与 3-SAT](#sat-与-3-sat)
    - [规约: $\\text{3-SAT} \\le \_p \\text{Independent Set}$](#规约-text3-sat-le-_p-textindependent-set)
    - [两个方向证明规约正确性](#两个方向证明规约正确性)
  - [NP-hard 与 NP-complete](#np-hard-与-np-complete)
    - [NP-hard 定义（至少这么难）](#np-hard-定义至少这么难)
    - [NP-complete 定义](#np-complete-定义)
  - [规约的传递性](#规约的传递性)
  - [如何证明一个问题 X 是 NP-complete？](#如何证明一个问题-x-是-np-complete)
  - [证明例子合集](#证明例子合集)


## 多项式规约（Polynomial-Time Reduction）

在复杂度理论中，“规约”是研究问题难度最重要的工具。

首先在这里先搞定规约是什么意思。

我们经常说：

> 如果问题 X 至少和问题 Y 一样难，意味着：如果我们能解决 X，就能顺便解决 Y。

这个是很好理解的。

形式化定义为多项式规约（Reduction）：

> $Y \le _p X$ \
> Y 可以用一个多项式时间的算法，加上若干次调用 X 的黑盒来解决。

**直观理解：**
- 我们想解决 Y
- 但我们不会
- 所以把 Y 转换成 X
- 只要有一个能解决 X 的黑盒，就能解决 Y

规约可以帮助我们比较问题的难度。

理解方法：
- 如果 所有人 都能在多项式时间内把 Y 换成 X
- 那么 X 至少不比 Y 简单


## Independent Set v.s. Vertex Cover: 最经典的互相规约

为了直观理解规约，我们先从两个简单且密切相关的问题开始。

### Independent Set Problem and Vertex Cover Set Problem

这两个问题的描述，已经在前两章讲述过了，这里不再赘述。

### Important FACT

非常重要：

在一个图 $G = (V, E)$ 中：$S$ 是独立集 ⇔ $V − S$ 是点覆盖

**直觉：**
- 如果 S 里面没有任意两个相邻点
- 那任何一条边至少有一个端点不在 $S$
- 所以补集 $V−S$ 一定覆盖所有边

反之亦然。

这个互补关系可以自然地导出规约。



### 两者互相规约

$\text{Independent Set} \le _p \text{Vertex Cover}$

**问题：G 是否存在独立集 ≥ k？**

转换为：

**问：G 是否存在点覆盖 ≤ n − k？**

理由：
- S 是独立集
- V−S 是点覆盖
- |V−S| = n − |S|
- 所以 |S| ≥ k ⇔ |V−S| ≤ n−k

因此，只要问黑盒：

“是否有点覆盖大小 ≤ n−k？”

如果黑盒回答 YES，则 Independent Set 答案也是 YES。

***

$\text{Vertex Cover} \le _p \text{Independent Set}$

同理：

判断“VC ≤ k” 等价于判断 “IS ≥ n−k”。

**这两个规约，是已知结论，可以直接使用。**

## Set Cover: Vertex Cover 更一般化的形式

Set Cover 的定义：

给定一个元素集合 $U$，以及若干个“子集” $S_1, ..., S_m$，问是否可以选 $\le k$ 个集合覆盖所有元素？

Vertex Cover 可以规约到 Set Cover：
- 元素 = 图的边
- 每个点 v 对应一个集合 S(v)，包含与 v 相连的所有边
- 选择点覆盖边 ⇔ 选择集合覆盖元素

因此：

$\text{Vertex Cover} \le _p \text{Set Cover}$

所以 Set Cover 至少和 Vertex Cover 一样难。

## 使用 Gadgets 的规约（从 SAT 到图）

接下来进入复杂度理论中的“gadget 构造”。
gadget 是一种小型结构，用来模拟逻辑关系。

本节目标：
- 介绍 clause、literal、assignment
- 再通过 gadget，把 3-SAT 规约到 Independent Set

这部分是 NP 完全性的核心技巧。


### clause（子句）与 literal（文字）
- 子句 = 用 OR 连接的若干 literal
- literal = 变量 x 或其否定 ¬x

其实这个概念前面是讲过的，当时提到SAT问题的时候。

$x_1 \vee ¬x_3 \vee x_4$

赋值什么时候满足子句？只要有一个 literal 为真，该子句就满足。

### SAT 与 3-SAT

SAT 问题：

是否存在对变量的赋值使所有子句都为真？

比如现在有若干子句: $C_1, C_2, ..., C_k$

现在要找到一个变量赋值，使得所有的子句都为真。

**3-SAT 是特例：每个子句恰好 3 个 literal。**

### 规约: $\text{3-SAT} \le _p \text{Independent Set}$

**这是 NP 完全理论最经典的规约之一。**

**(1) 为每个子句创建一个三角形 gadget**

例如子句：$x \vee y \vee ¬z$

![](../assets/3sat.png)

三角形保证：
- 同一个子句里不能同时选两个 literal
- 每个子句最多取 1 个点作为 IS 的成员

**这表示：**

“一个子句里至少选择一个为真”


**(2) 禁止 x 与 ¬x 同时被选（加入冲突边）**

如果 literal a 表示 “x=真”，literal b 表示 “x=假”
→ 不能两者都选入独立集
→ 在它们之间连一条边即可


![](../assets/3sat2.png)


### 两个方向证明规约正确性

A. 如果 3-SAT 可满足 → 存在大小 = 子句数 的独立集

对每个子句选一个为真的 literal：
- 每个 triangle 选一个
- 不会冲突（x 与 ¬x 不会同时为真）
- 因此形成 independent set

B. 如果存在大小 = 子句数 的独立集 → 可以构造可满足赋值

因为：
- 每个 triangle 只能选一个
- 总共选了 k 个（k=子句数）

→ 每个子句都选了一个 literal \
→ 将被选 literal 赋为真 → 得到满足赋值


## NP-hard 与 NP-complete

### NP-hard 定义（至少这么难）

一个问题 X 是 NP-hard，如果：

NP 中所有问题 Y 都可以规约到 X（$Y \le _p X$）。

**NP-hard 不要求：**
- X 在 NP 里
- X 可验证
- X 可决策

例子：停机问题（Halting Problem）


### NP-complete 定义

**（重要定义）一个问题 X 是 NP-complete，当且仅当：**
1. X ∈ NP（可验证）
2. 对所有 Y ∈ NP，Y ≤ₚ X（最难）

**性质：**
- NP-complete 是 NP 的“最难核心”
- 只要一个 NP-complete 问题能 poly-time 解决

则所有 NP 问题都能 poly-time 解决 \
则P = NP


## 规约的传递性

非常关键：

如果 Z ≤ₚ Y 且 Y ≤ₚ X，则 Z ≤ₚ X。

所以：
- 不需要从“所有 NP 问题”开始规约
- 只需要从一个已经 NP-complete 的问题规约即可

**SAT 是第一个被证明 NP-complete 的问题（Cook-Levin 定理），
因此 SAT 成为所有 NP 完全证明的“起点”。**

![](../assets/reduction_transitivity.png)

## 如何证明一个问题 X 是 NP-complete？

**最经典的三步法：**

**步骤 1：** 证明 X ∈ NP

给出 certificate 和 certifier。

**步骤 2：**选择一个已知 NP-complete 的问题 Y

例如：
- 3-SAT
- Independent Set
- Vertex Cover
- Hamiltonian Cycle

**步骤 3：** 证明 Y ≤ₚ X

也就是：

把实例 Y 编码成实例 X
并证明两个方向的正确性（YES→YES，NO→NO）

完成以上三步 → X 是 NP-complete。

## 证明例子合集

这里整理了一系列证明题目，里面包含了一系列常用的构造方法。

- [prove_np_complete.md](./prove_np_complete.md)