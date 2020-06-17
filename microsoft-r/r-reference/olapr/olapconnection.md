--- 

# required metadata 
title: "OlapConnection function (olapR) | Microsoft Docs" 
description: "   OlapConnection constructs a OlapConnection object. " 
keywords: "(olapR), OlapConnection" 
author: "dphansen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 




 # OlapConnection: olapR OlapConnection Creation 

 ## Description

`OlapConnection` constructs a "OlapConnection" object.



 ## Usage

```   
  OlapConnection(connectionString="Data Source=localhost; Provider=MSOLAP;")

  is.OlapConnection(ocs)

  print.OlapConnection(ocs)

```


 ## Arguments



 ### `connectionString`
 A valid connection string for connecting to Analysis Services 


 ### `ocs`
 An object of class "OlapConnection" 




 ## Details

`OlapConnection` validates and holds an Analysis Services connection string. By default, Analysis Services returns the first cube of the first database. To connect to a specific database, use the Initial Catalog parameter.



 ## Value

`OlapConnection` returns an object of type "OlapConnection". A warning is shown if the connection string is invalid.


 ## References
  For more information on Analysis Services connection strings: [`https://docs.microsoft.com/sql/analysis-services/instances/connection-string-properties-analysis-services`](https://docs.microsoft.com/sql/analysis-services/instances/connection-string-properties-analysis-services)



 ## See Also

[Query](Query.md), [executeMD](ExecuteMD.md), [execute2D](Execute2D.md)`, `[explore](Explore.md)


 ## Examples

 ```


  # Create the connection string. For a named instance, escape the instance name: localhost\my-other-instance
  cnnstr <- "Data Source=localhost; Provider=MSOLAP; initial catalog=AdventureWorksCube"
  olapCnn <- OlapConnection(cnnstr)
```

