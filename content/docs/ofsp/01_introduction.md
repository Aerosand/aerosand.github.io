---
uid: 20250722130605
title: 01_introduction
date: 2025-07-22
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
weight: 1
math: true
next: 
prev: 
comments: true
draft: false
---

> [!important] Visit https://aerosand.cc for recent updates.
> 

## 1. Preliminaries

 This series is designed to help the reader bridge the gap between "CFD Theory" and "OpenFOAM Getting Started".

> [warning] It is recommended to learn the fundamentals of Computational Fluid Dynamics and the Finite Volume Method before starting this series.
> 

## 2. Introduction

 What is OpenFOAM? The wiki explains it as follows

> OpenFOAM (for "Open-source Field Operation And Manipulation") is a C++ toolbox for the development of customized numerical solvers, and pre-/post- processing utilities for the solution of continuum mechanics problems, most prominently including computational fluid dynamics (CFD).
> 

 So we can use OpenFOAM to build C++-based solver applications that implement theories such as CFD.

## 3. Route

 We start with a simple implementation of a C++ program, get a brief understanding of compilation principles, take control of our project through make, move on to an understanding of OpenFOAM's wmake implementation, get to know OpenFOAM's basic program, and then gradually delve into the details of OpenFOAM's solver applications.

 {{% steps %}}

### Compilation Principles

1.  Compiling C++ Programs
2.  make manages program compilation
3.  wmake managed compilation
4.  OpenFOAM Application Building

### Data Interaction

1.  Input and output
2.  Command Line Arguments

### Base Classes

1.  Time
2.  Grid
3.  Field

### Solver

1.  Development Libraries
2.  First solver

### First look at the algorithms

1.  SIMPLE & PISO & PIMPLE Algorithms
2.  SIMPLE solver

 {{% /steps %}}

> [!note] Each section is explained with detailed code and operations.
> 

## 3. Environment and Tools

 Given the environment of OpenFOAM, we have chosen to develop this project in ubuntu 24.04 based on OpenFOAM version 2406, using the vscode tool for convenience.

> [caution]
> 
> - The version of [openfoam.com](http://openfoam.com/) has changed little, and the newer versions are suitable for this series.
> - The version of [openfoam.org](http://openfoam.org/) has had a major architectural change, and is not recommended as a starting point.

## 4. Recommendations

> [!tip]
> 
> - Suggest readers to follow the discussion of programming operations

<aside>
💡  Welcome to leave comments, feedback, suggestions and comments, sponsorship and reward. Feel free to leave comments, feedback, suggestions, opinions, and donations.

</aside>

![ Alipay](attachment:3be6af9a-4829-4dfd-997e-641dfd055ba9:alipay.jpg)

 Alipay