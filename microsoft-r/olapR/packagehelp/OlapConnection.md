--- 
 
# required metadata 
title: "olapR OlapConnection Creation" 
description: "OlapConnection constructs an OlapConnection object. " 
keywords: "olapR, OlapConnection" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 #`OlapConnection`: olapR OlapConnection Creation

 Applies to version 1.0.0 of package olapR.
 
 
 ##Description
 
`OlapConnection` constructs a "OlapConnection" object.
 
 
 
 ##Usage

```   
  OlapConnection(connectionString="Data Source=localhost; Provider=MSOLAP;")
  
  is.OlapConnection(ocs)
  
  print.OlapConnection(ocs)
 
```
 
 
 ##Arguments

   
    
 ### `connectionString`
 A valid connection string for connecting to Analysis Services 
  
    
 ### `ocs`
 An object of class "OlapConnection" 
  
 
 
 
 ##Details
 
`OlapConnection` validates and holds an Analysis Services connection string.
 
 
 
 ##Value
 
`OlapConnection` returns an object of type "OlapConnection". A warning is shown if the connection string is invalid.
 
 
 ##References
  For more information on Analysis Services connection strings: [`https://msdn.microsoft.com/en-us/library/dn140245.aspx`](https://msdn.microsoft.com/en-us/library/dn140245.aspx)
  
 
 
 ##See Also
 
[Query](Query.md), [executeMD](ExecuteMD.md), [execute2D](Execute2D.md)`, `[explore](Explore.md)
   
 
 ##Examples

 ```
   
  cnnstr <- "Data Source=localhost; Provider=MSOLAP;"
  olapCnn <- OlapConnection(cnnstr)
 
```
 
