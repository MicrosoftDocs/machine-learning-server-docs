--- 
 
# required metadata 
title: "olapR executeMD Methods" 
description: "Takes a Query object or an MDX string, and returns the result as a multi-dimensional array. " 
keywords: "olapR, executeMD" 
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
 
 
 
 
 #`executeMD`: olapR executeMD Methods

 Applies to version 1.0.0 of package olapR.
 
 
 ##Description
 
Takes a Query object or an MDX string, and returns the result as a multi-dimensional array.
 
 
 
 ##Usage

```   
  executeMD(olapCnn, query)
  executeMD(olapCnn, mdx)
 
```
 
 
 ##Arguments

   
    
 ### `olapCnn`
 Object of class "OlapConnection" returned by `OlapConnection()` 
  
    
 ### `query`
 Object of class "Query" returned by `Query()` 
  
    
 ### `mdx`
 String specifying a valid MDX query 
  
 
 
 
 ##Details
 
If a Query is provided:
`executeMD` validates a Query object (optional), generates an mdx query string from the Query object, executes the mdx query across an XMLA connection, and returns the result  as a multi-dimensional array.

If an MDX string is provided:
`executeMD` executes the mdx query across an XMLA connection, and returns the result  as a multi-dimensional array.
 
 
 
 ##Value
 
Returns a multi-dimensional array.
Returns an error if the Query is invalid.
 
 
 ##Note
 

 
 
 
 ##References
 
Creating a Demo OLAP Cube (the same as the one used in the examples): 
[`https://msdn.microsoft.com/en-us/library/ms170208.aspx`](https://msdn.microsoft.com/en-us/library/ms170208.aspx)

 
 
 
 ##See Also
 
[Query](Query.md), [OlapConnection](OlapConnection.md)`, `[execute2D](../../r-reference/olapr/execute2d.md)`, `[explore](Explore.md)`, `array
   
 
 ##Examples

 ```
   
  cnnstr <- "Data Source=localhost; Provider=MSOLAP;"
  olapCnn <- OlapConnection(cnnstr)
  
  qry <- Query()
  
  cube(qry) <- "[Analysis Services Tutorial]"
  columns(qry) <- c("[Measures].[Internet Sales Count]", "[Measures].[Internet Sales-Sales Amount]")
  rows(qry) <- c("[Product].[Product Line].[Product Line].MEMBERS") 
  pages(qry) <- c("[Sales Territory].[Sales Territory Region].[Sales Territory Region].MEMBERS")
  
  result1 <- executeMD(olapCnn, qry)
  
  mdx <- "SELECT {[Measures].[Internet Sales Count], [Measures].[Internet Sales-Sales Amount]} ON AXIS(0), {[Product].[Product Line].[Product Line].MEMBERS} ON AXIS(1), {[Sales Territory].[Sales Territory Region].[Sales Territory Region].MEMBERS} ON AXIS(2) FROM [Analysis Services Tutorial]"
  
  result2 <- executeMD(olapCnn, mdx)
 
```
 
