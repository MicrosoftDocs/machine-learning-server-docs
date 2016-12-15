---

# required metadata
title: "rxGetOption ScaleR Function (RevoScaleR package)"
description: "rxGetOption function in the RevoScaleR package in Microsoft R."
keywords: "RevoScaleR, ScaleR, rxGetOption"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "09/14/2016"
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

#rxGetOption function (RevoScaleR)

Global options for RevoScaleR. Function used to specify and retrieve options needed for RevoScaleR computations. These only need to be set once to carry out multiple computations.

## Usage
~~~~
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
		  hdfsPort = as.integer(Sys.getenv("REVOHADOOPPORT")), ...)

rxGetOption(opt)
rxIsExpressEdition()
~~~~

## Arguments

The following table shows the arguments to **rxGetOption** in order and their default values.

|Parameter | Description|
| --------- | --------- |
|initialize|Logical value. If TRUE, **rxOptions** resets all RevoScaleR options to their default value.|
|libDir|Character string specifying path to RevoScaleR's lib directory. For 32-bit versions, this defaults to the libs directory; for 64-bit versions, this defaults to the libs/x64 directory.|
|linkDllName|Character string specifying name of the RevoScaleR's DLL (Windows) or shared object (Linux).|
|cintSysDir|Character string specifying path to RevoScaleR's C/C++ interpreter (CINT) directory.|
|includeDir|Character string specifying path to RevoScaleR's include directory.|
|unitTestDir|Character string specifying path to RevoScaleR's RUnit-based test directory.|
|unitTestDataDir|character string specifying path to RevoScaleR's RUnit-based test data directory.|
|sampleDataDir|Character string specifying path to RevoScaleR's sample data directory.|
|demoScriptsDir|Character string specifying path to RevoScaleR's demo script directory.|
|blocksPerRead|Default value to use for `blocksPerRead` argument for many RevoScaleR functions. Represents the number of blocks to read within each read chunk.|
|reportProgress|Default value to use for reportProgress argument for many RevoScaleR functions. Options are:<br /><br />- 0: no progress is reported.<br />- 1: the number of processed rows is printed and updated.<br />- 2: rows processed and timings are reported.<br />- 3: rows processed and all timings are reported.|
|rowDisplayMax|Scalar integer specifying the maximum number of rows to display when using the verbose argument in RevoScaleR functions. The default of -1 displays all available rows.|
|memStatsReset|Boolean integer. If 1, reset memory status.|
|memStatsDiff|Boolean integer. If 1, the change of memory status is shown.|
|numCoresToUse|Scalar integer specifying the number of cores to use. If set to a value higher than the number of available cores, the number of available cores will be used. If set to -1, the number of available cores will be used. Increasing the number of cores to use will also increase the amount of memory required for RevoScaleR analysis functions.|
|numDigits|Controls the number of digits to use when converting numeric data to or from strings, such as when printing numeric values or importing numeric data as strings. The default is the current value of `options()$digits`, which defaults to 7. Beyond fifteen digits, however, results are likely to be unreliable.|
|showTransformFn|Logical value. If TRUE, the transform function is shown.|
|defaultDecimalColType|Used to specify a column's data type when only decimal values (possibly mixed with missing (NA) values) are encountered upon first read of the data and the column's type information is not specified via `colInfo` or `colClasses`. Supported types are “float32” and “numeric”, for 32-bit floating point and 64-bit floating point values, respectively.|
|defaultMissingColType|Used to specify a given column's data type when only missing (NAs) or blanks are encountered upon first read of the data and the column's type information is not specified via `colInfo` or `colClasses`. Supported types are “float32”, “numeric”, and “character” for 32-bit floating point, 64-bit floating point and string values, respectively.|
|computeContext|An **RxComputeContext** object representing the computational environment.<br /><br />- **RxLocalSeq**: compute locally, using sequential processing with **rxExec** High Performance Computing.<br />- **RxLocalParallel**: compute locally, using the 'parallel' package for processing with **rxExec** High Performance Computing.<br />- **RxForeachDoPar**: use the currently registered parallel backend for 'foreach' for processing with **rxExec** High Performance Computing.<br />- **RxHadoopMR**: use a Hadoop cluster for both High Performance Analytics for **rxExec** High Performance Computing.<br />- **RxInTeradata**: use a Teradata cluster for both High Performance Analytics for **rxExec** High Performance Computing.<br />- **RxHpcServer**: use a Microsoft HPC Server cluster for both High Performance Analytics for **rxExec** High Performance Computing.|
|dataPath|Character vector containing paths to search for local data sources. The default is to search just the current working directory. This will be ignored if `dataPath` is specified in the active compute context. See the Remarks section for more information regarding the path format.|
|outDataPath|Character vector containing paths for writing new output data files. New data files will be written to the first path that exists. The default is to write to the current working directory. This will be ignored if `outDataPath` is specified in the active compute context.|
|transformPackages|Character vector defining default set of R packages to be made available and preloaded for use in variable transformation functions. If at the default setting, when using **RxInTeradata** the default will be modified to a small package (RevoMods) in order to reduce memory requirements.|
|xdfCompressionLevel|Integer in the range of -1 to 9. The higher the value, the greater the amount of compression - resulting in smaller files but a longer time to create them. If `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible with the 6.0 release of Revolution R Enterprise. If set to -1, a default level of compression will be used.|
|fileSystem|Character string or **RxFileSystem** object indicating type of file system; "native" or **RxNativeFileSystem object** can be used for the local operating system, or an **RxHdfsFileSystem** object for the Hadoop file system.|
|useDoSMP|NULL. Deprecated. Use a **RxLocalParallel** compute context.|
|opt|Character string specifying the RevoScaleR option to obtain. A NULL is returned if the option does not exist.|
|useSparseCube|Logical value. If TRUE, sparse cube is used.|
|rngBufferSize|A positive integer scalar specifying the buffer size for the Parallel Random Number Generators (RNGs) in MKL.|
|dropMain|Logical value. If TRUE, main-effect terms are dropped before their interactions.|
|coefLabelStyle|Character string specifying the coefficient label style. The default is "Revo". If "R", R-compatible labels are created.|
|numTasks|Integer value. The default `numTasks` is used in **RxInSqlServer**.|
|hdfsHost|Character string specifying the host name of your Hadoop n`ameNode`. Defaults to `Sys.getenv("REVOHADOOPHOST")`, or "default" if no `REVOHADOOPHOST` environment variable is set.|
|hdfsPort|Integer scalar specifying the port number of your Hadoop `nameNode`, or a character string that can be coerced to numeric. Defaults to `as.integer(Sys.getenv("REVOHADOOPPORT"))`, or 0 if no REVOHADOOPPORT environment variable is set.|
|...|Additional arguments to be passed through.|


## Remarks

A full set of RevoScaleR options is set on load. Use **rxGetOption** to obtain the value of a single option.

The `dataPath` argument is a character vector of well formed directory paths, either in UNC (`"\\host\dir"`) or DOS (`"C:\dir"`). When specifying the paths, you must double the number of backslashes since R requires that backslashes be escaped. Updating the previous examples gives `"\\\\host\\dir"` for UNC-type paths and "`C:\\dir`" for DOS-type paths. Alternatively, you could specify a DOS-type path with single forward slashes such as the output from `system.file` (e.g. `"C:/Revolution/R-Enterprise-Node-7.4/R-3.1.3/library/RevoScaleR/SampleData"`). For Windows operating systems, the arguments that define a path must be in long format and not DOS 8.3 (short) format, e.g., `"C:\\Program Files\\RRO\\R-3.1.3\\bin\\x64"` or `"C:/Program Files/RRO/R-3.1.3/bin/x64"` are correct formats while `"C:/PROGRA~1/RRO/R-31~1.3/bin/x64"` is not.

**rxIsExpressEdition** is not currently functional.

## Return Value
For **rxOptions**, a list containing the original **rxOptions** is returned. If there is no argument specified, the list is returned explicitly, otherwise the list is returned as an invisible object. For **rxGetOption**, the current value of the requested option is returned. .

## Example
~~~~
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
~~~~

## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](functions-for-sql-server-data.md)
