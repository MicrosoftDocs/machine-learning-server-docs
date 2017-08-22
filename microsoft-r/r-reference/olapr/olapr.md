--- 
 
# required metadata 
title: "OlapR package for R (Microsoft Machine Learning Server) | Microsoft Docs" 
description: "Function help reference for the olapR R package of Microsoft Machine Learning Server. It is used to import data from OLAP cubes stored in SQL Server Analysis Services into R." 
keywords: "olapR" 
author: "richcalaway"
ms.author: "richcala" 
manager: "jhubbard" 
ms.date: "07/05/2017" 
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
 
# olapR package for R

The **olapR** library provides functions for importing data from OLAP cubes stored in SQL Server Analysis Services into R. It is only available in R Server for Windows.

| Package details | |
|--------|-|
| Version: |  1.0.0 |
| Runs on: | [SQL Server 2016 R Services or SQL Server 2017 Machine Learning Services (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services) |
| Built on: | R 3.3.x (included when you [install a product](../introducing-r-server-r-package-reference.md#how-to-install) that provides this package).|

## How to use olapR

The **olapR** library provides a simple R style API for generating and validating MDX queries against a SQL Server Analysis Services cube. **olapR** does not provide APIs for all MDX scenarios, but it does cover the most use cases including slice, dice, drilldown, rollup, and pivot scenarios in N dimensions. You can also input a direct MDX query to Analysis Services for queries that cannot be constructed using the olapR APIs.  

**Workflow for using olapR**

To execute an MDX query on an OLAP Cube, you need to first create a connection string (`olapCnn`) and validate using the function `OlapConnection(connectionString)`. The connection string must have a Data Source (such as localhost) and a Provider (MSOLAP). 

After the connection is established, construct the query using the Query() object. Set the query details using cube(), axis(), columns(), slicers(), and so forth. 

Finally, pass the `olapCnn` and query into either `executeMD` or `execute2D` to get a multidimensional array or a data frame back.

OLAP (Online Analytical Processing) cubes are essentially multi-dimensional spreadsheets. "Cubes" can extend to any number of dimensions, and can be operated using the MDX (MultiDimensional Expression) query language. 

> [!Important]
> **olapR** requires the Analysis Services OLE DB provider. If you do not have SQL Server Analysis Services installed on your computer, you can download the provider from Microsoft:
>[`https://msdn.microsoft.com/en-us/library/dn141152.aspx#Analysis Services OLE DB Provider`](https://msdn.microsoft.com/library/dn141152.aspx#Analysis Services OLE DB Provider)
>
>The exact version you should install for SQL Server 2016 is here:
>[`https://download.microsoft.com/download/8/7/2/872BCECA-C849-4B40-8EBE-21D48CDF1456/ENU/x64/SQL_AS_OLEDB.msi`](https://download.microsoft.com/download/8/7/2/872BCECA-C849-4B40-8EBE-21D48CDF1456/ENU/x64/SQL_AS_OLEDB.msi)
>
 
## Class library

|Function | Description |
|---------|-------------|
|[`OlapConnection`](olapconnection.md) |Create the connection string to access the Analysis Services Database. |
|[`Query`](query.md) |Construct a Query object to use on the Analysis Services Database. Use cube, axis, columns, rows, pages, chapters, slicers to add details to the query.|
|[`executeMD`](executemd.md) |Takes a Query object or an MDX string, and returns the result as a multi-dimensional array. |
|[`execute2D`](execute2d.md)|Takes a Query object or an MDX string, and returns the result as a 2D data frame. |
|[`explore`](explore.md)|Allows for exploration of cube metadata. |

##MDX concepts

MDX is the query language for multidimensional OLAP cubes. Cube data can be accessed using a variety of operations:

* Slicing - Taking a subset of the cube by picking a value for one dimension, resulting in a cube that is one dimension smaller.

* Dicing - Creating a subcube by specifying a range of values on multiple dimensions.

* Drill-Down/Up - Navigate from more general to more detailed data ranges, or vice versa.

* Roll-up - Summarize the data on a dimension.

* Pivot - Rotate the cube.

MDX queries are similar to SQL queries but, because of the flexibility of OLAP databases, can contain up to 128 query axes. The first four axes are named for convenience: Columns, Rows, Pages, and Chapters. It's also common to just use two (Rows and Columns), as shown in the following example.

~~~~
SELECT {[Measures].[Internet Sales Count], [Measures].[Internet Sales-Sales Amount]} ON COLUMNS, 
{[Product].[Product Line].[Product Line].MEMBERS} ON ROWS
FROM [Analysis Services Tutorial]
WHERE [Sales Territory].[Sales Territory Country].[Australia]
~~~~

Using the Analysis Services Tutorial Olap cube, this MDX query selects the internet sales count and sales amount and places them on the Column axis. On the Row axis it places all possible values of the "Product Line" dimension. Then, using the WHERE clause (which is the slicer axis in MDX queries), it filters the query so that only the sales from Australia matter. Without the slicer axis, we would roll up and summarize the sales from all countries.
 
 ##olapR examples

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

## Next steps

Add R packages to your computer by running setup for R Server or R Client: 

+ [R Client](../r-client/what-is-microsoft-r-client.md) 
+ [R Server](../what-is-microsoft-r-server.md)

For help with MDX concepts:

+ OLAP Cubes: [https://en.wikipedia.org/wiki/OLAP_cube](https://en.wikipedia.org/wiki/OLAP_cube)

+ MDX queries: [https://en.wikipedia.org/wiki/MultiDimensional_eXpressions](https://en.wikipedia.org/wiki/MultiDimensional_eXpressions)

+ Creating a Demo OLAP Cube (identical to examples): [https://msdn.microsoft.com/en-us/library/ms170208.aspx](https://msdn.microsoft.com/en-us/library/ms170208.aspx)  

## See also

 [R Package Reference](../introducing-r-server-r-package-reference.md) 


