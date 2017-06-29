--- 
 
# required metadata 
title: "Get a web service for consumption." 
description: " Get a web service for consumption on running on R Server. " 
keywords: "mrsdeploy, getService" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 
 
 #`getService`: Get a web service for consumption.

 Applies to version 1.1.0 of package mrsdeploy.
 
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
 
Other service methods: [deleteService](deleteservice.md),
[listServices](listservices.md),
[print.serviceDetails](print-servicedetails.md),
[publishService](publishservice.md),
[serviceOption](serviceoption.md),
[summary.serviceDetails](summary-servicedetails.md),
[updateService](updateservice.md)
   
 ##Examples

 ```
   
  ## Not run:
 
# Discover the `add-service` version `1.0.1`
api <- getService("add-service", "1.0.1")
 ## End(Not run) 
  
 
```
 
 
