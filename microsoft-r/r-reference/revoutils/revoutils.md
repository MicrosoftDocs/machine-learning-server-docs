---

# required metadata
title: "RevoUtils Functions"
description: "A package providing utility functions useful for script running on the Microsoft R ScaleR engine."
keywords: "RevoUtils package reference"
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "04/02/2017"
ms.topic: "reference"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# RevoUtils functions

A package providing utility functions.

## Class library

|Class | Description |
|------|-------------|
|[`getRevoRepos`](getrevorepos.md) | Path to the package repositories. |
|[`readNews`](readnews.md)  | Read R or package NEWS files.|
|[`Revo.home`](revo-home.md)  | Path the Home directory. |
|[`RevoInfo`](revoinfo.md)  | Functions giving information about Microsoft R.|
|[`totalSystemMemory`](totalsystemmemory.md) |Uses operating system tools to return total system memory. |

## Get help on RevoUtils functions from the R console

To see the **RevoUtils** functions that can be called from the R console:

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe` or another preferred R IDE such as R Tools for Visual Studio.
2. Load `RevoUtils` from the command line by typing `library(RevoUtils)`.
1. In the console, open the package help by typing the following at the R prompt: `help(package="RevoUtils")`.
1. In the help tab, review the list of functions for this package. Click a link to get the specific help page for that function.
 
> [!NOTE]
> To list all public functions, type library(help="RevoUtils") at the R prompt.
>

## See also

[Package Reference](../introducing-r-server-r-package-reference.md)

[Install R Server](../../what-is-microsoft-r-server.md)

[Install R Client](../../r-client/what-is-microsoft-r-client.md)