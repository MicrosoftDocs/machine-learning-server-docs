--- 
 
# required metadata 
title: "getService function (mrsdeploy) " 
description: " Get a web service for consumption on running on R Server. " 
keywords: "(mrsdeploy), getService" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/18/2017" 
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
 
 
 
 
 #getService: Get a web service for consumption. 
 ##Description
 
Get a web service for consumption on running on R Server.
 
 
 ##Usage

```   
  getService(name, v = character(0), destination = NULL)
 
```
 
 ##Arguments

   
  
 ### `name`
 The web service name. 
  
  
  
 ### `v`
 The web service version. 
  
  
  
 ### `destination`
 (optional) The codegen output directory location. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##See Also
 
Other service methods: [deleteService](deleteService.md),
[listServices](listServices.md),
[print.serviceDetails](print.serviceDetails.md),
[publishService](publishService.md),
[serviceOption](serviceOption.md),
[summary.serviceDetails](summary.serviceDetails.md),
[updateService](updateService.md)
   
 ##Examples

 ```
   
  ## Not run:
 
# Discover the `add-service` version `1.0.1`
api <- getService("add-service", "1.0.1")
 ## End(Not run) 
  
 
```
 
