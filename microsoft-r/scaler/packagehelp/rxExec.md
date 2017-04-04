--- 
 
# required metadata 
title: " Run A Function on Multiple Nodes or Cores " 
description: "Allows distributed execution of a function in parallel across nodes (computers) or cores of a compute context such as a cluster. " 
keywords: "RevoScaleR, rxExec, IO" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 
 #`rxExec`:  Run A Function on Multiple Nodes or Cores 

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 
Allows distributed execution of a function in parallel across nodes (computers) or cores 
of a "compute context" such as a cluster.
 
 
 
 ##Usage

```   
  rxExec(FUN,   ...  , elemArgs, elemType = "nodes", oncePerElem = FALSE, timesToRun = -1L, 
         packagesToLoad = NULL, execObjects = NULL, taskChunkSize = NULL, quote = FALSE,
         consoleOutput = NULL, autoCleanup = NULL, continueOnFailure = TRUE, 
         RNGseed = NULL, RNGkind = NULL, foreachOpts = NULL)
 
```
 
 
 ##Arguments

   
  
 ### `FUN`
 the function to be executed; the nodes or cores on which it is run are determined by the currently-active compute context and by the other arguments of [rxExec](rxExec.md). 
  
  
  
 ### ` ...`
 arguments passed to the function `FUN` each time it is executed.  Separate argument values can be sent for each computation by wrapping a vector or list of argument values in [rxElemArg](rxElemArg.md). 
  
  
  
 ### `elemArgs`
 a vector or list specifying arguments to `FUN`. This allows a different set of arguments to be passed to `FUN` each time it is executed.  The length of the vector or list must match the number of times the function will  be executed. Each of these elements will be passed in turn  to `FUN`. Using a list of lists allows multiple named or unnamed parameters to be passed. If `elemArgs` has length 1, that argument is passed to all compute  elements (and thus is an alternative to  ...). The elements of `elemArgs` may be named; if they are node names those elements will be passed to those nodes. Alternatively, they can be "rxElem1", "rxElem2" and so on. In this case, the list of returned values will have those corresponding names. See the Details section for more information. This is an alternative to using [rxElemArg](rxElemArg.md) one or more times.  
  
  
  
 ### `elemType`
 the distributed computing mode to be used. Handling of this parameter depends upon compute context, as follows:  
* `RxHpcServer` - Allowable types are  `"nodes"` (the default), `"cores"` or `"user"`. A `"nodes"` type means that only one instance of `FUN` is allowed to run at a time on any node (computer). Thus, that instance does not have to compete with other instances for shared resources such as cores, disks, and RAM. A `"cores"` type means that multiple instance may be run on a node, corresponding to the  number of cores on that node, but the number of instances running in parallel will not exceed the number of cores. Specify `"user"` only if you are supplying  your own distributed computing configuration file and want to override all  programatic settings.   
* `RxInTeradata` - Allowable types are  `"nodes"` (the default) If   `elemType="nodes"` the computation is performed on each AMP of the Teradata platform.   
* `local or `RxForeachDoPar`` - The `elemType` parameter is ignored.  
  
  
  
  
 ### `oncePerElem`
 logical flag. If `TRUE` and `elemType="nodes"`, `FUN` will be run exactly once on each specified node. In this case, each element of the return list will be named with the name of the node that computed that element. If `FALSE`, a node may  be used more than once (but never simultaneously). `oncePerElem` must be set to `FALSE`if `elemType="cores"`.  This parameter is ignored if the active compute context is local. 
  
  
  
 ### `timesToRun`
 integer specifying the the total number of instances of the function  `FUN` to run. If `timesToRun=-1`, the default, then times is set to the  length of the `elemArgs` argument, if it exists, else to the number of nodes or cores specified in the compute context object, if that is exact. In the latter case, if the `elemType="nodes"` and a single set of arguments is being passed to each node, each  element of the return list will be named with the name of the node that computed that element. If `timesToRun` is not -1, it must be consistent with this other information.  
  
  
  
 ### `packagesToLoad`
 optional character vector specifying additional packages to be  loaded on the nodes for this job. If provided, these packages are loaded after any `packagesToLoad` specified in the current distributed compute context. 
  
  
  
 ### `execObjects`
 optional character vector specifying additional objects to be  exported to the nodes for this job, or an environment containing these objects.  The specified objects are added to `FUN`'s environment, unless that environment is locked, in which case they are added to the environment in which `FUN` is evaluated. 
  
  
  
 ### `taskChunkSize`
 optional integer scalar specifying the number of tasks to be executed per compute element, or worker. By submitting tasks in chunks, you can avoid some of the overhead of starting new R processes over and over. For example, if you are running thousands of identical simulations on a cluster,  it makes sense to specify the `taskChunkSize` so that each  worker can do its allotment of tasks in a single R process.  This argument is incompatible with the `oncePerElem` argument; if both are supplied, this one is ignored. It is also incompatible with lists supplied to `elemArgs` with compute element names. 
  
  
  
 ### `quote`
 logical flag. If `TRUE`, underlying calls to `do.call` have the corresponding flag set to `TRUE`. This is primarily of use to the **doRSR** package, but may be of use to other users. 
  
   
  
 ### `consoleOutput`
 `NULL` or logical value. If `TRUE`, the console output from the  all of the processes is printed to the user console. Note that the output from different nodes or cores may be interleaved in an unpredictable way. If `FALSE`,  no console output is displayed. Output can be retrieved with the function  [rxGetJobOutput](rxGetJobOutput.md) for a non-waiting job. If not `NULL`,  this flag overrides the  value set in the compute context when the job was submitted. If `NULL`,  the setting in the compute context will be used.  This parameter is ignored  if the active compute context is local. 
  
  
  
 ### `autoCleanup`
 `NULL` or logical value. If `TRUE`, artifacts created by the distributed  computing job are deleted when the results are returned or retrieved using [rxGetJobResults](rxGetJobResults.md). If `FALSE`, the artifacts are not deleted,  and the results may be obtained repeatedly using [rxGetJobResults](rxGetJobResults.md),  and the console output via [rxGetJobOutput](rxGetJobOutput.md) until  [rxCleanupJobs](rxCleanup.md) is used to delete the artifacts. If not `NULL`, this flag overrides  the value set in the compute context when the job was submitted. If you routinely  set `autoCleanup=FALSE`, you may eventually fill your hard disk with  compute artifacts. If you set `autoCleanup=TRUE` and experience performance degradation on a Windows XP client, consider setting `autoCleanup=FALSE`.  This  parameter is ignored if the active compute context is local. 
  
  
  
 ### `continueOnFailure`
 `NULL` or logical value.  If `TRUE`, the default, then if an individual instance of a job fails due to a hardware or network failure, an attempt will be made to rerun that job.  (R syntax errors, however, will cause immediate failure as usual.) Furthermore, should a process instance of a job fail due to a user code failure, the rest of the processes will continue,  and the failed process will produce a warning when the output is collected.  Additionally, the position  in the returned list where the failure occured will contain the error as opposed to a result. This  parameter is ignored if the active compute context is local or `RxForeachDoPar`.  
  
  
  
 ### `RNGseed`
 `NULL`, the string `"auto"`, or an integer to be used as the seed for parallel random number generation. See the Details section for a description of how the `"auto"` string is used. 
  
  
  
 ### `RNGkind`
 `NULL` or a character string specifying the type of random number generator to be used. Allowable strings are the strings accepted by [rxRngNewStream](rxRng.md), `"auto"`, and, if the active compute context is local parallel, `"L'Ecuyer-CMRG"` (for compatibility with the parallel package). See the Details section for a description of how the `"auto"` string is used. 
  
  
  
 ### `foreachOpts`
 `NULL` or a list containing options to be passed to the foreach parallel computing backend. See foreach for details. 
  
 
 
 
 ##Details
 
`rxExec` has very limited functionality for `RxInSqlServer` for CTP3; computations
are performed sequentially.
There are two primary sets of use cases:  In the first set, each computing element 
(node or core) gets the same argument values; in this case, do not use `elemArgs` or 
`rxElemArg`.  In the second, each element gets a different set of 
arguments; use [rxElemArg](rxElemArg.md) for each argument that has different values, or 
an `elemArgs` whose length is equal to the number of times `FUN` will
be executed.

**Set 1 (All computing elements get the same arguments):**


1 
 `rxExec(FUN, arg1, arg2)`



**Set 2: Every computing element gets a different set of arguments.**
If [rxElemArg](rxElemArg.md) is used, the length of the vector or list for the enclosed argument
must equal the number of compute elements. For example,

`rxExec(FUN, arg1 = 1, arg2 = rxElemArg(c(1:5)))` 

If `elemArgs` is a nested list, the individual lists are passed to the compute resources according to the following:


1 
 The argument lists can be named according to which compute resource each component list should be assigned.
For example, `rxExec(FUN, elemArgs=list(compute1=list(arg1,arg2), compute2=list(arg3, arg4)))`. In this
case, the list of arguments must be the same length as the list of nodes requested for the current
compute context, and have the same names. If `oncePerElem=TRUE` and `elemType="nodes"`, then
the computation will be performed once on each requested node, and each node is assured of getting the
argument with its name. If `oncePerElem=FALSE`, there is no guarantee that each node will be used in
the processing, so arguments intended for a particular node may not be used; they must still be provided,
however.

The component names must be valid R syntactic names. If you have nodes on your cluster with names that
are not valid R syntactic names, use the function [rxMakeRNodeNames](rxMakeRNodeNames.md) on the node name to 
determine the appropriate name to give the list component. When the return value is a list with elements
named by compute node, the node names are as returned by the [rxMakeRNodeNames](rxMakeRNodeNames.md) function.



1 
 The arguments lists, if not named, will be passed to the compute resources allowed by the 
compute context according to their position in the list. This is useful when 
you don't care which nodes or cores the function is executed on but want 
different arguments to be executed on each resource. For example,
`rxExec(FUN, elemArgs=list(list(arg1,arg2), list(arg3, arg4)))` or
`rxExec(FUN, elemArgs=list(c(arg1, arg2), list(arg3, arg4)))`



The arguments `RNGseed` and `RNGkind` can be used to control random number generation in
the workers. By default, both are `NULL` and no special random number control is used. If either
or both `RNGseed` and `RNGkind` are set to `"auto"`, a parallel random number stream is
initialized on each worker, using the `"MT2203"` generator and separate substreams for
each worker. If other non-null valid values are supplied for these arguments, they are used as is for
the `"MT2203"` generator, which supports multiple substreams, but for other
`rxRngNewStream`-supported generators, the seed will be used as the starting point of a sequence
of seeds, one for each worker. In the special case of a local parallel compute context, the 
`"L'Ecuyer-CMRG"` generator case can be specified, in which case the parallel package's
`clusterSetRNGStream` function is called on the internally generated parallel cluster.
 
 
 
 ##Value
 
If a waiting compute context is active, a list with an element for each job, where each element contains the value(s) 
returned by that job's function call(s). 
If a non-waiting compute context is active, a jobInfo object. See [rxGetJobResults](rxGetJobResults.md).
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxElemArg](rxElemArg.md),
[rxGetJobResults](rxGetJobResults.md),
[rxGetJobStatus](rxGetJobResults.md),
[RxComputeContext](RxComputeContext.md),
[rxSetComputeContext](rxSetComputeContext.md),
[RxSpark](RxSpark.md),
[RxHadoopMR](RxHadoopMR.md),
[RxInTeradata](RxInTeradata.md), 
[RxForeachDoPar](RxForeachDoPar.md),
[RxLocalParallel](RxLocalParallel.md),
[RxLocalSeq](RxLocalSeq.md),
[rxGetNodeInfo](rxGetNodeInfo.md),
[rxMakeRNodeNames](rxMakeRNodeNames.md)
[rxRngNewStream](rxRng.md)
   
 ##Examples

 ```
   
  ## Not run:
 

myCluster <- RxHpcServer(
    headNode = "cluster-head", 
    shareDir = "\\AllShare\username",
    revoPath = file.path(defaultRNodePath, "bin", "x64"), 
    workingDir = "C:\\Users\\username", 
    wait = TRUE)

## Run function with no parameters
rxExec(getwd)

## Pass the same set of arguments to each compute element
rxExec(list.files, all.files=TRUE, full.names=TRUE)

## Run function with the same vector sent as the first
## argument to each compute element
## The values 1 to 10 will be printed 10 times
x <- 1:10
rxExec(print, x, elemType = "cores", timesToRun = 10)

## Pass a different argument value to each compute element
## The values 1 to 10 will be printed once each
rxExec(print, rxElemArg( x ), elemType = "cores")

## Extract different columns from a data frame on different nodes
set.seed(100)
myData <- data.frame(x = 1:100, y = rep(c("a", "b", "c", "d"), 25),
	z = rnorm(100), w = runif(100))
myVarsToKeep = list(
    c("x", "y"),
    c("x", "z"),
    c("x", "w"),
    c("y", "z"),
    c("z", "w"))
# myVarDataFrames will be a list of data frames
myVarDataFrames <- rxExec(rxDataStep, inData = myData, varsToKeep = rxElemArg(myVarsToKeep))

## Extract different rows from the data frame on different nodes
myRowSelection = list(
    expression(y == 'a'),
    expression(y == 'b'),
    expression(y == 'c'),
    expression(y == 'd'),
    expression(z > 0))

myRowDataFrames <- rxExec(rxDataStep, inData = myData, rowSelection = rxElemArg(myRowSelection))	

## Use the taskChunkSize argument
rxExec(sqrt, rxElemArg(1:100), taskChunkSize=50)
 ## End(Not run) 
  
 
```
 
 
