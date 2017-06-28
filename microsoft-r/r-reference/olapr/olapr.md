--- 
 
# required metadata 
title: "OlapR: OLAP Cube access in R" 
description: "A package that allows for data to be imported from OLAP cubes stored in SQL Server Analysis Services into R." 
keywords: "olapR" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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

 Applies to version 1.0.0 of package olapR. This package is in prerelease.
 
 ##Description

**olapR** provides a simple R style API for generating and validating MDX queries against a SQL Server Analysis Services cube. olapR does not provide APIs for all MDX scenarios, but it does cover the most use cases, including slice, dice, drilldown, rollup, and pivot scenarios in N dimensions. You can also input a direct MDX query to Analysis Services for queries that cannot be constructed using the olapR APIs.  


> [!Important]
> olapR requires the Analysis Services OLE DB provider. If you do not have SQL Server Analysis Services installed on your computer, you can download the provider from Microsoft:
>[`https://msdn.microsoft.com/en-us/library/dn141152.aspx#Analysis Services OLE DB Provider`](https://msdn.microsoft.com/en-us/library/dn141152.aspx#Analysis Services OLE DB Provider)
>
>The exact version you should install for SQL Server 2016 is here:
>[`https://download.microsoft.com/download/8/7/2/872BCECA-C849-4B40-8EBE-21D48CDF1456/ENU/x64/SQL_AS_OLEDB.msi`](https://download.microsoft.com/download/8/7/2/872BCECA-C849-4B40-8EBE-21D48CDF1456/ENU/x64/SQL_AS_OLEDB.msi)
>
 
 ##Details

 | Item | Data |
| :---| :--- |
|  Package  |  olapR |
|  Type  |  Package |
|  Version  |  0.0.0.9001 |
|  License  |  file LICENSE |

 ##How to use olapR

To execute an MDX query on an OLAP Cube, you need to first create a connection string (`olapCnn`) and validate using the function `OlapConnection(connectionString)`. The connection string must have a Data Source (such as localhost) and a Provider (MSOLAP). 

After the connection is established, construct the query using the Query() object. Set the query details using cube(), axis(), columns(), slicers(), and so forth. 

Finally, pass the `olapCnn` and query into either `executeMD` or `execute2D` to get a multidimensional array or a data frame back.

OLAP (Online Analytical Processing) cubes are essentially multi-dimensional spreadsheets. "Cubes" can extend to any number of dimensions, and can be operated using the MDX (MultiDimensional Expression) query language. 

## Function library

|Function | Description |
|---------|-------------|
|[`OlapConnection`](olapconnection.md) |Create the connection string to access the Analysis Services Database. |
|[`Query`](../../olapr/packagehelp/query.md) |Construct a Query object to use on the Analysis Services Database. Use cube, axis, columns, rows, pages, chapters, slicers to add details to the query.|
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

##References
 
OLAP Cubes: 
[`https://en.wikipedia.org/wiki/OLAP_cube`](https://en.wikipedia.org/wiki/OLAP_cube)

MDX queries: 
[`https://en.wikipedia.org/wiki/MultiDimensional_eXpressions`](https://en.wikipedia.org/wiki/MultiDimensional_eXpressions)

Creating a Demo OLAP Cube (the same as the one used in the examples): 
[`https://msdn.microsoft.com/en-us/library/ms170208.aspx`](https://msdn.microsoft.com/en-us/library/ms170208.aspx)

## Get help on olapR functions from the R console

To see the **olapR** functions that can be called from the R console:

1. With Microsoft R Server or R Client installed, launch an R console with `Rgui.exe` or another preferred R IDE such as R Tools for Visual Studio.
2. Load `olapR` from the command line by typing `library(olapR)`.
1. In the console, open the package help by typing the following at the R prompt: `help(package="olapR")`.
1. In the help tab, review the list of functions for this package. Click a link to get the specific help page for that function.
 
> [!NOTE]
> To list all public functions, type library(help="olapR") at the R prompt.
>

## See also

[Package Reference](../../package-reference.md)

[Install R Server](../../rserver.md)

[Install R Client](../../r-client.md)