---

# required metadata
title: "ScaleR Functions"
description: "ScaleR Functions"
keywords: "RevoScaleR, ScaleR"
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "06/13/2016"
ms.topic: "article"
ms.prod: "microsoftr"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

# RevoScaleR Functions

The `RevoScaleR` package includes hundreds of functions you can use for data analysis, for high-performance and distributed computing, SQL Server, Hadoop, and Teradata. About 180 functions can be called directly from the command-line.

## Functions by Context

Learn about the `RevoScaleR` functions recommended for your product edition or compute context:

+ [Microsoft R Compute Contexts](scaler-fx-r-server.md)

+ [Computing on a Hadoop Cluster](scaler-fx-hadoop.md)

+ [Computing on a Teradata Datawarehouse](scaler-fx-teradata.md)

+ [Computing on SQL Server](functions-for-sql-server-data.md)


>[!IMPORTANT]
>These are not exhaustive lists of functions in the RevoScaleR package. If you want to see the entire set of functions,  [follow these steps.](#findmore)

<a name="findmore"></a>
##See All Functions and Help Files

See the list of public functions and see the associated help pages using the following steps.

**To see the `RevoScaleR` functions that can be called from the commands-line:**

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe`, R Tools for Visual Studio, RStudio, or another IDE. 

1. In the console, return the number of objects by typing the following at the R prompt `>`:
   ```
   > search()
   ```

1. Identify the position of the object you are interested in. In the case of our example, RevoScaleR is in the fifth position.

1. At the R prompt, type `objects(<position>)` to reveal the set of functions such as:
   ```
   > objects(5)
   ```

   ![objects](../media/scaler-rconsole-obj.png)

1. At the R prompt, type `?<function_name>` to open the help file for that function, such as:
   ```
   > ?rxXdfData
   ```
