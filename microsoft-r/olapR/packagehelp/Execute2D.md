--- 
 
# required metadata 
title: "olapR execute2D Methods" 
description: "Takes a Query object or an MDX string, and returns the result as a data frame. " 
keywords: "olapR, execute2D" 
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
 
 
 
 
 #`execute2D`: olapR execute2D Methods

 Applies to version 1.0.0 of package olapR.
 
 
 ##Description
 
Takes a Query object or an MDX string, and returns the result as a data frame.
 
 
 
 ##Usage

```   
  execute2D(olapCnn, query)
  execute2D(olapCnn, mdx)
 
```
 
 
 ##Arguments

   
    
 ### `olapCnn`
 Object of class "OlapConnection" returned by `OlapConnection()` 
  
    
 ### `query`
 Object of class "Query" returned by `Query()` 
  
    
 ### `mdx`
 String specifying a valid MDX query 
  
 
 
 
 ##Details
 
If a query is provided:
`execute2D` validates a query object (optional), generates an mdx query string from the query object, executes the mdx query across, and returns the result as a data frame.

If an MDX string is provided:
`execute2D` executes the mdx query , and returns the result as a data frame.
 
 
 
 ##Value
 
A data frame if the MDX command returned a result-set. 
`TRUE` and a warning if the query returned no data. 
An error if the query is invalid
 
 
 ##Note
 
Multi-dimensional query results are flattened to 2D using a standard flattening algorithm.
 
 
 
 ##References
 
Creating a Demo OLAP Cube (the same as the one used in the examples): 
[`https://msdn.microsoft.com/en-us/library/ms170208.aspx`](https://msdn.microsoft.com/en-us/library/ms170208.aspx)

 
 
 
 ##See Also
 
[Query](Query.md)`, `[OlapConnection](OlapConnection.md)`, `[executeMD](ExecuteMD.md)`, `[explore](Explore.md)`, `data.frame
   
 
 ##Examples

 ```
   
  cnnstr <- "Data Source=localhost; Provider=MSOLAP;"
  olapCnn <- OlapConnection(cnnstr)
  
  qry <- Query()
  
  cube(qry) <- "[Analysis Services Tutorial]"
  columns(qry) <- c("[Measures].[Internet Sales Count]", "[Measures].[Internet Sales-Sales Amount]")
  rows(qry) <- c("[Product].[Product Line].[Product Line].MEMBERS") 
  pages(qry) <- c("[Sales Territory].[Sales Territory Region].[Sales Territory Region].MEMBERS")
  
  result1 <- execute2D(olapCnn, qry)
  
  mdx <- "SELECT {[Measures].[Internet Sales Count], [Measures].[Internet Sales-Sales Amount]} ON AXIS(0), {[Product].[Product Line].[Product Line].MEMBERS} ON AXIS(1), {[Sales Territory].[Sales Territory Region].[Sales Territory Region].MEMBERS} ON AXIS(2) FROM [Analysis Services Tutorial]"
  
  result2 <- execute2D(olapCnn, mdx)
 
```
 
