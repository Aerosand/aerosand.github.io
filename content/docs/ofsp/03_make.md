---
uid: 20250723185036
title: 03_make
date: 2025-07-23
update: 2025-07-23
authors:
  - name: Aerosand
    link: https://github.com/aerosand
    image: https://github.com/aerosand.png
tags:
  - ofsp2026
  - OpenFOAM
excludeSearch: false
toc: true
weight: 3
math: true
next: 
prev: 
comments: true
sidebar:
  exclude: false
draft: false
---

>[!note]
>上一篇讨论的编译过程虽然清晰，但是一步一步的执行十分繁琐

为了简化项目编译，以及兼顾理解编译细节，我们采用 make 工具来管理我们的开发项目。很多人也会使用 cmake 来构建项目，cmake 更加简洁高效，不过本质上也是基于 makefile 的。

我们可以为项目提供 makefile 文件，在 makefile 中描述所有执行步骤。这样只需要简单执行 makefile 文件就可以编译整个项目，大大方便调试运行。此外，在之前的项目里，所有代码文件都放在一起，十分不方便，所以我们对代码进行架构管理。

## 1. 项目

终端输入命令，建立本文项目

```terminal {fileName="terminal"}
ofsp
mkdir ofsp_031_make
code ofsp_031_make
```

继续使用终端命令或者使用 vscode 界面创建其他文件，最终文件结构如下

```terminal {fileName="terminal"}
tree
.
├── Aerosand
│   ├── Aerosand.cpp
│   ├── Aerosand.h
│   └── makefile
├── makefile
└── ofsp_031_make.cpp
```

>[!tip]
>此时可以认为我们建立了一个库 Aerosand，其中包含一个同名类 Aerosand

类 Aerosand 的声明 `Aerosand.h`，内容不变

```cpp {fileName="/Aersoand/Aerosand.h"}
#pragma once

class Aerosand
{
public:
    void setLocalTime(double t);
    double getLocalTime() const;


private:
    double localTime_;
};
```

类 Aerosand 的定义 `Aerosand.cpp`，内容不变

```cpp {fileName="/Aerosand/Aerosand.cpp"}
#include "Aerosand.h"

void Aerosand::setLocalTime(double t) {
    localTime_ = t;
}

double Aerosand::getLocalTime() const {
    return localTime_;
}
```

主源码 `ofsp_031_make.cpp` 需要修改头文件，其他内容不变

```cpp {fileName="/ofsp_031_make.cpp"}
#include <iostream>

#include "Aerosand/Aerosand.h"   // 因为路径变化，需要修改此行

using namespace std;

int main()
{
    int a = 1;
    double pi = 3.1415926;

    cout << "Hi, OpenFOAM!" << " Here we are." << endl;
    cout << a << " + " << pi << " = " << a + pi << endl;
    cout << a << " * " << pi << " = " << a * pi << endl;


    Aerosand mySolver;
    mySolver.setLocalTime(0.2);
    cout << "\nCurrent time step is : " << mySolver.getLocalTime() << endl;

    return 0;
}
```

## 2. make

自动化构建工具 make 可以根据 makefile 中的规则自动完成源代码的编译、链接过程。

makefile 文件的基本格式为

```makefile {fileName="makefile"}
<target>:  <support>
	<command>
```

基于前文对 C++ 项目编译原理的过程的讨论，我们可以为库 Aerosand 提供 makefile 文件，直白的给出编译规范

文件内容如下

```makefile {fileName="/Aerosand/makefile"}
Aerosand.o: Aerosand.cpp
	g++ -c -fPIC Aerosand.cpp -o Aerosand.o

libAerosand.so: Aerosand.o
	g++ -shared -fPIC Aerosand.o -o libAerosand.so 
```

