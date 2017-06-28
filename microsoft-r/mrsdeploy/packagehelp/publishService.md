--- 
 
# required metadata 
title: "Publishes an R code block or a Realtime model as a new web service." 
description: " Publishes an R code block or a Realtime model as a new web service running on R Server. " 
keywords: "mrsdeploy, publishService" 
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
 
 
 
 
 #`publishService`: Publishes an R code block or a Realtime model as a new web service.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Publishes an R code block or a Realtime model as a new web service running on
R Server.
 
 
 ##Usage

```   
  publishService(name, code, model = NULL, snapshot = NULL, inputs = list(),
    outputs = list(), artifacts = c(), v = character(0), alias = NULL,
    destination = NULL, descr = NULL, serviceType = c("Script", "Realtime"))
 
```
 
 ##Arguments

   
  
 ### `name`
 The web service name. 
  
  
  
 ### `code`
 R code to publish. `code` can take the form of:  
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
  
  
  
 ### `v`
 (optional) Web service version. If the version is left blank, a unique `guid` will be generated in its place. Useful during service  development before the author is ready to officially publish a semantic  version to share. 
  
  
  
 ### `alias`
 (optional) Defines predication RPC function used to consume the  service. 
  
  
  
 ### `destination`
 (optional) The codegen output directory location. 
  
  
  
 ### `descr`
 (optional) The description of the web service. 
  
  
  
 ### `serviceType`
 (optional) The type of the web service. Valid values are  'Script' and 'Realtime'. Defaults to 'Script'. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##See Also
 
Other service methods: [deleteService](../../r-reference/mrsdeploy/deleteservice.md),
[getService](../../r-reference/mrsdeploy/getservice.md), [listServices](../../r-reference/mrsdeploy/listservices.md),
[print.serviceDetails](../../r-reference/mrsdeploy/print-servicedetails.md),
[serviceOption](serviceOption.md),
[summary.serviceDetails](summary.serviceDetails.md),
[updateService](updateService.md)
   
 ##Examples

 ```
   
  ## Not run:
 

# --- Publish service using `code` by function handle
add <- function(x, y) {
  x + y
}

publishService(
   "add-service",
    code = add,
    inputs = list(x = "numeric", y = "numeric"),
    outputs = list(result = "numeric")
)
 
publishService(
   "add-service",
    code = add,
    inputs = list(x = "numeric", y = "numeric"),
    outputs = list(result = "numeric"),
    serviceType = 'Script'
)
                
# --- Publish service using `code` by represented string values
publishService(
   "add-service",
    code = "result <- x + y",
    inputs = list(x = "numeric", y = "numeric"),
    outputs = list(result = "numeric")
)

# --- Publish service using `code` by file-path
publishService(
   "add-service",
    code = "/path/to/add.R", 
    inputs = list(x = "numeric", y = "numeric"),
    outputs = list(result = "numeric")
)

# --- Publish service using Realtime model
publishService(
  "kyphosisLogRegModel",
  code = NULL,
  # --- `model` is required for web service with serviceType `Realtime` --- #
  model = rxLogit(Kyphosis ~ Age, data=kyphosis),
  v = "v1.0.0",
  alias = "predictKyphosisModel",
  # --- `serviceType` is required for this web service --- #
  serviceType = "Realtime"
)
 ## End(Not run) 
  
 
```
 
 
