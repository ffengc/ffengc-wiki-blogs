# 时间复杂度入门 - 什么是 P 问题

[中文](./complexity_intro_cn.md) | [English](./complexity_intro.md)

- [时间复杂度入门 - 什么是 P 问题](#时间复杂度入门---什么是-p-问题)
  - [计算复杂度基础概念简介](#计算复杂度基础概念简介)
    - [为什么要强调 poly-time?](#为什么要强调-poly-time)
  - [P 类问题](#p-类问题)
  - [伪多项式时间 (Pseudo-Polynomial Time)](#伪多项式时间-pseudo-polynomial-time)
  - [弱多项式时间 (Weakly Polynomial Time)](#弱多项式时间-weakly-polynomial-time)
  - [强多项式时间 (Strongly Polynomial Time)](#强多项式时间-strongly-polynomial-time)
  - [三种复杂度的终极对比（非常重要）](#三种复杂度的终极对比非常重要)

## 计算复杂度基础概念简介

在进入 NP、NP 完全性、多项式规约这些更深的主题之前，首先需要建立一套共同语言：
什么是“算法的复杂度”？什么叫“多项式时间”？为什么有些算法明明很快，却不算多项式？

先介绍三个最重要的复杂度概念：
- 多项式时间（Polynomial Time）
- 伪多项式时间（Pseudo-Polynomial Time）
- 弱多项式时间（Weakly Polynomial Time）

这三者会在后面内容重复出现！

> [!note]
> 后面“多项式时间”可能都会缩写成 poly-time，伪多项式时间可能会缩写成 pseudo-poly-time, 弱多项式时间可能会写成 weakly-poly-time. 这个写法不一定专业，只是为了后面简单而已。

### 为什么要强调 poly-time?

在理论计算机科学中，我们普遍把 poly-time 作为“有效算法”的判断标准。

因为：
1.	多项式时间算法的增长速度（如 $n$、$n\log n$、$n^2$、$n^3$）都可控;
2.	换电脑、换语言、换常数，不会影响其“级别”;
3.	多项式时间问题 = 工程上可扩展的问题;

例如：

|时间复杂度|是否可行|评价|
|---|---|---|
|$O(n)$|✔| linear time, 非常快|
|$O(n\log n)$|✔|sort alg.级别的, 快|
|$O(n^2)$|✔|这些都是没有问题的|
|$O(n^7)$|✔|仍然是poly-time, 这些都是可控的|
|$O(2^n)$|✘|指数爆炸|
|$O(n!)$|✘|爆炸|

因此，P 类问题（Polynomial-Time Problems） 代表“可在合理时间内解决的问题”。


## P 类问题

一个算法是多项式时间的，如果运行时间满足：$O(n^k)$, 其中 k 是某个常数。那么这个算法可以解决的问题，可以被称之为一个P问题。

> [!important]
> 注意，不是算法是P，而是这个问题，因为可以被poly-time的算法解决，所以这个问题属于P类问题。

经典多项式时间算法例子

1. Dijkstra 最短路径（非负权图）
   - Code: [CSCI570-Analysis-of-Algorithms-USC/Lecture2-BFS-DFS-Graph/Graph.hpp
](https://github.com/ffengc/CSCI570-Analysis-of-Algorithms-USC/blob/main/Lecture2-BFS-DFS-Graph/Graph.hpp)
   - 时间: $O(m \log n)$ 或 $O(n^2)$
   - 所以 Shortest Path 问题属于典型的 P 类问题。

2. 最大流（Max-Flow）问题
   - Note: [CSCI570-Analysis-of-Algorithms-USC/Lecture9-Network-Flow/Note.md](https://github.com/ffengc/CSCI570-Analysis-of-Algorithms-USC/blob/main/Lecture9-Network-Flow/Note.md)
   - | **算法**                              | **时间复杂度**           | **类型**            |
       | ------------------------------------- | ------------------------ | ------------------- |
       | **Ford–Fulkerson**                    | $O(C m)$                 | pseudo-polynomial   |
       | **Scaled version (Capacity Scaling)** | $O(m² log C)$            | weakly polynomial   |
       | **Edmonds–Karp**                      | $O(n m²)$                | strongly polynomial |
       | **Orlin + KRT (改进版)**              | $O(n m)$                 | strongly polynomial |
       | **Modern Approximation methods**      | $≈ O(m log n + m^{4/3})$ | almost linear time  |
    - 所以 Max Flow Problem 也是典型的 P 类问题

3. 最小生成树（MST）
   - Code: [CSCI570-Analysis-of-Algorithms-USC/Lecture2-BFS-DFS-Graph/Graph.hpp
](https://github.com/ffengc/CSCI570-Analysis-of-Algorithms-USC/blob/main/Lecture2-BFS-DFS-Graph/Graph.hpp)
   - Kruskal / Prim 都是 $O(m \log n)$
   - 都是 poly-time, 所以 MST Problem 也是 P 类问题

这些算法构成了“可高效求解”的基石。

⸻

## 伪多项式时间 (Pseudo-Polynomial Time)

一句话定义: 

> 伪多项式算法的运行时间依赖于输入数字的“数值大小 $V$”，而不是其“二进制位数 $\log V$”。

听起来抽象，但例子一看就懂。

**经典例子：0/1 背包动态规划**

- 在570课上是在动态规划章节详细讲过背包问题 (Knapsack Problem): [CSCI570-Analysis-of-Algorithms-USC/Lecture7-DP-part1](https://github.com/ffengc/CSCI570-Analysis-of-Algorithms-USC/tree/main/Lecture7-DP-part1)

- 但是对于背包问题，大家可以直接去 Carl 老师的代码随想录学习，讲解的非常清晰：[01knapsack](https://programmercarl.com/背包理论基础01背包-1.html)

- 然后我在跟随 Carl 老师学习过程中，自己也有整理笔记，里面还有非常多背包问题的相关题目，还有完全背包问题等等：[LeetsGoAgain/docs/dp.md](https://github.com/ffengc/LeetsGoAgain/blob/main/docs/dp.md)



**01 Knapsack Problem DP 时间复杂度：$O(nW)$**

其中：
- n = 物品数量
- W = 背包容量（数值大小）

**注意:**

$W = 10^9$ 的 bit-length 只有约 30 bits。

但是算法需要跑 十亿步。

因此，不是真正意义上的 polytime。

上面的解释是我当初不理解的时候，问大模型，大模型的解释。

这样解释还是很奇怪，我个人的理解是：

> 比如[不同路径](https://programmercarl.com/0062.不同路径.html#算法公开课)这个题目，和背包问题都是二维数组的DP，为什么前者是 poly-time, 后者不是？
> \
> 因为在不同路径这题中，我们的输入，是二维的！而在背包问题中，W容量只是一个数字！\
> 在不同路径这个问题中，如果你想要DP数组多一行，你就要把地图扩展一行，所以输入和输出的变化是同一量级的！
> 在背包问题中，想要DP数组多一行，我们只需要改一个数字W，让他加1就行了，而不需要输入多加一行这么个量级。\
> 所以在背包问题中，一个小小数字的+1，就会导致DP数组多加一行，这种变化其实是非常大的。

不知道这样能否让大家理解。

为什么叫“伪 (pseudo)”？

因为：
- 当 W 很小的时候，看起来像 $O(nW)$ 很快 → 像多项式
- 当 W 很大（但 bit-length 也很小）时 → 实际是指数级时间

例如：

|W|bit-length|DP 时间|评价|
|---|---|---|---|
|1000|10 bits|$O(n·1000)$|可行|
|10⁹|30 bits|$O(n·10^9)$|不可行|
|2¹⁰⁰|100 bits|$O(n·2¹⁰⁰)$|爆炸|

伪多项式算法常出现于 NP-Hard 问题的 DP 解法中，如：
- Knapsack
- Subset Sum

关于 NP难 是什么意思，我后面会继续讲解的。


## 弱多项式时间 (Weakly Polynomial Time)

弱多项式时间介于 “真正多项式” 与 “伪多项式”之间。

一句话定义: 

> 弱多项式时间算法依赖于 n 与输入数字的 bit-length ($\log V$)，而不是 $V$ 本身。

因此：
- weak-poly 属于 P（多项式）
- pseudo-poly 不属于 P

经典例子：线性规划（Linear Programming, LP）

LP 的求解算法（例如 Ellipsoid、Interior-Point）运行时间大致为：$\operatorname{poly}(n, B)$。

其中：
- n = 变量与约束的数量
- B = 输入数字的 bit-length（例如数字有 10 位，就 B=10）


## 强多项式时间 (Strongly Polynomial Time)

强多项式时间算法的定义：

$\operatorname{poly}(n) \quad \text{且不依赖} \log V \text{ 或 V 本身}$

纯粹依赖输入结构，而不是数字大小。

经典例子
- BFS / DFS
- Prim / Kruskal
- 匹配（Hopcroft–Karp 等）
- 部分最大流算法（如 Orlin）

强多项式算法是最稳健的复杂度类型。

## 三种复杂度的终极对比（非常重要）

下表是理解复杂度结构最关键的一个表格：

| **类型**               | **运行时间依赖**     | **示例**       | **属于 P？**                 |
|------------------------|-----------------------|----------------|------------------------------|
| **强多项式 (Strong Poly)** | $\operatorname{poly}(n)$               | MST、匹配      | ✔ 是                         |
| **弱多项式 (Weak Poly)**   | $\operatorname{poly}(n, log V)$        | 线性规划 LP    | ✔ 是                         |
| **伪多项式 (Pseudo-Poly)** | $\operatorname{poly}(n, V)$            | 背包 DP        | ✘ 不是（除非数字很小）       |

> [!tip]
> 伪多项式算法看起来“快”，但实际上对 bit-length 不能保证多项式增长，因此 NP-hard 问题不被视为已解决。