---
uid: 20250826191442
title: 05_basicClass
date: 2025-08-26
update: 2025-08-27
authors:
  - name: Aerosand
    link: https://github.com/aerosand
    image: https://github.com/aerosand.png
tags:
  - ofsp2026
  - OpenFOAM
excludeSearch: false
toc: true
weight: 5
math: true
next:
prev:
comments: true
sidebar:
  exclude: false
draft: false
---

## 0. 前言

前面我们已经了解了 C++ 原生实现、make 实现以及 OpenFOAM 的 wmake 实现。在正式进入到 OpenFOAM 实现之前，我们有必要稍微了解一点 OpenFOAM 中常见的类，以方便使用。

本文主要讨论

- [ ] 通过 OpenFOAM API 认识一些常见的类
- [ ] 认识 Vector 类
- [ ] 理解包含多个类的开发库
- [ ] 编译运行 tensor 程序
- [ ] 认识 `fvCFD.H`

## 1. 常见的类

我们可以通过官方 API  https://api.openfoam.com/2506/ 进行查询。

> [!tip]
> 改变上面网址的末尾版本好，可以打开对应版本的 API。

### Switch

可以通过 API 网站直接搜索 `Switch` 关键词找到 Switch Class Reference 页面，该页面提供了 Switch 类的相关内容，包括对应的 Source files 即 `Switch.H` 和 `Switch.C`。

也可以通过终端查找打开阅读源码。

终端输入命令，查找源码位置

```terminal {fileName="terminal"}
find $FOAM_SRC -iname switch.H
```

终端输出对应版本下的查找结果

```terminal {fileName="terminal"}
/usr/lib/openfoam/openfoam2406/src/OpenFOAM/primitives/bools/Switch/Switch.H
/usr/lib/openfoam/openfoam2406/src/OpenFOAM/lnInclude/Switch.H
```

基于上一篇的讨论，我们复制正确的路径，可以通过 vscode 本地阅读源码

```terminal {fileName="terminal"}
code /usr/lib/openfoam/openfoam2406/src/OpenFOAM/primitives/bools/Switch/Switch.H
```

> [!tip]
> 终端里的复制粘贴请使用 `ctrl + shift + c/v`

无论如何，`Switch.H` 部分代码如下

```cpp {fileName="Switch.H",link="https://develop.openfoam.com/Development/openfoam/blob/OpenFOAM-v2506/src/OpenFOAM/primitives/bools/Switch/Switch.H"}
/*---------------------------------------------------------------------------*\
  =========                 |
  \\      /  F ield         | OpenFOAM: The Open Source CFD Toolbox
   \\    /   O peration     |
    \\  /    A nd           | www.openfoam.com
     \\/     M anipulation  |
-------------------------------------------------------------------------------
    Copyright (C) 2011-2016 OpenFOAM Foundation
    Copyright (C) 2017-2023 OpenCFD Ltd.
-------------------------------------------------------------------------------
License
	...

Class
    Foam::Switch

Description
    A simple wrapper around bool so that it can be read as a word:
    true/false, on/off, yes/no, any/none.
    Also accepts 0/1 as a string and shortcuts t/f, y/n.

SourceFiles
    Switch.C

\*---------------------------------------------------------------------------*/

#ifndef Foam_Switch_H
#define Foam_Switch_H

#include "bool.H"
#include "stdFoam.H"

...
```

