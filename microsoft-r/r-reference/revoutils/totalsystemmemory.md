--- 

# required metadata 
title: " Obtain Total System Memory " 
description: " Uses operating system tools to return total system memory. " 
keywords: ", totalSystemMemory,  sysdata " 
Author: "dphansen"
ms.author: "davidph" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
#ms.technology: "" 
#ms.custom: "" 

--- 


 # totalSystemMemory:  Obtain Total System Memory  
 ## Description

Uses operating system tools to return total system memory.


 ## Usage

```   
  totalSystemMemory()

```

 ## Details

On Unix-alikes, checks the `/proc/meminfo` file for the total memory. If the system call
returns an error, the function returns `NA`. On Windows
systems, uses `wmic` to obtain the `TotalVisibleMemorySize`.


 ## Value

A numeric value representing total system memory in kBytes, or `NA`.









 ## Examples

 ```

  totalSystemMemory()
```






