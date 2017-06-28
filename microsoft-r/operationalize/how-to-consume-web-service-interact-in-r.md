---

# required metadata
title: "How to get and consume web services in R - Microsoft R Server | Microsoft Docs"
description: "Web service interaction and consumption functions in the mrsdeploy package."
keywords: "mrsdeploy package"
author: "j-martens"
manager: "jhubbard"
ms.date: "6/21/2017"
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

After a web service has been published or updated, any authenticated user can list, examine, and consume that web service. You can do so directly in R using the functions in the [mrsdeploy R package](../r-reference/mrsdeploy/mrsdeploy-package.md). The `mrsdeploy` R package is installed with both Microsoft R Server and Microsoft R Client.  Also note that application developers can also consume a web service in the [language of their choice via Swagger](how-to-build-api-clients-from-swagger-for-app-integration.md).

If you do not want to list, examine, or consume the web service in R, a set of [RESTful APIs](concept-api.md) are also available to provide direct programmatic access to a service's lifecycle directly.

To list, examine, or consume the web service outside of R, use the [RESTful APIs](concept-api.md), which provide direct programmatic access to a service's lifecycle.

<a name="auth"></a>

## Requirements

Before you can use the web service management functions in the `mrsdeploy` R package, you must:
+ Have access to an R Server instance that was  [properly configured](../r-reference/mrsdeploy/mrsdeploy-package.md#configure) to host web services. 

+ Authenticate with R Server using the `remoteLogin` or `remoteLoginAAD` functions in the `mrsdeploy` package as described in the article "[Connecting to R Server to use mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md)."


<a name="listServices"></a>

## Find and list web services

Any authenticated user can retrieve a list of web services using the `listServices` function in the `mrsdeploy` package.  

Use function arguments to return a specific web service or all labeled versions of a given web service. See the [package reference help page for listServices()](../r-reference/mrsdeploy/listservices.md) for the full description of all arguments.

Your ability to see the code inside the web service depends on your permissions. Did you publish the web service or do you have the "Owner" role?
+ If yes, then the code in the service is returned along with other metadata.
+ If no, then the code in the service is never returned in the metadata.

To learn more about the roles in your organization, contact your Microsoft R Server administrator.

|Function|Response|R Function Help|
|----|----|:----:|
|`listServices(...)`| R `list` containing service metadata.|[View](../r-reference/mrsdeploy/listservices.md)|

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

For a detailed example with listServices, check out these ["Workflow" examples](how-to-deploy-web-service-publish-manage-in-r.md#workflow).

Example output:

|R Server 9.1+|R Server 9.0|
|--------|--------|
|![9.1 output](./media/how-to-consume-web-service-interact-in-r/returns91.png)|![9.0 output](./media/how-to-consume-web-service-interact-in-r/returns90.png)|

<a name="getService"></a>

## Retrieve and examine service objects

Any authenticated user can retrieve a web service object for consumption. After the object is returned, you can look at its capabilities to see what the service can do and how it should be consumed.

After you've authenticated, use the `getService` function in the `mrsdeploy` package to retrieve a service object. See the [package reference help page for getService()](../r-reference/mrsdeploy/getservice.md) for the full description of all arguments. 


|Function|Response|R Function Help|
|----|----|:----:|
|`getService(...)`|Returns an [API instance](#api-client) (`client stub` for consuming that service and viewing its service holdings) as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class.|[View](../r-reference/mrsdeploy/getservice.md)|


Example:

```R
# Get service using getService() function from `mrsdeploy` package.
# Assign service to the variable `api`.
api <- getService("mtService", "v1.0.0")

# Print capabilities to see what service can do.
print(api$capabilities())
     
# Start interacting with the service, for example:
# Calling the function, `manualTransmission`
# contained in this service.
result <- api$manualTransmission(120, 2.8)
```

For a detailed example with getService, check out these ["Workflow" examples](how-to-deploy-web-service-publish-manage-in-r.md#workflow).


<a name="api-client"></a>

## Interact with API clients

When you publish, update, or get a web service, an API instance is returned as an [R6](https://cran.r-project.org/web/packages/R6/index.html) class. This instance is a `client stub` you can use to consume that service and view its service holdings. 

You can use the following supported public functions to interact with the API client instance.

| Function      | Description                                            |
| ------------- |--------------------------------------------------------|
| `print`       |	Print method that lists all members of the object      |
| `capabilities` | Report on the service features such as I/O schema, `name`, `version`	   |
| `consume`     |	Consume the service based on I/O schema                |
| consume _alias_ | Alias to the `consume` function for convenience (see `alias` argument for the [`publishService` function](../r-reference/mrsdeploy/publishservice.md)). |
| `swagger`     |	Displays the service's `swagger` specification         |
| `batch` |Define the data records to be batched. There are additional publish functions used to [consume a service asynchronously via batch execution](how-to-consume-web-service-asynchronously-batch.md#public-fx-batch).|


Example:

```R
# Get service using getService() function from `mrsdeploy`.
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

For a detailed example, check out these ["Workflow" examples](how-to-deploy-web-service-publish-manage-in-r.md#workflow).

<a name="consume-service"></a>

## Consume web services 

Web services are published to facilitate the consumption and integration of the operationalized models and code.  Whenever the web service is published or updated, a Swagger-based JSON file is generated automatically that define the service.

When you publish a service, you can let people know that it is ready for consumption. Users can get the Swagger file they need to consume the service directly in R or via the API. To make it easy for others to find your service, provide them with the service name and version number (or they can use [the `listServices` function](#listServices)).

Users can consume the service directly using a single consumption call. This approach is referred to as a "Request Response" approach and is described in the following section. Another approach is the [asynchronous "Batch" consumption approach](how-to-consume-web-service-asynchronously-batch.md), where users send as a single request to R Server, which then makes multiple asynchronous API calls on your behalf.
  
<a name="data-scientists-share"></a>

#### Collaborate with data scientists

Other data scientists may want to explore, test, and consume Web services directly in R using the functions in the `mrsdeploy` package. Quality engineers might want to bring the models in these web services into validation and monitoring cycles.

You can share the name and version of a web service with fellow data scientists so they can call that service in R using the functions in the `mrsdeploy` package.  After authenticating, data scientists can use the `getService` function in R to call the service. Then, they can get details about the service and start consuming it.

>[!NOTE]
> It is also possible to perform batch consumption as [described here](how-to-consume-web-service-asynchronously-batch.md).


In this example, replace the following remoteLogin() function with the correct login details for your configuration. Connecting to R Server using the `mrsdeploy` package is covered [in this article](how-to-connect-log-in-with-mrsdeploy.md).

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
   
# Get service using getService() function from `mrsdeploy`
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

Using the Swagger-based JSON file, application developers can generate client libraries for integration. Read "[How to integrate web services and authentication into your application](how-to-build-api-clients-from-swagger-for-app-integration.md)" for more details.  
   
Application developers can get the Swagger-based JSON file in one of these ways:

+ A data scientist, probably the one who published the service, can send you the Swagger-based JSON file. This approach is often faster than the following one. They can get the file in R with the following code and send it to the application developer:
   ```R
   api <- getService("<name>", "<version>")
   swagger <- api$swagger()
   cat(swagger, file = "swagger.json", append = FALSE) 
   ```

+ Or, the application developer can request the file  **as an authenticated user with an [active bearer token](how-to-build-api-clients-from-swagger-for-app-integration.md#authentication) in the request header** (since all API calls must be authenticated). The URL is formed as follows:
  ```
  GET /api/<service-name>/<service-version>/swagger.json
  ```


## See also

+ [mrsdeploy function overview](../r-reference/mrsdeploy/mrsdeploy-package.md)
+ [How to publish and manage web services in R](how-to-deploy-web-service-publish-manage-in-r.md)
+ [Quickstart: Deploying an R model as a web service](quickstart-publish-r-web-service.md)
+ [Connecting to R Server from mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md).
+ [Get started guide for data scientists](concept-operationalize-deploy-consume.md)
+ [How to integrate web services and authentication into your application](how-to-build-api-clients-from-swagger-for-app-integration.md)
+ [Asynchronous batch execution of web services in R](how-to-consume-web-service-asynchronously-batch.md)
+ [Execute on a remote Microsoft R Server](../r/how-to-execute-code-remotely.md)
