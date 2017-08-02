---

# required metadata
title: "How to publish and manage web services in R - Microsoft R Server | Microsoft Docs"
description: "Web service deployment functions in the mrsdeploy package in Microsoft R can be used for any arbitrary R code block."
keywords: "mrsdeploy package"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "8/1/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# How to publish and manage R web services in R Server

**Applies to:  Microsoft R Server 9.x** 

This article details how you can publish and manage your **analytic web services** directly in R. You can deploy your R models, scripts, and code as web services using the functions in the [mrsdeploy R package](../r-reference/mrsdeploy/mrsdeploy-package.md).  The mrsdeploy R package containing these functions is installed with both Microsoft R Server and Microsoft R Client.

These web services can be [consumed in R](how-to-consume-web-service-interact-in-r.md) by other authenticated users or in the [language of their choice via Swagger](how-to-build-api-clients-from-swagger-for-app-integration.md).

Using the mrsdeploy R package, you can [publish](#publishService), [update](#updateService), and  [delete](#deleteService) two kinds of R web services: **standard R web services** and **realtime R web services**.  Additionally, you can get a [list of all services](how-to-consume-web-service-interact-in-r.md#listServices), retrieve a [web service object](how-to-consume-web-service-interact-in-r.md#getService) for consumption, and [share services](how-to-consume-web-service-interact-in-r.md#consume-service) with others.

You can also publish or interact with a web service outside of R using the [RESTful APIs](concept-api.md), which provide direct programmatic access to a service's lifecycle.

<a name="auth"></a>

## Requirements

Before you can use the web service management functions in the mrsdeploy R package, you must:
+ Have access to an R Server instance that was  [properly configured](../r-reference/mrsdeploy/mrsdeploy-package.md#configure) to host web services. 

+ Authenticate with R Server using the remoteLogin() or remoteLoginAAD() functions in the mrsdeploy package as described in "[Connecting to R Server to use mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md)."

## Web service types

<a name="standard"></a>

### Standard R web services

 These web services offer fast execution and scoring of arbitrary R code and R models. They can contain R code, models, and model assets. They can also take specific inputs and provide specific outputs for those users who are integrating the services inside their applications.

Standard web services, like all web services, are identified by their name and version. Additionally, a standard web service is also defined by any R code, models, and any necessary model assets. When publishing a standard web service, you must also define the required inputs and any output the application developers use to integrate the service in their applications.

A code sample for publishing web services can be [found later in this article](#publishService).

|R source|Can come from|
|----|----|
|R code|- A filepath to a local R script, such as:<br>&nbsp;&nbsp;&nbsp;  `code = "/path/to/R/script.R"`<br>- A block of R code as a character string, such as:<br>&nbsp;&nbsp;&nbsp;  `code = "result <- x + y"`<br>- A function handle, such as:<br>&nbsp;&nbsp;&nbsp;  `code = function(hp, wt) {`<br>&nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `newdata <- data.frame(hp = hp, wt = wt)`<br>&nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `predict(model, newdata, type = "response")`<br>&nbsp;&nbsp;  `}`|
|R model|The R model can come from an object or a file-path to an external representation of R objects to be loaded and used with the code, including: <br>- A filepath to an RData file holding the external R objects to be loaded and used with the code, such as:<br>&nbsp;&nbsp;&nbsp;  `model = "/path/to/glm-model.RData"`<br>- A filepath to an R file that is evaluated into an environment and loaded, such as:<br>&nbsp;&nbsp;&nbsp;  `model = "/path/to/glm-model.R"`<br>- A model object, such as:<br>&nbsp;&nbsp;&nbsp;  `model = am.glm`|


<a name="realtime"></a>

### Realtime R web services

Once you've built a predictive model, in many cases the next step is to operationalize the model. That is to generate predictions from the pre-trained model in real time. In this scenario, where new data often become available one row at a time, latency becomes the critical metric. It is important to respond with the single prediction (or score) as quickly as possible.

Realtime web services, **introduced in R Server 9.1**, offer even lower latency and better load to produce results faster and score more models in parallel. The improved performance boost comes from the fact that these web services do not rely on an R interpreter at consumption time even though the services use the R objects created by the model. Therefore, no additional resources or time is spent spinning up an R session for each call. Additionally, since the model is cached in memory, it is only loaded once. This type of web takes only R models created with [supported functions](#realtime) and does not support arbitrary R code. 

Realtime web services, like all web services, are also identified by their name and version. These lower latency, faster load services take only a model object created with supported functions. No other R code is supported with Realtime web services. Additionally, you do not need to specify inputs or outputs since realtime web services default to data.frame inputs and outputs automatically. 

Realtime web services are supported only on Windows platforms only for R Server 9.1. However, the resulting web service can be consumed on any platform.

A code sample for publishing realtime services can be [found later in this article](#publishService).
A longer [realtime example](#realtime-example) is also available.

|R Source|Can come from|
|------|---------|
|R model|A model object created with supported functions, such as:<ul><li>These [RevoScaleR package](~/r-reference/revoscaler/revoscaler.md) functions: rxLogit, rxLinMod, rxBTrees, rxDTree, and rxDForest <br><br><li>These [MicrosoftML package](../r-reference/microsoftml/microsoftml-package.md) functions for machine learning and transform tasks: rxFastTrees, rxFastForest, rxLogisticRegression, rxOneClassSvm, rxNeuralNet, rxFastLinear, featurizeText, concat, categorical, categoricalHash, selectFeatures, featurizeImage, getSentiment, loadimage, resizeImage, extractPixels, selectColumns, and dropColumns<br><br>While mlTransform featurization is supported in realtime scoring, R transforms are not supported. Instead, use sp_execute_external_script .</ul>|

## Permissions for managing web services

Any authenticated user can:
+ Update and delete web services they have published
+ Retrieve any web service object for consumption
+ Retrieve a list of any or all web services

While any authenticated user can also publish a web service by default, roles can be used to further control permissions. Beginning in version 9.1, your administrator can also **[assign role-based authorization](configure-roles.md)** to further restrict the permissions around web services to give some users more control over web services than others. Ask your administrator for details on your role.

## Publish and update web services

To deploy your analytics, you must publish them as web services in R Server. Once hosted on R Server, you can update and manage them. They can also be consumed by other users. 

<a name="versioning"></a>

### Versioning

Every time a web service is published, a version is assigned to the web service. Versioning enables users to better manage the release of their web services. Versions help the users consuming your service to easily identify it. 

At publish time, you can specify an alphanumeric string that is meaningful to the users who consume the service in your organization. For example, you could use '2.0', 'v1.0.0', 'v1.0.0-alpha', or 'test-1'. Meaningful versions are helpful when you intend to share services with others. We highly a **consistent and meaningful versioning convention** across your organization or team such as [semantic versioning](http://semver.org/).

If you do not specify a version, a globally unique identifier (GUID) is automatically assigned by R Server. These GUID version numbers are harder to remember by the users consuming your services and are therefore less desirable. 

<a name="publishService"></a>

### Publish service

After you've authenticated, use the publishService() function in the mrsdeploy package to publish a web service.  See the [package reference for publishService()](../r-reference/mrsdeploy/publishservice.md) for the full description of all arguments.  

|Function|Response|R Help|
|----|----|:----:|
|publishService(...)|Returns an [API instance](how-to-consume-web-service-interact-in-r.md#api-client) client stub for consuming that service and viewing its service holdings) as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class.|[View](../r-reference/mrsdeploy/publishservice.md)

You can publish web services to a local R Server from your command line. If you [create a remote session](../r/how-to-execute-code-remotely.md#publish-remote-session), you can also publish a web service to a remote R Server from your local command line.  


Example of standard web service:

```R
# Publish a standard service 'mtService' version 'v1.0.0'
# Assign service to 'api' variable
api <- publishService(
     "mtService",
     code = manualTransmission,
     model = carsModel,
     inputs = list(hp = "numeric", wt = "numeric"),
     outputs = list(answer = "numeric"),
     v = "v1.0.0"
)
```

Example of realtime service (supported in R Server 9.1+): 

```R
# Publish a realtime service 'kyphosisService' version 'v1.0'
# Assign service to 'realtimeApi' variable
realtimeApi <- publishService(
     serviceType = "Realtime",
     name = "kyphosisService",
     code = NULL,
     model = kyphosisModel,
     v = "v1.0",
    alias = "kyphosisService"
)
```

For [full examples](#workflow), see the end of this article.

You can also follow the quickstart article "[Deploying an R model as a web service](quickstart-publish-r-web-service.md)."

Learn how to get a [list of all services](how-to-consume-web-service-interact-in-r.md#listServices), retrieve a [web service object](how-to-consume-web-service-interact-in-r.md#getService) for consumption, and [share services](how-to-consume-web-service-interact-in-r.md#consume-service) with others.

<a name="updateService"></a>

### Update service

To change a web service after you've published it, while retaining the same name and version, use the updateService() function. For arguments, specify what needs to change, such as the R code, model, and inputs. When you update a service, it overwrites that named version.

After you've authenticated, use the updateService() function in the mrsdeploy package to update a web service.

See the [package reference help page for updateService()](../r-reference/mrsdeploy/updateservice.md) for the full description of all arguments. 

|Function|Response|R Help|
|----|----|:----:|
|updateService(...)|Returns an [API instance](how-to-consume-web-service-interact-in-r.md#api-client) client stub for consuming that service and viewing its service holdings) as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class.|[View](../r-reference/mrsdeploy/updateservice.md)


>[!NOTE]
>If you want to change the name or version number, use the publishService() function instead and specify the new name or version number. 


Example: 

```R
# For web service called mtService with version number v1.0.0,
# update the model carsModel, code, inputs, and description. 
# Assign it to a variable called api.
api <- updateService(
     "mtService",
     "v1.0.0",
     code = manualTransmission,
     mode = carsModel,
     inputs = list(carData = "data.frame"),
     outputs = list(answer = "data.frame"),
     descr = "Updated after March data refresh."
)
```

<a name="data-types"></a>

### Supported I/O data types

The following table lists the supported data types for the [publishService](#publishService) and [updateService](#updateService) function input and output schemas.

|I/O data types|Full support?|
|--------|:----------:|
|numeric|Yes| 
|integer|Yes|
|logical|Yes|
|character|Yes|
|vector|Yes|
|matrix|Partial<br>(Not for logical & character matrices)|
|data.frame|Yes<br>Note: Coercing an object during <br>I/O is a user-defined task|


<a name="deleteService"></a>

## Delete web services

When you no longer want to keep a web service, you can delete it. Only the user who initially created the web service can use this function.

After you've authenticated, use the deleteService() function in the mrsdeploy package to delete a web service.

Each web service is uniquely defined by a name and version. See the [package reference help page for deleteService()](../r-reference/mrsdeploy/deleteservice.md) for the full description of all arguments. 

|Function|Response|R Help|
|----|----|:----:|
|deleteService(...)|If it is successful, it returns a success status and message such as _"Service mtService version v1.0.0 deleted."_ If it fails for any reason, then it stops execution with error message.|[View](../r-reference/mrsdeploy/deleteservice.md)

Example: 

```R
result <- deleteService("mtService", "v1.0.0")
print(result)
```

<a name="workflow"></a>

## Standard workflow examples

The following workflow examples demonstrate how to publish a web service, interact with it, and then consume it. 

Each standard web service example uses the same code and models and returns the same results. However the code and model are represented in different formats each time, such as R scripts, objects, and files.  

To learn more about standard web services, [see here](#standard).

### Before you begin

>[!IMPORTANT]
>Replace the connection details in the remoteLogin() function in each example with the details for your configuration. Connecting to R Server using the mrsdeploy package is covered [in this article](how-to-connect-log-in-with-mrsdeploy.md).

The base path for files is set to your working directory, but you can change that using ServiceOption as follows:

+ Specify a different base path for code and model arguments:  
  ```R
  opts <- serviceOption()
  opts$set("data-dir", "/base/path/to/some-other/location"))
  ```

+ Clear the path and expect the code and model files to be in the current working directory during publishService():
  ```R
  opts <- serviceOption() s
  opts$set("data-dir", NULL))
  ```

+ Clear the path and require a FULLY QUALIFIED path for code and model parameters in publishService():
  ```R
  opts <- serviceOption() s
  opts$set("data-dir", ""))
  ```


### 1. R code and model are objects

In this example, the code comes from an object (`code = manualTransmission`) and the model comes from a model object (`model = carsModel`).  For an example of inputs/outputs as dataframes, see ["Example 5"](#dataframe).


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

# Publish as service using publishService() function from 
# mrsdeploy package. Use the service name variable and provide
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



### 2. R code as object and RData as file 

In this example, the code is still an object (`code = manualTransmission`), but the model now comes from a Rdata file (`model = "transmission.RData"`). The result is still the same as in the first example.

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


### 3. Code and model as R scripts 

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

### 4. Code as script and model as a RData file

In this example, the code (`code = transmission-code.R,`) comes from an R script, and the model from an RData file (`model = "transmission.RData"`). The result is still the same as in the first example.

```R
# For R Server 9.0, load mrsdeploy package on R Server     
library(mrsdeploy)

# AAD login

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

<a name="dataframe"></a>

### 5. R code & model as objects, inputs/outputs as dataframes

In this example, the code comes from an object (`code = manualTransmission`) and the model comes from a model object (`model = carsModel`) as it was in example 1. However, in this example, the inputs and outputs are provided in the form of dataframes. 

```R
######## Create/Test Logistic Regression Model #########

# R Server 9.0, load mrsdeploy. Later versions can skip.    
library(mrsdeploy)

# Estimate the probability of a vehicle being fitted with 
# a manual transmission based on horsepower (hp) and weight (wt)

# load the mtcars dataset
data(mtcars)

# Split the mtcars dataset into 75% train and 25% test dataset
train_ind <- sample(seq_len(nrow(mtcars)), size = floor(0.75 * nrow(mtcars)))
train <- mtcars[train_ind,]
test <- mtcars[-train_ind,]

# Create glm model with training `mtcars` dataset
carsModel <- rxLogit(formula = am ~ hp + wt, data = train)

# Create a list to pass the data column info together with the model object
carsModelInfo <- list(predictiveModel = carsModel, colInfo = rxCreateColInfo(train))

# Produce a prediction function that can use the model and column info
manualTransmission <- function(carData) {
  newdata <- rxImport(carData, colInfo = carsModelInfo$colInfo)
  rxPredict(carsModelInfo$predictiveModel, newdata, type = "response")
}

# test function locally by printing results
print(manualTransmission(test))


############# Log into Microsoft R Server ##############

# Use `remoteLogin` to authenticate with R Server.
# REMEMBER: Replace with your login details
remoteLogin("http://localhost:12800", 
            username = "admin", 
            password = "{{YOUR_PASSWORD}}",
            session = FALSE)


############## Publish Model as a Service ##############

# Generate a unique serviceName for demos and assign to variable serviceName
serviceName <- paste0("mtService", round(as.numeric(Sys.time()), 0))

# Publish as service using publishService() function from 
# mrsdeploy package. Use the service name variable and provide
# unique version number. Assign service to the variable `api`
api <- publishService(
  serviceName,
  code = manualTransmission,
  model = carsModelInfo,
  inputs = list(carData = "data.frame"),
  outputs = list(answer = "data.frame"),
  v = "v1.0.0"
)

################## Consume Service in R ################

# Print capabilities that define the service holdings: service 
# name, version, descriptions, inputs, outputs, and the 
# name of the function to be consumed
print(api$capabilities())

# Consume service by calling function, `manualTransmission` contained in this service

# consume service using existing data frame `test`
result <- api$manualTransmission(test)
print(result$output("answer")) 

# consume service by constructing data frames with single row and multiple rows
emptyDataFrame <- data.frame(mpg = numeric(),
                             cyl = numeric(),
                             disp = numeric(),
                             hp = numeric(),
                             drat = numeric(),
                             wt = numeric(),
                             qsec = numeric(),
                             vs = numeric(),
                             am = numeric(),
                             gear = numeric(),
                             carb = numeric())

singleRowDataFrame <- rbind(emptyDataFrame, data.frame(mpg = 21.0,
                                                       cyl = 6,
                                                       disp = 160,
                                                       hp = 110,
                                                       drat = 3.90,
                                                       wt = 2.620,
                                                       qsec = 16.46,
                                                       vs = 0,
                                                       am = 1,
                                                       gear = 4,
                                                       carb = 4))
result <- api$manualTransmission(singleRowDataFrame)
print(result$output("answer"))

multipleRowsDataFrame <- rbind(emptyDataFrame, data.frame(mpg = c(21.0, 20.1),
                                                          cyl = c(6, 5),
                                                          disp = c(160, 159),
                                                          hp = c(110, 109),
                                                          drat = c(3.90, 2.94),
                                                          wt = c(2.620, 2.678),
                                                          qsec = c(16.46, 15.67),
                                                          vs = c(0, 0),
                                                          am = c(1, 1),
                                                          gear = c(4, 3),
                                                          carb = c(4, 2)))
result <- api$manualTransmission(multipleRowsDataFrame)
print(result$output("answer")) 


######### Get Swagger File for Service in R Now ########

# During this authenticated session, download the  
# Swagger-based JSON file that defines this service
swagger <- api$swagger()
cat(swagger, file = "swagger.json", append = FALSE)

# Now you can share Swagger-based JSON so others can consume it


######### Delete service version when finished #########

# User who published service or user with owner role can
# remove the service when it is no longer needed
status <- deleteService(serviceName, "v1.0.0")
status


################### Log off R Server ###################

remoteLogout()
```



<a name="realtime-example"></a>

## Realtime workflow example

In this example, the local model object (`model = kyphosisModel`) is generated using the `rxLogit` modeling function in the RevoScaleR package. 

Realtime web services were introduced in R Server 9.1. To learn more about the supported model formats, supported product versions, and supported platforms for realtime web services, [see here](#realtime).

```R
##          REALTIME WEB SERVICE EXAMPLE                ##
 
##########################################################
#   Create/Test Logistic Regression Model with rxLogit   #
##########################################################
    
# Create logistic regression model 
# using rxLogit modeling function from RevoScaleR package
# and the Rpart `kyphosis` dataset available to all R users
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
 
# Publish as service using publishService() function. 
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

+ [mrsdeploy function overview](../r-reference/mrsdeploy/mrsdeploy-package.md)
+ [Quickstart: Deploying an R model as a web service](quickstart-publish-r-web-service.md)
+ [Connecting to R Server from mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md).
+ [Get started guide for data scientists](concept-operationalize-deploy-consume.md)
+ [How to interact with and consume web services in R](how-to-consume-web-service-interact-in-r.md)
+ [How to integrate web services and authentication into your application](how-to-build-api-clients-from-swagger-for-app-integration.md)
+ [Asynchronous batch execution of web services in R](how-to-consume-web-service-asynchronously-batch.md)
+ [Execute on a remote Microsoft R Server](../r/how-to-execute-code-remotely.md)
