--- 
 
# required metadata 
title: "Get and Set the compute context" 
description: "Get or set the active compute context for RevoScalePy computationsrx_set_compute_context(computeContext: ‘RxComputeContext’)" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
<<<<<<< HEAD
ms.date: "06/26/2017" 
=======
ms.date: "06/27/2017" 
>>>>>>> heidist-revoscalepy
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

<<<<<<< HEAD
# rx_set_compute_context
=======
## rx_set_compute_context


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.computecontext.RxComputeContext.rx_set_compute_context(computeContext: revoscalepy.computecontext.RxComputeContext.RxComputeContext) -> revoscalepy.computecontext.RxComputeContext.RxComputeContext
```




<<<<<<< HEAD
## Description
=======
### Description
>>>>>>> heidist-revoscalepy

Get or set the active compute context for RevoScalePy computations

rx_set_compute_context(computeContext: ‘RxComputeContext’)
rx_get_compute_context()


<<<<<<< HEAD
## Parameters


### compute_context
=======
### Arguments


##### compute_context
>>>>>>> heidist-revoscalepy

character string specifying class name or description
of the specific class to instantiate, or an existing RxComputeContext object.
Choices include: “RxLocalSeq” or “local”, “RxLocalParallel” or “localpar”.


<<<<<<< HEAD
## Returns
=======
### Returns
>>>>>>> heidist-revoscalepy

rx_set_compute_context returns the previously active compute context
invisibly. rx_get_compute_context returns the active compute context.


<<<<<<< HEAD
## Author
=======
### Author
>>>>>>> heidist-revoscalepy

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


<<<<<<< HEAD
## See also


## Example
=======
### See also


### Example
>>>>>>> heidist-revoscalepy



```
localcc = RxLocalSeq()
inSqlServercc = RxInSqlServer('Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;')
rx_set_compute_context(localcc)
previouscc = rx_set_compute_context(inSqlServercc)
```

