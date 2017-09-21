--- 
 
# required metadata 
title: "listServices function (mrsdeploy) | Microsoft Docs" 
description: " List the different published web services on the R Server instance. " 
keywords: "(mrsdeploy), listServices" 
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
 
 
 
 
 #listServices: List the different published web services. 
 ##Description
 
List the different published web services on the R Server instance.
 
 
 ##Usage

```   
  listServices(name = character(0), v = character(0))
 
```
 
 ##Arguments

   
  
 ### `name`
 When a name is specified, returns only web services with that  name. Use quotes around the name string, such as "MyService". 
  
  
  
 ### `v`
 When specified, returns only web services with that version. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)


The service `name` and service `v` version are optional. This
call allows you to retrieve service information regarding:



1 
 All services published

1 
 All versioned services for a specific named service

1 
 A specific version for a named service



Users can use this information along with the `discover_service`
operation to interact with and consume the web service.
 
 
 ##Value
 
A list of services and service information.
 
 ##See Also
 
Other service methods: [deleteService](deleteService.md),
[getService](getService.md),
[print.serviceDetails](print.serviceDetails.md),
[publishService](publishService.md),
[serviceOption](serviceOption.md),
[summary.serviceDetails](summary.serviceDetails.md),
[updateService](updateService.md)
   
 ##Examples

 ```
   
  ## Not run:
 

# --- List all published services
services <- listServices()

# --- List all published versions of a service
services <- listServices("transmission")

# --- List a specific service version
service <- listServices("transmission", "1.0.1")
 ## End(Not run) 
  
 
```
 
