---
layout: post
title: 算法设计与分析-北京大学-屈婉玲
date: 2019-10-23
categories: Programming
tags: C++ 北大算法
status: publish
type: post
published: true
author: Quebradawill
---

## 1. 分治



## 2. 动态规划

动态规划（Dynamic Programming）：求解过程是<font color='magenta'>多阶段决策</font>过程，每步处理一个子问题，可用于求解组合优化问题。

**适用条件：**问题要满足<font color='magenta'>优化原则</font>或<font color='magenta'>最优子结构</font>性质，即： 一个最优决策序列的任何子序列本身一定是相对于子序列的初始和结束状态的最优决策序列。

<font color='blue'>动态规划设计要素：</font>1. 问题建模，优化的目标函数是什么？约束条件是什么？2. 如何划分子问题（边界）？3. 问题的优化函数值与子问题的优化函数值存在 着什么依赖关系？（递推方程）4. 是否满足优化原则？5. 最小子问题怎样界定？其优化函数值，即初值等于什么？

<font color='blue'>动态规划算法设计要素：</font>多阶段决策过程，每步处理一个子问题，界定子问题的边界。列出优化函数的递推方程及初值。问题要满足优化原则或最优子结构性质，即： 一个最优决策序列的任何子序列本身一定是相对于子序列的初始和结束状态的最优决策序列。

**实例1：矩阵相乘基本运算次数**

矩阵 $P,Q$ 的大小分别为 $p\times q$ 和 $q \times r $，则 $ P \times Q $ 的基本运算次数为 $ p \times q \times r $。

蛮力算法：加 $n$ 个括号的方法有 $\frac{1}{n+1} \binom {2n}{n}$ ，是一个 Catalan 数，是指数级别。

动态规划算法：（1）子问题划分：矩阵链 $A_i A_{i+1} \cdots A_j$，边界向量 $i,j$，输入向量：$<P_{i-1} P_i \cdots P_j>$，其最好划分运算次数：$m[i,j]$。（2）子划分的依赖：最优划分最后一次相乘发生在矩阵 $k$ 的位置，即 $ A_{i \cdots j} = A_{i \cdots k} A_{k + 1 \cdots j}$。（3）递推方程为：
$$
m[i,j] = \left\{\begin{array}
00 & i=j \\ 
\min_{i \leq k < j} \{ m[i,k] + m[k+1,j] + P_{i-1} P_k P_j \} & i < j
\end{array}\right.
$$


## 3. 贪心

## 4. 回溯