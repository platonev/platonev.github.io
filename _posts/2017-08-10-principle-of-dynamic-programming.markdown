---
layout: post
title: "动态规划原理"
date: 2017-08-10 10:31:49 +0800
categories: algorithm dynamic-programming
---
适合应用动态规划方法求解的最优化问题应该具备两个要素：**最优子结构**和**重叠子问题**；

## 最优子结构 
如果一个问题的最优解包含其子问题的最优解，则此问题具有*最优子结构*性质，使用动态规划方法时，用子问题的最优解来构造原问题的最优解。

> 注：具有最优子结构性质也可能意味着适合应用贪心算法。

**发掘最优子结构性质的一般过程：**
- 证明问题最优解的第一个组成部分是做出一个选择。（做出这次选择会产生一个或多个待解的子问题）
- 对于一个给定问题，在其可能的第一步选择中，假定已经知道哪种选择才会得到最优解。（现在不关心这个选择具体是如何得到的，只假定已经知道了这种选择）
- 给定可获得最优解的选择后，确定这次选择会产生哪些子问题，以及如何最好地刻画子问题空间。
- 作为构成原问题最优解的组成部分，每个子问题的解就是它本身的最优解。

## 重叠子问题
适合用动态规划方法求解的最优化问题应该具备的第二个性质是子问题空间必须足够“小”，问题的递归算法会反复地求解相同的子问题，而不是一直生成新的子问题。如果递归算法反复求解相同的子问题，则称最优化问题具有**重叠子问题**性质。

## 例子：最长公共子序列（longest common subsequence，LCS）

**问题描述：**

给定两个子序列X和Y，如果Z既是X的子序列，也是Y的子序列，则称它是X和Y的公共子序列。例如，如果X=<A,B,C,B,D,A,B>，Y=<B,D,C,A,B,A>，那么序列<B,C,A>就是X和Y的公共子序列。但它不是最长公共子序列，因为它的长度是3，而<B,C,B,A>也是X和Y的公共子序列，其长度为4。<B,C,B,A>和<B,D,A,B>都是X和Y的最长公共子序列，因为X和Y不存在长度大于等于5的公共子序列。

给定两个序列X=<x<sub>1</sub>,x<sub>2</sub>,...,x<sub>m</sub>>和Y==<y<sub>1</sub>,y<sub>2</sub>,...,y<sub>n</sub>>，求X和Y长度最长的公共子序列。

**步骤1: 刻画最长公共子序列的特征**

先定义序列的**前缀**：
给定一个序列X=<x<sub>1</sub>,x<sub>2</sub>,...,x<sub>m</sub>>，对于*i＝0,1,...,m*，定义X的第i前缀为X<sub>i</sub>=<x<sub>1</sub>,x<sub>2</sub>,...,x<sub>i</sub>>。例如，若X=<A,B,C,B,D,A,B>，则X<sub>4</sub>=<A,B,C,B>，X<sub>0</sub>为空字符串。

对于给定的两个序列X和Y，设Z=<z<sub>1</sub>,z<sub>2</sub>,...,z<sub>k</sub>>为X和Y的任意LCS，则：
- 如果 x<sub>m</sub>=y<sub>n</sub>，则 z<sub>k</sub>=x<sub>m</sub>=y<sub>n</sub> 且Z<sub>k－1</sub>是X<sub>m－1</sub>和Y<sub>n－1</sub>的一个LCS。
- 如果 x<sub>m</sub>≠y<sub>n</sub>，则 z<sub>k</sub>≠x<sub>m</sub> 意味着Z是X<sub>m－1</sub>和Y的一个LCS。
- 如果 x<sub>m</sub>≠y<sub>n</sub>，则 z<sub>k</sub>≠y<sub>n</sub> 意味着Z是X和Y<sub>n－1</sub>的一个LCS。

**步骤2：一个递归解**

从上面的描述可知，在求X=<x<sub>1</sub>,x<sub>2</sub>,...,x<sub>m</sub>>和Y==<y<sub>1</sub>,y<sub>2</sub>,...,y<sub>n</sub>>的一个LCS时，需要解决一个或两个子问题。
- 如果**x<sub>m</sub>=y<sub>n</sub>**，则应该求解X<sub>m－1</sub>和Y<sub>n－1</sub>的一个LCS，将x<sub>m</sub>=y<sub>n</sub>追加到这个LCS的末尾就得到X和Y的一个LCS。
- 如果**x<sub>m</sub>≠y<sub>n</sub>**，则需要求解两个子问题：**X<sub>m－1</sub>和Y的一个LCS**与**X和Y<sub>n－1</sub>的一个LCS**。两个LCS较长者即为X和Y的一个LCS。

**重叠子问题**:

可以很容易看出，为了求X和Y的一个LCS，可能需要求X<sub>m－1</sub>和Y的一个LCS与X和Y<sub>n－1</sub>的一个LCS。但这几个子问题都包含求解X<sub>m－1</sub>和Y<sub>n－1</sub>的LCS的子子问题。
