---

# required metadata
title: "Consume web services in Python - Machine Learning Server "
description: "Publish and consume Python web services with Machine Learning Server"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# List, get, and consume web services in Python

**Applies to:  Machine Learning Server**

This article is for data scientists who wants to learn how to find, examine, and consume the [analytic web services](../concept-what-are-web-services.md) hosted in Machine Learning Server using Python. Web services offer fast execution and scoring of arbitrary Python or R code and models. [Learn more about web services](../concept-what-are-web-services.md). This article assumes that you are proficient in Python.

After a web service has been published, any authenticated user can list, examine, and consume that web service. You can do so directly in Python using the functions in the [azureml-model-management-sdk package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md). The azureml-model-management-sdk package is installed with Machine Learning Server.  To list, examine, or consume the web service _outside_ of Python, use the [RESTful APIs](../concept-api.md) that provide direct programmatic access to a service's lifecycle or in a [preferred language via Swagger](../how-to-build-api-clients-from-swagger-for-app-integration.md).

By default, web service operations are available to authenticated users. However, your administrator can also assign [roles](../configure-roles.md)  (RBAC) to further control the permissions around web services. 

>[!IMPORTANT]
>**Web service definition:** In Machine Learning Server, your R and Python code and models can be deployed as web services. Exposed as web services hosted in Machine Learning Server, these models and code can be accessed and consumed in R, Python, programmatically using REST APIs, or using Swagger generated client libraries. Web services can be deployed from one platform and consumed on another. [Learn more...](../concept-what-are-web-services.md) 

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

You can use [supported public functions to interact with service object](../../python-reference/azureml-model-management-sdk/service.md).

Example code:

```Python
# -- List all versions of the service 'myService'--
client.list_services('myService')

# -- Retrieve the service object for myService v2.0
svc = client.get_service('myService', version='v2.0')

# -- Learn more about that service.
print(help(svc))

# -- View the service object's capabilities/schema
svc.capabilities()
```

>[!Note]
>You can only see the code stored within a web service if you have published the web service or are assigned to the "Owner" role. To learn more [about roles](../configure-roles.md) in your organization, contact your Machine Learning Server administrator.

<a name="consume-service"></a>

## Consume web services 

Web services are published to facilitate the consumption and integration of the operationalized models and code into systems and applications. Whenever the web service is deployed or updated, a Swagger-based JSON file, which defines the service, is automatically generated.

When you publish a service, you can let people know that it is ready for consumption by sharing its name and version with them. Web services are [consumed by data scientists, quality engineers, and application developer](../concept-what-are-web-services.md#consume). They can be consumed in Python, R, or via the API. 

Users can consume the service directly using a single consumption call, which is referred to as a "Request Response" approach. [Learn about the various approaches to consuming web services](../concept-what-are-web-services.md#consume).

### Consuming in Python

[After authenticating with Machine Learning Server](how-to-authenticate-in-python.md), users can also interact with and consume services using other functions in the [azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) Python package.

**[For a full example of a request-response consume, see this Jupyter notebook](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/operationalize/Explore_Consume_Python_Web_Services.ipynb)**

```Python
# Let's call the function `manualTransmission` in this service
res = svc.manualTransmission(120, 2.8)

# Pluck out the named output `answer`.
print(res.output('answer'))

# Get `swagger.json` that defines the service.
cap = svc.capabilities()
swagger_URL = cap['swagger']
print(swagger_URL)

# Print the contents of the swagger file.
print(svc.swagger())
```

### Sharing the Swagger with application developers

Application developers can call and integrate a web service into their applications using the service-specific Swagger-based JSON file and by providing any required inputs to that service. 
The Swagger-based JSON file is used to generate client libraries for integration. Read "[How to integrate web services and authentication into your application](../how-to-build-api-clients-from-swagger-for-app-integration.md)" for more details.  
   
The easiest way to share the Swagger file with an application developer is to use the code shown [in this Jupyter notebook](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/operationalize/Explore_Consume_Python_Web_Services.ipynb). Alternately, the application developer can request the file as an authenticated user with an [active bearer token](../how-to-build-api-clients-from-swagger-for-app-integration.md#authentication) in the request header using this API:
```
GET /api/{{service-name}}/{{service-version}}/swagger.json
```

## See also

+ [What are web services?](../concept-what-are-web-services.md)

+ [Jupyter notebook: how explore and consume a web service](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/operationalize/Explore_Consume_Python_Web_Services.ipynb)

+ [Package overview: azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md)

+ [Quickstart: Deploying an Python model as a web service](quickstart-deploy-python-web-service.md)

+ [How to authenticate with Machine Learning Server in Python](how-to-authenticate-in-python.md).

+ [How to publish and manage web services in Python](how-to-deploy-manage-web-services.md)

+ [How to integrate web services and authentication into your application](../how-to-build-api-clients-from-swagger-for-app-integration.md)
