---

# required metadata
title: "Working with web services in R"
description: "Web service deployment functions in the mrsdeploy package in Microsoft R can be used for any arbitrary R code block. A web service runs on R Server 9.0 instances."
keywords: "mrsdeploy package"
author: "j-martens"
manager: "jhubbard"
ms.date: "4/19/2017"
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

# How to interact with and consume web services in R

**Applies to:  Microsoft R Server 9.x**

After a web service has been published or updated, any authenticated user can list, examine, and consume that web service. You can do so directly in R using the functions in the [mrsdeploy R package](../mrsdeploy/mrsdeploy.md). This R package is installed with both Microsoft R Server and Microsoft R Client.  Also note that application developers can also consume a web service in the [language of their choice via Swagger](app-developer-get-started.md).

If you do not want to list, examine, or consume the web service in R, a set of [RESTful APIs](api.md) are also available to provide direct programmatic access to a service's lifecycle directly.

<a name="auth"></a>

## Requirements

Before you can use the web service management functions in the `mrsdeploy` R package, you must:
+ Have access to an R Server instance that was  [properly configured](../mrsdeploy/mrsdeploy.md#configure) to host web services. 

+ Authenticate with R Server using the `remoteLogin` or `remoteLoginAAD` functions in the `mrsdeploy` package as described in the article "[Connecting to R Server to use mrsdeploy](../operationalize/mrsdeploy-connection.md)".


<a name="listServices"></a>

## Find and list web services

Any authenticated user can retrieve a list of web services using the `listServices` function in the `mrsdeploy` package.  

Use function arguments to return a specific web service or all labeled versions of a given web service. See the [package reference help page for listServices()](../mrsdeploy/packagehelp/listServices.md) for the full description of all arguments.

Your ability to see the code inside the web service depends on your permissions. Did you publish the web service or do you have the "Owner" role?
+ If yes, then the code in the service is returned along with other metadata.
+ If no, then the code in the service is never returned in the metadata.

To learn more about the roles in your organization, contact your Microsoft R Server administrator.

|Function|Response|R Function Help|
|----|----|:----:|
|`listServices(...)`| R `list` containing service metadata.|[View](../mrsdeploy/packagehelp/listServices.md)|

Once you find the service you want, use [the `getService` function](#getService) to retrieve the service object for consumption.

Example code:

```R
# Return metadata for all services hosted on this R Server
serviceAll <- listServices()

# Return metadata for all versions of service "mtService" 
mtServiceAll <- listServices("mtService")

# Return metadata for version "v1" of service "mtService" 
mtService <- listServices("mtService", "v1")

# View service capabilities/schema for mtService v1. 
# For example, the input schema:
#   list(name = "wt", type = "numeric")
#   list(name = "dist", type = "numeric")
print(mtService)
```

Example output:

|R Server 9.1+|R Server 9.0|
|--------|--------|
|![9.1 output](../media/o16n/returns91.png)|![9.0 output](../media/o16n/returns90.png)|

<a name="getService"></a>

## Retrieve and examine service objects

Any authenticated user can retrieve a web service object for consumption. After the object is returned, you can look at its capabilities to see what the service can do and how it should be consumed.

After you've authenticated, use the `getService` function in the `mrsdeploy` package to retrieve a service object. See the [package reference help page for getService()](../mrsdeploy/packagehelp/getService.md) for the full description of all arguments. 


|Function|Response|R Function Help|
|----|----|:----:|
|`getService(...)`|Returns an [API instance](#api-client) (`client stub` for consuming that service and viewing its service holdings) as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class.|[View](../mrsdeploy/packagehelp/getService.md)|


Example:

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

<a name="api-client"></a>

## Interact with API clients

When you publish, update, or get a web service, an API instance is returned as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class. This instance is a `client stub` you can use to consume that service and view its service holdings. 

You can use the following supported public functions to interact with the API client instance.

| Function      | Description                                            |
| ------------- |--------------------------------------------------------|
| `print`       |	Print method that lists all members of the object      |
| `capabilities` | Report on the service features such as I/O schema, `name`, `version`	   |
| `consume`     |	Consume the service based on I/O schema                |
| consume _alias_ | Alias to the `consume` function for convenience (see `alias` argument for the [`publishService` function](../mrsdeploy/packagehelp/publishService.md)). |
| `swagger`     |	Displays the service's `swagger` specification         |
| `batch` |Define the data records to be batched. There are additional publish functions used to [consume a service asynchronously via batch execution](data-scientist-batch-mode.md#public-fx-batch).|


Example:

```R
# Get service using `getService()` function from `mrsdeploy`.
# Assign service to the variable `api`
api <- getService("mtService", "v1.0.0")

# Print capabilities to see what service can do.
print(api)

# Print the service name, version, inputs, outputs, and the
# Swagger-based JSON file used to consume the service 
cap <- api$capabilities()
print(cap$name)
print(cap$version)
print(cap$inputs)
print(cap$outputs)
print(cap$swagger)

# Start interacting with the service by calling it with the
# generic name `consume` based on I/O schema
result <- api$consume(120, 2.8)

# Or, start interacting with the service using the alias argument
# that was defined at publication time.
result <- api$manualTransmission(120, 2.8)

# Since you're authenticated, get this service's `swagger.json`.
swagger <- api$swagger(json = FALSE)
cat(swagger)
```

<a name="consume-service"></a>

## Consume web services 

Web services are published to facilitate the consumption and integration of the operationalized models and code.  Whenever the web service is published or updated, a Swagger-based JSON file is generated automatically that define the service.

When you publish a service, you can let people know that it is ready for consumption. Users can get the Swagger file they need to consume the service directly in R or via the API. To make it easy for others to find your service, provide them with the service name and version number (or they can use [the `listServices` function](#listServices)).

Users can consume the service directly using a single consumption call. This approach is referred to as a "Request Response" approach and is described below. Another approach is the [asynchronous "Batch" consumption approach](data-scientist-batch-mode.md), where users send as a single request to R Server, which then makes multiple asynchronous API calls on your behalf.
  
<a name="data-scientists-share"></a>

#### Collaborate with data scientists

Other data scientists may want to explore, test, and consume Web services directly in R using the functions in the `mrsdeploy` package. Quality engineers might want to bring the models in these web services into validation and monitoring cycles.

As the owner of the service, you can share the name and version number for the service with fellow data scientists so they can call the service in R using the functions in the `mrsdeploy` package.  After authenticating, data scientists can use the `getService` function in R to call the service. Then, they can get details about the service and start consuming it.

>[!NOTE]
> It is also possible to perform batch consumption as [described here](data-scientist-batch-mode.md).


In this example, replace the `remoteLogin()` function below with the correct login details for your configuration. Connecting to R Server using the `mrsdeploy` package is covered [in this article](mrsdeploy-connection.md).

```R
##########################################################################
#      Perform Request-Response Consumption & Get Swagger Back in R      #
##########################################################################

# Use `remoteLogin` to authenticate with R Server using the local admin 
# account. Use session = false so no remote R session started
remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)
   
# Get service using `getService()` function from `mrsdeploy`
# Assign service to the variable `api`.
api <- getService("mtService", "v1.0.0")

# Print capabilities to see what service can do.
print(api$capabilities())

# Start interacting with the service, for example:
# Calling the function, `manualTransmission` contained in this service.
result <- api$manualTransmission(120, 2.8)

# Print response output named `answer`
print(result$output("answer")) # 0.6418125  

# Since you're authenticated now, get `swagger.json`.
swagger <- api$swagger()
cat(swagger, file = "swagger.json", append = FALSE)
```

<a name="swagger-app-dev"></a>

#### Collaborate with application developers

Application developers can call and integrate a web service into their applications using the service-specific Swagger-based JSON file and by providing any required inputs to that service. 

Using the Swagger-based JSON file, application developers can generate client libraries for integration. Read "[Application Developer Get Started Guide](app-developer-get-started.md)" for more details.  
   
Application developers can get the Swagger-based JSON file in one of these ways:

+ A data scientist, probably the one who published the service, can send you the Swagger-based JSON file. This is often the faster approach. They can get the file in R with the following code and send it to the application developer:
   ```R
   api <- getService("<name>", "<version>")
   swagger <- api$swagger()
   cat(swagger, file = "swagger.json", append = FALSE) 
   ```

+ Or, the application developer can request the file  **as an authenticated user with an [active bearer token](app-developer-get-started.md#authentication) in the request header** (since all API calls must be authenticated). The URL is formed as follows:
  ```
  GET /api/<service-name>/<service-version>/swagger.json
  ```

<a name="workflow"></a>

## Workflow examples 

The following workflow examples demonstrate how to publish a web service, interact with it, and then consume it. 

### Before you begin

>[!IMPORTANT]
>Be sure to replace the `remoteLogin()` function in each of the examples below with the correct login details for your configuration. Connecting to R Server using the `mrsdeploy` package is covered [in this article](mrsdeploy-connection.md).

The base path for files is set to your working directory, but you can change that as follows:

+ To specify a different base path for `code` and `model` arguments, use:  
  ```R
  opts <- serviceOption()
  opts$set("data-dir", "/base/path/to/some-other/location"))
  ```

+ To clear the path and specify full paths, use:
  ```R
  opts <- serviceOption() s
  opts$set("data-dir", NULL))
  ```

### Standard web service examples

Each standard web service example uses the same code and models and returns the same results. However, in each example, that code and model are represented in different ways such as R scripts, objects, files, and so on.  

To learn more about standard web services, [see here](#standard).

#### 1. R code and model are objects

In this example, the code comes from an object (`code = manualTransmission`) and the model comes from a model object (`model = carsModel`).


```R
##########################################################
#       Create & Test a Logistic Regression Model        #
##########################################################

# For R Server 9.0, load mrsdeploy package on R Server     
library(mrsdeploy)

# Use logistic regression equation of vehicle transmission 
# in the data set mtcars to estimate the probability of 
# a vehicle being fitted with a manual transmission 
# based on horsepower (hp) and weight (wt)


# Create glm model with `mtcars` dataset
carsModel <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)

# Produce a prediction function that can use the model
manualTransmission <- function(hp, wt) {
     newdata <- data.frame(hp = hp, wt = wt)
     predict(carsModel, newdata, type = "response")
}
   
# test function locally by printing results
print(manualTransmission(120, 2.8)) # 0.6418125

##########################################################
#            Log into Microsoft R Server                 #
##########################################################
   
# Use `remoteLogin` to authenticate with R Server using 
# the local admin account. Use session = false so no 
# remote R session started
# REMEMBER: Replace with your login details
remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)

##########################################################
#             Publish Model as a Service                 #
##########################################################

# Generate a unique serviceName for demos 
# and assign to variable serviceName
serviceName <- paste0("mtService", round(as.numeric(Sys.time()), 0))

# Publish as service using `publishService()` function from 
# `mrsdeploy` package. Use the service name variable and provide
# unique version number. Assign service to the variable `api`
api <- publishService(
     serviceName,
     code = manualTransmission,
     model = carsModel,
     inputs = list(hp = "numeric", wt = "numeric"),
     outputs = list(answer = "numeric"),
     v = "v1.0.0"
)

##########################################################
#                 Consume Service in R                   #
##########################################################
   
# Print capabilities that define the service holdings: service 
# name, version, descriptions, inputs, outputs, and the 
# name of the function to be consumed
print(api$capabilities())
   
# Consume service by calling function, `manualTransmission`
# contained in this service
result <- api$manualTransmission(120, 2.8)

# Print response output named `answer`
print(result$output("answer")) # 0.6418125   

##########################################################
#       Get Swagger File for this Service in R Now       #
##########################################################
   
# During this authenticated session, download the  
# Swagger-based JSON file that defines this service
swagger <- api$swagger()
cat(swagger, file = "swagger.json", append = FALSE)

# Now you can share Swagger-based JSON so others can consume it

##########################################################
#          Delete service version when finished          #
##########################################################

# User who published service or user with owner role can
# remove the service when it is no longer needed
status <- deleteService(serviceName, "v1.0.0")
status

##########################################################
#                   Log off of R Server                  #
##########################################################

# Log off of R Server
remoteLogout()
```



#### 2. R code as object and `.RData` as file 

In this example, the code is still an object (`code = manualTransmission`), but the model now comes from an .Rdata file (`model = "transmission.RData"`). The result is still the same as in the first example.

```R
# For R Server 9.0, load mrsdeploy package on R Server     
library(mrsdeploy)

# --- AAD login ----------------------------------------------------------------

# Use `remoteLogin` to authenticate with R Server using 
# the local admin account. Use session = false so no 
# remote R session started
# REMEMBER: Replace with your login details
remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)

model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)
save(model, file = "transmission.RData")

manualTransmission <- function(hp, wt) {
  newdata <- data.frame(hp = hp, wt = wt)
  predict(model, newdata, type = "response")
}

# test locally: 0.6418125
print(manualTransmission(120, 2.8))

# Generate a unique serviceName for demos 
# and assign to variable serviceName
serviceName <- paste0("mtService", round(as.numeric(Sys.time()), 0))

api <- publishService(
   serviceName,
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

services <- listServices(serviceName)
services

serviceName <- services[[1]]
serviceName

api <- getService(serviceName$name, serviceName$version)
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


#### 3. Code and model as .R scripts 

In this example, the code (`code = transmission-code.R,`) and the model comes from R scripts (`model = "transmission.R"`). The result is still the same as in the first example.


```R
# For R Server 9.0, load mrsdeploy package on R Server     
library(mrsdeploy)

# --- AAD login ----------------------------------------------------------------

# Use `remoteLogin` to authenticate with R Server using 
# the local admin account. Use session = false so no 
# remote R session started
# REMEMBER: Replace with your login details
remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)

# Information can come from a file
model <- "model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)"
code <- "newdata <- data.frame(hp = hp, wt = wt)\n
         answer <- predict(model, newdata, type = "response")"

cat(model, file = "transmission.R", append = FALSE)
cat(code, file = "transmission-code.R", append = FALSE)

# Generate a unique serviceName for demos 
# and assign to variable serviceName
serviceName <- paste0("mtService", round(as.numeric(Sys.time()), 0))

api <- publishService(
   serviceName,
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

services <- listServices(serviceName)
services

serviceName <- services[[1]]
serviceName

api <- getService(serviceName$name, serviceName$version)
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

#### 4. Code as script and model as .RData file

In this example, the code (`code = transmission-code.R,`) comes from an R script, and the model from an .RData file (`model = "transmission.RData"`). The result is still the same as in the first example.

```R
# For R Server 9.0, load mrsdeploy package on R Server     
library(mrsdeploy)

# --- AAD login ----------------------------------------------------------------

# Use `remoteLogin` to authenticate with R Server using 
# the local admin account. Use session = false so no 
# remote R session started
# REMEMBER: Replace with your login details
remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)

# model
model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)
save(model, file = "transmission.RData")

# R code
code <- "newdata <- data.frame(hp = hp, wt = wt)\n
         answer <- predict(model, newdata, type = "response")"
cat(code, file = "transmission-code.R", sep="n", append = TRUE)

# Generate a unique serviceName for demos 
# and assign to variable serviceName
serviceName <- paste0("mtService", round(as.numeric(Sys.time()), 0))

api <- publishService(
   serviceName,
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

services <- listServices(serviceName)
services

serviceName <- services[[1]]
serviceName

api <- getService(serviceName$name, serviceName$version)
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

<a name="realtime-example"></a>

### Realtime web service example

In this example, the local model object (`model = kyphosisModel`) is generated using the `rxLogit` modeling function in the RevoScaleR package. Please note that the Rpart `kyphosis` dataset is available to all R users by default. 

To learn more about the supported model formats, supported product versions, and supported platforms for realtime web services, [see here](#realtime).

```R
##          REALTIME WEB SERVICE EXAMPLE                ##
 
##########################################################
#   Create/Test Logistic Regression Model with rxLogit   #
##########################################################
    
# Create logistic regression model 
# using rxLogit modeling function from RevoScaleR package
# and the standard `kyphosis` dataset
kyphosisModel <- rxLogit(Kyphosis ~ Age, data=kyphosis)
 
# Test the model locally
testData <- data.frame(Kyphosis=c("absent"), Age=c(71), Number=c(3), Start=c(5))
rxPredict(kyphosisModel, data = testData)  # Kyphosis_Pred: 0.1941938
 
##########################################################
#            Log into Microsoft R Server                 #
##########################################################
   
# Use `remoteLogin` to authenticate with R Server using 
# the local admin account. Use session = false so no 
# remote R session started
# REMEMBER: replace with the login info for your organization
remoteLogin("http://localhost:12800", 
            username = "admin", 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)

##########################################################
#    Publish Kyphosis Model as a Realtime Service        #
##########################################################

# Generate a unique serviceName for demos 
# and assign to variable serviceName
serviceName <- paste0("kyphosis", round(as.numeric(Sys.time()), 0))
 
# Publish as service using `publishService()` function. 
# Use the variable name for the service and version `v1.0`
# Assign service to the variable `realtimeApi`.
realtimeApi <- publishService(
     serviceType = "Realtime",
     name = serviceName,
     code = NULL,
     model = kyphosisModel,
     v = "v1.0",
     alias = "kyphosisService"
)
 
##########################################################
#           Consume Realtime Service in R                #
##########################################################
   
# Print capabilities that define the service holdings: service 
# name, version, descriptions, inputs, outputs, and the 
# name of the function to be consumed
print(realtimeApi$capabilities())
   
# Consume service by calling function contained in this service
realtimeResult <- realtimeApi$kyphosisService(testData)

# Print response output
print(realtimeResult$outputParameters) # 0.1941938   
 
##########################################################
#         Get Service-specific Swagger File in R         #
##########################################################
   
# During this authenticated session, download the  
# Swagger-based JSON file that defines this service
rtSwagger <- realtimeApi$swagger()
cat(rtSwagger, file = "realtimeSwagger.json", append = FALSE)
 
# Share Swagger-based JSON with those who need to consume it
```

## See also

+ [mrsdeploy function overview](../mrsdeploy/mrsdeploy.md)
+ [Quickstart: Deploying an R model as a web service](quickstart-publish-web-service.md)
+ [Connecting to R Server from mrsdeploy](mrsdeploy-connection.md).
+ [Data scientist get started guide](data-scientist-get-started.md)
+ [Asynchronous batch execution of web services in R](data-scientist-batch-mode.md)
+ [Execute on a remote Microsoft R Server](remote-execution.md)
+ [Application developer get started guide](app-developer-get-started.md)