--- 
 
# required metadata 
title: "selectColumns function (MicrosoftML) " 
description: " Selects a set of columns to retrain, dropping all others. " 
keywords: "(MicrosoftML), selectColumns, transform" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/13/2017" 
ms.topic: "reference" 
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
 
 
 
 
 #selectColumns: Selects a set of columns, dropping all others 
 ##Description
 
Selects a set of columns to retrain, dropping all others.
 
 
 ##Usage

```   
  selectColumns(vars, ...)
 
```
 
 ##Arguments

   
  
 ### `vars`
 Specifiies character vector or list of the names of the variables to keep. 
  
  
  
 ### ` ...`
 Additional arguments sent to compute engine. 
  
 
 
 ##Value
 
A `maml` object defining the transform.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
 
