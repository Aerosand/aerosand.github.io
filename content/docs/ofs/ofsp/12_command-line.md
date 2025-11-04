---
uid: 20251020195957
title: 12_commandLine
date: 2025-10-20
update: 2025-10-27
authors:
  - name: Aerosand
    link: https://github.com/aerosand
    image: https://github.com/aerosand.png
tags:
  - ofsp
  - ofsp2026
  - OpenFOAM
excludeSearch: false
toc: true
weight: 13
math: true
next:
prev:
comments: true
sidebar:
  exclude: false
draft: false
---
## 0. 前言

OpenFOAM 的很多应用都可以通过命令行来给定运行参数。虽然用户有时候会觉得有些陌生，但是用好命令行可以高效灵活的处理各种问题。

新同学很难不注意到求解器总是有几个固定的头文件，包括 `setRootCase.H` ，`createTime.H` 和 `createMesh.H` 。网络上找到的代码解析常常只有一句注释介绍功能，也许无法消除困惑感。

本文同样会从 C++ 开始，简单介绍命令行参数，然后进一步讨论 `setRootCase.H` 头文件。

本文主要讨论


## 1. OpenFOAM 命令行

参考阅读：https://doc.openfoam.com/2312/fundamentals/command-line/

OpenFOAM 命令行基础的使用格式如下

```terminal {fileName="terminal"}
<application> <options> <arguments>
```

比如，实践中有典型的命令行使用如下

```terminal {fileName="terminal"}
blockMesh -case debug_case
```

使用 `-help` 选项可以查看更多的命令行选项，例如终端输入 `blockMesh -help` ，终端会输出如下内容

```terminal {fileName="terminal"}
Usage: blockMesh [OPTIONS]
Options:
  -case <dir>       Case directory (instead of current directory)
  -dict <file>      Alternative blockMeshDict
  -merge-points     Geometric point merging instead of topological merging
                    [default for 1912 and earlier].
  -no-clean         Do not remove polyMesh/ directory or files
  -region <name>    Specify mesh region (default: region0)
  -sets             Write cellZones as cellSets too (for processing purposes)
  -time <time>      Specify a time to write mesh to (default: constant)
  -verbose          Force verbose output. (Can be used multiple times)
  -write-vtk        Write topology as VTU file and exit
  -doc              Display documentation in browser
  -help             Display short help and exit
  -help-compat      Display compatibility options and exit
  -help-full        Display full help and exit

Block mesh generator.

  The ordering of vertex and face labels within a block as shown below.
  For the local vertex numbering in the sequence 0 to 7:
    Faces 0, 1 (x-direction) are left, right.
    Faces 2, 3 (y-direction) are front, back.
    Faces 4, 5 (z-direction) are bottom, top.

                        7 ---- 6
                 f5     |\     :\     f3
                 |      | 4 ---- 5     \
                 |      3.|....2 |      \
                 |       \|     \|      f2
                 f4       0 ---- 1
    Y  Z
     \ |                f0 ------ f1
      \|
       o--- X

Using: OpenFOAM-2406 (2406) - visit www.openfoam.com
Build: _9bfe8264-20241212 (patch=241212)
Arch:  LSB;label=32;scalar=64
```


一般，数据处理和后处理的时候也会大量使用命令行。

例如，使用

```
// terminal
foamPostProcess -help
```

具体的使用技巧暂不展开，以后会专门讨论。

我们先简单回顾一下C++中命令行参数的使用。


## 2. C++ 实现

### 2.1. 项目准备

终端输入命令，建立项目

```terminal {fileName="terminal"}
ofsp
mkdir ofsp_12_arg
code ofsp_12_arg
```

通过 vscode ，使用 F1 输入 `Create C++ project` ，输入 `ofsp_12_arg` 作为项目名称，选择 `/ofsp` 作为父目录，初始化项目。

终端输入命令，测试初始项目

```terminal {fileName="terminal"}
make run
```

终端输出 `Hello world!` 表示初始项目运行成功。

### 2.2. 命令行

我们在初学 C++ 的时候，主函数的参数一般留空，即有如下形式

```cpp
...
int main() // 省略参数列表
{
	...
}
```

当进一步深入 C++ 开发时，了解到主函数的更一般写法为

```cpp
...
int main(int argc, char *argv[]) {}
// 或者
int main(int argc, char **argv) {}
```

其中

- `argc` 是 `argument count` 的缩写，保存程序运行时传递给主函数的参数个数
- `argv` 是 `argument vector` 的缩写，保存程序运行时传递给主函数的具体参数的字符型指针，每个指针都指向一个具体的参数。
	- `argv[0]` 指向程序运行时的全路径名称
	- `argv[1]` 指向程序运行时命令行中执行程序名后第一个字符串
	- `argv[2]` 指向程序运行时命令行中执行程序名后第二个字符串
	- 其他以此类推

### 2.3. 主源码

主源码 `src/main.cpp` 如下所示

```cpp {fileName="/src/main.cpp"}
#include <iostream>

int main(int argc, char *argv[])
{
    std::cout << "Number of arguments = " << argc << std::endl;

    for (int i=0; i<argc; ++i)
    {
        std::cout << "Argument " << i << ": "
            << argv[i] << std::endl;
    }

    return 0;
}
```

终端输入命令，编译运行

```terminal {fileName="terminal"}
make run
```

终端输出运行结果如下

```terminal {fileName="terminal"}
Number of arguments = 1
Argument 0: ./output/main
```

如果运行时增加参数，例如

```terminal {fileName="terminal"}
./output/main hi hey hello
```

终端输出运行结果如下

```terminal {fileName="terminal"}
Number of arguments = 4
Argument 0: ./output/main
Argument 1: hi
Argument 2: hey
Argument 3: hello
```


通过运行结果可以看到，`argc` 当然就是参数的总个数，`argv[0]`则是也就是应用名称本身，其他参数按顺序类推。

比如，对于以下命令行

```terminal {fileName="terminal"}
blockMesh -case debug_case
```

其中，`argc` 等于 `3`，而 `argv[0]` 是`blockMesh`，`argv[1]` 是 `-case`，`argv[2]` 是 `debug_case` 。

总结来说，命令行中的每一个参数其实都可以在程序中通过主函数参数被调用，以便参加程序运行和计算。

