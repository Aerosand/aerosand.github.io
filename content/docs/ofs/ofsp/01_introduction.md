---
uid: 20250722130605
title: 01_introduction
date: 2025-07-22
update: 2025-09-01
authors:
  - name: Aerosand
    link: https://github.com/aerosand
    image: https://github.com/aerosand.png
tags:
  - ofsp2026
  - OpenFOAM
  - ofsp
excludeSearch: false
toc: true
weight: 1
math: true
next:
prev:
comments: true
draft: false
category:
  - ofsp2026
---

> [!important]
> 访问 https://aerosand.cc 以获取最近更新。

## 0.前置

本系列旨在帮助读者衔接“CFD 基础理论”和“OpenFOAM 入门实践”两个部分。

> [!warning]
> 建议先学习计算流体力学基础以及有限体积法，之后再开始本系列的学习。

## 1.介绍

OpenFOAM 是什么呢？引用 wiki 解释如下

> OpenFOAM (for "Open-source Field Operation And Manipulation") is a C++ toolbox for the development of customized numerical solvers, and pre-/post-processing utilities for the solution of continuum mechanics problems, most prominently including computational fluid dynamics (CFD).

所以我们可以使用 OpenFOAM 来构建基于 C++ 的实现 CFD 等理论的求解器应用。

## 2.路线

我们从简单的 C++ 程序实现开始，简单了解编译原理，通过 make 逐渐掌控我们的项目，过渡到了解 OpenFOAM 的 wmake 实现方式，然后认识 OpenFOAM 的基本程序，然后逐渐深入了解 OpenFOAM 的求解器应用细节。

{{% steps %}}

### 编译原理

1. C++ 程序的编译
2. make 管理程序编译
3. wmake 管理程序编译
4. OpenFOAM 应用构建

### 数据交互

1. 输入输出
2. 命令行参数

### 基础类

1. 时间
2. 网格
3. 场

### 求解器

1. 开发库
2. 第一个求解器

### 算法初见

1. SIMPLE & PISO & PIMPLE 算法
2. SIMPLE 求解器

{{% /steps %}}

> [!note]
> 每个部分都会有详细的代码和操作解释。


## 3.环境和工具

鉴于 OpenFOAM 的使用环境，我们选择在 <mark style="background: #FFB86CA6;">ubuntu 24.04</mark> 系统环境中，基于 <mark style="background: #FFB86CA6;">OpenFOAM 2406</mark> 版本进行开发讨论，方便起见使用 vscode 工具。

> [!caution]
> - openfoam.com 的版本变化较小，较新的版本均适合本系列讨论使用
> - openfoam.org 的版本架构大改，暂不推荐入门


## 4.建议

> [!tip]
> - 建议读者动手跟随讨论编程操作
