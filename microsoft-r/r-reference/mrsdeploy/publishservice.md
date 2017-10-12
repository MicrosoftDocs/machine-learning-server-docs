--- 
 
# required metadata 
title: "publishService function (mrsdeploy) " 
description: " Publishes an R code block or a Realtime model as a new web service running on R Server. " 
keywords: "(mrsdeploy), publishService" 
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
 
 
 
 
 #publishService: Publishes an R code block or a Realtime model as a new web service. 
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
 A string representing the web service name. Use quotes such as  "MyService". We recommend a name that is easily understood and remembered. 
  
  
  
 ### `code`
 Required for standard web services. The R code to publish.  If you use a path, the base path is your local current working directory. `code` can take the form of:  
*   A function handle such as  `code = function(hp, wt) {` `newdata <- data.frame(hp = hp, wt = wt)` `predict(model, newdata, type = "response")` `}` 
*   A block of arbitrary R code as a character string such as`code = "result <- x + y"` 
*   A filepath to a local `.R` file containing R code such as `code = "/path/to/R/script.R"` 
 If serviceType is 'Realtime', code has to be NULL. 
  
  
  
 ### `model`
 (optional) For standard web services, an `object` or a filepath to an external representation of R objects to be loaded and used  with `code`. The specified file can be: 
*   Filepath to a local `.RData` file holding R objects to be loaded. For example,  `model = "/path/to/glm-model.RData"` 
*   Filepath to a local `.R` file that is evaluated into an environment and loaded. For example,  `model = "/path/to/glm-model.R"` 
*   An object. For example,  `model = "model = am.glm"` 
 For realtime web services (serviceType = 'Realtime'), only an R model  object is accepted. For example, `model = "model = am.glm"` 
  
  
  
 ### `snapshot`
 (optional) Identifier of the session snapshot to load. Can replace the model argument or be merged with it. If serviceType is 'Realtime', snapshot has to be NULL. 
  
  
  
 ### `inputs`
 (optional) Defines the web service input schema. If empty, the service will not accept inputs. `inputs` are defined as a named list  `list(x = "logical")` which describes the input parameter  names and their corresponding **data types**:  
*   `numeric` 
*   `integer` 
*   `logical` 
*   `character` 
*   `vector` 
*   `matrix` 
*   `data.frame` 
 If serviceType is 'Realtime', inputs has to be NULL. Once published,  service inputs automatically default to  list(inputData = 'data.frame'). 
  
  
  
 ### `outputs`
 (optional) Defines the web service output schema. If empty, the service will not return a response value. `outputs` are defined as a  named list `list(x = "logical")` that describes the output parameter  names and their corresponding **data types**:  
*   `numeric` 
*   `integer` 
*   `logical` 
*   `character` 
*   `vector` 
*   `matrix` 
*   `data.frame` 
 Note: If `code` is defined as a `function`, then only one output value can be claimed. If serviceType is 'Realtime', outputs has to be NULL. Once published,  service outputs automatically default to  list(inputData = 'data.frame'). 
  
  
  
 ### `artifacts`
 (optional) A character vector of filenames defining which file artifacts should be returned during service consumption. File content is encoded as a `Base64 String`. 
  
  
  
 ### `v`
 (optional) Defines a unique alphanumeric web service version.  If the version is left blank, a unique `guid` is generated in its place. Useful during service development before the author is ready to officially publish a semantic version to share. 
  
  
  
 ### `alias`
 (optional) An alias name of the predication remote procedure call (RPC) function used to consume the service. If code is a function,  then it will use that function name by default. 
  
  
  
 ### `destination`
 (optional) The codegen output directory location. 
  
  
  
 ### `descr`
 (optional) The description of the web service. 
  
  
  
 ### `serviceType`
 (optional) The type of the web service. Valid values are  'Script' and 'Realtime'. Defaults to 'Script', which is for a standard web  service, contains arbitrary R code, R scripts and/or models. 'Realtime' contains only a supported model object (see 'model' argument above). 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##See Also
 
Other service methods: [deleteService](deleteService.md),
[getService](getService.md), [listServices](listServices.md),
[print.serviceDetails](print.serviceDetails.md),
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
 
