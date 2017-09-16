---

# required metadata
title: "ScaleR Functions"
description: "ScaleR Functions"
keywords: "RevoScaleR, RevoScaleR"
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "04/18/2017"
ms.topic: "reference"
ms.prod: "microsoft-r"

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

# Comparison of Base R and RevoScaleR Functions

This topic provides a list of the functions provided by the **RevoScaleR** package and lists comparable functions included in the base distribution of R.  

##  <a name="bkmk_DataInputAndOutput"></a> Data Input and Output  

|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxGetInfo|Retrieves header information from an .XDF file or summary information from a data frame|`str()`<br /><br /> `names()`<br /><br /> `colNames()`|  
|rxGetVarInfo|Retrieves variable information from an .XDF file or data frame|`names()`<br /><br /> `str()`<br /><br /> `nrow()`<br /><br /> `min()`<br /><br /> `max()`|  
|RxSasData|Creates a SAS data source object|`foreign::read.ssd()`|  
|RxSpssData|Creates an SPSS data source object|`foreign::read.ssps()`|    
|rxOpen|Opens a data source for reading|`read.table()` etc.|  
|rxReadNext|Reads data from a data source|`read.table()`, etc.|  

##  <a name="bkmk_DataManipulation"></a> Data Manipulation  and Chunking  

|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxDataStep|Transforms and subsets data in .XDF files or data frames|`transform()`<br /><br /> `with()`<br /><br /> `within()`<br /><br /> `subset()`|  
|rxFactors|Recodes a factor variable, or converts a non\-factor variable into a factor|`factor()`|  
|rxSort|Performs multi\-key sorting of the variables in an .XDF file or data frame|`sort()`<br /><br /> `order()`|  
|rxMerge|Merges two .XDF files or two data frames using a variety of merge types|`merge()`<br /><br /> `rbind()`<br /><br /> `cbind()`|  
|rxSplit|Splits an .XDF file or a data frame into multiple .XDF files or data frames|`split()`|  

##  <a name="bkmk_DescriptiveStatistics"></a> Descriptive Statistics and Cross\-Tabulation  

|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxSummary|Generates summary statistics for a data frame, including computations by group|`summary()`<br /><br /> `lapply(x, …)`|  
|rxQuantile|Computes approximate quantiles for an .XDF file or data frame without sorting|`quantile()`|  
|rxCrossTabs|Creates a cross\-tabulation  of data based on a formula provided as parameter|`xtabs()`|  
|rxCube|Creates a cross\-tabulation of data based on formula provided as parameter<br /><br /> This function is an alternative to `rxCrossTabs` and is designed for efficient representation.|`xtabs()`|  
|rxMarginals|Creates a marginal summary for an **xtab** object|`addmargins()`<br /><br /> `colSums()`<br /><br /> `rowSums()`|
|as.crosstabs|Converts cross tabulation results to an **xtab** object|`xtabs()`|  
|rxChiSquaredTest|Performs a chi\-squared test on an **xtab** object|`chisq.test()`|  
|rxFisherTest|Performs Fisher's Exact Test on an **xtab** object|`fisher.test()`|  
|rxKendallCor|Computes Kendall's Tau Rank Correlation Coefficient using an **xtab** object|`cor(…, method="kendall")`|  

##  <a name="bkmk_StatisticalModeling"></a> Statistical Modeling  

|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxLinMod|Fits a linear model to data|`lm()`|  
|rxCovCor|Calculates the covariance, correlation, or sum of squares \(cross\-product\) matrix for a set of variables|`cor()`<br /><br /> `cov()`<br /><br /> `crossprod()`|  
|rxCov|Calculates the covariance matrix for a set of variables|`cov()`|  
|rxCor|Calculates the correlation matrix for a set of variables|`cov()`|  
|rxLogit|Fits a logistic regression model to data|`glm(…, family="binomial")`|   
|rxGlm|Fits a generalized linear model to data|`glm()`|  
|rxDTree|Fits a classification or regression tree to data|`tree::tree()`<br /><br /> `rpart::rpart()`|  
|rxPredict|Calculates predictions for fitted models|`predict()`|  
|rxKmeans|Performs K\-means clustering|`cluster::kmeans()`|  



##  <a name="bkmk_BasicGraphing"></a> Basic Graphing  


|**rx function**|**Description**|**Nearest base R function**|  
|-|-|-|  
|rxHistogram|Creates a histogram from data|`hist()`|  
|rxLinePlot|Creates a line plot from data|`plot()`<br /><br /> `lines()`|  


## See Also  
 [SQL Server R Services Features and Tasks](https://msdn.microsoft.com/en-us/library/mt590811.aspx)  
