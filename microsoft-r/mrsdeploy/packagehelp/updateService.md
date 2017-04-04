--- 
 
# required metadata 
title: "Updates an existing web service." 
description: " Updates an existing web service on an R Server instance. " 
keywords: "mrsdeploy, updateService" 
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
 
 
 
 
 #`updateService`: Updates an existing web service.

 Applies to version 1.0 of package mrsdeploy.
 
 ##Description
 
Updates an existing web service on an R Server instance.
 
 
 ##Usage

```   
  updateService(name, v, code = NULL, model = NULL, snapshot = NULL,
    inputs = NULL, outputs = NULL, alias = NULL, destination = NULL,
    descr = NULL)
 
```
 
 ##Arguments

   
  
 ### `name`
 The web service name. 
  
  
  
 ### `v`
 The web service version. 
  
  
  
 ### `code`
 (optional) R code to publish. `code` can take the form of:  
*   A function handle. 
*   A block of R code. 
*   A File-path to an `.R` file containing a block of R code. 
 
  
  
  
 ### `model`
 (optional) An object or a file-path to an external  representation of R objects to be loaded and used with `code`.  The specified file can be: 
*   File-path to an `.RData` file holding R objects to be loaded. 
*   File-path to an `.R` file which will be evaluated into an environment and loaded. 
 
  
  
  
 ### `snapshot`
 (optional) Identifier of the snapshot to load. 
  
  
  
 ### `inputs`
 (optional) Defines the web service input schema. If empty, the service will not accept inputs. `inputs` are defined as a named list  `list(x = "logical")` which describe the input parameter  names and their corresponding **Data Types**:  
*   `numeric` 
*   `integer` 
*   `logical` 
*   `character` 
*   `vector` 
*   `matrix` 
*   `data.frame` 
 . 
  
  
  
 ### `outputs`
 (optional) Defines the web service output schema. If empty, the service will not return a response value. `outputs` are defined as a  named list `list(x = "logical")` which describe the output parameter  names and their corresponding **Data Types**:  
*   `numeric` 
*   `integer` 
*   `logical` 
*   `character` 
*   `vector` 
*   `matrix` 
*   `data.frame` 
 Note: If `code` is defined as a `function` then only one output value can be claimed. 
  
  
  
 ### `alias`
 (optional) Defines predication RPC function used to consume the service. 
  
  
  
 ### `destination`
 (optional) The codegen output directory location. 
  
  
  
 ### `descr`
 (optional) The description of the web service. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)



Updates an existing service by `name` and version `v`.
 
 
 ##See Also
 
Other service.methods: [deleteService](deleteService.md),
[getService](getService.md), [listServices](listServices.md),
[publishService](publishService.md), [serviceOption](serviceOption.md)
   
 ##Examples

 ```
   
  ## Not run:
 

# Updates a service's `descr` description field
updateService(
   "add",
   "1.0.1",
   descr = "Update the description field."
)
 ## End(Not run) 
    
 
```
 
 
