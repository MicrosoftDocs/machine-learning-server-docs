--- 
 
# required metadata 
title: "Get Distributed Computing Jobs" 
description: "Returns a list of job objects associated with the given compute context and matching the specified parameters." 
keywords: "" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "jhubbard" 
ms.date: "07/13/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# rx_get_jobs


**Applies to: SQL Server 2017 RC1**


## Usage



```
revoscalepy.computecontext.RxJob.rx_get_jobs()
```




## Description

Returns a list of job objects associated with the given compute context and matching the specified parameters.


## Arguments


### compute_context

A compute context object.


### exact_match

Determines if jobs are matched using the full compute
context, or a simpler subset. If True, only jobs which use the same context
object are returned. If False, all jobs which have the same headNode (if
available) and ShareDir are returned.


### start_time

A time, specified as a POSIXct object. If specified, only
jobs created at or after start_time are returned.


### end_time

A time, specified as a POSIXct object. If specified, only
jobs created at or before end_time are returned.


### states

If specified (as a list of strings of states that can include
“none”, “finished”, “failed”, “canceled”, “undetermined” “queued”or
“running”), only jobs in those states are returned. Otherwise, no filtering
is performed on job state.


### verbose

If True (the default), a brief summary of each job is printed
as it is found. This includes the current job status as returned by
rx_get_job_status, the modification time of the job, and the current job ID
(this is used as the component name in the returned list of job information
objects). If no job status is returned, the job status shows none.


## Returns

Returns a list of job information objects based on the compute context.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)


## See also


## Example
