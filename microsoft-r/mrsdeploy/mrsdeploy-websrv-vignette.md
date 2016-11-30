---

# required metadata
title: "mrsdeploy Web Service functions in Microsoft R (vignette)"
description: "Web service deployment functions in the mrsdeploy package in Microsoft R can be used for any arbitrary R code block. A web service runs on R Server 9.0 instances."
keywords: "mrsdeploy package vignette"
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

# mrsdeploy Web Service functions in Microsoft R (vignette)

This article provides the vignette documentation for Web service functions in the [mrsdeploy package](mrsdeploy.md).

## Using web services in mrsdeploy

Through the **mrsdeploy** package, you can deploy R script or code as a *web service* that you create and manage using functions in the package. After the service is published, you can use it as a help object in code or script, or as an all-inclusive solution with data and visuals.

The complete list of functions include the following:

- [Publish](#publish) - Publish a new service to a local or remote R Server instance.
- [Update](#update) - Update an existing service
- [Discover](#discover) - Get an existing service by `name` and `version`
- [List](#list) - List service meta-data
- [Delete](#delete) - Delete a published service

### Publish

The `publish_service` function publishes a new web service.

**Arguments**

- `name` - (Required) Defines the name of the service.
- `code` - (Required) Defines the R code that will be ran. The provided `code` value can either be:
    1. A filepath to an R script `code = "/path/to/R/script.R"`
    2. A block of R code as a character string `code = "result <- x + y"`
    3. A function handle:
    ```R
    code = function(hp, wt) {
      newdata <- data.frame(hp = hp, wt = wt)
      predict(model, newdata, type = 'response')
    }
    ```
- `model` - (Optional) A filepath to a binary object `.RData` file or a filepath to
    an R Script
- `inputs` - (Optional) A `List` which defines the web service input schema
- `outputs` - (Optional) A `List` which defines the web service output schema
- `v` - (Optional) Defines a unique web service version
- `alias` - (Optional) The predication RPC function used to consume the service
- `descr` - (Optional) The description of the web service.

**Response**

An [Api](api.html) instance.

[R6](https://cran.r-project.org/web/packages/R6/index.html)

**Example**

```R
api <- publish_service(
  'addition',
   code = 'result <- x + y',
   inputs = list(x = 'numeric', y = 'numeric'),
   outputs = list(result = 'numeric'),
   v = 'v1.0.0',
   alias = 'add'
)

result <- api$add(10, 10)
```
See more [examples](#service-examples).

### Update

The `update_service` function updates a published web service.

**Arguments**

- `name` - (Required) Defines the name of the service
- `v` - (Required) Defines the web service version
- `code` - (Optional) Defines the R code that will be ran. The provided `code`
    value can either be:
    1. A filepath to an R script `code = "/path/to/R/script.R"`
    2. A block of R code as a character string `code = "result <- x + y"`
    3. A function handle:
    ```R
    code = function(hp, wt) {
      newdata <- data.frame(hp = hp, wt = wt)
      predict(model, newdata, type = 'response')
    }
    ```
- `model` - (Optional) A filepath to a binary object `.RData` file or a filepath to
    an R Script
- `inputs` - (Optional) A `List` which defines the web service input schema by `name` and `type`
- `outputs` - (Optional) A `List` which defines the web service output schema by `name` and `type`
- `alias` - (Optional) The predication RPC function used to consume the service
- `descr` - (Optional) The description of the web service.

**Response**

An [Api](api.html) instance.

**Example**

```R
api <- update_service(
  'addition',
  'v1.0.0',
  descr = 'Update the description for the `addition` service.'
)
```

See more [examples](#service-examples).

### Discover

The `discover_service` function gets a published web service for consumption.

**Arguments**

- `name` - (Required) Defines the name of the service
- `v` - (Required) Defines the web service version

**Response**

An [Api](api.html) instance.

**Example**

```R
api <- discover_service('addition', 'v1.0.0')

result <- api$add(10, 20)
```

See more [examples](#service-examples).

### List

The `list_service` function retrieves published web service meta-data.

**Arguments**

- `name` - (Optional) Defines the name of the service
- `v` - (Optional) Defines the web service version

**Response**

 R `list` containing service meta-data.

**Example**

```R
# Return all
services <- list_service()

# Return meta-data for all versions of the `addition` service
services <- list_service('addition')

# Return meta-data regarding the version `v1.0.0` of the `addition` service
addition <- list_service('addition', 'v1.0.0')
```

See more [examples](#service-examples).

### Delete

The `delete_service` function deletes a published web service. Only the user who initially created the web service can use this function.

**Arguments**

- `name` - (Required) Defines the name of the service
- `v` - (Required) Defines the web service version

**Response**

_Success_:

Success status and message.

_Failure_:

stops execution with error message.

**Example**

```R
success <- delete_service('addition', 'v1.0.0')
print(success)
```
```R
[1] "Service addition version v1.0.0 deleted."
```

See more [examples](#service-examples).



## Service examples

**Authenticate using on prem Active Directory**

```R
host <- 'http://localhost:12800'
remote_login(host, username = '{{USERNAME}}', password = '{{PASSWORD}}', session = FALSE)
```

### Publish Service

**Build and save a model (optional)**

By use of the logistic regression equation of vehicle transmission in the data set mtcars, estimate the probability of a vehicle being fitted with a manual transmission.

```R
model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)
save(model, file = 'transmission.RData')
```

**Produce a `prediction` function that can use the `model`**
```R
manual_transmission <- function(hp, wt) {
  newdata <- data.frame(hp = hp, wt = wt)
  predict(model, newdata, type = 'response')
}
```

**Test locally**
```R
print(manual_transmission(120, 2.8))
```

**Publish new service**

```R
#
# Publish a new version `v1.0.0` service named `transmission`
# POST /api/transmission/v1.0.0
#
api <- publish_service(
  'transmission',
   code = manual_transmission,
   model = 'transmission.RData',
   inputs = list(hp = 'numeric', wt = 'numeric'),
   outputs = list(answer = 'numeric'),
   v = 'v1.0.0'
)
```

**Consume remotely**

```R
result <- api$manual_transmission(120, 2.8)
print(result$output('answer'))
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
save(model, file = 'transmission.RData')
```

**Update the `prediction` function to reflect the model**
```R
manual_transmission <- function(wt, disp) {
  newdata <- data.frame(wt = wt, disp = disp)
  predict(model, newdata, type = 'response')
}
```

**Test locally**
```R
print(manual_transmission(2.1, 180))
```

**Update existing Service**

```R
#
# Update the version `v1.0.0` service named `transmission` with new `model`
# POST /api/transmission/v1.0.0
#
api <- update_service(
  'transmission',
  'v1.0.0'
  code = manual_transmission,
  mode = 'transmission.RData',
  inputs = list(wt = 'numeric', dist = 'numeric')
)

```

**Consume remotely**
```R
result <- api$manual_transmission(2.1, 180)
print(result$output('answer'))
```
```R
## 0.2361081
```

### List supported service (meta-data)

**List all services currently published (meta-data)**
```R
services <- list_service()
```

**List all `versions` of the `transmission` service (meta-data)**
```R
services <- list_service('transmission')
```

**List `transmission` service for version `v1.0.0` (meta-data)**
```R
transmission <- list_service('transmission', 'v1.0.0')

# View service capabilities/schema. For example, the input schema:
#   list(name = 'wt', type = 'numeric')
#   list(name = 'dist', type = 'numeric')
print(transmission$inputParameterDefinitions)
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

$code
[1] "answer <- (function (hp, wt) \n{\n    newdata <- data.frame(hp = hp, wt = wt)\n    predict(model, newdata, type = \"response\")\n})(hp, wt)"

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
manual_transmission
```

### Get a service

**Get a published service by `name` and `version`**
```R
api <- discover_service('transmission', 'v1.0.0')
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

$code
[1] "answer <- (function (hp, wt) \n{\n    newdata <- data.frame(hp = hp, wt = wt)\n    predict(model, newdata, type = \"response\")\n})(hp, wt)"

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
manual_transmission
```

**Consume remotely**
```R
result <- api$manual_transmission(2.1, 180)
print(result$output('answer'))
```
```R
 ## 0.2361081
```

### Delete service

**Delete Service by `name` and `version`**
```R
result <- delete_service('transmission', 'v1.0.0')
print(result$success)
```

```R
## TRUE
```

## Services with packages

Packages need to be installed by an administrator.

**Build and save a model**
```R
require("lme4")
set.seed(1)

train <- sleepstudy[sample(nrow(sleepstudy), 120),]
model <- lm(Reaction ~ Days + Subject, data = train)
save(model, file = 'sleepy.RData')
```

**Produce a `prediction` function that can use the `model`**
```R
sleepy_predict <- function(newdata) {
   if(!require("lme4")) { require("lme4") }

   predict(model, newdata = newdata)
}
```

**Publish new service**

```R
#
# Publish a new version `v1.0.0` service named `sleepy`
# POST /api/sleepy/v1.0.0
#
api <- publish_service(
  'sleepy',
   code = sleepy_predict,
   model = 'sleepy.RData',
   inputs = list(newdata = 'dataframe'),
   v = 'v1.0.0'
)
```

**Consume remotely**

```R

require("lme4")

result <- api$sleepy_predict(sleepstudy)
result$output('answer'))
```

## Services with rx Functions

RevoScaleR analysis functions can be used within an mrsdeploy web service like any other package.


**Dataset**


The adult data set is a widely used machine learning data set.The data set is
available from the machine learning data repository at [UC Irvine](http://archive.ics.uci.edu/ml/datasets/Adult)

**Supply a fitted model object and a set of new data**

Here we will keep the model separate in a individual R file named
`adult.R`

```R
# http://archive.ics.uci.edu/ml/datasets/Adult
adultDataFile <- file.path("C:/MRS/Data", "adult.data.txt")

adultTrain <- rxImport(adultDataFile, stringsAsFactors = TRUE)
names(adultTrain) <-c("age", "workclass", "fnlwgt", "education",
                      "education_num", "marital_status", "occupation",
                      "relationship", "race", "sex", "capital_gain",
                      "capital_loss", "hours_per_week","native_country",
                      "income")

adultTree <- rxDTree(income ~ age + sex + hours_per_week, pweights = "fnlwgt",
                     data = adultTrain)
```

**Produce a `prediction` function that can use the `model`**
```R
adult_pred <- function(newdata) {
   rxPredict(adultTree, data = newdata, type="vector")
}
```

**Test locally**
```R
source(adult.R)

newdata <-read.table("C:/data/adult.test", skip=1, sep=",", stringsAsFactors=TRUE)

names(newdata) <-c("age", "workclass", "fnlwgt", "education", "education_num",
                   "marital_status", "occupation", "relationship","race", "sex",
                   "capital_gain", "capital_loss",  "hours_per_week","native_country",
                   "income")

# The result shows that the fitted model accurately classifies about 77%
# of the test data.
print(adult_pred(newdata))
```

```R
## 0.7734169
```

**Publish new service**

```R
#
# Publish a new version `v1.0.0` service named `adult`
# POST /api/adult/v1.0.0
#
api <- publish_service(
  'adult',
   code = adult_pred,
   model = 'adult.R', # similarly you can use an .RData
   inputs = list(newdata = 'dataframe'),
   v = 'v1.0.0'
)
```

**Consume remotely**

```R
newdata <-read.table("C:/data/adult.test", skip=1, sep=",", stringsAsFactors=TRUE)
names(newdata) <-c("age", "workclass", "fnlwgt", "education", "education_num",
                   "marital_status", "occupation", "relationship","race", "sex",
                   "capital_gain", "capital_loss",  "hours_per_week","native_country",
                   "income")

result <- api$adult_pred(newdata)
print(result$output('answer'))
```

```R
## 0.7734169
```

## See also

[mrsdeploy Function Reference](mrsdeploy.md)

[Vignette introduction](mrsdeploy-intro-vignette.md)

[Package Reference](../package-reference.md)
