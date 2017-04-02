--- 
 
# required metadata 
title: "OlapR: OLAP Cube access in R" 
description: "   NOTE: This package is in prerelease.    **IMPORTANT:**    If you do not have SQL Server Analysis Services installed on your computer you MUST get the OLE DB driver described here:   [`https://msdn.microsoft.com/en-us/library/dn141152.aspx#Analysis Services OLE DB Provider`](https://msdn.microsoft.com/en-us/library/dn141152.aspx#Analysis Services OLE DB Provider)       The exact version you should install for SQL Server 2016 is here:   [`https://download.microsoft.com/download/8/7/2/872BCECA-C849-4B40-8EBE-21D48CDF1456/ENU/x64/SQL_AS_OLEDB.msi`](https://download.microsoft.com/download/8/7/2/872BCECA-C849-4B40-8EBE-21D48CDF1456/ENU/x64/SQL_AS_OLEDB.msi)       A package that allows for data to be imported from OLAP cubes stored in SQL Server Analysis Services into R.       OlapR specifically generates MDX queries for the user with a simple, R style API, allowing for easy query authoring and validation even without complete knowledge of the MDX language. Although OlapR does not provide this API for all MDX scenarios, all of the most common and useful slice, dice, drilldown, rollup, and pivot scenarios in N dimensions are available through OlapR.       For anything that is too complex for OlapR to handle, the user can input a direct MDX query and OlapR will handle passing and executing that query on the Analysis Services server. " 
keywords: "olapR" 
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
 
 
 
 #`olapR`: OlapR: OLAP Cube access in R

 Applies to version 1.0.0 of package olapR.
 
 
 ##Description
 
NOTE: This package is in prerelease.

**IMPORTANT:** 
If you do not have SQL Server Analysis Services installed on your computer you MUST get the OLE DB driver described here:
[`https://msdn.microsoft.com/en-us/library/dn141152.aspx#Analysis Services OLE DB Provider`](https://msdn.microsoft.com/en-us/library/dn141152.aspx#Analysis Services OLE DB Provider)


The exact version you should install for SQL Server 2016 is here:
[`https://download.microsoft.com/download/8/7/2/872BCECA-C849-4B40-8EBE-21D48CDF1456/ENU/x64/SQL_AS_OLEDB.msi`](https://download.microsoft.com/download/8/7/2/872BCECA-C849-4B40-8EBE-21D48CDF1456/ENU/x64/SQL_AS_OLEDB.msi)


A package that allows for data to be imported from OLAP cubes stored in SQL Server Analysis Services into R. 

OlapR specifically generates MDX queries for the user with a simple, R style API, allowing for easy query authoring and validation even without complete knowledge of the MDX language. Although OlapR does not provide this API for all MDX scenarios, all of the most common and useful slice, dice, drilldown, rollup, and pivot scenarios in N dimensions are available through OlapR. 

For anything that is too complex for OlapR to handle, the user can input a direct MDX query and OlapR will handle passing and executing that query on the Analysis Services server.
 
 
 
 ##Details
 
Package: olapR

Title: OLAP Cube access in R

Version: 0.0.0.9001

Description: OLAP Cube access in R

License: file LICENSE



To execute an MDX query on an Olap Cube, you need to first create a connection string (olapCnn) and validate using the function OlapConnection(connectionString). The connection string must have a Data Source (e.g. localhost) and a Provider (MSOLAP). 

Then, construct the query using the Query() object, and set the query details using cube(), axis(), columns(), slicers(), etc. 

Finally, pass the olapCnn and query into either executeMD or execute2D to get multidimensional array or a data frame back.

Overhead and Query Construction


* 
[OlapConnection](OlapConnection.md): Create the connection string to access the Analysis Services Database.

* 
[Query](Query.md): Construct a Query object to use on the Analysis Services Database. Use cube, axis, columns, rows, pages, chapters, slicers to add details to the query.



Execute


* 
[executeMD](ExecuteMD.md): Takes a Query object or an MDX string, and returns the result as a multi-dimensional array.

* 
[execute2D](Execute2D.md): Takes a Query object or an MDX string, and returns the result as a 2D data frame.



Explore


* 
[explore](Explore.md): Allows for exploration of cube metadata.


 
 
 
 ##Note
 
OLAP (Online Analytical Processing) cubes are essentially multi-dimensional spreadsheets. "Cubes" can extend to any number of dimensions, and can be operated using the MDX (MultiDimensional Expression) query language. 

Data in the cubes can be accessed using a variety of operations:


* 
Slicing - Taking a subset of the cube by picking a value for one dimension, resulting in a cube that is one dimension smaller

* 
Dicing - Creating a subcube by specifying a range of values on multiple dimensions

* 
Drill-Down/Up - Navigate from more general to more detailed data ranges, or vice versa

* 
Roll-up - Summarize the data on a dimension

* 
Pivot - Rotate the cube



MDX queries are similar to SQL queries but, because of the flexibility of OLAP databases, can comtain up to 128 query axes. The first four axes are named for convenience: Columns, Rows, Pages, and Chapters.

**Example MDX:**

SELECT {[Measures].[Internet Sales Count], [Measures].[Internet Sales-Sales Amount]} ON COLUMNS, 
{[Product].[Product Line].[Product Line].MEMBERS} ON ROWS
FROM [Analysis Services Tutorial]
WHERE [Sales Territory].[Sales Territory Country].[Australia]

Using the Analysis Services Tutorial Olap cube, this MDX query selects the internet sales count and sales amount and places them on the Column axis. On the Row axis it places all possible values of the "Product Line" dimension. Then, using the WHERE clause (which is the slicer axis in MDX queries), it filters the query so that only the sales from Australia matter. Without the slicer axis, we would roll up and summarize the sales from all countries.

 
 
 
 ##References
 
OLAP Cubes: 
[`https://en.wikipedia.org/wiki/OLAP_cube`](https://en.wikipedia.org/wiki/OLAP_cube)

MDX queries: 
[`https://en.wikipedia.org/wiki/MultiDimensional_eXpressions`](https://en.wikipedia.org/wiki/MultiDimensional_eXpressions)

Creating a Demo OLAP Cube (the same as the one used in the examples): 
[`https://msdn.microsoft.com/en-us/library/ms170208.aspx`](https://msdn.microsoft.com/en-us/library/ms170208.aspx)

 
 
 
 
 ##Examples

 ```
   
    cnnstr <- "Data Source=localhost; Provider=MSOLAP;"
    olapCnn <- OlapConnection(cnnstr)
    
    qry <- Query()
    
    cube(qry) <- "[Analysis Services Tutorial]"
    columns(qry) <- c("[Measures].[Internet Sales Count]", "[Measures].[Internet Sales-Sales Amount]")
    rows(qry) <- c("[Product].[Product Line].[Product Line].MEMBERS") 
    slicers(qry) <- c("[Sales Territory].[Sales Territory Country].[Australia]")
    
    result1 <- executeMD(olapCnn, qry)
    
    mdx <- "SELECT {[Measures].[Internet Sales Count], [Measures].[Internet Sales-Sales Amount]} ON AXIS(0), {[Product].[Product Line].[Product Line].MEMBERS} ON AXIS(1) FROM [Analysis Services Tutorial] WHERE [Sales Territory].[Sales Territory Country].[Australia]"
    
    result2 <- execute2D(olapCnn, mdx)
 
```
 
