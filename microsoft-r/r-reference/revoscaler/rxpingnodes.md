--- 
 
# required metadata 
title: " Simple Test of Compute Cluster Nodes " 
description: " This function provides a simple test of the compute context's ability to perform a round trip through one or more  computation nodes. " 
keywords: "RevoScaleR, rxPingNodes, IO" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
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
 
 
 #`rxPingNodes`:  Simple Test of Compute Cluster Nodes 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
This function provides a simple test of the compute context's ability to perform a round trip through one or more 
computation nodes.
 
 
 
 ##Usage

```   
  rxPingNodes(computeContext = rxGetOption("computeContext"), timeout = 0, filter = NULL)
 
```
 
 
 ##Arguments

   
  
 ### `computeContext`
 A distributed compute context. [RxHpcServer](revoscaler-deprecated.md), and  [RxInTeradata](rxinteradata.md) contexts are supported.  See the details section for more information. 
  
  
  
 ### `timeout`
 A non-negative integer value.  Real valued numbers will be cast to integers.  This parameter sets a total (real clock) time to attempt to perform a ping.  The timer is not started until  the system has first determined which nodes are unavailable (meaning down, unreachable or not usable for jobs,  such as scheduler only nodes on LSF).   At least one attempt to complete a ping is performed regardless of the setting of `timeout`.  If the default value of `0` is used, there is no timeout. 
  
  
  
 ### `filter`
 `NULL`, or a character vector containing one or more of the ping states.  If `NULL`, no filtering is  performed.  If a character vector of one or more of the ping states is provided, then only those nodes determined to be in the  states enumerated will be returned. 
  
  
 
 
 
 ##Details
 
This function provides an application level "ping" to one or more nodes in a cluster or cloud.  To this end, a trivial job is launched for 
each node to be pinged.  A successful ping means that communication between the end user and the scheduler and communication between the 
end user and the shared data directory was successful; that R was launched and ran a trivial function successfully on the host being pinged; 
and that the results were returned successfully.

While this function does not test certain aspects of the messaging required for HPA functions (e.g., `rxSummary`), it does allow the user 
to easily test the majority of the end-to-end job functionality within a supported cloud or cluster.

The compute context provided is used to determine the cluster or queue 
that is to be pinged.  Furthermore, the nodes to be pinged will be determined in the usual fashion; that is, 
`NULL` in the `nodes` indicates use of all nodes; values in the `groups` or `queue` fields will 
cause the set of nodes to be checked to be the intersection between all the nodes in the `groups` or `queue` 
specified, and the set of nodes specifically specified in the `nodes` parameter, and so forth.  Note that for 
clusters and clouds that do have a head node, `computeOnHeadNode` is respected.  For more 
information, see the compute context constructors or the [rxGetNodeInfo](rxgetnodeinfo.md) for more information.

Most other values in the compute context are respected when determing how a ping will be sent.  The following fields in particular are of note 
when using this tool:



###`priority` 
May be used to allow the ping jobs to run sooner than other longer running jobs.


###`exclusive` 
Should usually be avoided


###`autoCleanup` 
Should almost always be set to `TRUE`; however, may be of use to a system administrator diagnosing a problem.



 
 
 
 ##Value
  An object of type `rxPingResults`.  This is essentially a list in which component is named using an [rxMakeRNodeNames](rxmakernodenames.md) translated 
node name in the same manner and for the same reasons described for [rxGetNodeInfo](rxgetnodeinfo.md), with the `getWorkersOnly` parameter set to FALSE.  
Each element of this list contains two
elements: `nodeName` which holds the true, unmangled name of the node, and `status`, which contains a character scalar with one of 
the following values:


###`unavail`
The node failed its scheduler level check prior to an attempt to actually ping the node.  This does not necessarily mean that the node is not not functional;  rather, it only means that it cannot support having a job run on it.


###`success`
The round trip job was a success.


###`failedJob`
The scheduler failed the job.  This could be due to permissions, corrupt libraries, or a problem relating to the GUID directory.


###`failedSession`
The R process on the worker host was started, but failed.


###`timeout`
The ping was sent, but a response was never received.  This could be due to a problem with the installation, or other long running jobs being queued ahead of the ping job, or a system failure.


###`failedAmps`
Not all of the AMPs were successful - for [RxInTeradata](rxinteradata.md).


An `as.vector` method is provided for the `rxPingResults` object which returns a character vector of the non-mangled 
([rxMakeRNodeNames](rxmakernodenames.md) translated) node names for use in another compute context, filtered by the `filter` parameter originally 
provided.

Finally, the `rxPingResults` object has a `logical` attribute associated with it: `allOk`.  This attribute is set to `TRUE` if all of 
the pinged nodes' states (after filtering) were set to `"success"`.  Otherwise, this attribute is set to `FALSE`.

 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[rxGetAvailableNodes](rxgetavailablenodes.md),
[RxInTeradata](rxinteradata.md),
[rxMakeRNodeNames](rxmakernodenames.md),
[rxGetNodeInfo](rxgetnodeinfo.md),
[rxGetAvailableNodes](rxgetavailablenodes.md).
   
 
 
 ##Examples

 ```
   
  ## Not run:
 

# Attempts to ping all the nodes for the current compute context.
rxPingNodes()

# Pings all the nodes from myCluster, returning values only for those that are
# currently not operational
rxPingNodes( myCluster, filter=c("unavail","failedJob","failedSession") )

# Pings all the nodes from myCluster; times out after 2 minutes
rxPingNodes( myCluster, timeout=120 )

# Extract the allOk attribute from the return value
attr(rxPingNodes(), "allOk")
 ## End(Not run) 
  
 
```
 
 
 
 
 
 
 
 
 
