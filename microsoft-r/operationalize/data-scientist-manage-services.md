---

# required metadata
title: "Working with web services in R"
description: "Web service deployment functions in the mrsdeploy package in Microsoft R can be used for any arbitrary R code block. A web service runs on R Server 9.0 instances."
keywords: "mrsdeploy package"
author: "j-martens"
manager: "jhubbard"
ms.date: "02/14/2017"
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

**Applies to:  Microsoft R Server 9.0.1 & 9.1**

This article describes how you can interact with and manage analytic web services directly in R using functions in the [mrsdeploy package](../mrsdeploy/mrsdeploy.md). This R package is installed with both Microsoft R Client and Microsoft R Server. Note that a set of [RESTful APIs](api.md) are also available to provide direct programmatic access to a service's lifecycle directly.

Using `mrsdeploy` with a [properly configured R Server](../mrsdeploy/mrsdeploy.md#configure) allows you to publish R functions (models, R scripts, arbitrary R code) exposed as **analytic web services** in as little as a single line of R code. A web service might contain not only the model, but also the prediction script used to create it.

After hosted in R Server, these web services can be discovered by other authenticated users. These users can [consume the web services in R](data-scientist-get-started.md) or in the [language of their choice via Swagger](app-developer-get-started.md).


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


## Publish and manage in R

<a name="publishService"></a>

### Publish web services

In order to deploy your analytics, you must publish them as new web services running on R Server. Each service is uniquely defined by a `name` and `version`.  Additionally, each web service can include R code and any necessary model assets, the required inputs, and the output application developers will need to integrate in their applications. 

<a name="type"></a>

#### Service Types

In R Server 9.1 and higher, you can publish several types of web services. They are:

+ `R`: This standard web service type offers fast execution and scoring of arbitrary R code and R models. You can specify this service type using the `publishService` function in the `mrsdeploy` package or using the API directly.  

+ `Realtime`: This web service type offers even lower latency than the R type for supported R models published without any other R code. With lower latency, you have better load and can score more models in parallel. The additional scoring performance boost you experience when consuming a web service of this type is due to:
   + No additional resources or time spent spinning up an R sessions for each call. Supported models are scored without the need for this session.
   + Once a model is loaded into memory once, they won't need to be reloaded for the next call
   
   Only a limited number of model types and scoring functions are supported. For a list of prediction functions supported in this release, see @@Supported Prediction Functions.  <Link to Jeannine's real-time overview @@LINK COMING LATER>
   
   This service type takes a data.frame as input and also outputs a data.frame. You can specify this service type using the `publishService` function or using the API directly. 

+ `Python`:  This web service type offers fast execution and scoring of Python code and models. You can create an `Python` type service via the API directly on Windows platforms only. It is not available through the `mrsdeploy` R package.


In R Server 9.0.1, only the `R` service type is supported.

#### Function arguments and response

The `mrsdeploy` function for publishing as web services is `publishService`. 

|Function|Response|
|----|----|
|`publishService(...)`|Returns an [API instance](#api-client) (`client stub` for consuming that service and viewing its service holdings) as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class.|




From your local commandline, you can publish web services to a local R Server or remotely if you set up a remote session.

>[!IMPORTANT]
>In the case where you are working with a [remote R session](../operationalize/remote-execution.md), keep in mind that you can only publish from the local session. If you attempt to publish remotely, it will fail with this message: `Error in curl::curl_fetch_memory(uri, handle = h) : URL using bad/illegal format or missing URL`. If you are in your remote session, [switch back](../operationalize/remote-execution.md#switch) to the local commandline to publish your service. 

The following arguments are accepted for `publishService`:

|Arguments|Description|
|----|----|
|`name` *|The unique web service name. It is a string so use quotes such as "MyService". We recommend that you use a name that is easy understood and fits a nice URL so people can remember it.|
|`serviceType`|The type of service that is produced when published ([Learn more about types](#type)). Options include:<br>1. `serviceType = R`: contains any arbitrary R code or models. If omitted or null, `R` is assumed. <br>2. `serviceType = Realtime`: contains only a supported model object (see `model` argument below).<br>Note: `Python` web services are supported for Windows users via the API only.|
|`code` *|Required when `serviceType = R`, this is the R code to publish. If you use a path, the base path is your local current working directory.  `code` can take the form of:<br>1. A filepath to a local R script. For example:<br> &nbsp;  &nbsp; `code = "/path/to/R/script.R"`<br>2. A block of R code as a character string. For example:<br> &nbsp;  &nbsp; `code = "result <- x + y"`<br>3. A function handle. For example:<br> &nbsp;  &nbsp; `code = function(hp, wt) {`<br> &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `newdata <- data.frame(hp = hp, wt = wt)`<br> &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `predict(model, newdata, type = "response")`<br> &nbsp;&nbsp;  &nbsp;&nbsp; `}`<br><br>If `serviceType = Realtime`, omit `code` argument or set to NULL.|
|`model`|If `serviceType = R`, an `object` or a file-path to an external representation of R objects to be loaded and used with `code`. The specified file can be:<br>1. File-path to a local `.RData` file holding R objects to be loaded. For example:<br> &nbsp;  &nbsp; `model = "/path/to/glm-model.RData"`<br>2. File-path to a local `.R` file which will be evaluated into an environment and loaded. For example:<br> &nbsp;  &nbsp; `model = "/path/to/glm-model.R"`<br>3. An object. For example:<br> &nbsp;  &nbsp; `model = am.glm`<br><br>If `serviceType = Realtime`, a model object.  Currently, only a limited number of model types and scoring functions are supported. For a list of prediction functions supported in this release, see Supported Prediction Functions @@LINK COMING LATER. For example, `model = rxPredict.glm`<br>|
|`snapshot`|Available only for `serviceType = R`. Identifier of the snapshot to load. Can replace the `model` argument or be merged with it.|
|`inputs`|Available only for `serviceType = R`. (Note: `Realtime` defaults to data.frame automatically.) Defines the web service input schema. If empty, the service will not accept inputs. `inputs` are defined as a named list `list(x = "logical")` which describe the input parameter names and their corresponding [data types](#data-types).|
|`outputs` |Available only for `serviceType = R`. (Note: `Realtime` defaults to data.frame automatically.) Defines the web service output schema. If empty, the service will not return a response value. `outputs` are defined as a named list `list(x = "logical")` which describe the output parameter names and their corresponding  [Data Types](#data-types)<br>Note: If `{code}` is defined as a `{function}` then only one output value can be claimed.|
|`v` |Defines a unique alphanumeric web service version. If the version is left blank, a unique `{guid}` will be generated in its place. Useful during service development before the author is ready to officially publish a semantic version to share. [Learn more...](#versioning)|
|`alias` |An alias name of the predication remote procedure call (RPC) function used to consume the service. If `code` is a function, it will use that function name by default. See [Api](#api-client).|
|`destination` |The codegen output directory location.|
|`descr` |The description of the web service.|

<sup>&#42;</sup> If an argument is marked with an asterisk (&#42;), then the argument is __required__ by the function.


#### Example

See full examples in the ["Workflow" examples](#workflow) at the end of this article.

Example of a `R` runtime web service:
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

Example of a `Realtime` web service:
```R
# Publish web service called kyphosisLogRegModel 
# Supply a supported model object for Realtime type 
# Assign version number v1.0.0
api <- publishService(
     "kyphosisLogRegModel",
     serviceType = "Realtime",
     code = NULL,
     model = rxLogit(Kyphosis ~ Age, data=kyphosis),
     v = "v1.0.0",
)
```

<a name="data-types"></a>

#### I/O data types

The following table lists the supported data types for the [publishService](#publishService) and [updateService](#updateService) function input and output schemas:

|I/O data types|Full support?|
|--------|:----------:|
|`numeric`|Yes| 
|`integer`|Yes|
|`logical`|Yes|
|`character`|Yes|
|`vector`|Yes|
|`matrix`|Partial<br>(Not for logical & character matrices)|
|`data.frame`|Yes<br>Note: Coercing an object during <br>I/O is a user-defined task|



<a name="versioning"></a>

### Version web services

Every time a web service is published, a version is assigned to the web service. Versioning enables users' publishing services to better manage the release of their services and for those users consuming the services to more easily identify what they are 
calling. 

When publishing, you can specify a version. If you forget to assign one, one is generated for you.

+ To specify a version, use any alphanumeric string that is meaningful to those who will consume the service in your organization, such as `2.0`, `v1.0.0`, `v1.0.0-alpha`, `v1.0.0-test`, `test-1`. Meaningful versions are very helpful when you are ready to share your services with others. We highly recommend that everyone in your organization use a **consistent and meaningful versioning convention** such as [semantic versioning](http://semver.org/) when publishing services. 

+ If you do not specify a version, a globally unique identifier (GUID) is automatically assigned by R Server as a unique reference 
number. These GUID version numbers are harder to remember for those consuming your services, so they are not popular. 

### Update web services

If you want to change your web service after you've published it, but keep the same name and version, you can use `updateService`. and specify what needs to change, such as the R code, model, inputs, and so on. You can change one or more of the arguments at the same time. When you update a service, it overwrites that named version.

>[!NOTE]
>If you want to change the name or version number, use the `publishService` function instead and specify the new name or version number. 

The `mrsdeploy` function for updating as web services is `updateService`. 

|Function|Response|
|----|----|
|`updateService(...)`|Returns an [API instance](#api-client) (`client stub` for consuming that service and viewing its service holdings) as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class.|

<a name="updateService"></a>

The following arguments are accepted for `updateService`:

|Arguments|Description|
|----|----|
|`name`*|The web service name you want to update. It is a string so use quotes such as "MyService".|
|`v`*|Identifies the version of the web service to be updated.|
|`code`|If `serviceType = R`, the R code to publish if you are updating the code. If you use a path, the base path is your local current working directory.  `code` can take the form of:<br>1. A filepath to a local R script. For example:<br> &nbsp;  &nbsp; `code = "/path/to/R/script.R"`<br>2. A block of R code as a character string. For example:<br> &nbsp;  &nbsp; `code = "result <- x + y"`<br>3. A function handle. For example:<br> &nbsp;  &nbsp; `code = function(hp, wt) {`<br> &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `newdata <- data.frame(hp = hp, wt = wt)`<br> &nbsp;&nbsp;  &nbsp;&nbsp;  &nbsp;&nbsp; `predict(model, newdata, type = "response")`<br> &nbsp;&nbsp;  &nbsp;&nbsp; `}`<br><br>If `serviceType = Realtime`, `code` does not accepted.|
|`model`|If `serviceType = R`, an `object` or a file-path to an external representation of R objects to be loaded and used with `code`. The specified file can be:<br>1. File-path to a local `.RData` file holding R objects to be loaded. For example:<br> &nbsp;  &nbsp; `model = "/path/to/glm-model.RData"`<br>2. File-path to a local `.R` file which will be evaluated into an environment and loaded. For example:<br> &nbsp;  &nbsp; `model = "/path/to/glm-model.R"`<br>3. An object. For example:<br> &nbsp;  &nbsp; `model = am.glm`<br><br>If `serviceType = Realtime`, a model object. Currently, only a limited number of model types and scoring functions are supported. For a list of prediction functions supported in this release, see Supported Prediction Functions @@LINK COMING LATER. For example, `model = rxPredict.glm`|
|`snapshot` |Available only for `serviceType = R`. (Note: `Realtime` defaults to data.frame automatically.) Identifier of the snapshot to load. Can replace the `model` argument or be merged with it.|
|`inputs` |Available only for `serviceType = R`. Defines the web service input schema. If empty, the service will not accept inputs. `inputs` are defined as a named list `list(x = "logical")` which describe the input parameter names and their corresponding [data types](#data-types).|
|`outputs` |Available only for `serviceType = R`.  (Note: `Realtime` defaults to data.frame automatically.) Defines the web service output schema. If empty, the service will not return a response value. `outputs` are defined as a named list `list(x = "logical")` which describe the output parameter names and their corresponding  [Data Types](#data-types)<br>Note: If `{code}` is defined as a `{function}` then only one output value can be claimed.|
|`alias` |An alias name of the predication remote procedure call (RPC) function used to consume the service. If `code` is a function, it will use that function name by default. See [Api](#api-client).|
|`destination` |The codegen output directory location.|
|`descr` |The description of the web service.|

<sup>&#42;</sup> If an argument is marked with an asterisk (&#42;), then the argument is __required__ by the function.

For more information on input and output data types, see the above section [I/O Data Types](#data-types).

#### Example

```R
# For web service called mtService with version number v1.0.0,
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

```R
result <- deleteService("mtService", "v1.0.0")
print(result)
```

## Interact with services in R

### List web services

Any authenticated user can retrieve a list of web services using the `listServices` function. You can use arguments to filter the list to return a specific web service or all labeled versions of a given web service.

You can only see the R code for the web services that you published or manage. If you are not the user who published the service or you are not assigned to the "Owner" role, then you will not be able see the actual R code used when the web service was published.

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

```R
# Return metadata for all services hosted on this R Server
allServices <- listServices()

# Return metadata for every version of the 
# service "mtService" hosted on this R Server
mtServiceAll <- listServices("mtService")

# Return metadata for version v1.0.0 of the 
# service "mtService" hosted on this R Server
mtServiceV1 <- listServices("mtService", "v1")

# View service capabilities/schema. 
# For example, the input schema:
#   list(name = "wt", type = "numeric")
#   list(name = "dist", type = "numeric")
print(mtService)
```

In R Server 9.1 and later, the example returns:
```R
$creationTime
[1] "2017-02-13T19:44:26.2611422"

$name
[1] "mtService"

$version
[1] "v1"

$description
NULL

$snapshotId
[1] "05053e85-c9d0-43cb-9be8-8dccf2b5da54"

$versionPublishedBy
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

$myPermissionsOnService
[1] "read/write"
```


In R Server 9.0.1, the example returns:
```R
$creationTime
[1] "2017-02-13T19:44:26.2611422"

$name
[1] "mtService"

$version
[1] "v1"

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

Any authenticated user can retrieve a web service object using the `getService` function that makes it possible for the service to be consumed. After the object is returned, you can look at its capabilities to see what the service can do and how it should be consumed.

The `mrsdeploy` function for retrieving a service object is `getService`. 

|Function|Response|
|----|----|
|`getService(...)`|Returns an [API instance](#api-client) (`client stub` for consuming that service and viewing its service holdings) as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class.|

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

<a name="api-client"></a>

### Interact with API clients

When you publish, update or get a web service, an API instance is returned as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class. This instance is a `client stub` for consuming that service and viewing its service holdings. 

You can use the following supported public functions to interact with the API client instance.

| Function      | Description                                            |
| ------------- |--------------------------------------------------------|
| `print`       |	Print method that lists all members of the object      |
| `capabilities` | Report on the service features such as I/O schema, `name`, `version`	   |
| `consume`     |	Consume the service based on I/O schema                |
| consume _alias_ | Alias to the `consume` function for convenience (see `alias` argument for the `publishService` function). |
| `swagger`     |	Displays the service's `swagger` specification         |
| `batch` |Define the data records to be batched... In addition to the public functions above, there are many functions you can use to  consume a service asynchronously via batch execution. [For public functions for batch, see this article](data-scientist-batch-mode.md#public-fx-batch).|


#### Example

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

### Consume web services 

After a web service has been published, it can be consumed. Whenever the web service is published or updated, a Swagger-based JSON file is generated automatically to define the service to facilitate consumption and integration.

When you publish a service, you should let people know that is ready for them to try out. Users can get the Swagger file they need to consume the service directly in R or via the API.  If you do not provide them with a service name or version, they can discover the service on their own using the `listServices` function described earlier in this article.

Users can consume the service directly using a single consumption call. This approach is referred to as a "Request Response" approach and is described below. Another approach is the [asynchronous "Batch" consumption approach](data-scientist-batch-mode.md), where users send as a single request to R Server, which then makes multiple asynchronous API calls on your behalf.

<a name="data-scientists-share"></a>

#### Collaborate with data scientists

Other data scientist may want to explore, test, and consume Web services directly in R using the functions in the `mrsdeploy` package installed with Microsoft R Server and R Client. Quality engineers might want to bring the models in these web services into validation and monitoring cycles.

As the owner of the service, you can share the name and version number for the service with fellow data scientists so they can call the service in R using the functions in the `mrsdeploy` package.  After authenticating, data scientists can use the `getService` function in R to call the service. Then, they can get details about the service and start consuming it.

>[!NOTE]
> It is also possible to perform batch consumption as [described here](data-scientist-batch-mode.md).

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

Application developers can call and integrate web services into their applications using each service-specific Swagger-based JSON file along with the required inputs. 

After the application developer has this Swagger-based JSON file, he or she can create client libraries for integration. Read "[Application Developer Get Started Guide](app-developer-get-started.md)" for more details.  
   
Get the Swagger-based JSON file in one of two ways:

+ A data scientist can send them the Swagger-based JSON file they've downloaded, which can sometimes be a faster approach. In R, you can get the file with the following code and send it to the application developer:
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

## Workflows: publish-to-consume 

The following workflow examples demonstrate how to publish a web service, interact with it, and consume it. 

In each example of a web services of `serviceType = R`, the values of the R code (`code`) and the model (`model`) are represented in different ways (as files, objects, ...), but in each case the result is the same.  For web services of `serviceType = R`, keep in mind that:
+ R code must come from: 
  + A filepath to a local R script
  + A block of R code as a character string
  + A function handle
+ A model can come:
  + File path to an `.RData` file holding the external R objects to be loaded and used with the code
  + File path to an `.R` file which will be evaluated into an environment and loaded
  + A model object

In the example of a web services of `serviceType = Realtime`, keep in mind that:
+ R code is not supported
+ The model must be:
  + A model object
  + A supported model formats @@ADD LINK TO SUPPORTED FORMATS



The base path for files is set to your working directory.  
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

### Using local objects for code and model (runtime R service)

In this example, the code comes from an object (`code = manualTransmission`) and the model comes from a model object (`model = carsModel`).

```R
##########################################################
#       Create & Test a Logistic Regression Model        #
##########################################################

# Load mrsdeploy package on R Server     
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
remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)

##########################################################
#             Publish Model as a Service                 #
##########################################################

# Publish as service using `publishService()` function from 
# `mrsdeploy` package. Name service "mtService" and provide
# unique version number. Assign service to the variable `api`
api <- publishService(
     "mtService",
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
status <- deleteService("mtService", "v1.0.0")
status

##########################################################
#                   Log off of R Server                  #
##########################################################

# Log off of R Server
remoteLogout()
```



### Using local `.RData` file  (runtime R service)

In this example, the code is still an object (`code = manualTransmission`), but the model now comes from an .Rdata file (`model = "transmission.RData"`). The result is still the same as in the first example.

```R
library(mrsdeploy)

# --- AAD login ----------------------------------------------------------------

# Use `remoteLogin` to authenticate with R Server using 
# the local admin account. Use session = false so no 
# remote R session started
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

mtService <- services[[1]]
mtService

api <- getService(mtService$name, mtService$version)
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


### Using `.R` files (runtime R service)

In this example, the code (`code = transmission-code.R,`) and the model comes from R scripts (`model = "transmission.R"`). The result is still the same as in the first example.

```R
library(mrsdeploy)

# --- AAD login ----------------------------------------------------------------

# Use `remoteLogin` to authenticate with R Server using 
# the local admin account. Use session = false so no 
# remote R session started
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

mtService <- services[[1]]
mtService

api <- getService(mtService$name, mtService$version)
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

### Using `.RData` and `.R` (runtime R service)


In this example, the code (`code = transmission-code.R,`) comes from an R script, and the model from an .RData file (`model = "transmission.RData"`). The result is still the same as in the first example.

```R
library(mrsdeploy)

# --- AAD login ----------------------------------------------------------------

# Use `remoteLogin` to authenticate with R Server using 
# the local admin account. Use session = false so no 
# remote R session started
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

mtService <- services[[1]]
mtService

api <- getService(mtService$name, mtService$version)
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


### Using a supported local model object (Realtime service)

In this example, the code comes from an object (`code = manualTransmission`) and the model comes from a model object (`model = carsModel`).

```R
##########################################################
#       Create & Test a Logistic Regression Model        #
##########################################################

# Load mrsdeploy package on R Server     
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
remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)

##########################################################
#             Publish Model as a Service                 #
##########################################################

# Publish as service using `publishService()` function from 
# `mrsdeploy` package. Name service "mtService" and provide
# unique version number. Assign service to the variable `api`
api <- publishService(
     "mtService",
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
status <- deleteService("mtService", "v1.0.0")
status

##########################################################
#                   Log off of R Server                  #
##########################################################

# Log off of R Server
remoteLogout()
```

## See also

+ [mrsdeploy function overview](../mrsdeploy/mrsdeploy.md)
+ [Connecting to R Server from mrsdeploy](../operationalize/mrsdeploy-connection.md).
+ [Data scientist get started guide](data-scientist-get-started.md)
+ [Asynchronous batch execution of web services in R](../operationalize/data-scientist-batch-mode.md)
+ [Execute on a remote Microsoft R Server](../operationalize/remote-execution.md)
+ [Application developer get started guide](../operationalize/app-developer-get-started.md)