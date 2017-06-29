--- 
 
# required metadata 
title: " Provides information about all nodes on a cluster. " 
description: " Provides information about the capabilities of the nodes on a cluster. " 
keywords: "RevoScaleR, rxGetNodeInfo, IO" 
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
 
 
 #`rxGetNodeInfo`:  Provides information about all nodes on a cluster. 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Provides information about the capabilities of the nodes on a cluster.
 
 
 
 ##Usage

```   
  rxGetNodeInfo( computeContext = NULL, ..., namesOnly = FALSE, makeRNodeNames = FALSE, getWorkersOnly = TRUE )
 
```
 
 
 ##Arguments

   
  
 ### `computeContext`
 A distributed compute context (preferred), a jobInfo object, or (deprecated) a character scalar containing the name of the Microsoft HPC cluster head node being queried.  If you are interested in information about a particular node, be sure that node is included in the group specified in the compute context, or that the compute context has `groups=NULL` (HPC compute contexts).    Setting `nodes=NULL` and `groups=NULL` or `queue="all"` in the compute context is the best way to ensure getting information on all nodes. 
  
  
  
 ### ` ...`
 additional arguments for modifying the compute context before requesting the node information.  These arguments must be in the compute context constructor. For example, see [RxInTeradata](rxinteradata.md). 
  
  
  
 ### `namesOnly`
 logical. If `TRUE`, only a vector containing the names of the nodes will be returned. 
  
  
  
 ### `makeRNodeNames`
 logical. If `TRUE`, names of the nodes will be normalized for use  as R variables.  See [rxMakeRNodeNames](rxmakernodenames.md) for details on name mangling. 
  
  
  
 ### `getWorkersOnly`
 logical.  If `TRUE`, returns only those nodes within the cluster that are configured to actually execute jobs (where applicable; currently LSF only.). 
  
 
 
 
 ##Details
 
This function will return information on all the nodes on a given cluster if the `nodes` 
slot of the compute context is `NULL` and `groups` and `queue` are not 
specified in the compute context. If either `groups` or `queue` is specified,
information is provided only for nodes within the specified groups or queue, 
using the normal rules described in the compute context for taking the unions of node sets 
and nodes in queues.  The best way to get information on all nodes is to ensure that both 
the nodes and groups slots are set to `NULL` for HPC, and that nodes and 
queues are set to `NULL` and `"all"`, respectively, for LSF.

Note that unlike `rxGetAvailableNodes`, the return value of this function will 
contain all nodes in the compute context regardless of their state.  
(rxGetAvailableNodes returns only online nodes).

If the return value of this function is used to populate node names elsewhere and 
`namesOnly` is `TRUE`, `makeRNodeNames` should be set to the default `FALSE`.
If `namesOnly` is `FALSE`, the `nodeName` element of each list element should 
be used, not the names of the list element, given that the list element names will be 
mangled to be proper R names.  
This operation is performed because names with a dash (-) are not permitted as variable names 
(the R interpreter interprets this as a minus operation).  Thus, the mangling process replaces the 
dashes with an underscore _ to form the variable names.
See [rxMakeRNodeNames](rxmakernodenames.md) for details on name mangeling.

Also, note that  
node names are always forced to all capital letters (node names should be case agnostic, but in 
some cluster related software, it is necessary to force them to be upper case.
In general, however, machine names are always treated as being case insensitive.  
No other changes are made to node names during the mangling process.
)
 
 
 
 ##Value
 
If `namesOnly` is `TRUE`, a character vector containing the names of the nodes. 
If `makeRNodeNames` is `TRUE`, these names will be normalized for use as R variables.

If `namesOnly` is `FALSE`, a named list of lists, where each top level name is a node name 
on the cluster, normalized for use as an R variable.  See [rxMakeRNodeNames](rxmakernodenames.md) for
more details.

Each named element in the list will contain some or all of the following:


###`nodeName`
character scalar.  The true name of the node (as opposd to the list item name).


###`cpuSpeed`
float.  The processor speed in MHz for Microsoft HPC, or the CPU factor for LSF.


###`memSize`
integer.  The amount of RAM in MB.


###`numCores`
integer.  The number of cores.


###`numSockets`
integer.  The number of CPU sockets.


###`nodeState`
character scalar.  May be either "online", "offline", "draining" (shutting down, but still with jobs in queue), or "unknown".


###`isReachable`
logical.  Determines if there is an operational network path between the head node and the given compute node.


###`groupsOrQueues`
list of character scalars.  The node groups (on MS HPC) or queues (under LSF) to which the node belongs.


###`numPEs`
integer. The number of Parsing Engines (PEs) - for [RxInTeradata](rxinteradata.md).


###`numAmps`
integer. The number of Access Module Processors (AMPs) - for [RxInTeradata](rxinteradata.md).


###`nodeId`
integer. The ID of the node - for [RxInTeradata](rxinteradata.md).


 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[rxGetAvailableNodes](rxgetavailablenodes.md),
[RxInTeradata](rxinteradata.md),
[rxMakeRNodeNames](rxmakernodenames.md).
   
 ##Examples

 ```
   
  ## Not run:
 
# Returns list of lists with information about each specified node
rxGetNodeInfo(myCluster, nodes = "comp1,comp2,comp3")
rxGetNodeInfo(myCluster, nodes = c("g1","g2","g3"))

# Returns a vector with the names of the nodes in the cluster, excluding the 
# head node if computeOnHeadNode is set to FALSE in the compute context
rxGetNodeInfo(myCluster, namesOnly = TRUE)

# Node names in the returned vector are converted to valid R names
rxGetNodeInfo(myCluster, namesOnly = TRUE, makeRNodeNames = TRUE)

 ## End(Not run) 
  
 
```
 
 
