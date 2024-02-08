---

# required metadata
title: "Deploy, update, & delete Python web services - Machine Learning Server "
description: "Publish, update, and delete Python web services with Machine Learning Server and Microsoft R Server"
keywords: 
author: "chuckheinzelman"
ms.author: "charlhe"
manager: "cgronlun"
ms.date: 2/16/2018
ms.topic: "how-to"
ms.service: mlserver

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

# Deploy and manage web services in Python 

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

**Applies to: Machine Learning Server**

This article is for data scientists who wants to learn how to deploy and manage Python code/models as [analytic web services](../concept-what-are-web-services.md) hosted in Machine Learning Server. This article assumes you are proficient in Python.

Using the [azureml-model-management-sdk](/sql/machine-learning/python/reference/azureml-model-management-sdk/azureml-model-management-sdk)  Python package, which ships with Machine Learning Server, you can develop, test, and [deploy these Python analytics](#publishService) as web services in your production environment. This package can also be [installed locally on a Windows machine](../../install/python-libraries-interpreter.md), but requires a connection to a Machine Learning Server instance at runtime. [RESTful APIs](../concept-api.md) are also available to provide direct programmatic access to a service's lifecycle.

By default, web service operations are available to authenticated users. However, your administrator can also [assign role-based authorization (RBAC)](../configure-roles.md) to further control the permissions around web services. 

>[!IMPORTANT]
>**Web service definition:** In Machine Learning Server, your R and Python code and models can be deployed as web services. Exposed as web services hosted in Machine Learning Server, these models and code can be accessed and consumed in R, Python, programmatically using REST APIs, or using Swagger generated client libraries. Web services can be deployed from one platform and consumed on another. [Learn more...](../concept-what-are-web-services.md)

## Overview

To deploy your analytics, you must publish them as web services in Machine Learning Server. Web services offer fast execution and scoring of arbitrary Python or R code and models. [Learn more about web services](../concept-what-are-web-services.md). 

When you deploy Python code or a model as a web service, a [service object](/sql/machine-learning/python/reference/azureml-model-management-sdk/service) containing the client stub for consuming that service is returned.

Once hosted on Machine Learning Server, you can update and manage them.  Authenticated users can  [consume web services in Python](how-to-consume-web-services.md) or in a [preferred language via Swagger](../how-to-build-api-clients-from-swagger-for-app-integration.md).

<a name="auth"></a>

## Requirements

Before you can use the web service management functions in the [azureml-model-management-sdk](/sql/machine-learning/python/reference/azureml-model-management-sdk/azureml-model-management-sdk) Python package, you must:
+ Have access to a Python-enabled instance of Machine Learning Server that was  [properly configured](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) to host web services. 

+ Authenticate with Machine Learning Server in Python as described in "[Connecting to Machine Learning Server](how-to-authenticate-in-python.md)."


<a name="publishService"></a>

## Web service definitions

The list of available parameters that can be defined for a web service depends on whether it is a standard or real-time web service. 

### Parameters for standard web services

Standard web services, like all web services, are identified by their name and version. Additional parameters can be defined for the service, including:
+ A description
+ Arbitrary Python code as a function or a string, such as `code_fn=(run, init)` or `code_str=('run', 'init')`
+ Models
+ Any model assets
+ Inputs
+ Outputs for integrators
+ Artifacts

For a full example, try out the [quickstart guide for deploying web services in Python](quickstart-deploy-python-web-service.md).

This table presents the supported data types for the input and output schemas of Python web services.  Write these as a reference `bool` or as a string `'bool'`.

|I/O data types| Description |
|--------|-----|
|Float &rarr; float|Array &rarr; numpy.array |
|Integer &rarr; int|Matrix &rarr; numpy.matrix |
|Boolean &rarr; bool&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Dataframe &rarr; numpy.DataFrame|
|String &rarr; str||

### Parameters for real-time web services

[Real-time web services](../concept-what-are-web-services.md#realtime) are also identified by their name and version. The following additional parameters can be defined for the real-time service:
+ A description
+ A model created with certain [supported functions](../concept-what-are-web-services.md#realtime) and serialized with [revoscalepy.rx_serialize_model](../../python-reference/revoscalepy/rx-serialize-model.md)

Dataframes are assumed for inputs and outputs. Code is not supported.

For a full example of real-time web services in Python, try out [this Jupyter notebook](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/operationalize/Publish_Realtime_Web_Service_in_Python.ipynb).

## Deploy and update services

### Publish a service

To publish a service, specify the name, version, and any other needed parameters. Be sure to end with '.deploy'.

Here is a standard web service example:
```Python
service = client.service('myService')\
        .version('v1.0')\
        .description('cars model')\
        .code_fn(manualTransmission, init)\
        .inputs(hp=float, wt=float)\
        {{...}}
        .deploy()
```

Here is a real-time web service example:

<a name="realtime-example"></a>

```Python
# Serialize the model using revoscalepy.rx_serialize_model
from revoscalepy import rx_serialize_model
s_model = rx_serialize_model(model, realtime_scoring_only=True)

# Publish the real-time service 
webserv = client.realtime_service('myService') \
        .version('1.0') \
        .serialized_model(s_model) \
        .description('this is a realtime model') \
        .deploy()
```

For a full example of real-time web services in Python, try out [this Jupyter notebook](https://github.com/Microsoft/ML-Server-Python-Samples/blob/master/operationalize/Publish_Realtime_Web_Service_in_Python.ipynb).

### Update an existing service version

To update an existing web service, specify the name and version of that service and the parameters that need changed. When you use '.redeploy', the service version is overwritten and a new [service object](/sql/machine-learning/python/reference/azureml-model-management-sdk/service) containing the client stub for consuming that service is returned.

Update the service with a '.redeploy' such as:
```Python
# Redeploy this standard service 'myService' version 'v1.0'
# Only the description has changed.
service = client.service('myService')\
        .version('v1.0')\
        .description('The updated description for cars model')\
        .redeploy()
```

### Publish a new service version

You can deploy different versions of a web service.  To publish a [new version of the service](../concept-what-are-web-services.md#versioning), specify the same name, a new version, and end with '.deploy', such as:

```Python
service = client.service('myService')\
        .version('v2.0')\
        .description('cars model')\
        .code_fn(manualTransmission, init)\    
        .inputs(hp=float, wt=float)\
        {{...}}
        .deploy()
```

<a name="deleteService"></a>

## Delete web services

When you no longer want to keep a web service that you have published, you can delete it.  If you are [assigned to "Owner" role](../configure-roles.md), you can also delete the services of others.

You can call 'delete_service' on the 'DeployClient' object to delete a specific service on Machine Learning Server.

```Python
# -- List all services to find the one to delete--
client.list_services()
# -- Now delete myService v1.0
client.delete_service('myService', version='v1.0')
```

If successful, the status  _'True'_  is returned. If it fails, then the execution stops and an error message is returned.

## See also

[What are web services?](../concept-what-are-web-services.md)

[Quickstart: Deploying web services in Python](quickstart-deploy-python-web-service.md).

[How to authenticate in Python](how-to-authenticate-in-python.md)

[How to find, get, and consume web services](how-to-consume-web-services.md)

[How to consume web services in Python synchronously (request/response)](how-to-consume-web-services.md)    
 
[How to consume web services in Python asynchronously (batch)](how-to-consume-web-services-async.md)    
 