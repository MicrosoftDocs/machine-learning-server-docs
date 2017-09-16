---

# required metadata
title: "Publish, deploy, update, and delete Python web services - Machine Learning Server | Microsoft Docs"
description: "Publish, update, and delete Python web services with Microsoft R Server"
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

# Deploy and manage web services in Python 

**Applies to: Machine Learning Server**

This article is for data scientists who wants to learn how to deploy and manage Python code/models as analytic web services hosted in Machine Learning Server. This article assumes you are proficient in Python.

Web services offer fast execution and scoring of arbitrary Python or R code and models. [Learn more about web services](../concept-what-are-web-services.md).

Using the [azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md)  Python package, which ships with Machine Learning Server, you can develop, test, and [deploy these Python analytics](#publishService) as web services in your production environment. This package can also be [installed locally](../../install/python-libraries-interpreter.md), but requires a connection to a Machine Learning Server instance at runtime. [RESTful APIs](../concept-api.md) are also available to provide direct programmatic access to a service's lifecycle.

By default, web service operations are available to authenticated users. However, your administrator can also [assign role-based authorization (RBAC)](../configure-roles.md) to further control the permissions around web services. 



<a name="auth"></a>

## Requirements

Before you can use the web service management functions in the [azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) Python package, you must:
+ Have access to a Python-enabled instance of Machine Learning Server that was  [properly configured](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) to host web services. 

+ Authenticate with Machine Learning Server in Python as described in "[Connecting to Machine Learning Server](how-to-authenticate-in-python.md)."


<a name="publishService"></a>

## Deploy web services

To deploy your analytics, you must publish them as web services in Machine Learning Server. Once hosted on Machine Learning Server, you can update and manage them.  Web services can be [consumed in Python](how-to-consume-web-service.md) by other authenticated users or in the [language of their choice via Swagger](../how-to-build-api-clients-from-swagger-for-app-integration.md). Both deploy and publish are used synonymously.

After authenticating, you can deploy as a web service. A [service object](../../python-reference/azureml-model-management-sdk/service.md) containing the client stub for consuming that service is returned.

### Standard web services

Standard web services, like all web services, are identified by their name and version. Additionally, a standard web service is also defined by any code, models, and any necessary model assets. When deploying, you should also define the required inputs and any output the application developers use to integrate the service in their applications. Additional parameters are possible -- many of which are shown in the following example.

In Python, you can define your code in the form of a function or a string, such as
`code_fn=(run, init)` or `code_str=('run', 'init')`.

You can define an 'init' function to be bootstrapped to the web service. This 'init' function handles service initialization. Use it to load the packages, datasets, and global variables you need when the service is called the first time. Then, benefit from the performance enhancement since the server no longer needs to reloading these each time it is run. One caveat, however, is that all imports are scoped to the 'init' function and not to the global namespace. Consequently, you must still import the modules in each run or consume function. 

<a name="deploy-example"></a>
Example: 

```Python
# Publish a standard service called 'TxService'
# Give it a version of '1.0'
# Assign the service to 'service' variable
service = client.service('TxService')\
        .version('1.0')\
        .code_fn(manualTransmission, init)\
        .inputs(hp=float, wt=float)\
        .outputs(answer=pd.DataFrame)\
        .models(cars_model=cars_model)\
        .description('My first python model')\
        .deploy()
```

For a full example, try out the [quickstart for deploying web services](quickstart-deploy-python-web-service.md).

<a name=realtime-example></a>

### Realtime web services

Realtime web services offer even lower latency to produce results faster and score more models in parallel. [Learn more...](../concept-what-are-web-services.md)

Realtime web services are also identified by their name and version. However, unlike standard web services, you **cannot** specify the following for realtime web services:
+ inputs and outputs (dataframes are assumed)
+ code (only serialized models are supported)

Realtime web services only accept models created with [the supported functions from packages installed with the product](../concept-what-are-web-services.md#realtime). 

In Python, you must manually serialize the model before deploying a realtime service. Use the [rx_serialize_model function](../../python-reference/revoscalepy/rx-serialize-model.md) from the [revoscalepy package](../../python-reference/revoscalepy/revoscalepy-package.md) installed with Machine Learning Server. Other serialization functions are not supported.

Example: 

```Python
# serialize the model using revoscalepy.rx_serialize_model
from revoscalepy import rx_serialize_model
s_model = rx_serialize_model(model, realtime_scoring_only=True)

# To publish the realtime service, 
# initiate a 'realtimeDefinition' object.
# Then annotate the object with other parameters
webserv = client.realtime_service(linear_model) \
        .version('1.0') \
        .serialized_model(s_model) \
        .description('this is a realtime model') \
        .deploy()
```

Learn how to get a [list of all services](how-to-consume-web-services.md), retrieve a [web service object](how-to-consume-web-services.md) for consumption, and [share services](how-to-consume-web-services.md) with others.

### Publishing new versions

You can keep different versions of a web service. Update the version each time you use '.deploy'. [Learn more about versioning](#versioning).

<a name="data-types"></a>

### Input and output data types

The following table lists the supported data types for the input and output schemas of Python web services.  You can write these as a reference `bool` or as a string `'bool'`.

|I/O data types||
|--------|-----|
|Float &rarr; float|Array &rarr; numpy.array |
|Integer &rarr; int|Matrix &rarr; numpy.matrix |
|Boolean &rarr; bool&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|Dataframe &rarr; numpy.DataFrame|
|String &rarr; str||

<a name="updateService"></a>

## Update web services

To update an existing web service without changing its name or version, specify the following parameters:
+ The name and version to identify the service to be updated.
+ The parameters that need changed, such as the code, model, description, inputs, or outputs.
+ '.redeploy' instead of '.deploy'  

When you update a service, it overwrites that named version and returns a [service object](../../python-reference/azureml-model-management-sdk/service.md) containing the client stub for consuming that service.

In this example, we update the service with a new description. 

```Python
# Redeploy this standard service 'TxService' version '1.0'
# Update only what changed. Here it is the description.
service = client.service('TxService')\
        .version('1.0')\
        .description('The updated description')\
        .redeploy()
```

>[!NOTE]
>To change either the name or the version of a web service, use '.deploy' instead of '.redeploy' along with the new name or version. 

<a name="deleteService"></a>

## Delete web services

When you no longer want to keep a web service that you have published, you can delete it. You can also delete the services of others if you are [assigned to a role](../configure-roles.md) with those permissions. 

You can call 'delete_service' on the 'DeployClient' object to delete a specific service on Machine Learning Server.

```Python
# -- List all services to find the one to delete--
client.list_services()
# -- Now delete TxService 1.0
client.delete_service('TxService', version='1.0')
```

If it is successful, the success status  _'True'_  is returned. If it fails, then execution stops and an error message is produced.

## See also

[What are web services](../concept-what-are-web-services.md)

[Authenticate in Python](how-to-authenticate-in-python.md)

[Find and consume web services](how-to-consume-web-services.md)