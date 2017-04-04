--- 
 
# required metadata 
title: "Publishes an R code block as a new web service." 
description: " Publishes an R code block as a new web service running on R Server. " 
keywords: "mrsdeploy, publishService" 
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
 
 
 
 
 #`publishService`: Publishes an R code block as a new web service.

 Applies to version 1.0 of package mrsdeploy.
 
 ##Description
 
Publishes an R code block as a new web service running on R Server.
 
 
 ##Usage

```   
  publishService(name, code, model = NULL, snapshot = NULL, inputs = list(),
    outputs = list(), v = character(0), alias = NULL, destination = NULL,
    descr = NULL)
 
```
 
 ##Arguments

   
  
 ### `name`
 The web service name. 
  
  
  
 ### `code`
 R code to publish. `code` can take the form of:  
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
  
  
  
 ### `v`
 (optional) Web service version. If the version is left blank, a unique `guid` will be generated in its place. Useful during service  development before the author is ready to officially publish a semantic  version to share. 
  
  
  
 ### `alias`
 (optional) Defines predication RPC function used to consume the  service. 
  
  
  
 ### `destination`
 (optional) The codegen output directory location. 
  
  
  
 ### `descr`
 (optional) The description of the web service. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##See Also
 
Other service.methods: [deleteService](deleteService.md),
[getService](getService.md), [listServices](listServices.md),
[serviceOption](serviceOption.md), [updateService](updateService.md)
   
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
 ## End(Not run) 
  
 
```
 
 
