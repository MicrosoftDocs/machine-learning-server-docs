---

# required metadata
title: "mrsdeploy Functions"
description: "mrsdeploy Functions"
keywords: "mrsdeploy package reference"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/20/2016"
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

# mrsdeploy Functions

The `mrsdeploy` package provides functions for establishing a remote session in a console application and for publishing and managing a Web service backed by an R code block or script that you provide. The package is installed as part of R Server 9.0 and R Client 9.0 on all supported platforms.

Command line interaction between local client and remote server requires `mrsdeploy` on both ends.

This topic presents a curated list of functions commonly used by Microsoft R users. These functions can be called directly from the command line. If you want to see the entire set of `mrsdeploy` functions, [follow these steps.](#findmore)

## Remote Execution Functions

## Web Service Functions


<a name="findmore"></a>
##See All Functions and Help Files

See the list of public functions and see the associated help pages using the following steps.

**To see the `mrsdeploy` functions that can be called from the command-line:**

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe` or another preferred R IDE such as R Tools for Visual Studio (RTVS).

1. In the console, return the number of objects by typing the following at the R prompt `>`:
   ```
   > search()
   ```

1. Identify the position of the object you are interested in. In the case of our example, mrsdeploy is in the fifth position.

   ![objects](../media/scaler-rconsole-obj.png)

1. At the R prompt, type `objects(<position>)` to reveal the set of functions such as:
   ```
   > objects(5)
   ```

1. Find the name of the function in which you are interested.

1. At the R prompt, type `?<function_name>` to open the help file for that function, such as:
   ```
   > ?rxXdfData
   ```
