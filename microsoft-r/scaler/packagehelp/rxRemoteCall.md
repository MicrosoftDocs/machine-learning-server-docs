--- 
 
# required metadata 
title: " Remote calling mechanism for distributed computing." 
description: "Function that is executed on the master node in a distributed computing environment. This function should **not** be called directly by the user and, rather, serves as a necessary and convenient mechanism for performing remote R calls on the master node in a distributed computing environment." 
keywords: "RevoScaleR, rxRemoteCall, datasets" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 #`rxRemoteCall`:  Remote calling mechanism for distributed computing.

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 Function that is executed on the master node in a distributed computing environment.
This function should **not** be called directly by the user and, rather, serves as a
necessary and convenient mechanism for performing remote R calls on the master node in
a distributed computing environment. 
 
 
 ##Usage

```   rxRemoteCall(rxCommArgsList = NULL) 
```
 
 ##Arguments

   
    
 ### `rxCommArgsList`
 list of arguments 
  
 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
