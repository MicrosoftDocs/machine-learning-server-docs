--- 
 
# required metadata 
title: " Obtain Total System Memory " 
description: " Uses operating system tools to return total system memory. " 
keywords: ", totalSystemMemory,  sysdata " 
author: "richcalaway"
ms.author: "richcala" 
manager: "jhubbard" 
ms.date: "03/27/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
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
 
 
 #`totalSystemMemory`:  Obtain Total System Memory  
 ##Description
 
Uses operating system tools to return total system memory.
 
 
 ##Usage

```   
  totalSystemMemory()
 
```
 
 ##Details
 
On Unix-alikes, checks the `/proc/meminfo` file for the total memory. If the system call
returns an error, the function returns `NA`. On Windows
systems, uses `wmic` to obtain the `TotalVisibleMemorySize`.
 
 
 ##Value
 
A numeric value representing total system memory in kBytes, or `NA`.
 
 ##Author(s)
 
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
 
 ##Examples

 ```
   
  totalSystemMemory()
 
```
 
 
 
 
 
 
