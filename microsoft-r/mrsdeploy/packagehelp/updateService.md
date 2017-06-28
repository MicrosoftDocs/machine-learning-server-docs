--- 
 
# required metadata 
title: "Updates an existing web service." 
description: " Updates an existing web service on an R Server instance. " 
keywords: "mrsdeploy, updateService" 
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
 
 
 
 
 #`updateService`: Updates an existing web service.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Updates an existing web service on an R Server instance.
 
 
 ##Usage

```   
  updateService(name, v, code = NULL, model = NULL, snapshot = NULL,
    inputs = NULL, outputs = NULL, artifacts = c(), alias = NULL,
    destination = NULL, descr = NULL, serviceType = c("Script", "Realtime"))
 
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
 If serviceType is 'Realtime', code has to be NULL. 
  
  
  
 ### `model`
 (optional) An object or a file-path to an external  representation of R objects to be loaded and used with `code`.  The specified file can be: 
*   File-path to an `.RData` file holding R objects to be loaded. 
*   File-path to an `.R` file which will be evaluated into an environment and loaded. 
 If serviceType is 'Realtime', model has to be an R object. 
  
  
  
 ### `snapshot`
 (optional) Identifier of the snapshot to load. If serviceType is 'Realtime', snapshot has to be NULL. 
  
  
  
 ### `inputs`
 (optional) Defines the web service input schema. If empty, the service will not accept inputs. `inputs` are defined as a named list  `list(x = "logical")` which describe the input parameter  names and their corresponding **Data Types**:  
*   `numeric` 
*   `integer` 
*   `logical` 
*   `character` 
*   `vector` 
*   `matrix` 
*   `data.frame` 
 . If serviceTypeis 'Realtime', inputs has to be NULL. Once published, service's inputs will default to  list(inputData = 'data.frame'). 
  
  
  
 ### `outputs`
 (optional) Defines the web service output schema. If empty, the service will not return a response value. `outputs` are defined as a  named list `list(x = "logical")` which describe the output parameter  names and their corresponding **Data Types**:  
*   `numeric` 
*   `integer` 
*   `logical` 
*   `character` 
*   `vector` 
*   `matrix` 
*   `data.frame` 
 Note: If `code` is defined as a `function` then only one output value can be claimed. If serviceType is 'Realtime', outputs has to be NULL. Once published, service's outputs will default to  list(outputData = 'data.frame'). 
  
  
  
 ### `artifacts`
 (optional) A character vector of filenames defining which file artifacts should be returned during service consumption. File content is encoded as a `Base64 String`. 
  
  
  
 ### `alias`
 (optional) Defines predication RPC function used to consume the service. 
  
  
  
 ### `destination`
 (optional) The codegen output directory location. 
  
  
  
 ### `descr`
 (optional) The description of the web service. 
  
  
  
 ### `serviceType`
 (optional) The type of the web service. Valid values are  'Script' and 'Realtime'. Defaults to 'Script'. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)



Updates an existing service by `name` and version `v`.
 
 
 ##See Also
 
Other service methods: [deleteService](../../r-reference/mrsdeploy/deleteservice.md),
[getService](../../r-reference/mrsdeploy/getservice.md), [listServices](../../r-reference/mrsdeploy/listservices.md),
[print.serviceDetails](print.serviceDetails.md),
[publishService](publishService.md),
[serviceOption](serviceOption.md),
[summary.serviceDetails](summary.serviceDetails.md)
   
 ##Examples

 ```
   
  ## Not run:
 

# Updates a service's `descr` description field
updateService(
   "add",
   "1.0.1",
   descr = "Update the description field."
)

# Updates a service's `descr` description field using optional `serviceType`
updateService(
   "add",
   "1.0.1",
   descr = "Update the description field.",
   serviceType = "Script"
)

# Updates a Realtime service's `descr` description field using `serviceType`
updateService(
 "kyphosisLogRegModel",
 "v1.0.0",
 descr = "Update the description for `kyphosisLogRegModel` Realtime service.",
 # --- `serviceType` below is required for this web service --- #
 serviceType = "Realtime"
)
 ## End(Not run) 
    
 
```
 
 
