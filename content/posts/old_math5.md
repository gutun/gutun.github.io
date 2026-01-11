---
title: 旧笔记(5)
subtitle:
date: 2026-01-11T08:56:05+08:00
slug: c020459
draft: false
author:
  name: GUTUN
  link:
  email:
  avatar:
description:
keywords:
license:
comment: false
weight: 0
tags:
  - 数学
  - 过去
categories:
  - 数学
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRelated: false
hiddenFromFeed: false
summary:
toc: true
math: true
lightgallery: false
password:
message:
repost:
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->

**问题**：假设$X,X_1,X_2\overset{i.i.d}{\sim}F$，什么情况下$\max(X_1,X_2)\overset{d}{=}\sqrt{X}$呢？

设 $X$ 的分布函数为 $F(x)=P(X<x),  X_{1}, X_{2} \overset{i.i.d}{\sim} X$ ，则 $U_{1}=\max \left\{X_{1}, X_{2}\right\}$ 和 $U_{2}=\sqrt{X}$ 的分布函数分别为 $F_{1}(x)=F^{2}(x)$ 和 $F_{2}(x)=F\left(x^{2}\right)$ 。

由 $\sqrt{X}$ 存在有 $F(0)=P(X<0)=0$ 。

当 $F_{1}(x)=F_{2}(x)$ 时，我们有 $F(1)=F^{2}(1)$ ，则 $F(1)=0$ 或 $F(1)=1$ 。事实上，对于 $\forall x>1$ ，有 $F(x) \leq F\left(x^{2}\right)=F^{2}(x)$ ，另一方面由 $0 \leq F(x) \leq 1$ 有 $F^{2}(x) \leq F(x)$ ，知 $F(x)=F^{2}(x)$ 。由 $F(+\infty)=1$ 简单分析可知 $F(1+)=1$ 。

接下来考虑 $0<x<1$ 的情况，这实际是要解函数方程 $F\left(x^{2}\right)=F^{2}(x)$ 。这个问题有几个简单的解，例如均匀分布 $U(0,1)$ 的分布函数 $F(x)=x, x \in[0,1]$ ，以及 0 或 1 上的单点分布，复杂一些的解包括 $F(x)=x^{n}, x \in[0,1]$ 。

对于一般情况，我参考了[这里](https://math.stackexchange.com/questions/605876/solutions-for-the-functional-equation-fx2-fx2)。

在这个回答中，作者考虑了 $h(u)=\log _{2}\left(-\log _{2}\left(F\left(2^{-2^{u}}\right)\right)\right)$ ，这个函数与 $F$ 单调性相同，而且满足 $h(u+1)=h(u)+1$ 。作者提出，只要选择 $h:[0,1] \rightarrow \mathbb{R}$ 满足 $h(1)=h(0)+1$ ，再将其通过 $h(v)=h(v-\lfloor v\rfloor)+\lfloor v\rfloor$ 延拓到 $\mathbb{R}$ 上，就都满足 $F\left(x^{2}\right)=F^{2}(x)$ 。

在这个问题中，我们只要选择单调的 $h$ 就可以满足条件。例如当选择 $h(u)=u$ 时，$F(x)=x$。

*于2021年12月25日*