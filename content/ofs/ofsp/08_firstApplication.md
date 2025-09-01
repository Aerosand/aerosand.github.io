---
uid: 20250901123439
title: 08_firstApplication
date: 2025-09-01
update: 2025-09-01
authors:
  - name: Aerosand
    link: https://github.com/aerosand
    image: https://github.com/aerosand.png
tags:
excludeSearch: false
toc: true
weight:
math: true
next:
prev:
comments: true
sidebar:
  exclude: false
draft: false
---

## 0. 前言

之前讨论了编译原理、动态库的链接和 OpenFOAM 常见的类，现在我们使用 OpenFOAM ，完成一个较为完整的开发流程。

本文主要讨论

- [ ] 理解 OpenFOAM 标准应用的文件架构
- [ ] 认识使用脚本
- [ ] 调用其他位置的开发库
- [ ] 编译运行 firstApp 项目

## 1. fvCFD.H

在实际开发中，除了之前提到了 vector 和 tensor 等，我们还需要用到更多的和 FVM 相关的类来离散求解偏微分方程。OpenFOAM 提供 `fvCFD.H` ，其中包含了大部分和 FVM 相关的头文件，包括 tensor 类等。使用 `fvCFD.H` 可以大大减少主源码要写的头文件数量。

终端输入命令