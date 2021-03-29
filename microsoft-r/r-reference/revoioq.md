--- 
 
# required metadata 
title: "RevoIOQ function (RevoIOQ) - Machine Learning Server " 
description: "Installation and operation qualification of Machine Learning Server" 
keywords: "(RevoIOQ), RevoIOQ, package" 
author: "dphansen"
ms.author: "davidph" 
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
 
 
 # RevoIOQ package

 The **RevoIOQ** package provides a function for testing installation and operational qualification of R packages provided in Machine Learning Server, Microsoft R Server, and R Client.
 
| Package details | Description |
|--------|-|
| Current version: |  8.0.8 |
| Built on: | R 3.3.2 |
| Package distribution: | [Machine Learning Server](../what-is-machine-learning-server.md) </br>[R Client (Windows and Linux)](../r-client/what-is-microsoft-r-client.md) <br/>[R Server 9.1 and earlier](../what-is-microsoft-r-server.md)   <br/>[SQL Server 2016 and later (Windows only)](https://docs.microsoft.com/sql/advanced-analytics/getting-started-with-machine-learning-services)   <br/> [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) <br/>[Azure Data Science Virtual Machines](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm) |   

 
## How to use RevoIOQ

Load and run RevoIOQ from a command-line interface or R IDE. The following command executes tests and returns results.

1. Start Revo64 (on Linux) or an R IDE providing an R prompt.
2. Load RevoIOQ: `library("RevoIOQ")`
3. Execute the function: `RevoIOQ()`

## Syntax

```   
  RevoIOQ(printText=TRUE, printHTML=TRUE, outdir=if (file.access(getwd(), mode=2)) file.path(tempdir(),"RevoIOQ") else file.path(getwd(),"RevoIOQ"), 
          basename=paste("Revo-IOQ-Report", format(Sys.time(), "%m-%d-%Y-%H-%M-%S"), sep="-"),
          view=TRUE, clean=TRUE, runTestFileInOwnProcess=TRUE, testLocal = FALSE, testScaleR = TRUE)
 
```
 
 ## Arguments

   
 ### `printText`
 logical flag. If `TRUE`, an RUnit test report in ASCII text format is produced. 
  
    
 ### `printHTML`
 logical flag. If `TRUE`, an RUnit test report in HTML format is produced. 
  
    
 ### `outdir`
 character string representing path to output report directory. 
  
    
 ### `basename`
 character string denoting the name of the output file (sans the extension). 
  
    
 ### `view`
 logical flag. If `TRUE` and either `printText=TRUE` or `printHTML=TRUE`, then  the resulting output file is shown, respectively. 
  
    
 ### `clean`
 logical flag. If `TRUE`, then graphics files created during the tests are removed. 
  
    
 ### `runTestFileInOwnProcess`
 logical flag. If `TRUE`, each test file  is run in a separate R process. 
  
    
 ### `testLocal`
 logical flag. If `TRUE`, a set of basic tests is created and run for all locally installed packages. 
  
    
 ### `testScaleR`
 logical flag. If `TRUE`, the RevoScaleR unit tests are included in the test suite, if available. 
  
 
 ## Return value
 
`TRUE` if all tests are successful, `FALSE` otherwise.
 
 
 
 
