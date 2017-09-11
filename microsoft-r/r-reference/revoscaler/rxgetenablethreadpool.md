--- 
 
# required metadata 
title: "rxGetEnableThreadPool function (RevoScaleR) | Microsoft Docs" 
description: " Gets or sets the current state of the thread pool (in a ready state or created ad hoc). " 
keywords: "(RevoScaleR), rxGetEnableThreadPool, rxSetEnableThreadPool, iteration" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
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
 
 
 
 #rxGetEnableThreadPool:  Get or Set Thread Pool State  
 ##Description
 
Gets or sets the current state of the thread pool (in a ready state or created ad hoc).
 
 
 
 ##Usage

```   
  rxGetEnableThreadPool()
  rxSetEnableThreadPool( enable )
 
```
 
 ##Arguments

   
  
 ### `enable`
  Logical scalar. If `TRUE`, the thread pool is instantiated and maintained in a ready state. If `FALSE`, threads are created in an ad hoc fashion; that is, they are created as needed.  
  
 
 
 ##Details
 
The `rxSetEnableThreadPool` function is used on Linux to turn the thread pool on and off.  When the thread pool is on, 
it means that there exists a pool of threads that persist between calls, and are ready for processing.  
When the thread pool is off, it means that threads will be created on an ad hoc basis when calls requiring
threading are made. By default, the thread pool is turned off to allow R processes that have loaded RevoScaleR
to fork successfully as a mechanism for spawning (for example, by the `multicore` package).

On the Windows platform, the thread pool always persists, regardless of any user settings. 

There is a significant speed increase associated with having the thread pool ready and waiting to do work.  If 
you are sure that you will not be using any spawning mechanism that uses fork, you may want to put the following 
at the end of your Rprofile.site.  Make absolutely sure that it is after any lines that are used to load RevoScaleR:

` invisible( rxSetEnableThreadPool( TRUE ) ) `

The following are possible cases where fork may be called.  Obviously, this is not an all-inclusive list:



* 
`nohup` to launch jobs.  This will cause a fork to be called.

* 
Using `multicore` and/or `doMC` to launch R processes with RevoScaleR



Note that when using [rxExec](rxExec.md), the default behavior for any worker node process on a Linux host will be to have 
the thread pool off (set to create threads in an ad hoc manner).  If the function passed to [rxExec](rxExec.md) is going to make 
multiple calls into RevoScaleR functions, you will probably want to include a call to `rxSetEnableThreadPool(TRUE)` as
the first line of the function that you pass to [rxExec](rxExec.md).

For distributed HPA functions run on worker nodes, threading is always automatically handled for the user.

The `rxGetEnableThreadPool` function can be used to determine whether the thread pool is instantiated or not.  Note that on Windows,
it will always be instantiated; on Linux, whether it is always instantiated or created on an ad hoc basis
is controlled by `rxSetEnableThreadPool`.  At startup on Linux, the thread pool state wil be off; that is,
threads will be created on an ad hoc basis.
 
 
 ##Value
 
`rxSetEnableThreadPool` returns the state of the thread pool prior to making the call (as opposed to the updated state).  Thus, this function returns
`TRUE` if the thread pool was instantiated and in a ready state prior to making the call, or `FALSE` if the thread pool 
was set to be created in an ad hoc fashion. 
 

 


 
 
 ##See Also
 
[rxOptions](rxOptions.md)
   
 
 ##Examples

 ```
   
  ## Not run:
 
rxGetEnableThreadPool()
rxSetEnableThreadPool(TRUE)
rxGetEnableThreadPool()
  ## End(Not run) 
  
 
```
 
 
