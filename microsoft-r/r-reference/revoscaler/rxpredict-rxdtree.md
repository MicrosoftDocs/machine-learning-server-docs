--- 
 
# required metadata 
title: "Prediction for Large Data Classification and Regression Trees" 
description: "     Calculate predicted or fitted values for a data set from an rxDTree object. " 
keywords: "RevoScaleR, rxPredict.rxDTree, models, tree, classif, regression, classification" 
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
 
 
 #`rxPredict.rxDTree`: Prediction for Large Data Classification and Regression Trees

 Applies to version 9.1.0 of package RevoScaleR.
 
 
 ##Description
 
Calculate predicted or fitted values for a data set from an rxDTree object.
 
 
 ##Usage

```   
  
 ## S3 method for class `rxDTree':
rxPredict  (modelObject, data = NULL, outData = NULL, 
      predVarNames = NULL, writeModelVars = FALSE, extraVarsToWrite = NULL, 
      append = c("none", "rows"), overwrite = FALSE,
      type = c("vector", "prob", "class", "matrix"), removeMissings = FALSE,
      computeResiduals = FALSE, residType = c("usual", "pearson", "deviance"), residVarNames = NULL,
      blocksPerRead = rxGetOption("blocksPerRead"), reportProgress = rxGetOption("reportProgress"),
      verbose = 0, xdfCompressionLevel = rxGetOption("xdfCompressionLevel"),
        ...  )
      
 
```
 
 ##Arguments

   
    
 ### `modelObject`
  object returned from a call to `rxDTree`. 
  
    
 ### `data`
  either a data source object, a character string  specifying a .xdf file, or a data frame object. 
  
    
 ### `outData`
  file or existing data frame to store predictions;  can be same as the input file or `NULL`.  If not `NULL`, must be an .xdf file if `data` is an .xdf file  or a data frame if `data` is a data frame. 
  
    
 ### `predVarNames`
  character vector specifying name(s) to give to the prediction results. 
  
  
    
 ### `writeModelVars`
  logical value. If `TRUE`, and the output file is different from the input file,  variables in the model will be written to the output file in addition to the predictions.  If variables from the input data set are transformed in the model,  the transformed variables will also be written out. 
  
  
    
 ### `extraVarsToWrite`
 `NULL` or character vector of additional variables names from the input data to include in the `outData`.  If `writeModelVars` is `TRUE`, model variables will be included as well. 
  
  
    
 ### `append`
  either `"none"` to create a new files or `"rows"` to append rows to an existing file.  If `outData` exists and `append` is `"none"`, the `overwrite` argument must be set to `TRUE`.  You can append only to [RxTeradata](rxteradata.md) data source. Ignored for data frames.    
  
  
    
 ### `overwrite`
  logical value. If `TRUE`, an existing `outData` will be overwritten.  `overwrite` is ignored if appending rows. Ignored for data frames.  
  
  
    
 ### `type`
  see predict.rpart for details. 
  
    
 ### `removeMissings`
  logical value.  If `TRUE`, rows with missing values are removed and  will not be included in the output data. 
  
    
 ### `computeResiduals`
  logical value. If `TRUE`, residuals are computed. 
  
    
 ### `residType`
  see residuals.rpart for details. 
  
    
 ### `residVarNames`
  character vector specifying name(s) to give to the residual results. 
  
  
    
 ### `blocksPerRead`
  number of blocks to read for each chunk of data  read from the data source. 
  
    
 ### `reportProgress`
  integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
 
  
    
 ### `verbose`
  integer value.  If `0`, no verbose output is printed during calculations.  Integer values from `1` to `4` provide increasing amounts of information are provided. 
  
    
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9 indicating the compression level for the output data if written to an `.xdf` file.  The higher the value, the greater the amount of compression - resulting in smaller files but a longer time to create them. If `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression will be used. 
   
  
    
 ### ` ...`
  additional arguments to be passed directly to the Microsoft R Services Compute Engine. 
  
 
 
 ##Details
 
Prediction for large data models requires both a fitted model object and a data set, either the
original data (to obtain fitted values and residuals) or a new data set containing the same set
of variables as the original fitted model. Notice that this is different from the behavior of
`predict`, which can usually work on the original data simply by referencing the fitted model.


 
 
 ##Value
 
Depending on the form of `data`, this function variously returns a data frame or a data source
representing a .xdf file.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##References
 
Breiman, L., Friedman, J. H., Olshen, R. A. and Stone, C. J. (1984)
*Classification and Regression Trees*.
Wadsworth.

Therneau, T. M. and Atkinson, E. J. (2011)
*An Introduction to Recursive Partitioning Using the RPART Routines*.
[`http://r.789695.n4.nabble.com/attachment/3209029/0/zed.pdf`](http://r.789695.n4.nabble.com/attachment/3209029/0/zed.pdf)
.

Yael Ben-Haim and Elad Tom-Tov (2010)
A streaming parallel decision tree algorithm.
*Journal of Machine Learning Research* **11**, 849--872. 
 
 
 ##See Also
 
rpart, rpart.control, rpart.object,
[rxDTree](rxdtree.md).
   
 ##Examples

 ```
   
  set.seed(1234)
  
  # classification
  iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
  iris.dtree <- rxDTree(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width, 
      data = iris[iris.sub, ], cp = 0.01)
  iris.dtree
  
  table(rxPredict(iris.dtree, iris[-iris.sub, ], type = "class")[[1]], 
      iris[-iris.sub, "Species"])
  
  # regression
  infert.nrow <- nrow(infert)
  infert.sub <- sample(infert.nrow, infert.nrow / 2)
  infert.dtree <- rxDTree(case ~ age + parity + education + spontaneous + induced, 
      data = infert[infert.sub, ], cp = 0.01)
  infert.dtree
         
  hist(rxPredict(infert.dtree, infert[-infert.sub, ])[[1]] - 
      infert[-infert.sub, "case"])
          
  
 
```
 
 
 
 
 
 
