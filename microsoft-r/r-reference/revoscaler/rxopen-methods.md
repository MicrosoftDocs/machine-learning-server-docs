--- 

# required metadata 
title: "rxOpen-methods methods (revoAnalytics) | Microsoft Docs" 
description: " These functions manage **RevoScaleR** data source objects. " 
keywords: "(revoAnalytics), rxOpen-methods, rxOpen, rxClose, rxIsOpen, rxReadNext, rxWriteNext, rxClose-methods, rxIsOpen-methods, rxReadNext-methods, rxWriteNext-methods, rxOpen,RxDataSource-method, rxClose,RxDataSource-method, rxIsOpen,RxDataSource-method, rxReadNext,RxDataSource-method, rxWriteNext,data.frame,RxDataSource-method, methods, file, connection" 
author: "chuckheinzelman"
ms.author: "charlhe" 
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

















 # rxOpen-methods: Managing RevoScaleR Data Source Objects 
 ## Description

These functions manage **RevoScaleR** data source objects.


 ## Usage

```   
  rxOpen(src, mode = "r")
  rxClose(src, mode = "r")
  rxIsOpen(src, mode = "r")
  rxReadNext(src)
  rxWriteNext(from, to, ...)

```

 ## Arguments



 ### `from`
 data frame object. 


 ### `src`
 RxDataSource object. 


 ### `to`
 RxDataSource object. 


 ### `mode`
 character string specifying the mode (`r` or `w`) to open the file. 


 ### ` ...`
 any other arguments to be passed on. 



 ## Value

For `rxOpen` and `rxClose`, a logical indicating whether the operation
was successful.
For `rxIsOpen`, a logical indicating whether or not the RxDataSource is
open for the specified `mode`.
For `rxReadNext`, either a data frame or a list depending upon the value of
the `returnDataFrame` property within `src`.


 ## Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


 ## See Also

[rxNewDataSource](rxNew.md),
[RxXdfData](RxXdfData.md).

 ## Examples

 ```

  ds <- RxXdfData(file.path(rxGetOption("sampleDataDir"), "claims.xdf"))
  # ds contains only one block of data
  rxOpen(ds) # must open the file before rxReadNext
  rxReadNext(ds) # get the first block
  rxReadNext(ds)
  rxClose(ds)

  # Use a data source to compute means by processing the data in chunks
  # Data processing functions: for each chunk, compute sums of columns and
  # number of rows, then update the results computed from previous chunks
  processData <- function(dframe)
    list(sumCols = colSums(dframe), numRows = nrow(dframe))
  updateResults <- function(x, y)
    list(sumCols = x$sumCols + y$sumCols, numRows = x$numRows + y$numRows)

  # Create data source
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
  ds <- RxXdfData(censusWorkers, varsToKeep = c("age", "incwage"),
                  blocksPerRead = 2)

  # Process data and update results
  rxOpen(ds)
  resList <- processData(rxReadNext(ds))
  while(TRUE)
  {
      df <- rxReadNext(ds)
      if (nrow(df) == 0)
          break
      resList <- updateResults(resList, processData(df))
  }
  rxClose(ds)

  # Compute the means of the variables from the accumulated results
  varMeans <- resList$sumCols / resList$numRows
  varMeans
```




