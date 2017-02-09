---

# required metadata
title: "Managing Web Services in Microsoft R"
description: "Web service deployment functions in the mrsdeploy package in Microsoft R can be used for any arbitrary R code block. A web service runs on R Server 9.0 instances."
keywords: "mrsdeploy package"
author: "j-martens"
manager: "jhubbard"
ms.date: "02/08/2017"
ms.topic: "article"
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

# Working with web services in R

This article describes how you can interact with and manage analytic web services directly in R using functions in the [mrsdeploy package](../mrsdeploy/mrsdeploy.md). This R package is installed with both Microsoft R Client and Microsoft R Server. Note that a set of [RESTful APIs](api.md) are also available to provide direct programmatic access to a service's lifecycle directly.

Using `mrsdeploy` with a [properly configured R Server](../mrsdeploy/mrsdeploy.md#configure) allows you to publish R functions (models, R scripts, arbitrary R code) exposed as **analytic web services** in as little as a single line of R code. A web service might contain not only the model, but also the prediction script used to create it.

Once hosted in R Server, these web services can be discovered by other authenticated users. These users can [consume the web services in R](data-scientist-get-started.md) or in the [language of their choice via Swagger](app-developer-get-started.md).


## Permissions and function descriptions
In addition to the authentication functions, the following functions are used to bundle R code or R scripts as web services.

>[!IMPORTANT]
>To use the functions in the `mrsdeploy` package, you must log into R Server as an authenticated user.  Learn about the login/logout functions and their arguments in the article "[Connecting to R Server to use mrsdeploy](../operationalize/mrsdeploy-connection.md)".

Any authenticated user can publish an analytic web service to a given R Server instance. They can also retrieve a list of the available services hosted in R Server as well as retrieve service objects from them for consumption them regardless of whether they published those services or not.

In this release, you can only manage (update/delete) the web services you've published. You cannot manage the services published by other users. 

|Web service functions | Description |Your Service|Other Services
|---------|-------------|-----|----|
|`publishService` |Publishes R code as a web service on R Server. |Yes|N/A|
|`updateService` |Updates an existing web service on an R Server instance. |Yes |No|
|`deleteService `|Deletes a web service on an R Server instance. |Yes|No|
|`listServices` |Returns a list of published web services on the R Server instance. |Yes|Yes|
|`getService` |Returns a web service object for consumption. |Yes|Yes|


<!--
## Permissions 

Any authenticated user can publish an analytic web service to a given R Server instance. 

In this release, you can manage (update/delete) the web services you've published; however, you can not manage the services published by other users. 

Any authenticated user can also retrieve a list of the available services hosted in R Server as well as retrieve service objects from them and consume them regardless of whether they published those services or not.

|Published web service actions|Your services|Other services|
|----|:----:|:----:|
|Update a service|Yes |
|Delete a service|Yes |
|List the services|Yes|Yes|
|Retrieve a service object|Yes|Yes|
-->

<a name="version"></a>

## Web Service Versions
 
Every web service is versioned, which allows the user who published a service to roll back to an older version at any time as well as designate specific versions of a web service for production usage.

We highly recommend that everyone in your organization who is publishing web services use a consistent and meaningful versioning convention such as [semantic versioning](http://semver.org/). Versioning improves the release of services for service contributors and owners, and makes it easier for service consumers to identify the services they are calling. 

If you do not provide a version number when publishing, a Globally Unique Identifier (GUID) is automatically used as a version. But that GUID version number is not easy to remember for those consuming your services. Since the GUID is not a meaningful version number, the intention is allow the user publishing the service to treat the service as a <i>development</i> version (seen only by the author) until it is ready to be promoted and shared. From there, a more meaningful version number can be chosen during service publishing to represent a stable release.
<!--
@@ TESTING
testing is done by publishing in your dev sandbox without a version and consuming in R to see that it matches what you developed locally. You can use the functions to update and using package functions and eventually when satisfied you can publish with a version for consumption by others. 
-->
## Publish and manage in R

<a name="publishService"></a>
### Publish web services

In order to deploy your analytics, you must publish them as new web services running on R Server. Each service is uniquely defined by a `name` and `version`.  Additionally, each web service includes the R code and any necessary model assets, the required inputs, and the output application developers will need to integrate in their applications. 

The `mrsdeploy` function for publishing as web services is `publishService`. 

|Function|Response|
|----|----|
|`publishService(...)`|Returns an API instance as an [R6](https://cran.r-project.org/web/packages/R6/index.html)|

From your local commandline, you can publish web services to a local R Server or remotely if you set up a remote session.

>[!IMPORTANT]
>In the case where you are working with a [remote R session](../operationalize/remote-execution.md), keep in mind that you can only publish from the local session. If you attempt to publish remotely, it will fail with this message: `Error in curl::curl_fetch_memory(uri, handle = h) : URL using bad/illegal format or missing URL`. If you are in your remote session, [switch back](../operationalize/remote-execution.md#switch) to the local commandline to publish your service. 

The following arguments are accepted for `publishService`:

|Arguments|Description|
|----|----|
|`name` *|The unique web service name. It is a string so use quotes such as "MyService". We recommend that you use a name that is easy understood and fits a nice URL so people can remember it.|
|`code` *|R code to publish. `code` can take the form of:<br>1. A filepath to an R script. For example:<br> &nbsp;  &nbsp; `code = "/path/to/R/script.R"`<br>2. A block of R code as a character string. For example:<br> &nbsp;  &nbsp; `code = "result <- x + y"`<br>3. A function handle. For example:<br> &nbsp;  &nbsp; `code = function(hp, wt) {`<br> &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `newdata <- data.frame(hp = hp, wt = wt)`<br> &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `predict(model, newdata, type = "response")`<br> &nbsp;&nbsp;  &nbsp;&nbsp; `}`|
|`model`|An `object` or a file-path to an external representation of R objects to be loaded and used with `code`. The specified file can be:<br>1. File-path to an `.RData` file holding R objects to be loaded. For example:<br> &nbsp;  &nbsp; `model = "/path/to/glm-model.RData"`<br>2. File-path to an `.R` file which will be evaluated into an environment and loaded. For example:<br> &nbsp;  &nbsp; `model = "/path/to/glm-model.R"`<br>3. An object. The .R file will be evaluated into an environment and loaded internally. For example:<br> &nbsp;  &nbsp; `model = am.glm`|
|`snapshot`|Identifier of the snapshot to load. Can replace the `model` argument or be merged with it.|
|`inputs`|Defines the web service input schema. If empty, the service will not accept inputs. `inputs` are defined as a named list `list(x = "logical")` which describe the input parameter names and their corresponding [data types](#data-types).|
|`outputs` |Defines the web service output schema. If empty, the service will not return a response value. `outputs` are defined as a named list `list(x = "logical")` which describe the output parameter names and their corresponding  [Data Types](#data-types)<br>Note: If `{code}` is defined as a `{function}` then only one output value can be claimed.|
|`v` |Defines a unique web service version. If the version is left blank, a unique `{guid}` will be generated in its place. Useful during service development before the author is ready to officially publish a semantic version to share.|
|`alias` |An alias name of the predication RPC function used to consume the service. If `code` is a function it will use that function name by default. See [Api](#api-client).|
|`destination` |The codegen output directory location.|
|`descr` |The description of the web service.|
<sup>&#42;</sup> If an argument is marked with an asterisk (&#42;), then the argument is __required__ by the function.


#### Example

See full examples in the ["Workflow" examples](#workflow) at the end of this article.

```R
# Publish web service called mtService and 
# assign version number v1.0.0
api <- publishService(
     "mtService",
     code = manualTransmission,
     model = carsModel,
     inputs = list(hp = "numeric", wt = "numeric"),
     outputs = list(answer = "numeric"),
     v = "v1.0.0"
)
```
<a name="data-types"></a>

#### I/O data types

The following table lists the supported data types for the [publishService](#publishService) and [updateService](#updateService) function input and output schemas:

|I/O data types:|`numeric` |`integer`|`logical`|`character`|`vector`|`matrix`|`data.frame`|
|--------|----------|---------|---------|-----------|--------|--------|------------|
|**Fully Supported?**<br><br><br>|TRUE<br><br><br>|TRUE<br><br><br>|TRUE<br><br><br>|TRUE<br><br><br>|TRUE<br><br><br>|FALSE<br>(logical & character matrix<br>not supported) |TRUE<br>Note:  Coercing an object during I/O<br> is a user-defined task|


### Update web services

If you want to change your web service after you've published it, but keep the same name and version, you can use `updateService`. and specify what needs to change, such as the R code, model, inputs, and so on. You can change one or more of the arguments at the same time. When you update a service, it overwrites that named version.

>[!NOTE]
>If you want to change the name or version number, use the `publishService` function instead and specify the new name or version number. 

The `mrsdeploy` function for updating as web services is `updateService`. 

|Function|Response|
|----|----|
|`updateService(...)`|Returns an API instance as an [R6](https://cran.r-project.org/web/packages/R6/index.html)|

<a name="updateService"></a>

The following arguments are accepted for `updateService`:

|Arguments|Description|
|----|----|
|`name`*|The web service name you want to update. It is a string so use quotes such as "MyService".|
|`v`*|Identifies the version of the web service to be updated.|
|`code` *|R code to publish. `code` can take the form of:<br>1. A filepath to an R script. For example:<br> &nbsp;  &nbsp; `code = "/path/to/R/script.R"`<br>2. A block of R code as a character string. For example:<br> &nbsp;  &nbsp; `code = "result <- x + y"`<br>3. A function handle. For example:<br> &nbsp;  &nbsp; `code = function(hp, wt) {`<br> &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `newdata <- data.frame(hp = hp, wt = wt)`<br> &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `predict(model, newdata, type = "response")`<br> &nbsp;&nbsp;  &nbsp;&nbsp; `}`|
|`model`|An `object` or a file-path to an external representation of R objects to be loaded and used with `code`. The specified file can be:<br>1. File-path to an `.RData` file holding R objects to be loaded. For example:<br> &nbsp;  &nbsp; `model = "/path/to/glm-model.RData"`<br>2. File-path to an `.R` file which will be evaluated into an environment and loaded. For example:<br> &nbsp;  &nbsp; `model = "/path/to/glm-model.R"`<br>3. An object. The .R file will be evaluated into an environment and loaded internally. For example:<br> &nbsp;  &nbsp; `model = am.glm`|
|`snapshot` |Identifier of the snapshot to load. Can replace the `model` argument or be merged with it.|
|`inputs` |Defines the web service input schema. If empty, the service will not accept inputs. `inputs` are defined as a named list `list(x = "logical")` which describe the input parameter names and their corresponding [data types](#data-types).|
|`outputs` |Defines the web service output schema. If empty, the service will not return a response value. `outputs` are defined as a named list `list(x = "logical")` which describe the output parameter names and their corresponding  [Data Types](#data-types)<br>Note: If `{code}` is defined as a `{function}` then only one output value can be claimed.|
|`alias` |An alias name of the predication RPC function used to consume the service. If `code` is a function it will use that function name by default. See [Api](#api-client).|
|`destination` |The codegen output directory location.|
|`descr` |The description of the web service.|
<sup>&#42;</sup> If an argument is marked with an asterisk (&#42;), then the argument is __required__ by the function.

For more information on input and output data types, see the above section [I/O Data Types](#data-types).

#### Example

```R
# For web service called transmission with version number v1.0.0,
# update the model carsModel, code, inputs, and description. 
# Assign it to a variable called api.
api <- updateService(
     "mtService",
     "v1.0.0",
     code = manualTransmission,
     mode = carsModel,
     inputs = list(wt = "numeric", dist = "numeric")
     descr = "Updated after March data refresh."
)
```

### Delete web services

When you no longer want to keep a web service, you can delete it. Only the user who initially created the web service can use this function.


The `mrsdeploy` function for deleting a web services is `deleteService`. 

|Function|Response|
|----|----|
|`deleteService(...)`|If it is successful, it returns a success status and message such as `"Service mtService version v1.0.0 deleted."`. If it fails for any reason, then it stops execution with error message.|


<a name="deleteService"></a>

The following arguments are accepted for `deleteService`:

|Arguments|Description|
|----|----|
|`name`*|The web service name you want to delete. It is a string so use quotes such as "MyService".|
|`v`*|Identifies the version of the web service to be deleted.|
<sup>&#42;</sup> If an argument is marked with an asterisk (&#42;), then the argument is __required__ by the function.

#### Example

Example:
```R
result <- deleteService("mtService", "v1.0.0")
print(result)
```

## Interact with services in R

### List web services

Any authenticated user can retrieve a list of web services using the `listServices` function. You can use arguments to restrict the list to return a specific web service or all versions of a given web service. 

The `mrsdeploy` function for listing available web services is `listServices`. 

|Function|Response|
|----|----|
|`listServices(...)`| R `list` containing service metadata.|


<a name="listServices"></a>
The following arguments are accepted for `listServices`:

|Arguments|Description|
|----|----|
|`name`|When specified, will return only web services with that name. It is a string so use quotes such as "MyService".|
|`v`|When specified, will return only web services with that name and specific version number.|
<sup>&#42;</sup> If an argument is marked with an asterisk (&#42;), then the argument is __required__ by the function.

#### Example

Example:
```R
# Return metadata for all services hosted on this R Server
services <- listServices()

# Return metadata for every version of the 
# service "mtService" hosted on this R Server
services <- listServices("mtService")

# Return metadata for version v1.0.0 of the 
# service "mtService" hosted on this R Server
addition <- listServices("mtService", "v1.0.0")

# View service capabilities/schema. 
# For example, the input schema:
#   list(name = "wt", type = "numeric")
#   list(name = "dist", type = "numeric")
print(mtService)
```

Returns:
```R
$creationTime
[1] "2017-02-13T19:44:26.2611422"

$name
[1] "mtService"

$version
[1] "v1.0.0"

$description
NULL

$snapshotId
[1] "05053e85-c9d0-43cb-9be8-8dccf2b5da54"

$owner
[1] "rserver-user"

$inputParameterDefinitions
$inputParameterDefinitions[[1]]
$inputParameterDefinitions[[1]]$name
[1] "hp"

$inputParameterDefinitions[[1]]$type
[1] "numeric"

$inputParameterDefinitions[[2]]
$inputParameterDefinitions[[2]]$name
[1] "wt"

$inputParameterDefinitions[[2]]$type
[1] "numeric"

$outputParameterDefinitions
$outputParameterDefinitions[[1]]
$outputParameterDefinitions[[1]]$name
[1] "answer"

$outputParameterDefinitions[[1]]$type
[1] "numeric"

$operationId
manualTransmission
```

### Retrieve service objects

Any authenticated user can retrieve a web service object using the `getService` function that makes it possible for the service to be consumed. Once the object is returned, you can look at its capabilities to see what the service can do and how it should be consumed.

```
getService(arguments...)
```

The `mrsdeploy` function for retrieving a service object is `getService`. 

|Function|Response|
|----|----|
|`getService(...)`|An API instance as an [R6](https://cran.r-project.org/web/packages/R6/index.html)|

<a name="getService"></a>

The following arguments are accepted for `getService`:

|Arguments|Description|
|----|----|
|`name`*|The name of the web service whose object you want to retrieve. It is a string so use quotes such as "MyService".|
|`v`*|The version of the web service whose object you want to retrieve.|
<sup>&#42;</sup> If an argument is marked with an asterisk (&#42;), then the argument is __required__ by the function.

#### Example

```R
# Get service using `getService()` function from `mrsdeploy` package.
# Assign service to the variable `api`.
api <- getService("mtService", "v1.0.0")

# Print capabilities to see what service can do.
print(api$capabilities())
     
# Start interacting with the service, for example:
# Calling the function, `manualTransmission`
# contained in this service.
result <- api$manualTransmission(120, 2.8)
```

<!--
### CONSUME @@

Once you get the service object you can consume it. SHOULD THIS BE PART OF GETSERVICE

-->












<a name="workflow"></a>

## Workflows: publish-to-consume 


### Local `model` object and remote session

```R
library(mrsdeploy)

# --- Local R session ----------------------------------------------------------

# Logistic regression vehicle transmission to estimate 
# the probability of a vehicle being fitted with a 
# manual transmission base on horsepower (hp) and weight (wt)

# Create glm model with `mtcars` dataset
carsModel <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)

# Wrap prediction in a function
manualTransmission <- function(hp, wt) {
  newdata <- data.frame(hp = hp, wt = wt)
  predict(carsModel, newdata, type = "response")
}

# test function by printing results
print(manualTransmission(120, 2.8)) # 0.6418125

# --- AAD login and remote session creation--------------------------------------

# Authenticate with Azure AD using `mrsdeploy` pkg function
# session = TRUE so that a remote R session started
remoteLoginAAD(
       "https://rserver.contoso.com:12800", 
       authuri = "https://login.windows.net", 
       tenantid = "microsoft.com", 
       clientid = "5599bff3-2ec2-4975-9068-28acf86a3b6f", 
       resource = "b9667d00-1c06-4b9d-a94f-06ecb71822b0", 
       session = TRUE 
)

# --- Remote R session ---------------------------------------------------------

# Create glm model with `mtcars` dataset from the remote R session on R Server
carsModel <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)

manualTransmission <- function(hp, wt) {
  newdata <- data.frame(hp = hp, wt = wt)
  predict(carsModel, newdata, type = "response")
}

# test function by printing results
print(manualTransmission(120, 2.8)) # 0.6418125

ls()
pause()  # return to the local session

# --- Local R session ----------------------------------------------------------

# load object `model` into GlobalEnv of the local R session
status <- getRemoteObject(c("carsModel"))
ls()

# Use unique service names to avoid collisions between examples
mtService <- paste0("transmission", round(as.numeric(Sys.time()), 0))

# Publish as service using `publishService()` function from 
# `mrsdeploy` package. Name service "mtService" and provide
# unique version number. Assign service to the variable `api`
api <- publishService(
  "mtService",
  code = manualTransmission,
  model = model,
  inputs = list(hp = "numeric", wt = "numeric"),
  outputs = list(answer = "numeric"),
  v = "v1.0.1"
)

api

# Consume service by calling function, `manualTransmission`
# contained in this service
result <- api$manualTransmission(120, 2.8)

# Print response output named `answer`
print(result$output("answer")) # 0.6418125

# During this authenticated session, download the  
# Swagger-based JSON file that defines this service
swagger <- api$swagger()
cat(swagger, file = "swagger.json", append = FALSE)

# Generate list of all versions of a service named mtService
services <- listServices("mtService")
services

transmission <- services[[1]]
transmission

# To get the JSON file at a later time, use getService to 
# return the service object before calling the swagger.
api <- getService(transmission$name, transmission$version)
api

# Start interacting with the service, for example:
# Calling the function, `manualTransmission`
# contained in this service.
result <- api$manualTransmission(120, 2.8)

# Print response output named `answer`
print(result$output("answer")) # 0.6418125

# Print capabilities that define the service holdings: service 
# name, version, descriptions, inputs, outputs, and the 
# name of the function to be consumed
cap <- api$capabilities()
cap


cap$swagger

# Remove the service when you no longer need it
status <- deleteService(cap$name, cap$version)
status

# Log off of R Server
remoteLogout()
```

### Local `.RData` file

```R
library(mrsdeploy)

# --- AAD login ----------------------------------------------------------------

remoteLoginAAD(
  "https://dhost.com",
  authuri = "https://login.windows.net",
  tenantid = "microsoft.com",
  clientid = "5599bff3-2ec2-4975-9068-28acf86a3b6f",
  resource = "b9667d00-1c06-4b9d-a94f-06ecb71822b0",
  session = FALSE
)

model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)
save(model, file = "transmission.RData")

manualTransmission <- function(hp, wt) {
  newdata <- data.frame(hp = hp, wt = wt)
  predict(model, newdata, type = "response")
}

# test locally: 0.6418125
print(manualTransmission(120, 2.8))

# base dir for `code` and `model` args
opts <- serviceOption()
opts$set("data-dir", getwd())

# Use unique service names to avoid collisions between examples
mtService <- paste0("transmission", round(as.numeric(Sys.time()), 0))

api <- publishService(
   "mtService",
   code = manualTransmission,
   model = "transmission.RData",
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(answer = "numeric"),
   v = "v1.0.2"
)

api

result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

swagger <- api$swagger()
cat(swagger)

swagger <- api$swagger(json = FALSE)
swagger

services <- listServices("mtService")
services

transmission <- services[[1]]
transmission

api <- getService(transmission$name, transmission$version)
api
result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

cap <- api$capabilities()
cap
cap$swagger

status <- deleteService(cap$name, cap$version)
status

services <- listServices("mtService")
length(services) # gone

# Error: No service found for `"mtService"` version "v1.0.2"
api <- getService("mtService", "v1.0.2")

remoteLogout()
```


### Local `.R` file

```R
library(mrsdeploy)

# --- AAD login ----------------------------------------------------------------

remoteLoginAAD(
  "https://dhost.com",
  authuri = "https://login.windows.net",
  tenantid = "microsoft.com",
  clientid = "5599bff3-2ec2-4975-9068-28acf86a3b6f",
  resource = "b9667d00-1c06-4b9d-a94f-06ecb71822b0",
  session = FALSE
)

# Information can come from a file
model <- "model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)"
code <- "newdata <- data.frame(hp = hp, wt = wt)\n
         answer <- predict(model, newdata, type = "response")"

cat(model, file = "transmission.R", append = FALSE)
cat(code, file = "transmission-code.R", append = FALSE)

# Use unique service names to avoid collisions between examples
"mtService" <- paste0("transmission", round(as.numeric(Sys.time()), 0))

api <- publishService(
   "mtService",
   code = "transmission-code.R",
   model = "transmission.R",
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(answer = "numeric"),
   v = "v1.0.3",
   alias = "manualTransmission"
)

api

result <- api$manualTransmission(120, 2.8)
result
print(result$output("answer")) # 0.6418125

swagger <- api$swagger()
cat(swagger)

swagger <- api$swagger(json = FALSE)
swagger

services <- listServices("mtService")
services

transmission <- services[[1]]
transmission

api <- getService(transmission$name, transmission$version)
api
result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

cap <- api$capabilities()
cap
cap$swagger

status <- deleteService(cap$name, cap$version)
status

services <- listServices("mtService")
length(services) # gone

# Error: No service found for "transmission" version "v1.0.3"
api <- getService("mtService", "v1.0.3")

remoteLogout()
```

### Local `.RData` and `.R`

```R
library(mrsdeploy)

# --- AAD login ----------------------------------------------------------------

remoteLoginAAD(
  "https://dhost.com",
  authuri = "https://login.windows.net",
  tenantid = "microsoft.com",
  clientid = "5599bff3-2ec2-4975-9068-28acf86a3b6f",
  resource = "b9667d00-1c06-4b9d-a94f-06ecb71822b0",
  session = FALSE
)

# model
model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)
save(model, file = "transmission.RData")

# R code
code <- "newdata <- data.frame(hp = hp, wt = wt)\n
         answer <- predict(model, newdata, type = "response")"
cat(code, file = "transmission-code.R", sep="n", append = TRUE)

# base dir for `code` and `model` args
opts <- serviceOption()
opts$set("data-dir", getwd())

# Use unique service names to avoid collisions between examples
"mtService" <- paste0("transmission", round(as.numeric(Sys.time()), 0))

api <- publishService(
   "mtService",
   code = "transmission-code.R",
   model = "transmission.RData",
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(answer = "numeric"),
   v = "v1.0.4",
   alias = "manualTransmission"
)

api

result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

swagger <- api$swagger()
cat(swagger)

swagger <- api$swagger(json = FALSE)
swagger

services <- listServices("mtService")
services

transmission <- services[[1]]
transmission

api <- getService(transmission$name, transmission$version)
api
result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

cap <- api$capabilities()
cap
cap$swagger

status <- deleteService(cap$name, cap$version)
status

services <- listServices("mtService")
length(services) # gone

# Error: No service found for `"mtService"` version "v1.0.4"
api <- getService("mtService", "v1.0.4")

remoteLogout()
```



