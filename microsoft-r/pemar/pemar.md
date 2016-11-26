---

# required metadata
title: "PemaR Functions"
description: "PemaR function reference for the RevoPemaR package in Microsoft R."
keywords: "RevoPemaR, PemaR"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/26/2016"
ms.topic: "article"
ms.prod: "microsoft-r"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: "r-server"
ms.custom: ""

---

# PemaR Functions

The `RevoPemaR` package provides a framework for writing custom Parallel External Memory Algorithms (PEMA) for execution on R Server. An external memory algorithm is used to process data in chunks, in parallel, typically on different nodes of a server cluster. The results are then combined and processed at the end (or at the end of each iteration). The `RevoPemaR` package makes use of the R reference classes introduced by John Chambers in R 2.12.

This topic is a high-level description of package functionality. For step-by-step instructions on usage, see [Get started with PemaR](~/pemar-getting-started.md).

## Class library

|Class | Description |
|------|-------------|
|PemaBaseClass|A base reference class generator for parallel external memory algorithms.|
|setPemaClass|Returns a generator function for creating a parallel external memory algorithm reference class.|
|pemaCompute|Estimates a parallel external memory algorithm as described by a PEMA reference class object. |

<a name="findmore"></a>
##How to view function help in the package

R Packages often include embedded help pages, documenting the syntax and parameters of each function. To view the list of functions and associated help pages for `RevoPemaR`, follow these steps.

1. Launch an R console with `Rgui.exe` or start another preferred R IDE such as R Tools for Visual Studio (RTVS) or RStudio.
2. At the command line, type `library(RevoPemaR)`.
3. Type `help(RevoPemaR)`.


## See also

[Get started with PemaR](../pemar-getting-started.md)

[Package Help](../package-reference.md)

[Write custom chunking algorithms in ScaleR](../scaler-getting-started-4-write-chunking-algorithms.md)
