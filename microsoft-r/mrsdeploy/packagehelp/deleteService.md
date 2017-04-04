--- 
 
# required metadata 
title: "Delete a web service.." 
description: " Delete a web service on an R Server instance. " 
keywords: "mrsdeploy, deleteService" 
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
 
 
 
 
 #`deleteService`: Delete a web service..

 Applies to version 1.0 of package mrsdeploy.
 
 ##Description
 
Delete a web service on an R Server instance.
 
 
 ##Usage

```   
  deleteService(name, v)
 
```
 
 ##Arguments

   
  
 ### `name`
 The web service name. 
  
  
  
 ### `v`
 The web service version. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##See Also
 
Other service.methods: [getService](getService.md),
[listServices](listServices.md), [publishService](publishService.md),
[serviceOption](serviceOption.md), [updateService](updateService.md)
   
 ##Examples

 ```
   
  ## Not run:
 
# Delete the `add-service` version `1.0.1`
deleteService("add-service", "1.0.1")
 ## End(Not run) 
  
 
```
 
 
