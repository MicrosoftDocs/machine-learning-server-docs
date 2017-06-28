--- 
 
# required metadata 
title: "List the different published web services." 
description: " List the different published web services on the R Server instance. " 
keywords: "mrsdeploy, listServices" 
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
 
 
 
 
 #`listServices`: List the different published web services.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
List the different published web services on the R Server instance.
 
 
 ##Usage

```   
  listServices(name = character(0), v = character(0))
 
```
 
 ##Arguments

   
  
 ### `name`
 The optional web service name. 
  
  
  
 ### `v`
 The optional web service version. 
  
 
 
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
 
Other service methods: [deleteService](deleteservice.md),
[getService](getservice.md),
[print.serviceDetails](print-servicedetails.md),
[publishService](publishservice.md),
[serviceOption](serviceoption.md),
[summary.serviceDetails](summary-servicedetails.md),
[updateService](updateservice.md)
   
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
 
 
