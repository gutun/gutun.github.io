---
title: 旧笔记(3) - 逆Wishart分布的期望的证明
subtitle:
date: 2026-01-10T10:27:53+08:00
slug: 5ec9d5a
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
  - 统计学
  - 过去
categories:
  - 数学
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRelated: false
hiddenFromFeed: false
summary:
toc: true
math: 
  enable: true
lightgallery: false
password:
message:
repost:
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->

## 前言
之前在某个参数估计的无偏性证明上犯了蠢，本来是只要代入正态均值的期望就可以得到结论，我却钻入了牛角尖，到最后想要证明逆Wishart分布的期望。虽然对于解题已经没有意义，既然已经查了资料，就稍微研究一下，我在[Wikipedia](https://zh.wikipedia.org/wiki/%E9%80%86%E5%A8%81%E6%B2%99%E7%89%B9%E5%88%86%E4%BD%88)上找到了结果，但是没有找到相关的证明。最后在T. W. Anderson的《An Introduction to Multivariate Statistical Analysis》的P273-274上找到了证明。虽然这个证明很简短，但是因为我对高代实在是不够熟练，还是花了我很长时间去理解。

## 证明
**定理** 设$\boldsymbol{A}\sim W_p(n,\Sigma)$，那么$E\boldsymbol{A}^{-1}=\dfrac{1}{n-p-1}\Sigma^{-1}$.   
**证明** 考虑$\boldsymbol{B}\sim W_p(n,\boldsymbol{I})$，那么$\boldsymbol{B}\overset{d}{=}\boldsymbol{X}'\boldsymbol{X}$，其中$\boldsymbol{X}$是来自$N_p(0,\boldsymbol{I})$的$n\times p$数据阵，那么由于$\boldsymbol{X}$的每一行都是独立同分布的正态向量，可知$E\boldsymbol{B}^{-1}$的对角元全相同，非对角元也全相同（*这里确实不太好理解，但是也只能好好理解一下了……*），于是设$E\boldsymbol{B}^{-1}=k_1\boldsymbol{I}+k_2\epsilon\epsilon'$，其中$\epsilon=(1,\dots,1)'$。   
考虑任意正交矩阵$\boldsymbol{Q}$，有$\boldsymbol{Q}\boldsymbol{B}\boldsymbol{Q}'\sim W_p(n,\boldsymbol{I})$，于是$E\boldsymbol{B}^{-1}=E(\boldsymbol{Q}\boldsymbol{B}\boldsymbol{Q}')^{-1}=\boldsymbol{Q}E\boldsymbol{B}^{-1}\boldsymbol{Q}'$，代入得$k_2\epsilon\epsilon'=k_2\boldsymbol{Q}\epsilon\epsilon'\boldsymbol{Q}'$，如果$k_2\neq 0$，那么有$\epsilon\epsilon'=\boldsymbol{Q}\epsilon\epsilon'\boldsymbol{Q}'$对任意正交矩阵$\boldsymbol{Q}$成立，注意到$\epsilon\epsilon'$是全1矩阵，这是不可能的，（*这里可以取$\boldsymbol{Q}$为证明正态样本均值与方差独立时用到的矩阵矩阵，或者其他的，只要找出一个元素不同即可*），从而$k_2=0$，进而$E\boldsymbol{B}^{-1}=k_1\boldsymbol{I}$。   
此时只需要考虑$\boldsymbol{B}$的对角元。事实上，它的每一个对角元的逆都服从$\chi^2_{n-p+1}$（*见张润楚《多元统计分析》定理2.2.11*），而$E(\chi^2_{n-p+1})^{-1}=(n-p-1)^{-1}$，从而$E\boldsymbol{B}^{-1}=(n-p-1)^{-1}\boldsymbol{I}$。   
而对于一般的$\boldsymbol{A}$，存在$\boldsymbol{C}$使得$\Sigma=\boldsymbol{C}\boldsymbol{C}'$，从而有$\boldsymbol{A}\overset{d}{=}\boldsymbol{C}\boldsymbol{B}\boldsymbol{C}'$，进而$E\boldsymbol{A}^{-1}=E(\boldsymbol{C}\boldsymbol{B}\boldsymbol{C}')^{-1}=(\boldsymbol{C}')^{-1}E\boldsymbol{B}^{-1}\boldsymbol{C}^{-1}=\dfrac{1}{n-p-1}(\boldsymbol{C}\boldsymbol{C}')^{-1}=\dfrac{1}{n-p-1}\Sigma^{-1}$。

## 一些补充
### 有关$\boldsymbol{Q}$的选择
只要取
$$\boldsymbol{Q}=\begin{pmatrix}\frac{1}{\sqrt{n}}&\frac{1}{\sqrt{n}}&\frac{1}{\sqrt{n}}&\dots&\frac{1}{\sqrt{n}} \\ 
\frac{1}{\sqrt{2}}&-\frac{1}{\sqrt{2}}&0&\dots&0 \\
\frac{1}{\sqrt{2\cdot 3}}&\frac{1}{\sqrt{2\cdot 3}}&-\frac{1}{\sqrt{2\cdot 3}}&\dots&0 \\
\vdots&\vdots&\vdots&&\vdots \\ \frac{1}{\sqrt{(n-1)n}}&\frac{1}{\sqrt{(n-1)n}}&\frac{1}{\sqrt{(n-1)n}}&\dots&-\frac{1}{\sqrt{(n-1)n}} \end{pmatrix},$$
并注意到第一行的元素和为$\sqrt{n}$，第$n$列的元素和为$\dfrac{\sqrt{n-1}-1}{\sqrt{n(n-1)}}$，就可以得到$\boldsymbol{Q}\epsilon\epsilon'\neq\epsilon\epsilon'\boldsymbol{Q}$。

### 有关$\chi^2$分布的逆
最开始直接的想法是$F$分布的期望与分子无关，那么可以直接得到$E(\chi^2_n)^{-1}=\dfrac{1}{n-2}$，但仔细一想这样直接过来还是有点问题。网上看到可以考虑其推广逆$\Gamma$分布的期望，但是总觉得这样有点杀鸡用牛刀，还是更希望用我自己学过的内容来解决。   
在查阅资料的时候了解到一些知识，这个分布的期望直接求密度函数再用定义就可以[求出](https://math.stackexchange.com/questions/20912/calculation-of-inverse-of-chi-squares-expectation)，我稍微算了一下，打公式太麻烦就不写了。但对于更一般的情况，我了解到一些比如非中心的分布的期望，一般正态二次型的期望，非中心Wishart分布的期望，我打算学习之后再写一篇笔记。

**评价于2026年1月10日**：我记得这是多元统计分析课上学到的内容。我后面没有再深入看过矩阵估计相关的内容，从现在的视角来看，高维协方差阵的估计推断问题也稍微有些过时了。不过协方差阵的逆应当是作为precision matrix，研究Gaussian Graph Model的一个重要工具，所以$E[\boldsymbol{B}^{-1}]$的元素$b_{ij}$应该是与$X_i,X_j|X_{-i,-j}$的独立性密切相关。唉，还要感谢曾经的学长让我了解了这些东西，虽然之后我断绝了合作关系。