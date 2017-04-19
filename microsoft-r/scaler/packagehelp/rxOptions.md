--- 
 
# required metadata 
title: "Global Options for RevoScaleR" 
description: " Functions to specify and retrieve options needed for **RevoScaleR** computations. These need to be set only once to carry out multiple computations. " 
keywords: "RevoScaleR, rxOptions, rxGetOption, rxIsExpressEdition, environment, error, print" 
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
 
 
 
 
 #`rxOptions`: Global Options for RevoScaleR

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Functions to specify and retrieve options needed for **RevoScaleR**
computations. These need to be set only once to carry out multiple
computations.
 
 
 ##Usage

```   
  rxOptions(initialize = FALSE,
            libDir,
            linkDllName = ifelse(.Platform$OS.type == "windows", "RxLink.dll", "libRxLink.so.2"),
            cintSysDir = setCintSysDir(),
            includeDir = setRxIncludeDir(),
            unitTestDir = system.file("unitTests", package = "RevoScaleR"),
            unitTestDataDir = system.file("unitTestData", package = "RevoScaleR"),
            sampleDataDir = system.file("SampleData", package = "RevoScaleR"),
            demoScriptsDir = system.file("demoScripts", package = "RevoScaleR"),
            blocksPerRead = 1,
            reportProgress = 2,
            rowDisplayMax = -1,
            memStatsReset = 0,
            memStatsDiff = 0,
            numCoresToUse = 4,
            numDigits = options()$digits, 
            showTransformFn = FALSE,
            defaultDecimalColType = "float32",
            defaultMissingColType = "float32",
            computeContext = RxLocalSeq(),
            dataPath = ".",
            outDataPath = ".",
            transformPackages = c("RevoScaleR","utils","stats","methods"),
            xdfCompressionLevel = 1,
            fileSystem = "native",
            useDoSMP = NULL,
            useSparseCube = FALSE,
            rngBufferSize = 1,
            dropMain = TRUE,          
            coefLabelStyle = "Revo",
            numTasks = 1,
            hdfsHost = Sys.getenv("REVOHADOOPHOST"),
            hdfsPort = as.integer(Sys.getenv("REVOHADOOPPORT")),
            unixRPath = "/usr/bin/Revo64-8",
            mrsHadoopPath = "/usr/bin/mrs-hadoop",
            spark.executorCores = 2,
            spark.executorMem = "4g",
            spark.executorOverheadMem = "4g",
            spark.numExecutors = 65535,
            traceLevel = 0,
            ...)
            
  rxGetOption(opt, default = NULL)
  rxIsExpressEdition()
 
```
 
 
 ##Arguments

   
    
 ### `initialize`
 logical value. If `TRUE`, `rxOptions` resets all **RevoScaleR** options to their default value. 
  
  
    
 ### `libDir`
 character string specifying path to **RevoScaleR**'s lib directory. For 32-bit versions, this defaults to the `libs` directory; for 64-bit versions, this defaults to the `libs/x64` directory. 
  
  
    
 ### `linkDllName`
 character string specifying name of the **RevoScaleR**'s DLL (Windows) or shared object (Linux). 
  
  
    
 ### `cintSysDir`
 character string specifying path to **RevoScaleR**'s C/C++ interpreter (CINT) directory. 
  
  
    
 ### `includeDir`
 character string specifying path to **RevoScaleR**'s include directory. 
  
  
    
 ### `unitTestDir`
 character string specifying path to **RevoScaleR**'s **RUnit**-based test directory. 
   
  
    
 ### `unitTestDataDir`
 character string specifying path to **RevoScaleR**'s **RUnit**-based test data directory. 
  
  
    
 ### `sampleDataDir`
 character string specifying path to **RevoScaleR**'s sample data directory. 
  
  
    
 ### `demoScriptsDir`
 character string specifying path to **RevoScaleR**'s demo script directory. 
  
  
    
 ### `blocksPerRead`
 default value to use for `blocksPerRead` argument for many **RevoScaleR** functions. Represents the number of blocks to read within each read chunk. 
  
  
    
 ### `reportProgress`
 default value to use for `reportProgress` argument for many **RevoScaleR** functions. Options are:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
 
  
  
    
 ### `rowDisplayMax`
 scalar integer specifying the maximum number of rows to display when using the `verbose` argument in **RevoScaleR** functions. The  default of `-1` displays all available rows. 
  
  
    
 ### `memStatsReset`
 boolean integer. If `1`, reset memory status 
  
  
    
 ### `memStatsDiff`
 boolean integer. If `1`, the change of memory status is shown. 
  
  
    
 ### `numCoresToUse`
 scalar integer specifying the number of cores to use. If set to a value higher than the number of available cores, the number of available cores will be used.  If set to `-1`, the number of available cores will be used. Increasing the number of cores to use will also increase the amount of memory required for **RevoScaleR** analysis functions. 
  
   
    
 ### `numDigits`
 controls the number of digits to to use when  converting numeric data to or from strings, such as when printing  numeric values or importing numeric data as strings. The default is  the current value of `options()$digits`, which defaults to 7. Beyond fifteen digits, however, results are likely to be unreliable. 
  
  
    
 ### `showTransformFn`
 logical value. If `TRUE`, the transform function is shown. 
  
  
    
 ### `defaultDecimalColType`
 Used to specify a column's data type when  only decimal values (possibly mixed with missing (`NA`) values) are encountered upon first read of the data and the column's type information is not specified via `colInfo` or `colClasses`. Supported types are "float32" and "numeric", for 32-bit floating point and 64-bit floating point values, respectively. 
  
  
    
 ### `defaultMissingColType`
 Used to specify a given column's data type when  only missings (`NA`s) or blanks are encountered upon first read of the data  and the column's type information is not specified via `colInfo` or `colClasses`. Supported types are "float32", "numeric", and "character" for 32-bit floating point, 64-bit floating point and string values, respectively.  
  
  
   
    
 ### `computeContext`
 an [RxComputeContext](RxComputeContext.md) object representing the computational environment.  
*   [RxLocalSeq](RxLocalSeq.md): compute locally, using sequential processing with [rxExec](rxExec.md) High Performance Computing. 
*   [RxLocalParallel](RxLocalParallel.md): compute locally, using the `'parallel'` package for processing with [rxExec](rxExec.md) High Performance Computing. 
*   [RxForeachDoPar](RxForeachDoPar.md): use the currently registered parallel backend for 'foreach' for processing with [rxExec](rxExec.md) High Performance Computing.   
*  [RxHadoopMR](RxHadoopMR.md): use a Hadoop cluster for both High Performance Analytics for [rxExec](rxExec.md) High Performance Computing. 
*  [RxInTeradata](RxInTeradata.md): use a Teradata cluster for both High Performance Analytics and for [rxExec](rxExec.md) High Performance Computing. 
*   [RxHpcServer](RevoScaleR-deprecated.md): use a Microsoft HPC Server cluster for both High Performance Analytics and for [rxExec](rxExec.md) High Performance Computing.                                        
 
   
   
    
 ### `dataPath`
 character vector containing paths to search for local data sources. The default is to search just the current working directory. This will be ignored if `dataPath` is specified in the active compute context. See the Details section for more information regarding the path format. 
  
  
    
 ### `outDataPath`
 character vector containing paths for writing new output data files. New data files will be written to the first path that exists. The default is to write to the current working directory. This will be ignored if `outDataPath` is specified in the active compute context. 
  
   
    
 ### `transformPackages`
 character vector defining default set of R packages to be made available and preloaded for use in variable transformation functions. If at the default setting, when using [RxInTeradata](RxInTeradata.md) the default will be modified to a small package (`RevoMods`) in order to reduce memory requirements. 
  
   
    
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9. The higher the value, the greater the amount of compression - resulting in smaller files but a longer time to create them. If `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression will be used. 
   
  
    
 ### `fileSystem`
 character string or [RxFileSystem](RxFileSystem.md) object indicating type of file system; `"native"` or `RxNativeFileSystem` object can be used for the local operating system, or an `RxHdfsFileSystem` object for the Hadoop file system. 
  
  
    
 ### `useDoSMP`
 `NULL`. Deprecated. Use a [RxLocalParallel](RxLocalParallel.md) compute context. 
  
   
    
 ### `opt`
 character string specifying the **RevoScaleR** option to obtain. A `NULL` is returned if the option does not exist. 
  
  
    
 ### `useSparseCube`
 logical value. If `TRUE`, sparse cube is used. 
  
  
    
 ### `rngBufferSize`
 a positive integer scalar specifying the buffer size for the Parallel Random Number Generators (RNGs) in MKL. 
  
  
    
 ### `dropMain`
 logical value. If `TRUE`, main-effect terms are dropped before their interactions. 
  
  
    
 ### `coefLabelStyle`
 character string specifying the coefficient label style. The default is "Revo". If "R", R-compatible labels are created. 
  
  
    
 ### `numTasks`
 integer value. The default `numTasks` use in [RxInSqlServer](RxInSqlServer.md). 
  
  
    
 ### `hdfsHost`
 character string specifying the host name of your Hadoop nameNode. Defaults to Sys.getenv("REVOHADOOPHOST"), or `"default"` if no REVOHADOOPHOST environment variable is set. 
  
  
    
 ### `hdfsPort`
 integer scalar specifying the port number of your Hadoop nameNode, or a character string that can be coerced to numeric. Defaults to `as.integer(Sys.getenv("REVOHADOOPPORT"))`, or `0` if no REVOHADOOPPORT environment variable is set. 
  
  
    
 ### `unixRPath`
 The path to R executable on a Unix/Linux node. By default it points to a path corresponding to this client's version. 
  
  
    
 ### `mrsHadoopPath`
 Points to entry point to Hadoop MR which is deployed on every cluster node when MRS for Hadoop is installed. This script implements logic that determines which hadoop command should be called. 
  
  
    
 ### `traceLevel`
 Specifies the traceLevel that MRS will run with. This parameter controls MRS Logging features as well as Runtime Tracing of ScaleR functions. Levels are inclusive, (i.e. level `3:INFO` includes levels `2:WARN` and `1:ERROR` log messages). The options are:   
*   `0`: `DISABLED` - Tracing/Logging disabled. 
*   `1`: `ERROR`- `ERROR` coded trace points are logged to MRS log files 
*   `2`: `WARN`- `WARN` and `ERROR` coded trace points are logged to MRS log files. 
*   `3`: `INFO`- `INFO`, `WARN`, and `ERROR` coded trace points are logged to MRS log files. 
*   `4`: `DEBUG`- All trace points are logged to MRS log files. 
*   `5`: `RESERVED` - If set, will log at `DEBUG` granularity  
*   `6`: `RESERVED` - If set, will log at `DEBUG` granularity  
*   `7`: `TRACE`- ScaleR functions Runtime Tracing is activated and MRS log level is set to `DEBUG` granularity. 
 
  
  
    
 ### ` ...`
 additional arguments to be passed through. 
  
  
    
 ### `default`
 default value for an option that is returned if option is not found 
  
  
     
 
 
 ##Details
 
A full set of **RevoScaleR** options is set on load. Use `rxGetOption`
to obtain the value of a single option.

The `dataPath` argument is a character vector of well formed directory paths, either in UNC ("`\\host\dir`") or DOS ("`C:\dir`").
When specifying the paths, you must double the number of backslashes since R requires that backslashes be escaped. Updating the previous
examples gives "`\\\\host\\dir`" for UNC-type paths and "`C:\\dir`" for DOS-type paths. Alternatively, you could specify a 
DOS-type path with single forward slashes such as the output from `system.file` (e.g. "`C:/Revolution/R-Enterprise-Node-7.4/R-3.1.3/library/RevoScaleR/SampleData`").
For Windows operating systems, the arguments that define a path must be in long format and not DOS 8.3 (short) format, e.g., 
"`"C:\\Program Files\\RRO\\R-3.1.3\\bin\\x64"` or "`C:/Program Files/RRO/R-3.1.3/bin/x64`" are correct formats
while `"C:/PROGRA~1/RRO/R-31~1.3/bin/x64"` is not. 

`rxIsExpressEdition` is not currently functional.
 
 
 
 ##Value
 
For `rxOptions`, a list containing the original rxOptions is returned. If
there is no argument specified, the list is returned explicitly, otherwise the
list is returned as an invisible object. For `rxGetOption`, the current
value of the requested option is returned.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[RevoScaleR](revoAnalytics-package.in.md),
[RxLocalSeq](RxLocalSeq.md),
[RxLocalParallel](RxLocalParallel.md),
[RxForeachDoPar](RxForeachDoPar.md),
[RxHadoopMR](RxHadoopMR.md),
[RxSpark](RxSpark.md),
[RxInSqlServer](RxInSqlServer.md),
[RxInTeradata](RxInTeradata.md).
   
 
 ##Examples

 ```
   
  # Get the location of the sample data sets for RevoScaleR
  dataDir <- rxGetOption("sampleDataDir")
  # See the current settings for options
  rxOptions() 
  ## Not run:
 
rxOptions(reportProgress = 0) # by default, don't report progress
rxOptions()$reportProgress # show value
rxOptions(TRUE) # reset all options
rxOptions()$reportProgress # 2

# Setup to run analyses on HPC cluster
myCluster <- RxHpcServer(
    # Location of Revolution R Enterprise on each node
    revoPath = file.path(defaultRNodePath, "bin", "x64"),  
    # Location of big data files on each node
    dataPath = "C:\\data",	
    # Name of cluster's head node											
    headNode = "cluster-head2", 
    # User directory for read/write                                      	
    shareDir = "\\AllShare\\myName"                            
    )

rxOptions( computeContext = myCluster )
 ## End(Not run) 
  
 
```
 
 
 
 
