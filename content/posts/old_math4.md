---
title: 旧笔记(4) - Fisher有序样本聚类的R函数
subtitle:
date: 2026-01-10T10:37:35+08:00
slug: aaf5ede
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
math: false
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
之前在大作业里想要用到课上学到的有序样本聚类方法，但是在网上居然没有找到有关的包，唯一找到yihui在2005年写的[远古程序](https://yihui.org/cn/2005/10/17-26-00/)，顺便了解了一下R语言的前世今生。   
搜到yihui的网页就顺便看了看他的简历，对这位之前只在研究RMarkdown时看过他的文档的大神的经历实在敬佩不已。2005年时yihui正也是大三学生，这让我也有了自己写函数的勇气。虽然时过境迁，编程已经比15年前普及太多，编写这么一个函数远用不了2天，但也算是对我自己的一个激励吧。   

## 程序及说明
程序的思想主要来自于张润楚的《多元统计分析》10.5节，在此就不多做介绍了。   
```r
OrdinalCluster <- function(x, K = 0, D = NULL, pic = FALSE){
  if(!is.data.frame(x)){
    stop("x must be a data frame.")
  }
  n <- nrow(x)
  if(K == 0){
    K <- n
  }
  else{
    if(as.integer(K) != as.numeric(K)) stop("K must be a integer.")
    if(K < 0) stop("K must be positive.")
    if(K > n) stop("K must be less than the sample size.")
  }
  if(is.null(D)){
    D <- function(x, i ,j){
      xp <- as.data.frame(x[i:j,])
      x_mean <- colMeans(xp)
      x_mean_m <- matrix(rep(x_mean, nrow(xp)), nrow(xp), byrow = TRUE)
      return(sum((xp-x_mean_m)^2))
    }
  }
  D_matrix <- matrix(NA, n, n)
  for (i in 1:n) {
    for (j in 1:i) {
      D_matrix[i,j] <- D(x, i, j)
    }
  }
  D_matrix[upper.tri(D_matrix)] <- t(D_matrix)[upper.tri(D_matrix)]
  e_matrix <- matrix(NA, n, K)
  j <- matrix(NA, n, K)
  for (p in 1:K) {
    for (i in p:n) {
      if(p == 1){
        e_matrix[i,p] <- D_matrix[1,i]
        j[i,p] <- 1
      }
      else{
        e_p <- double(i-p+1)
        for (t in p:i) {
          e_p[t-p+1] <- e_matrix[t-1, p-1] + D_matrix[t, i]
        }
        j[i,p] <- which.min(e_p) + p - 1
        e_matrix[i,p] <- e_p[j[i,p]-p+1]
      }
    }
  }
  e_min <- e_matrix[n, ]
  P <- matrix(NA, K, n)
  for (p in 1:K) {
    for (i in p:1) {
      if(i == p){
        P[p,i] <- j[n,p]
      }
      else{
        P[p,i] <- j[P[p,i+1]-1, i]
      }
    }
  }
  result <- list()
  result$D_matrix <- D_matrix
  if(K == 0){
    result$e_min <- e_min
    result$P <- P
  }
  else{
    result$P <- P[K,1:K]
  }
  if(pic == TRUE){
    e_pic <- plot(2:length(e_min),e_min[-1], type = "l", xlab = "K", ylab = "e_min")
    result$e_pic <- e_pic
  }
  return(result)
}
```

参数说明如下。

+ `x`必须是一个数据框，它的每一行作为一个样本。对于一元的样本请使用`as.data.frame(x)`进行转换。
+ `K`是样本聚类数量，如果不指定，则默认考虑所有聚类可能。
+ `D`是类的直径函数，默认为类内样本与其中心欧氏距离的平方和，自定义函数的参数格式为`D(x,i,j)`，其中`x`为样本，`i,j`为样本序号。
+ `pic`为是否画出$K-\min e(P(n,K))$图来确定聚类个数，默认为`FALSE`。如果指定了`K`，那么只能画出不超过`K`个点的折线图。

返回值说明如下。

+ `D_matrix`是包括了所有可能分类的直径。
+ `e_min`只有当没有指定`K`时才会返回，它表示样本在不同聚类数量下的误差函数的值。这里误差函数使用的各聚类的直径之和。
+ `P`是聚类的划分，记录了各个类的第一个样本的序号。没有指定`K`时返回包含所有可能聚类数的划分矩阵，否则返回聚成`K`类的划分。
+ `e_pic`是$K-\min e(P(n,K))$图，只有在`pic=TRUE`时才会返回。

## 后记
虽然只是一个简单的函数，但是还是花费了不少心思进行修改和调试。最终拿来和书上的一维数据进行了验证，结果是正确的，只是多元数据的验证还没有办法，毕竟就是因为没找到这方面的资料才自己写的函数。

*于2020年6月22日*

**评价于2026年1月10日**：说起有序样本聚类，那不就是一维变点检测吗。