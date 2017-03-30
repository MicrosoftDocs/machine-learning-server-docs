---

# required metadata
title: "Quick start: Parallel processing on partitioned data with rxExecBy"
description: "Quick start for Microsoft R: Parallel processing on partitioned data with rxExecBy"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/29/2017"
ms.topic: "get-started-article"
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

# Quick start: Parallel processing on partitioned data with rxExecBy

You can partition an arbitrary dataset into thousands or millions of partitions, and then iterate over each partition using almost any user-defined or analytical or statistical function from the collection of Microsoft R packages. The new `rxExecBy` function in RevoScaleR streamlines this task by scanning, sorting, and grouping data into partitions, and then calling whatever function you have specified over each partition. Inputs include a data source, a key used to group and partition data, and a function used to perform an operation.

Consider the airline dataset with flight delay data values for multiple airports, over multiple years. In just the small dataset alone, there are over #### data points. Suppose you wanted to understand the flight delays by day of the week, for example, the average delay for Mondays, Tuesdays, and so forth. The script in this tutorial shows you how to accomplish this task using the new `rxExecBy` function.

## Prerequisites

R Server or R Client

Tools and sample data

## Load, process, and return results

TBD

## See Also

TBD