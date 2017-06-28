--- 
 
# required metadata 
title: " Function to generate a 'colInfo' list from a data source " 
description: " Generates a `colInfo` list from a data source that can be used in `rxImport` or an `RxDataSource` constructor. " 
keywords: "RevoScaleR, rxCreateColInfo, file, connection" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 #`rxCreateColInfo`:  Function to generate a 'colInfo' list from a data source 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Generates a `colInfo` list from a data source that can be used in `rxImport` or
an `RxDataSource` constructor.
 
 
 ##Usage

```   
  rxCreateColInfo(data, includeLowHigh = FALSE, factorsOnly = FALSE, 
       varsToKeep = NULL, sortLevels = FALSE, computeInfo = TRUE,
        useFactorIndex = FALSE)
 
```
 
 ##Arguments

   
    
 ### `data`
  An [RxDataSource](../../scaler/packagehelp/rxdatasource.md) object, a character string containing an .xdf file name, or a data frame.  An object returned from [rxGetVarInfo](../../scaler/packagehelp/rxgetvarinfoxdf.md) is also supported.  
  
    
 ### `includeLowHigh`
  If `TRUE`, the low/high values will be included in the `colInfo` object.  Note that this will override any actual low/high values in the data set if the `colInfo` object is applied to a different data source.  
  
    
 ### `factorsOnly`
  If `TRUE`, only column information for factor variables will be included in the output.  
  
  
    
 ### `varsToKeep`
  `NULL` to include all variables, or character vector of variables to include.  
  
  
    
 ### `sortLevels`
  If `TRUE`, factor levels will be sorted. If factor levels represent integers, they will be put in numeric order.    
  
    
 ### `computeInfo`
  If `TRUE`, a pass through the data will be taken for non-xdf data sources in order to compute factor levels and low/high values.  
  
    
 ### `useFactorIndex`
  If `TRUE`, the `factorIndex` variable type will be used instead of `factor`.  
  
 
 
 ##Details
 
This function can be used to ensure consistent factor levels when importing a series of text files to xdf.
It is also useful for repeated analysis on non-xdf data sources.
 
 
 ##Value
 
A `colInfo` list that can be used as input for [rxImport](../../scaler/packagehelp/rximport.md) and in data sources such as
[RxTextData](../../scaler/packagehelp/rxtextdata.md) and [RxSqlServerData](../../scaler/packagehelp/rxsqlserverdata.md).
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](http://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[RxDataSource-class](../../scaler/packagehelp/rxdatasource-class.md),
[RxTextData](../../scaler/packagehelp/rxtextdata.md),
[RxSqlServerData](../../scaler/packagehelp/rxsqlserverdata.md),
[RxSpssData](../../scaler/packagehelp/rxspssdata.md),
[RxSasData](../../scaler/packagehelp/rxsasdata.md),
[RxOdbcData](../../scaler/packagehelp/rxodbcdata.md),
[RxTeradata](../../scaler/packagehelp/rxteradata.md),
[RxXdfData](../../scaler/packagehelp/rxxdfdata.md),
[rxImport](../../scaler/packagehelp/rximport.md).
   
 ##Examples

 ```
   
  # Get the low/high values and factor levels before using a data source
  # for import or analysis
  
  # Create a text data source, specifying the 'yearsEmploy' should be a factor
  mort1 <- file.path(rxGetOption("sampleDataDir"), "mortDefaultSmall2000.csv")
  mort1DS <- RxTextData(file = mort1, colClasses = c(yearsEmploy = "factor", default = "logical"))
  
  # By default, rxCreateColInfo will make a pass through the data to compute factor levels
  # and low/high values.  We'll also request that the levels be sorted
  mortColInfo <- rxCreateColInfo(data = mort1DS, includeLowHigh = TRUE, sortLevels = TRUE)
  
  # Re-create the data source, now using the computed colInfo
  mort1DS <- RxTextData(file = mort1, colInfo = mortColInfo)
  # Import the data
  mort1DF <- rxImport(mort1DS)
  levels(mort1DF$yearsEmploy)
  
  # Or use the text data source directly in an analysis 
  # (not needing a pass through the data to compute the factor levels)
  logitObj <- rxLogit(default~yearsEmploy, data = mort1DS)
  
  ##############################################################################################
  
  # Train a model on one imported data set, then score using another
  # Train a model on the first year of the data, importing it from text to a data frame
  
  mort1 <- file.path(rxGetOption("sampleDataDir"), "mortDefaultSmall2000.csv")
  mort1DS <- RxTextData(file = mort1, colClasses = c(yearsEmploy = "factor", default = "logical"))
  # Since we haven't specified factor levels, they will be created 'first come, first serve'
  mort1DF <- rxImport(mort1DS)
  levels(mort1DF$yearsEmploy)
  # Estimate a logit model
  logitObj <- rxLogit(default~yearsEmploy, data = mort1DF)
  
  # Now import the second year of data
  mort2 <- file.path(rxGetOption("sampleDataDir"), "mortDefaultSmall2001.csv")
  mort2DS <- RxTextData(file = mort2, colClasses = c(yearsEmploy = "factor", default = "logical"))
  mort2DF <- rxImport(mort2DS)
  # The levels are in a different order
  levels(mort2DF$yearsEmploy)
  
  # If we try to use the model estimated from the first data set to predict on the seoond,
  # predOut <- rxPredict(logitObj, data = mort2DF)
  # We will get an error
  #ERROR: order of factor levels in the data are inconsistent with
  #the order of the model coefficients
  
  # Instead, we can extract the colInfo from the first data set
  mortColInfo <- rxCreateColInfo(data = mort1DF)
  # And use it when importing the second
  mort2DS <- RxTextData(file = mort2, colInfo = mortColInfo)
  mort2DF <- rxImport(mort2DS)
  predOut <- rxPredict(logitObj, data = mort2DF)
  head(predOut)
  
 
```
 
 
 
