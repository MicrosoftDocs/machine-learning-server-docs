---

# required metadata
title: "mrsdeploy Web Service functions in Microsoft R"
description: "Web service deployment functions in the mrsdeploy package in Microsoft R can be used for any arbitrary R code block. A web service runs on R Server 9.0 instances."
keywords: "mrsdeploy package"
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "11/30/2016"
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

# mrsdeploy Web Service functions in Microsoft R

This article describes Web service functions in the [mrsdeploy package](mrsdeploy.md).

## Using web services in mrsdeploy

Through the **mrsdeploy** package, you can deploy R script or code as a *web service* that you create and manage using functions in the package. After the service is published, it can be consumed by others from R or from your language of choice via [Swagger](http://swagger.io/).

## Service APIs

### Publish

Publishes an R code block as a new web service running on R Server.

>[!IMPORTANT]
>In the case where you are working with a [remote R session](../operationalize/remote-execution.md) and you want to publish a web service, do so in your local session. If you try to publish remotely, you'll get this message: `Error in curl::curl_fetch_memory(uri, handle = h) : URL using bad/illegal format or missing URL`. Instead, use the remote execution function `pause()` to return the R command line in your local session, publish your service, and then use the `resume()` function to continue running R code from the remote command line in the remote R session. [Learn more...](../operationalize/remote-execution.md#switch)

**Arguments**

- `name` - (required) The web service name.
- `code` - (required) R code to publish. `code` can take the form of:
    1. A filepath to an R script `code = "/path/to/R/script.R"`
    2. A block of R code as a character string `code = "result <- x + y"`
    3. A function handle:
    ```R
    code = function(hp, wt) {
      newdata <- data.frame(hp = hp, wt = wt)
      predict(model, newdata, type = "response")
    }
    ```
- `model` - (optional) An `object` or a file-path to an external representation of R objects to be loaded and used with `code`. The specified file can be:
    1. File-path to an `.RData` file holding R objects to be loaded.
    2. File-path to an `.R` file which will be evaluated into an environment and loaded.

- `snapshot` (optional) Identifier of the snapshot to load. Can replace the `model` argument or be merged with it.
- `inputs` - (optional) Defines the web service input schema. If empty, the service will not accept inputs. `inputs` are defined as a named list `list(x = "logical")` which describe the input parameter
   names and their corresponding [Data Types](#io-data-types)
- `outputs` - (optional) Defines the web service output schema. If empty, the service will not return a response value. `outputs` are defined as a named list `list(x = "logical")` which describe the output parameter names and their corresponding  [Data Types](#io-data-types)
    Note: If `{code}` is defined as a `{function}` then only one output value can be claimed.
- `v` - (optional) Defines a unique web service version. If the version is left blank, a unique `{guid}` will be generated in its place. Useful during service development before the author is ready to officially publish a semantic version to share.
- `alias` - (optional) An alias name of the predication RPC function used to consume the service. If `code` is a function it will use that function name by default. See [Api](#api-client).
- `destination` (optional) The codegen output directory location.
- `descr` - (optional) The description of the web service.

**Response**

An [Api](#api-client) instance as an [R6](https://cran.r-project.org/web/packages/R6/index.html)

**Example**

```R
api <- publishService(
  "addition",
   code = "result <- x + y",
   inputs = list(x = "numeric", y = "numeric"),
   outputs = list(result = "numeric"),
   v = "v1.0.0",
   alias = "add"
)
```

See more [examples](#publish-service).

### Update

The `updateService` function updates a published web service.

**Arguments**

- `name` - (required) The web service name.
- `v` - (required) Defines a unique web service version.
- `code` - (optional) R code to publish. `code` can take the form of:
    1. A filepath to an R script `code = "/path/to/R/script.R"`
    2. A block of R code as a character string `code = "result <- x + y"`
    3. A function handle:
    ```R
    code = function(hp, wt) {
      newdata <- data.frame(hp = hp, wt = wt)
      predict(model, newdata, type = "response")
    }
    ```
- `model` - (optional) An `object` or a file-path to an external representation of R objects to be loaded and used with `code`. The specified file can be:
    1. File-path to an `.RData` file holding R objects to be loaded.
    2. File-path to an `.R` file which will be evaluated into an environment and loaded.

- `snapshot` (optional) Identifier of the snapshot to load. Can replace the `model` argument or be merged with it.
- `inputs` - (optional) Defines the web service input schema. If empty, the service will not accept inputs. `inputs` are defined as a named list `list(x = "logical")` which describe the input parameter
   names and they're corresponding [Data Types](#io-data-types)
- `outputs` - (optional) Defines the web service output schema. If empty, the service will not return a response value. `outputs` are defined as a named list `list(x = "logical")` which describe the output parameter names and theire corresponding  [Data Types](#io-data-types)
    Note: If `{code}` is defined as a `{function}` then only one output value can be claimed.
- `alias` - (optional) An alias name of the predication RPC function used to consume the service. If `code` is a function it will use that function name by default. See [Api](#api-client).
- `destination` (optional) The codegen output directory location.
- `descr` - (optional) The description of the web service.

**Response**

An [Api](#api-client) instance as an [R6](https://cran.r-project.org/web/packages/R6/index.html)

**Example**

```R
api <- updateService(
  "addition",
  "v1.0.0",
  descr = "Update the description for the `addition` service."
)
```

See more [examples](#update-service).

### Discover

The `getServices` function gets a published web service for consumption.

**Arguments**

- `name` - (Required) Defines the name of the service
- `v` - (Required) Defines the web service version

**Response**

An [Api](#api-client) instance as an [R6](https://cran.r-project.org/web/packages/R6/index.html)

**Example**

```R
api <- getServices("addition", "v1.0.0")

result <- api$add(10, 20)
```
See more [examples](#get-a-service).

### List

The `listServices` function retrieves published web service meta-data.

**Arguments**

- `name` - (Optional) Defines the name of the service.
- `v` - (Optional) Defines the web service version.

**Response**

 R `list` containing service meta-data.

**Example**

```R
# Return all
services <- listServices()

# Return meta-data for all versions of the `addition` service
services <- listServices("addition")

# Return meta-data regarding the version `v1.0.0` of the `addition` service
addition <- listServices("addition", "v1.0.0")
```

See more [examples](#list-service).

### Delete

The `deleteService` function deletes a published web service. Only the user who initially created the web service can use this function.

**Arguments**

- `name` - (Required) Defines the name of the service
- `v` - (Required) Defines the web service version

**Response**

_Success_:

Success status and message.

_Failure_:

Stops execution with error message.

**Example**

```R
success <- deleteService("addition", "v1.0.0"")
print(success)
```
```R
[1] "Service addition version v1.0.0 deleted."
```

See more [examples](#delete-service).

## API Client

Defines the client instance of a published service. This acts as the `client stub`
to consume the service and view its service holdings.

The services `Api` client instance is returned from: [publishService](#publish), [updateService](#update), [getServices](#discover).

**Supported public functions:**

| Function      | Description                                            |
| ------------- |--------------------------------------------------------|
| `print`       |	Print method that lists all members of the object      |
| `capabilities` | Report on the service features such as I/O schema, `name`, `version`	   |
| `consume`     |	Consume the service based on I/O schema                |
| consume _alias_ | Alias to the `consume` function for convenience see [publish alias](#publish) |
| `swagger`     |	Displays the service's `swagger` specification         |

**Examples:**

```R
api <- getServices("transmission", "v1.0.0")
print(api)

cap <- api$capabilities()
print(cap$name)
print(cap$version)
print(cap$inputs)
print(cap$outputs)
print(cap$swagger)

# consume
result <- api$consume(120, 2.8)

# consume with rpc `alias`. Alias to the `consume` function above
result <- api$manualTransmission(120, 2.8)

swagger <- api$swagger()
cat(swagger)

swagger <- api$swagger(json = FALSE)
cat(swagger)

```

## I/O Data Types

Supported _Data Types_ for the [publish](#publish) and [update](#update) service input and output schemas:

| Type          | Fully Supported                                        |
| ------------- |--------------------------------------------------------|
| `numeric`     |	TRUE                                                   |
| `integer`     |	TRUE                                                   |
| `logical`     |	TRUE                                                   |
| `character`   | TRUE	                                                 |
| `vector`      | TRUE 	                                                 |
| `matrix`      | FALSE	(logical, character matrix not supported)        |
| `data.frame`  |	TRUE ***note**  coercing the object during I/O is a user defined task

## Swagger

```R
model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)
save(model, file = "transmission.RData")

manualTransmission <- function(hp, wt) {
  newdata <- data.frame(hp = hp, wt = wt)
  predict(model, newdata, type = "response")
}

api <- publishService(
  "transmission",
   code = manualTransmission,
   model = "transmission.RData",
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(answer = "numeric"),
   v = "v1.0.0"
)

print(api)
```
### Capabilities

View the service capabilities, notice the `swagger(json = TRUE)` function:

```R
<TransmissionApi>
name: transmission
description:
version: v1.0.0
date: 2016-10-26T16:18:26.9563885
snapshotId: 3fd27552-c1c9-4fa3-8185-39061c1bf6cd
inputs:
  hp: numeric
  wt: numeric
outputs:
  answer: numeric
operationId: manualTransmission
swagger: http://localhost:12800/api/transmission/v1.0.0/swagger.json
public-functions:
  consume: consume(hp, wt)
  manualTransmission: manualTransmission(hp, wt)
  capabilities: capabilities()
  print: print()
  swagger: swagger(json = TRUE)
```

Inspect the actual `http://localhost:12800/api/transmission/v1.0.0/swagger.json`:

```R
swagger <- api$swagger()
cat(swagger)

# `json = FALSE` returns a list() defined by `jsonlite::toJSON()`
swagger <- api$swagger(json = FALSE)
```

## Service Examples

First authenticate using _Azure Active Directory_ or _On Premises Active Directory_


```R
remoteLoginAAD(
  "https://dhost.com",
  authuri = "https://login.windows.net",
  tenantid = "microsoft.com",
  clientid = "5599bff3-2ec2-4975-9068-28acf86a3b6f",
  resource = "b9667d00-1c06-4b9d-a94f-06ecb71822b0",
  session = FALSE
)
```

### Publish Service

**Build and save a model (optional)**

By use of the logistic regression equation of vehicle transmission in the data set mtcars, estimate the probability of a vehicle being fitted with a manual transmission.

```R
model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)
```

**Produce a `prediction` function that can use the `model`**
```R
manualTransmission <- function(hp, wt) {
  newdata <- data.frame(hp = hp, wt = wt)
  predict(model, newdata, type = "response")
}
```

**Test locally**
```R
print(manualTransmission(120, 2.8))
```

**Publish new service**

```R
#
# Publish a new version `v1.0.0` service named `transmission`
# POST /api/transmission/v1.0.0
#
api <- publishService(
  "transmission",
   code = manualTransmission,
   model = model,
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(answer = "numeric"),
   v = "v1.0.0"
)
```

**Consume remotely**

```R
result <- api$manualTransmission(120, 2.8)
print(result$output("answer"))
```
```R

## 0.6418125
```

### Update Service

**Build and save an updated new model**

Change the model. We now want to create a model that helps us to predict the probability of a vehicle having a V engine or a straight engine given a weight
and engine displacement in cubic inches.

```R
model <- glm(formula= vs ~ wt + disp, data = mtcars, family = binomial)
```

**Update the `prediction` function to reflect the model**
```R
manualTransmission <- function(wt, disp) {
  newdata <- data.frame(wt = wt, disp = disp)
  predict(model, newdata, type = "response")
}
```

**Test locally**
```R
print(manualTransmission(2.1, 180))
```

**Update existing Service**

```R
#
# Update the version `v1.0.0` service named `transmission` with new `model`
# POST /api/transmission/v1.0.0
#
api <- updateService(
  "transmission",
  "v1.0.0",
  code = manualTransmission,
  mode = model,
  inputs = list(wt = "numeric", dist = "numeric")
)

```

**Consume remotely**
```R
result <- api$manualTransmission(2.1, 180)
print(result$output("answer"))
```
```R
## 0.2361081
```

### List Service

**List all services currently published (meta-data)**
```R
services <- listServices()
```

**List all `versions` of the `transmission` service (meta-data)**
```R
services <- listServices("transmission")
```

**List `transmission` service for version `v1.0.0` (meta-data)**
```R
transmission <- listServices("transmission", "v1.0.0")

# View service capabilities/schema. For example, the input schema:
#   list(name = "wt", type = "numeric")
#   list(name = "dist", type = "numeric")
print(transmission)
```
```R
$creationTime
[1] "2016-10-13T19:44:26.2611422"

$name
[1] "transmission"

$version
[1] "v1.0.0"

$description
NULL

$snapshotId
[1] "05053e85-c9d0-43cb-9be8-8dccf2b5da54"

$owner
[1] "deployruser"

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

### Get a service

**Get a published service by `name` and `version`**
```R
api <- discoverService("transmission", "v1.0.0")
```

**View service capabilities/schema and holdings**
```R
print(api$capabilities())
```

```R
$creationTime
[1] "2016-10-13T19:44:26.2611422"

$name
[1] "transmission"

$version
[1] "v1.0.0"

$description
NULL

$snapshotId
[1] "05053e85-c9d0-43cb-9be8-8dccf2b5da54"

$owner
[1] "deployruser"

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

**Consume remotely**
```R
result <- api$manualTransmission(2.1, 180)
print(result$output("answer"))
```
```R
 ## 0.2361081
```

### Delete service

**Delete Service by `name` and `version`**
```R
result <- deleteService("transmission", "v1.0.0")
print(result)
```

```R
## [1] "Service transmission version v1.0.0 deleted."
```

## Workflow Examples

There are different approaches to _publishing_ and _updating_  a service depending on
your workflow needs as it relates to the `code` and `model` arguments.
All approaches are equivalent.

#### The `code` argument

The `code` argument is **required** and can be populated three different ways:

1. It can be populated with a file-path to an R script `code = "/path/to/script.R"`

```R
publishService(
   "add-service",
   code = "/path/to/addition-code.R",
   inputs = list(x = "numeric", y = "numeric"),
   outputs = list(result = "numeric")
)
```

Where the file `addition-code.R` contains the following R:

```R
result <- x + y
```

**Note** there is no function rather just the supporting source.

2. It can be populated with a block of R code as a character string `code = "result <- x + y"`

```R
publishService(
   "add-service",
   code = "result <- x + y",
   inputs = list(x = "numeric", y = "numeric"),
   outputs = list(result = "numeric")
)
```

3. It can be populated with a function handle:

```R
addition <- function(x, y) {
  x + y
}

publishService(
   "add-service",
   code = addition,
   inputs = list(x = "numeric", y = "numeric"),
   outputs = list(result = "numeric")
)
```

#### The `model` argument

Similar to the `code` argument, the `model` argument also supports different
_shapes_ depending on your workflow needs.

The `model` argument is **optional** and can be populated three ways:

1. It can be populated with an `object`

```R
am.glm <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)

publishService(
   "transmission",
   code = "an <- predict(am.glm, data.frame(hp=hp, wt=wt), type = 'response')",
   model = am.glm,
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(an = "numeric")
)
```

2. It can be populated with a file-path to an `.RData` file `model = "/path/to/model.RData"`

```R

publishService(
   "transmission",
   code = "an <- predict(am.glm, data.frame(hp=hp, wt=wt), type = 'response')",
   model = "/path/to/glm-model.RData",
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(an = "numeric")
)
```

Where the file `glm-model.RData` could have been created prior to publishing or updating:

```R
am.glm <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)
save(am.gml, file = "glm-model.RData")
```

3. It can be populated with a file-path to an R script `model = "/path/to/model.R"`

```R
publishService(
   "transmission",
   code = "an <- predict(am.glm, data.frame(hp=hp, wt=wt), type = 'response')",
   model = "/path/to/glm-model.R",
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(an = "numeric")
)
```

Where the file `glm-model.R` contains the following R:

```R
am.glm <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)
```

**Note** The `.R` file  will be evaluated into an environment and loaded
internally.

### Local `model` object

Publish a service based on local `model` object

```R
library(mrsdeploy)

# --- Local R session ----------------------------------------------------------

# logistic regression vehicle transmission to
# estimate the probability of a vehicle being
# fitted with a manual transmission base on
# horsepower (hp) and weight (wt)
model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)

manualTransmission <- function(hp, wt) {
  newdata <- data.frame(hp = hp, wt = wt)
  predict(model, newdata, type = "response")
}

print(manualTransmission(120, 2.8)) # 0.6418125

# --- AAD login ----------------------------------------------------------------

remoteLoginAAD(
  "https://dhost.com",
  authuri = "https://login.windows.net",
  tenantid = "microsoft.com",
  clientid = "5599bff3-2ec2-4975-9068-28acf86a3b6f",
  resource = "b9667d00-1c06-4b9d-a94f-06ecb71822b0"
)

# --- Remote R session ---------------------------------------------------------

model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)

manualTransmission <- function(hp, wt) {
  newdata <- data.frame(hp = hp, wt = wt)
  predict(model, newdata, type = "response")
}

print(manualTransmission(120, 2.8)) # 0.6418125

ls()
pause()

# --- Local R session ----------------------------------------------------------

ls()
rm(model) # clean local for demo
ls()

# load object `model` into GlobalEnv of the local R session
status <- getRemoteObject(c("model"))
ls()

# pseudo `unique` service name so no collision in example
serviceName <- paste0("transmission", round(as.numeric(Sys.time()), 0))

api <- publishService(
  serviceName,
  code = manualTransmission,
  model = model,
  inputs = list(hp = "numeric", wt = "numeric"),
  outputs = list(answer = "numeric"),
  v = "v1.0.0"
)

api

result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

swagger <- api$swagger()
cat(swagger)

services <- listServices(serviceName)
services

transmission <- services[[1]]
transmission

api <- getServices(transmission$name, transmission$version)
api
result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

cap <- api$capabilities()
cap
cap$swagger

status <- deleteService(cap$name, cap$version)
status

remoteLogout()
```

### Local `.RData` file on disk

Publish service using local `.RData` file on disk

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

# pseudo `unique` service name so no collision in example
serviceName <- paste0("transmission", round(as.numeric(Sys.time()), 0))

api <- publishService(
   serviceName,
   code = manualTransmission,
   model = "transmission.RData",
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(answer = "numeric"),
   v = "v1.0.0"
)

api

result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

swagger <- api$swagger()
cat(swagger)

swagger <- api$swagger(json = FALSE)
swagger

services <- listServices(serviceName)
services

transmission <- services[[1]]
transmission

api <- getServices(transmission$name, transmission$version)
api
result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

cap <- api$capabilities()
cap
cap$swagger

status <- deleteService(cap$name, cap$version)
status

services <- listServices(serviceName)
length(services) # gone

# Error: No service found for `serviceName` version "v1.0.0"
api <- getServices(serviceName, "v1.0.0")

remoteLogout()
```

#### local `.RData` and `.R` files from disk

Publish service by local `.RData` and `.R` files from disk

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

# pseudo `unique` service name so no collision in example
serviceName <- paste0("transmission", round(as.numeric(Sys.time()), 0))

api <- publishService(
   serviceName,
   code = "transmission-code.R",
   model = "transmission.RData",
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(answer = "numeric"),
   v = "v1.0.0",
   alias = "manualTransmission"
)

api

result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

swagger <- api$swagger()
cat(swagger)

swagger <- api$swagger(json = FALSE)
swagger

services <- listServices(serviceName)
services

transmission <- services[[1]]
transmission

api <- getServices(transmission$name, transmission$version)
api
result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

cap <- api$capabilities()
cap
cap$swagger

status <- deleteService(cap$name, cap$version)
status

services <- listServices(serviceName)
length(services) # gone

# Error: No service found for `serviceName` version "v1.0.0"
api <- getServices(serviceName, "v1.0.0")

remoteLogout()
```

### Local `.R` files on disk

Publish service by local `.R` files on disk

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

# Demo this information can come from a file
model <- "model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)"
code <- "newdata <- data.frame(hp = hp, wt = wt)\n
         answer <- predict(model, newdata, type = "response")"

cat(model, file = "transmission.R", append = FALSE)
cat(code, file = "transmission-code.R", append = FALSE)

# pseudo `unique` service name so no collision in example
serviceName <- paste0("transmission", round(as.numeric(Sys.time()), 0))

api <- publishService(
   serviceName,
   code = "transmission-code.R",
   model = "transmission.R",
   inputs = list(hp = "numeric", wt = "numeric"),
   outputs = list(answer = "numeric"),
   v = "v1.0.0",
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

services <- listServices(serviceName)
services

transmission <- services[[1]]
transmission

api <- getServices(transmission$name, transmission$version)
api
result <- api$manualTransmission(120, 2.8)
print(result$output("answer")) # 0.6418125

cap <- api$capabilities()
cap
cap$swagger

status <- deleteService(cap$name, cap$version)
status

services <- listServices(serviceName)
length(services) # gone

# Error: No service found for "transmission" version "v1.0.0"
api <- getServices(serviceName, "v1.0.0")

remoteLogout()
```

## See also

[mrsdeploy Function Reference](mrsdeploy.md)

[Remote Execution](../operationalize/remote-execution.md)

[Package Reference](../package-reference.md)