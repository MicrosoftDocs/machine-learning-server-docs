---

# required metadata
title: "Consume web services in Python - Machine Learning Server | Microsoft Docs"
description: "Publish and consume Python web services with Machine Learning Server"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "9/25/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""
---

# List, get, and consume web services in Python

**Applies to:  Machine Learning Server**

This article is for data scientists who wants to learn how to find, examine, and consume the [analytic web services](../concept-what-are-web-services.md) hosted in Machine Learning Server using Python. Web services offer fast execution and scoring of arbitrary Python or R code and models. [Learn more about web services](../concept-what-are-web-services.md). This article assumes that you are proficient in Python.

After a web service has been published, any authenticated user can list, examine, and consume that web service. You can do so directly in Python using the functions in the [azureml-model-management-sdk package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) or in a [preferred language via Swagger](../how-to-build-api-clients-from-swagger-for-app-integration.md). The azureml-model-management-sdk package is installed with Machine Learning Server.  To list, examine, or consume the web service _outside_ of Python, use the [RESTful APIs](../concept-api.md) that provide direct programmatic access to a service's lifecycle.

By default, web service operations are available to authenticated users. However, your administrator can also assign [roles](../configure-roles.md)  (RBAC) to further control the permissions around web services. 

<a name="auth"></a>

## Requirements

Before you can use the web service management functions in the [azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) Python package, you must:
+ Have access to a Python-enabled instance of Machine Learning Server that was  [properly configured](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) to host web services. 

+ Authenticate with Machine Learning Server in Python as described in "[Connecting to Machine Learning Server in Python](how-to-authenticate-in-python.md)."

<a name="list_services"></a>

## Find and list web services

Any authenticated user can retrieve a list of web services using the `list_services` function on the DeployClient object. You can use arguments to return a specific web service or all labeled versions of a given web service. 
 
```Python
## -- Return metadata for all services hosted on this server
client.list_services()

## -- Return metadata for all versions of service "myService" 
client.list_services('myService')

## -- Return metadata for a specific version "v1.0" of service "myService" 
client.list_services('myService', version='v1.0')
```


Once you find the service you want, use the [get_service](#get_service)  function to retrieve the service object for consumption.

<a name="get_service"></a>

## Retrieve and examine service objects

Authenticated users can retrieve the [web service object](../../python-reference/azureml-model-management-sdk/service.md) in order to get the client stub for consuming that service. Use the `get_service` function from the azureml-model-management-sdk package to retrieve the object. 

After the object is returned, you can use a help function to explore the published service, such as `print(help(myServiceObject))`. You can call the help function on any azureml-model-management-sdk functions, even those that are dynamically generated to learn more about them. 

You can also print the capabilities that define the service holdings to see what the service can do and how it should be consumed. Service holdings include the service name, version, descriptions, inputs, outputs, and the name of the function to be consumed. [Learn more about capabilities...](../../python-reference/azureml-model-management-sdk/service.md#capabilities)

You can only see the code stored within a web service if you have published the web service or are assigned to the "Owner" role. To learn more [about roles](../configure-roles.md) in your organization, contact your Machine Learning Server administrator.

You can use [supported public functions to interact with service object](../../python-reference/azureml-model-management-sdk/service.md).

Example code:

```Python
# -- List all versions of the service 'myService'--
client.list_services('myService')

# -- Now get the service object for myService v2.0
svc = client.get_service('myService', version='v2.0')
# -- Learn more about that service.
print(help(svc))
# -- View the service capabilities/schema for this service
svc.capabilities()
```

<a name="consume-service"></a>

## Consume web services 

Web services are published to facilitate the consumption and integration of the operationalized models and code.  Whenever the web service is published or updated, a Swagger-based JSON file is generated automatically that define the service.

When you publish a service, you can let people know that it is ready for consumption. Users can get the Swagger file they need to consume the service directly in R or via the API. To make it easy for others to find your service, provide them with the service name and version number (or they can use [the list_services function](#list_services)).

Users can consume the service directly using a single consumption call. This approach is referred to as a "Request Response" approach and is described in the following section. Another approach is the [asynchronous "Batch" consumption approach](how-to-consume-web-service-asynchronously-batch.md), where users send as a single request to Machine Learning Server, which then makes multiple asynchronous API calls on your behalf.
  
<a name="data-scientists-share"></a>

#### Collaborate with data scientists

Other data scientists may want to explore, test, and consume Web services directly in R using the functions in the mrsdeploy package. Quality engineers might want to bring the models in these web services into validation and monitoring cycles.

You can share the name and version of a web service with fellow data scientists so they can call that service in R using the functions in the mrsdeploy package.  After authenticating, data scientists can use the getService() function in R to call the service. Then, they can get details about the service and start consuming it.

You can also [build a client library directly in R using the httr package](https://blogs.msdn.microsoft.com/rserver/2017/07/20/using-r-to-generate-api-client-from-swagger/).

>[!NOTE]
> It is also possible to perform batch consumption as [described here](how-to-consume-web-service-asynchronously-batch.md).


In this example, replace the following remoteLogin() function with the correct login details for your configuration. Connecting to Machine Learning Server using the mrsdeploy package is covered [in this article](how-to-connect-log-in-with-mrsdeploy.md).

```R
##########################################################################
#      Perform Request-Response Consumption & Get Swagger Back in R      #
##########################################################################

# Use `remoteLogin` to authenticate with the server using the local admin 
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

Using the Swagger-based JSON file, application developers can generate client libraries for integration. Read "[How to integrate web services and authentication into your application](../how-to-build-api-clients-from-swagger-for-app-integration.md)" for more details.  
   
Application developers can get the Swagger-based JSON file in one of these ways:

+ A data scientist, probably the one who published the service, can send you the Swagger-based JSON file. This approach is often faster than the following one. They can get the file in R with the following code and send it to the application developer:
   ```R
   api <- getService("<name>", "<version>")
   swagger <- api$swagger()
   cat(swagger, file = "swagger.json", append = FALSE) 
   ```

+ Or, the application developer can request the file  **as an authenticated user with an [active bearer token](../how-to-build-api-clients-from-swagger-for-app-integration.md#authentication) in the request header** (since all API calls must be authenticated). The URL is formed as follows:
  ```
  GET /api/<service-name>/<service-version>/swagger.json
  ```

## See also

+ [What are web services?](../concept-what-are-web-services.md)
+ [Package overview: azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md)
+ [Quickstart: Deploying an Python model as a web service](quickstart-deploy-python-web-service.md)
+ [How to authenticate with Machine Learning Server in Python](how-to-authenticate-in-python.md).
+ [How to publish and manage web services in Python](how-to-deploy-manage-web-services.md)
+ [How to integrate web services and authentication into your application](../how-to-build-api-clients-from-swagger-for-app-integration.md)
