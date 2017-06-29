--- 
 
# required metadata 
title: "Retrieve, set, and list the different service options." 
description: " Retrieve, set, and list the different service options. " 
keywords: "mrsdeploy, serviceOption" 
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
 
 
 
 
 #`serviceOption`: Retrieve, set, and list the different service options.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Retrieve, set, and list the different service options.
 
 
 ##Usage

```   
  serviceOption()
 
```
 
 ##Arguments

   
  
 ### `name`
 The option name. 
  
  
  
 ### `value`
 The option value to be set. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
A list of the current service options.
 
 ##See Also
 
Other service methods: [deleteService](deleteservice.md),
[getService](getservice.md), [listServices](listservices.md),
[print.serviceDetails](print-servicedetails.md),
[publishService](publishservice.md),
[summary.serviceDetails](summary-servicedetails.md),
[updateService](updateservice.md)
   
 ##Examples

 ```
   
  ## Not run:
 

# --- set option name|value
opts <- serviceOption()
opts$set('base_dir', getwd())

# --- get option
opts <- serviceOption()
base_dir <- opts$get('base_dir')

# --- list options
options <- serviceOption()$get()
 ## End(Not run) 
  
 
```
 
 
