--- 
 
# required metadata 
title: "Generate HPC Server Object" 
description: " DEPRECATED Creates a compute context object for use with a Microsoft Windows HPC Server cluster.  This is the main generator for S4 class RxHpcServer. " 
keywords: "RevoScaleR, ScaleR, KEYWORDS" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "02/17/2017" 
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
 
 
 #Generate HPC Server Object 
 ##Description
 
DEPRECATED Creates a compute context object for use with a Microsoft Windows HPC Server cluster. 
This is the main generator for S4 class RxHpcServer.
 
 
 ##Usage

```   
  RxHpcServer(object, headNode = "", revoPath = NULL, shareDir = "", workingDir = NULL,
              dataPath = NULL, outDataPath = NULL, wait = TRUE, consoleOutput = FALSE, 
              configFile = NULL, nodes = NULL, computeOnHeadNode = FALSE, minElems = -1, maxElems = -1,
              priority = 2, exclusive = FALSE, autoCleanup = TRUE, dataDistType = "all", 
              packagesToLoad = NULL, email = NULL, resultsTimeout = 15, groups = "ComputeNodes"
              )
 
```
 
 
 ##Arguments

   
  
    
 >  `object` -  object of class RxHpcServer. This argument is optional. If supplied, the values of  the other specified arguments are used to replace those of `object` and the modified object is returned.  
  
    
 >  `headNode` -  character string specifying the name of the cluster's head node, which is not  to be confused with the master node.  
  
    
 >  `revoPath` -  character string specifying the path to the directory on the cluster  nodes containing the files R.exe and Rterm.exe.  The invocation of R on each  node must be identical (this is an HPC constraint). See the Details section for more  information regarding the path format.  
  
    
 >  `shareDir` -  character string specifying the directory on the head node that is  shared among all the nodes of the cluster and any client host. You must have permissions to read and write  in this directory. See the Details section for more information regarding the path format.  
  
    
 >  `workingDir` -  character string specifying a working directory for the processes  on the cluster.  If `NULL`, will default to the standard Windows user directory (that is, the value of the environment variable `USERPROFILE`).  
  
    
 >  `dataPath` -  character vector defining the search path(s) for the data source(s). See the Details section for more information regarding the path format.  
  
    
 >  `outDataPath` -  `NULL` or character vector defining the search path(s) for   new output data file(s).  If not `NULL`, this overrides any specification for `dataPath` in [rxOptions](rxOptions.md)    
  
    
 >  `wait` -  logical value. If `TRUE`, the job will be blocking and will not return until   it has completed or has failed. If `FALSE`, the job will be non-blocking return immediately,  allowing you to continue running other R code. The object `rxgLastPendingJob` is created with the job information. You can pass this object to the   [rxGetJobStatus](rxGetJobResults.md) function to check on the processing status of the job.  [rxWaitForJob](rxWaitForJob.md) will change a non-waiting job  to a waiting job. Conversely, pressing ESC changes a waiting job to a non-waiting job, provided that the HPC scheduler has accepted the job. If you press ESC before the job has been accepted, the job is canceled.  
  
    
 >  `consoleOutput` -  logical scalar. If `TRUE`, causes the standard output  of the R processes to be printed to the user console.   
  
    
 >  `configFile` -  character string specifying the path to the XML template (on your  local computer) that will be consumed by the job scheduler. If `NULL` (the default),  the standard template is used. See the Details section for more information regarding  the path format. We recommend that the user rely upon the default setting for `configFile`.  
  
    
 >  `nodes` -  character string containing a comma-separated list of requested  nodes, e.g., `"compute1,compute2,compute3"`, or a character vector like `c("compute1", "compute2", "compute3")`. If you specify the nodes to be used,  `minElems` and `maxElems` are ignored if greater than the number of specified nodes. See the Details section for more information.  The nodes parameter forces use of the specified nodes, rather than simply requesting them.  This feature is provided so that jobs requiring only 10 out of 100 nodes, for example,  do not force you to deploy your data to all 100 nodes.   Should you need finer control, you will need to generate a new XML template through the MS  HPC job scheduler. See the Microsoft HPC Server 2008 documentation for details.    
  
    
 >  `computeOnHeadNode` -  If `FALSE` and nodes are automatically being selected, the head  node of the cluster will not be among the nodes to be used.  Furthermore, if `FALSE` and the  head node is explicitly specified in the node list, a warning is issued, but the job proceeds with  a warning and the head node excluded from the node list.  If `TRUE`,  then the head node is treated exactly as any other node for the purpose of job resource allocations. Note that setting this flag to `TRUE` does not guarantee inclusion of the head node in a job; it simply allows the head node to be listed in an explicit list, or to be included by automatic node usage generation.  
  
    
 >  `minElems` -  minimum number of nodes required for a run. If `-1`, the value for `maxElems` is used. If both `minElems` and `maxElems` are `-1`, the number of nodes specified in the `nodes` argument is used. See the Details section for more information.  
  
    
 >  `maxElems` -  maximum number of nodes required for a run. If `-1`, the value for `minElems` is used. If both `minElems` and `maxElems` are `-1`, the number of nodes specified in the `nodes` argument is used. See the Details section for more information.  
  
    
 >  `priority` -  The priority of the job.  Allowable values are 0 (lowest), 1 (below normal), 2 (normal, the default), 3 (above normal), and 4 (highest). See the Microsoft HPC Server 2008 documentation  for using job scheduling policy options and job templates to manage job priorities. These policy options may also affect the behavior of the `exclusive` argument.    
    
 >  `exclusive` -  logical value. If `TRUE`, no other jobs can run on a compute node at the same time as this job. This may fail if you do not have administrative privileges  on the cluster, and may also fail depending on the settings of your job scheduling policy options.  
  
    
 >  `autoCleanup` -  logical scalar. If `TRUE`, the default behavior is to clean up the  temporary computational artifacts and delete the result objects upon retrival.  If `FALSE`,  then the computational results are not deleted, and the results may be acquired using  [rxGetJobResults](rxGetJobResults.md), and the output via [rxGetJobOutput](rxGetJobOutput.md) until the  [rxCleanupJobs](rxCleanup.md) is used to delete the results and other artifacts. Leaving this flag set to `FALSE` can result in accumulation of compute artifacts which you may eventually need to delete before they fill up your hard drive.If you set `autoCleanup=TRUE`and experience performance degradation on a Windows XP client, consider  setting `autoCleanup=FALSE`.  
  
    
 >  `dataDistType` -  a character string denoting how the data has been distributed. Type `"all"` means that the entire data set has been copied to each compute node. Type `"split"` means that the data set has been partitioned and that each compute node contains a different set of rows.  
  
    
 >  `packagesToLoad` -  optional character vector specifying additional packages to be  loaded on the nodes when jobs are run in this compute context.   
  
    
 >  `email` -  optional character string specifying an email address to which a job complete email should be sent.  Note that the cluster administrator will have to enable email notifications for such job completion mails to be received.  
  
    
 >  `resultsTimeout` -  A numeric value indicating for how long attempts should be made  to retrieve results from the cluster.  Under normal conditions, results are available immediately upon completion of the job.   However, under certain high load conditions, the processes on the nodes have reported as completed, but the results have not been fully committed to disk by the operating system.  Increase this parameter if results retrievial is failing on high load clusters.  
  
    
 >  `groups` -  Optional character vector specifying the groups from which nodes should be selected.  If `groups`is specified and `nodes` is `NULL`, all the nodes in the groups specified will be candidates for computations.   If both `nodes` and `groups` are specified, the candidate nodes will be the intersection of the set of nodes  explicitly specified in `nodes` and the set of all the nodes in the `groups` specified.    Note that the default value of `"ComputeNodes"` is the name of the Microsoft defined group that includes all physically present nodes.  Another Microsoft default group name used is `"HeadNodes"` which includes all nodes set up to act as head nodes.  While these  default names can be changed, this is not recommended.  Note also that [rxGetNodeInfo](rxGetNodeInfo.md) will honor group filtering.   See the help for `rxGetNodeInfo` for more information.  
  
 
 
 
 ##Details
 
A job is associated with the compute context in effect at the time the job
was submitted. If the compute context subsequently changes, the compute context of the
job is not affected.

For Windows operating systems, the arguments that define a path must be in long format, 
with double backslashes separating directories, and *not* DOS 8.3 (short) format, 
e.g., `"C:\\Program Files\\RRO\\R-3.1.3\\bin\\x64"` is the correct format 
while `"C:/PROGRA~1/RRO/R-31~1.3/bin/x64"` is not.

If you accept the default values for the `nodes` argument and 
both the `minElems` and `maxElems` arguments, *all* nodes 
are detected and used.

To allow the cluster to allocate the nodes to be used via the `minElems` 
and `maxElems` arguments, pass `NULL` (the default) for the 
`nodes` argument.

While the scheduler will allocate the number of resources requested by
`minElems` and `maxElems`, there is no guarantee that all of the requested 
resources will actually be given work to do. So, for example, if you set 
`minElems` to 5 and `maxElems` to 10, the scheduler will allocate somewhere 
between 5 and 10 compute elements to your job, but it may decide that the job 
will complete fastest if only three of the allocated elements are actually given 
work to do.
 
 
 
 ##Value
 
object of class RxHpcServer.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](http://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxGetJobStatus](rxGetJobResults.md),
[rxGetJobOutput](rxGetJobOutput.md),
[rxGetJobResults](rxGetJobResults.md), 
[RxHadoopMR](RxHadoopMR.md),
[RxInTeradata](RxInTeradata.md), 
[RxComputeContext](RxComputeContext.md),
[rxSetComputeContext](rxSetComputeContext.md),
[RxHpcServer-class](RxHPCServer-class.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
baseHPC <- RxHpcServer(
    headNode = "cluster-head", 
    revoPath = file.path(defaultRNodePath, "bin", "x64"), 
    shareDir = "\\AllShare\\myUserDir",
    workingDir = "C:\\Users\\username",
    dataPath = file.path(defaultRNodePath, "library", "RevoScaleR", "SampleData"),
    wait = FALSE, 
    consoleOutput = TRUE,
    nodes = c("COMPUTE1", "COMPUTE2"), 
    minElems = -1, 
    maxElems = -1, 
    priority = 4, 
    exclusive = FALSE,
    autoCleanup = TRUE)
  
# Create a modified RxHpcServer object for waiting jobs  
modifiedHPC <- RxHpcServer(baseHPC, wait = TRUE)

# Tell RevoScaleR to use baseHPC compute context
rxSetComputeContext(computeContext=baseHPC)

# Change compute context to use modifiedHPC
rxSetComputeContext(computeContext=modifiedHPC)
 ## End(Not run) 
  
 
```
 
 
 
