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

# ScaleR Functions

The `RevoScaleR` package includes hundreds of functions you can use for data analysis, for high-powered and distributed computing, and when working with Hadoop. About 180 functions can be called directly from the command-line.


## Functions for Data Analysis

|![import](../media/scaler-puzzle1.png)|![import](../media/scaler-puzzle2.png)|![import](../media/scaler-puzzle3.png)|![import](../media/scaler-puzzle4.png)|![import](../media/scaler-puzzle5.png)|  
|-|-|-|-|-|
|**Import & Export**|**Manipulate, <br /> Clean, Chunk**|**Visualize**|**Analyze, Learn, <br /> Predict**|**???**|
|<!--COL-1-->[`rxImport()`](rxClose.md) <br /> [`rxXdfToText()`]() <br /> [`rxGetInfo()`]() <br /> [`rxSetInfo()`]() <br /> [`rxGetVarInfo()`]() <br /> [`rxSetVarInfo()`]() <br /> [`rxGetVarNames()`]() <br /> [`rxCompressXdf()`]() <br /> [`RxXdfData()`]()<br />[`RxTextData()`]() <br /> [`RxSasData()`]() <br /> [`RxSpssData()`]() <br /> [`RxOdbcData()`]()|<!--COL-2-->[`rxDataStep()`]() <br /> [`rxFactors()`]() <br /> [`rxSort()`]() <br /> [`rxMerge()`]() <br /> [`rxSplit()`]()|<!--COL-3--> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() |<!--COL-4-->_Descriptive Statistics <br/>& Cross-Tabulation:_ <br /> [`()`]() <br />[`()`]() <br /> [`()`]() <br /> <br /> _Statistical Modeling:_ <br /> [`()`]() <br /> <br /> _Prediction:_ <br /> [`rxPredict()`]() <br /> [`()`]()|<!--COL-5-->  |

<br>
<br>

|![import](../media/scaler-puzzle1.png)  |![import](../media/scaler-puzzle1.png)|  
|----------------------------------------|--------------------------------------|
|**Data Sources**                        |**Utility Functions**                 |
|<!--COL-1-->[`rxImport()`](rxClose.md)<br />[`rxXdfToText()`]()<br />[`rxGetInfo()`]()<br />[`rxSetInfo()`]()<br />[`rxGetVarInfo()`]()<br />[`rxSetVarInfo()`]()<br />[`rxGetVarNames()`]()<br />[`rxCompressXdf()`]()<br />[`RxXdfData()`]()<br />[`RxTextData()`]()<br />[`RxSasData()`]()<br />[`RxSpssData()`]()<br />[`RxOdbcData()`]()<br />|<!--COL-2-->[`()`]()<br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]() <br /> [`()`]()| 


<br>
<br>


## Functions for High Powered Computing & Distributed Computing

|![import](../media/scaler-puzzle1.png)|![import](../media/scaler-puzzle1.png)|  
|--------------------------------------|--------------------------------------|
|**Compute Context**                   |**Others**                            |
|<!--COL-1-->[`RxComputeContext()`]() <br /> [`RxForeachDoPar()`]() <br /> [`RxLocalParallel()`]() <br /> [`RxLocalSeq()`]() <br /> [`rxGetComputeContext()`](rxGetComputeContext.md) <br /> [`rxSetComputeContext()`](rxSetComputeContext.md)|<!--COL-2--> [`rxGetAvailableNodes()`]() <br /> [`rxGetNodeInfo()`]() <br /> [`rxPingNodes()`]() <br /> [`rxExec()`]() <br /> [`rxGetJobStatus()`]() <br /> [`rxGetJobResults()`]() <br /> [`rxGetJobOutput()`]() <br /> [`rxGetJobs()`]() <br /> [`rxLocateFile()`]()| 



##Learn More

Get the set of public functions and see the associated help pages using the following steps.

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
