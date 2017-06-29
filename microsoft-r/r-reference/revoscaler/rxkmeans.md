--- 
 
# required metadata 
title: "K-Means Clustering" 
description: " Perform k-means clustering on small or large data. " 
keywords: "RevoScaleR, rxKmeans, print.rxKmeans, cluster, multivariate, clustering" 
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
 
 
 
 #`rxKmeans`: K-Means Clustering

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Perform k-means clustering on small or large data.
 
 
 ##Usage

```   
  rxKmeans(formula, data, 
           outFile = NULL, outColName = ".rxCluster", 
           writeModelVars = FALSE, extraVarsToWrite = NULL,
           overwrite = FALSE, numClusters = NULL, centers = NULL, 
           algorithm = "Lloyd", numStartRows = 0, maxIterations = 1000, 
           numStarts = 1, rowSelection = NULL, 
           transforms = NULL, transformObjects = NULL,
           transformFunc = NULL, transformVars = NULL, 
           transformPackages = NULL, transformEnvir = NULL,
           blocksPerRead = rxGetOption("blocksPerRead"),
           reportProgress = rxGetOption("reportProgress"), verbose = 0,
           computeContext = rxGetOption("computeContext"),
           xdfCompressionLevel = rxGetOption("xdfCompressionLevel"), ...)
  
 ## S3 method for class `rxKmeans':
print  (x, header = TRUE, ...)
 
```
 
 ##Arguments

   
    
 ### `formula`
 formula as described in [rxFormula](rxformula.md). 
  
  
    
 ### `data`
 a data source object, a character string specifying a .xdf file, or a data frame object. 
  
  
    
 ### `outFile`
 either an RxXdfData data source object or a character string specifying the .xdf file for storing the resulting cluster indexes. If `NULL`, then no cluster indexes are stored to disk. Note that in the case that the input data is a data frame, the cluster indexes are returned automatically. Note also that, if `rowSelection` is specified and not `NULL`, then `outFile` cannot be the same as the `data` since the resulting set of cluster indexes will generally not have the same number of rows as the original data source. 
  
  
    
 ### `outColName`
 character string to be used as a column name for the resulting  cluster indexes if `outFile` is not `NULL`. Note that make.names is used on `outColName` to ensure that the column name is valid. If the `outFile` is an `RxOdbcData` source, dots are first converted to underscores. Thus, the default `outColName` becomes `"X_rxCluster"`. 
  
  
    
 ### `writeModelVars`
 logical value. If `TRUE`, and the output file is different from the input file, variables in the model will be written to the output file in addition to the cluster numbers. If variables from the input data set are transformed in the model, the transformed variables will also be written out. 
  
  
    
 ### `extraVarsToWrite`
 `NULL` or character vector of additional variables names from the input data to include in the `outFile`.  If `writeModelVars` is `TRUE`, model variables will be included as well. 
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, an existing `outFile` with an existing column named `outColName` will be overwritten. 
  
  
    
 ### `numClusters`
 number of clusters `k` to create. If `NULL`, then the `centers`argument must be specified. 
  
  
    
 ### `centers`
 a `k x p` numeric matrix containing a set of initial (distinct) cluster centers. If `NULL`, then the `numClusters` argument must be specified. 
  
  
    
 ### `algorithm`
 character string defining algorithm to use in defining the clusters. Currently supported algorithms are `"Lloyd"`. This argument is case insensitive. 
  
  
    
 ### `numStartRows`
 integer specifying the size of the sample used to choose initial centers. If 0, (the default), the size is chosen as the minimum of the number of observations or 10 times the number of clusters. 
  
  
    
 ### `maxIterations`
 maximum number of iterations allowed. 
  
  
    
 ### `numStarts`
 if `centers` is `NULL`, `k` rows are randomly selected from the data source  for use as initial starting points. The `numStarts` argument defines the number of these random sets that are  to be chosen and evaluated, and the best result is returned. If `numStarts` is 0, the first `k` rows in the data set are used.  Random selection of rows is only supported for .xdf data sources using the native file system and data frames. If the .xdf file is compressed, the random sample is taken from a maximum of the first 5000 rows of data.  
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### `transformFunc`
 variable transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
  
  
    
 ### `transformPackages`
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent  `baseenv()` is used instead. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### `reportProgress`
 integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 
  
  
  
 ### `computeContext`
 a valid [RxComputeContext](rxcomputecontext.md).  The `RxSpark`, `RxHadoopMR`, and `RxInTeradata` compute  contexts distribute the computation among the nodes specified by the  compute context; for other compute contexts, the  computation is distributed if possible on the local computer. 
  
  
   
     
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9 indicating the compression  level for the output data if written to an `.xdf` file.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the Revolution Compute Engine. 
  
  
    
 ### `x`
 object of class rxKmeans. 
  
  
    
 ### `header`
 logical value. If `TRUE`, header information is printed. 
  
 
 
 
 ##Details
 
Performs scalable k-means clustering using the classical Lloyd algorithm.

For reproducibility when using random starting values, you can pass a random seed
by specifying `seed=`*value* as part of your call. See the Examples.
 
 
 
 ##Value
 
An object of class "rxKmeans" which is a list with components: 


###`cluster`
A vector of integers indicating the cluster to which each point is allocated. This information is always returned if the data source is a data frame. If the data source is not a data frame and `outFile` is specified. i.e., not `NULL`, the the cluster indexes are written/appended to the specified file with a column name as defined by `outColName`.



###`centers`
matrix of cluster centers.
 

###`withinss`
within-cluster sum of squares (relative to the center) for each cluster.


###`totss`
total within-cluster sum of squares.
 

###`tot.withinss`
sum of the `withinss` vector.


###`betweenss`
between-cluster sum of squares.
 

###`size`
number of points in each cluster.


###`valid.obs`
number of valid observations.


###`missing.obs`
number of missing observations.


###`numIterations`
number iterations performed.


###`params`
parameters sent to Microsoft R Services Compute Engine.


###`formula`
formula as described in [rxFormula](rxformula.md).


###`call`
the matched call.

 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##References
 
Lloyd, S. P. (1957, 1982) Least squares quantization in PCM. Technical Note, Bell Laboratories. 
Published in 1982 in *IEEE Transactions on Information Theory* 28, 128-137. 
 
 
 ##See Also
 
kmeans.
   
  
 ##Examples

 ```
   
  # Create data
  N <- 1000 
  sd1 <- 0.3
  mean1 <- 0 
  sd2 <- 0.5
  mean2 <- 1 
  set.seed(10)
  data <- rbind(matrix(rnorm(N, sd = sd1, mean = mean1), ncol = 2),  
                matrix(rnorm(N, mean = mean2, sd = sd2), ncol = 2)) 
  colnames(data) <- c("x", "y") 
  DF <- data.frame(data)
  XDF <- paste(tempfile(), "xdf", sep=".")
  if (file.exists(XDF)) file.remove(XDF)
  rxDataStep(inData = DF, outFile = XDF)
  
  centers <- DF[sample.int(NROW(DF), 2, replace = TRUE),] # grab 2 random rows for starting
  
  # Example using an XDF file as a data source
  rxKmeans(~ x + y, data = XDF, centers = centers)
  
  # Example using a local data frame file as a data source
  z <- rxKmeans(~ x + y, data = DF, centers = centers)
  
  # Show a plot of the results
  # By design, the data in 2-space populates two groups of points centered about 
  # points (0,0) and (1,1). The spread about the mean is based on a random set of 
  # points drawn from a Gaussian distribution with standard deviations 0.3 and 0.5. 
  # As a visual, the resulting plot shows two circles drawn at the centers with radii 
  # equal to the corresponding standard deviations.
  plot(DF, col = z$cluster, asp = 1, 
       main = paste("Lloyd k-means Clustering: ", z$numIterations, "iterations"))
  symbols(mean1, mean1, circle = sd1, inches = FALSE, add = TRUE, fg = "black", lwd = 2) 
  symbols(mean2, mean2, circle = sd2, inches = FALSE, add = TRUE, fg = "red", lwd = 2)
  points(z$centers, col = 2:1, bg = 1:2, pch = 21, cex = 2) # big filled dots for centers
  
  # Example using randomly selected rows from data source as initial centers
  # but with seed set for reproducibility
  ## Not run:
 
z <- rxKmeans(~ x + y, data = DF, numClusters = 2, seed=18)
 ## End(Not run) 
  
  
  # Example using first rows from Spss data source as initial centers
  spssFile <- file.path(rxGetOption("sampleDataDir"),"claims.sav")                
  spssDS <- RxSpssData(spssFile, colClasses = c(cost = "integer"))                        
  resultSpss <- rxKmeans(~cost, data = spssDS, numClusters = 2, numStarts = 0)	
 
```
 
 
 
 
 
 
 
 
 
 
 
 
 
