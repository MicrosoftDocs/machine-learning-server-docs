---

# required metadata
title: "Microsoft R Server - Known Issues"
description: "Known Issues with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "paulettem"
ms.date: "08/01/2016"
ms.topic: "get-started-article"
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
ms.technology: 
  - deployr
  - r-server
ms.custom: ""

---

# Known Issues with Microsoft R Server 2016

## RevoScaleR

### Distributed Computing:
 + On SLES 11 systems, there have been reports of threading interference between the Boost and
MKL libraries.
 + The value of consoleOutput that is set in the RxHadoopMR compute context when wait=FALSE
determines whether or not consoleOutput will be displayed when rxGetJobResults is called; the
value of consoleOutput in the latter function is ignored.
 + When using RxInTeradata, if you encounter an intermittent failure, simply try resubmitting your
R command.
 + The rxDataStep function does not support the varsToKeep and varsToDrop arguments in
RxInTeradata.
 + The ‘dataPath’ and ‘outDataPath’ arguments for the RxHadoopMR compute context are not yet
supported.
 + The ‘rxSetVarInfo’ function is not supported when accessing xdf files with the RxHadoopMR
compute context.

### Data Import and Manipulation
 + Appending to an existing table is not supported when writing to a Teradata database.
 + When reading VARCHAR columns from a database, white space will be trimmed. To prevent this,
enclose strings in non-white-space characters.
 + When using functions such as rxDataStep to create database tables with VARCHAR columns, the
column width is estimated based on a sample of the data. If the width can vary, it may be
necessary to pad all strings to a common length.
 + Using a transform to change a variable's data type is not supported when repeated calls to
rxImport or rxTextToXdf are used to import and append rows, combining multiple input files
into a single .xdf file.
 + When importing data from the Hadoop Distributed File System, attempting to interrupt the
computation may result in exiting Revolution R Enterprise.

### Analysis Functions
 + Composite xdf data set columns are removed when running rxPredict(.) with rxDForest(.) in
Hadoop and writing to the input file
 + The rxDTree function does not currently support in-formula transformations; in particular, using
the F() syntax for creating factors on the fly is not supported. However, numeric data will be
automatically binned.
 + Ordered factors are treated the same as factors in all RevoScaleR analysis functions except
rxDTree.

## DeployR

 + On Linux, if you attempt to change the DeployR RServe port using the `adminUtilities.sh`, the script incorrectly updates Tomcat's `server.xml` file, which prevents Tomcat from starting, and does not update the necessary the `Rserv.conf` file. You must revert back to an earlier version of `server.xml` to restore service.

 + Using `deployrExternal()` on the DeployR Server to reference a file that in a specified folder produces a ‘Connection Error’ due to an improperly defined environment variable. To workaround this issue, you must prefix the `REVODEPLOYR8_1_HOME` environment variable. <br><br>For example, the following code would produce the error:
   ```
   irisdf<-read.csv(file = deployrExternal("irisE.csv", isPublic= TRUE))
   ```
   The workaround can only be achieved by:
   ```
   dplhome<-paste(Sys.getenv("REVODEPLOYR8_1_HOME"),"..",sep="/")
   extfile<-file.path(dplhome,deployrExternal("irisE.csv",isPublic=TRUE))
   irisdf<-read.csv(extfile)
   ```

## RevoIOQ Package

 + If the RevoIOQ function is run concurrently in separate processes, some tests may fail.

## RevoMods Package
 + The RevoMods timestamp() function, which masks the standard version from the utils package,
is unable to find the C_addhistory object when running in  an Rgui, Rscript,
etc. session. If you are calling timestamp(), call the utils version directly as
utils::timestamp().

## R Base and Recommended Packages
In the nls function, use of the “port” algorithm occasionally causes the R front-end to stop
unexpectedly. The nls help file advises caution when using this algorithm; Revolution
recommends avoiding it altogether and using either the default Gauss-Newton or plinear
algorithms. 

