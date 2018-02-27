--- 
 
# required metadata 
title: "getPoolStatus function (mrsdeploy) | Microsoft Docs" 
description: " Get the status of the dedicated pool for a published web service running on Machine Learning Server. " 
keywords: "(mrsdeploy), getPoolStatus" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "02/05/2018" 
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
 
 
 
 
 #getPoolStatus: Get the status of the dedicated pool for a published web service 
 ##Description
 
Get the status of the dedicated pool for a published web service running on 
Machine Learning Server.
 
 
 ##Usage

```   
  getPoolStatus(name, version)
 
```
 
 ##Arguments

   
  
 ### `name`
 A string representing the web service name. To the dedicated pool status, you must provide the specific name and version of the  service that the pool is associated with. 
  
  
  
 ### `version`
 A string representing the web service version. To check the dedicated pool status, you must provide the specific name and version of the  service that the pool is associated with. 
  
 
 
 ##See Also
 
Other dedicated pool methods: [configureServicePool](ConfigureServicePool.md),
[deleteServicePool](DeleteServicePool.md)
   
 ##Examples

 ```
   
  ## Not run:
 

getPoolStatus(
   name = "myService",
   version = "v1"
)
 ## End(Not run) 
  
                                
 
```
 
