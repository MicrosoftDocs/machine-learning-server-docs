---

# required metadata
title: "Microsoft R Server - Known Issues"
description: "Known Issues with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "04/16/2017"
ms.topic: "article"
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

# Known Issues and Announcements for Microsoft R Server 9.1

## Known issues

#### rxMerge() behaviors in RxSpark compute context

**Applies to: RevoScaleR package > rxMerge function**

In comparison with the local compute context, rxMerge() used in a RxSpark compute context has slightly different bheaviors:

1.	NULL return value.
2.	Column order may be different.
3.	Factor columns may be written as character type.
4.	In a local compute context, duplicate column names are made unique by adding “.”, plus the extensions provided by the user via the duplicateVarExt parameter (for example “Visibility.Origin”). In an RxSpark compute context, the “.” is omitted.

#### Error during Ensembling: "Transform pipeline 0 contains transforms that do not implement IRowToRowMapper"

**Applies to: MicrosoftML package > Ensembling** 

Certain machine learning transforms that don’t implement the **IRowToRowMapper** interface will fail during Ensembling. Examples include getSentiment() and featurizeImage().

To work around this error, you can pre-featurize data using rxFeaturize(). The only other alternative is to avoid mixing Ensembling with transforms that produce this error. Finally, you could also wait until the issue is fixed in the next release.

#### rxExecBy() terminates unexpectedly if NA values do not have a factor level

**Applies to: RevoScaleR package > rxExecBy function**

R script using rxExecBy will suddenly abort if the data set presents factor columns containing NA values, and NA is not a factor level. For example, consider a variable for Gender with 3 factor levels: Female, Male, Unknown. If values exist  that are not one of these values, the function will fail.

There are two possible workarounds:

+ Option 1: Add an 'NA' level using **addNA()** to catch the "not applicable" case.
+ Option 2: Clean the input dataset (remove the NA values)

Pseudo code for the first option might be:

`> dat$Gender = addNA(dat$Gender)`

Output would now include a 4th factor level called NA that would catch all values not covered by the other factors:

```
> rxGetInfo(dat, getVarInfo = TRUE)

Data frame: dat 
Number of observations: 97 
Number of variables: 1 
Variable information: 
Var 1: Gender
       4 factor levels: Female Male Unknown NA
```

## Deprecated and Discontinued functions

#### RevoScaleR Package

|Function| Status | Replacement |
|-----------|----|--------|
|`rxGetNodes` | Deprecated | [`rxGetAvailableNodes`](../scaler/packagehelp/rxGetAvailableNodes.md)| 
|`RxHpcServer` | Deprecated | [`RxSpark`](../scaler/packagehelp/rxSpark.md) or [`RxHadoopMR`](../scaler/packagehelp/rxHadoopMR.md)| 
|`rxImportToXdf` | Deprecated | [`rxImport`](../scaler/packagehelp/rxImport.md) |
|`rxDataStepXdf` | Deprecated | [`rxDataStep`](../scaler/packagehelp/rxDataStep.md) |
|`rxDataFrameToXdf` | Deprecated | [`rxDataStep`](../scaler/packagehelp/rxDataStep.md) |
|`rxXdfToDataFrame` | Deprecated | [`rxDataStep`](../scaler/packagehelp/rxDataStep.md) |
|`rxSortXdf` | Deprecated | [`rxSort`](../scaler/packagehelp/rxSortXdf.md) |
|`rxGetVarInfoXdf` |Discontinued |[`rxGetVarInfo`](../scaler/packagehelp/rxGetVarInfoXdf.md))|
|`rxGetInfoXdf` |Discontinued |[`rxGetInfo`](../scaler/packagehelp/rxGetInfoXdf.md))|

For more information, see [discontinued RevoScaleR functions](../scaler/packagehelp/RevoScaleR-defunct.md) and [deprecated RevoScaleR functions](../scaler/packagehelp/RevoScaleR-deprecated.md).

#### RevoMods Package

In **RevoMods** functions are discontinued (all were intended for use solely by the R Productivity Environment discontinued in Microsoft R Server 8.0.3):

+ `?` (use the standard R `?`, previously masked)
+ `q` (use the standard R `q` function, previously masked)
+ `quit` (use the standard R `quit` function, previously masked)
+ `revoPlot` (use the standard R `plot` function)
+ `revoSource` (use the standard R `source` function)

## Previous releases 

### Microsoft R Server 9.0.1

#### Package: RevoScaleR > Distributed Computing
 + On SLES 11 systems, there have been reports of threading interference between the Boost and
MKL libraries.
 + The value of consoleOutput that is set in the `RxHadoopMR` compute context when `wait=FALSE`
determines whether or not `consoleOutput` will be displayed when `rxGetJobResults` is called; the
value of `consoleOutput` in the latter function is ignored.
 + When using `RxInTeradata`, if you encounter an intermittent failure, simply try resubmitting your
R command.
 + The `rxDataStep` function does not support the `varsToKeep` and `varsToDrop` arguments in
`RxInTeradata`.
 + The `dataPath` and `outDataPath` arguments for the `RxHadoopMR` compute context are not yet
supported.
 + The `rxSetVarInfo` function is not supported when accessing xdf files with the `RxHadoopMR`
compute context.
 + When specifying a non-default `RNGkind` as an argument to `rxExec`, identical random number streams can be generated unless the `RNGseed` is also specified.
 + When using small test data sets on a Teradata appliance, some test failures may occur due to insufficient data on each AMP.
 + Adding multiple new columns using `rxDataStep` with `RxTeradata` data sources fails in local compute context. As a workaround, use `RxOdbcData` data sources or the `RxInTeradata` compute context.


#### Package: RevoScaleR > Data Import and Manipulation
 + Appending to an existing table is not supported when writing to a Teradata database.
 + When reading `VARCHAR` columns from a database, white space will be trimmed. To prevent this,
enclose strings in non-white-space characters.
 + When using functions such as `rxDataStep` to create database tables with `VARCHAR` columns, the
column width is estimated based on a sample of the data. If the width can vary, it may be
necessary to pad all strings to a common length.
 + Using a transform to change a variable's data type is not supported when repeated calls to
`rxImport` or `rxTextToXdf` are used to import and append rows, combining multiple input files
into a single .xdf file.
 + When importing data from the Hadoop Distributed File System, attempting to interrupt the
computation may result in exiting the software.

#### Package: RevoScaleR > Analysis Functions
 + Composite xdf data set columns are removed when running `rxPredict(.)` with `rxDForest(.)` in
Hadoop and writing to the input file.
 + The `rxDTree` function does not currently support in-formula transformations; in particular, using
the `F()` syntax for creating factors on the fly is not supported. However, numeric data will be
automatically binned.
 + Ordered factors are treated the same as factors in all RevoScaleR analysis functions except
`rxDTree`.

#### Package: RevoIOQ

 + If the `RevoIOQ` function is run concurrently in separate processes, some tests may fail.

#### Package: RevoMods
 + The `RevoMods` timestamp() function, which masks the standard version from the utils package, is unable to find the `C_addhistory` object when running in  an Rgui, Rscript, etc. session. If you are calling `timestamp()`, call the `utils` version directly as `utils::timestamp()`.

#### R Base and Recommended Packages
+ In the `nls` function, use of the `port` algorithm occasionally causes the R front-end to stop unexpectedly. The `nls` help file advises caution when using this algorithm. We recommend avoiding it altogether and using either the default Gauss-Newton or plinear algorithms.

#### Operationalize (Deploy & Consume Web Services) _features formerly referred to as DeployR_

+ When Azure active directory authentication is the only form of authentication enabled, it is not possible to run diagnostics.
 

### Microsoft R Server 8.0.5

#### Package: RevoScaleR > Distributed Computing
 + On SLES 11 systems, there have been reports of threading interference between the Boost and
MKL libraries.
 + The value of consoleOutput that is set in the `RxHadoopMR` compute context when `wait=FALSE`
determines whether or not `consoleOutput` will be displayed when `rxGetJobResults` is called; the
value of `consoleOutput` in the latter function is ignored.
 + When using `RxInTeradata`, if you encounter an intermittent failure, simply try resubmitting your
R command.
 + The `rxDataStep` function does not support the `varsToKeep` and `varsToDrop` arguments in
`RxInTeradata`.
 + The `dataPath` and `outDataPath` arguments for the `RxHadoopMR` compute context are not yet
supported.
 + The `rxSetVarInfo` function is not supported when accessing xdf files with the `RxHadoopMR`
compute context.

#### Package: RevoScaleR > Data Import and Manipulation
 + Appending to an existing table is not supported when writing to a Teradata database.
 + When reading `VARCHAR` columns from a database, white space will be trimmed. To prevent this,
enclose strings in non-white-space characters.
 + When using functions such as `rxDataStep` to create database tables with `VARCHAR` columns, the
column width is estimated based on a sample of the data. If the width can vary, it may be
necessary to pad all strings to a common length.
 + Using a transform to change a variable's data type is not supported when repeated calls to
`rxImport` or `rxTextToXdf` are used to import and append rows, combining multiple input files
into a single .xdf file.
 + When importing data from the Hadoop Distributed File System, attempting to interrupt the
computation may result in exiting the software.

#### Package: RevoScaleR > Analysis Functions
 + Composite xdf data set columns are removed when running `rxPredict(.)` with `rxDForest(.)` in
Hadoop and writing to the input file.
 + The `rxDTree` function does not currently support in-formula transformations; in particular, using
the `F()` syntax for creating factors on the fly is not supported. However, numeric data will be
automatically binned.
 + Ordered factors are treated the same as factors in all RevoScaleR analysis functions except
`rxDTree`.

#### DeployR

 + On Linux, if you attempt to change the DeployR RServe port using the `adminUtilities.sh`, the script incorrectly updates Tomcat's `server.xml` file, which prevents Tomcat from starting, and does not update the necessary the `Rserv.conf` file. You must revert back to an earlier version of `server.xml` to restore service.

 + Using `deployrExternal()` on the DeployR Server to reference a file that in a specified folder produces a ‘Connection Error’ due to an improperly defined environment variable. For this reason, you must log into the Administration Console and go to **The Grid** tab. In that tab, edit **Storage Context value for each and every node** and specify the **full path** to the external data directory on that node’s machine, such as `<DEPLOYR_INSTALLATION_DIRECTORY>/deployr/external/data`.

#### RevoIOQ Package

 + If the `RevoIOQ` function is run concurrently in separate processes, some tests may fail.

#### RevoMods Package
 + The `RevoMods` timestamp() function, which masks the standard version from the utils package, is unable to find the `C_addhistory` object when running in  an Rgui, Rscript, etc. session. If you are calling `timestamp()`, call the `utils` version directly as `utils::timestamp()`.

#### R Base and Recommended Packages
+ In the nls function, use of the `port` algorithm occasionally causes the R front-end to stop unexpectedly. The nls help file advises caution when using this algorithm. We recommend avoiding it altogether and using either the default Gauss-Newton or plinear algorithms.
