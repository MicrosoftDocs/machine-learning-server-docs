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

# Deploy and delete web services in Python 

**Applies to: Machine Learning Server**

This article is for data scientists who wants to learn how to deploy and manage Python code/models as web services hosted in Machine Learning Server. This article assumes you are proficient in Python.

Using the [azureml-model-management-sdk Python package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md), which ships with Machine Learning Server, you can develop, test, and ultimately [deploy](#publishService) these Python analytics as web services in your production environment. This package can also be [installed locally](../../install/python-libraries-interpreter.md), but requires a connection to a Machine Learning Server instance at runtime.

These web services can be [consumed in Python](how-to-consume-web-service.md) by other authenticated users or in the [language of their choice via Swagger](../how-to-build-api-clients-from-swagger-for-app-integration.md).  You can also deploy or interact with a web service outside of R using the [RESTful APIs](../concept-api.md), which provide direct programmatic access to a service's lifecycle.

<a name="auth"></a>

## Requirements

Before you can use the web service management functions in the [azureml-model-management-sdk Python package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md), you must:
+ Have access to a Python-enabled instance of Machine Learning Server that was  [properly configured](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) to host web services. 

+ Authenticate with Machine Learning Server in Python as described in "[Connecting to Machine Learning Server](how-to-authenticate-in-python.md)."

## Standard vs realtime services

<a name="standard"></a>

### Standard web services

These web services offer fast execution and scoring of arbitrary Python or R code and models. They can contain code, models, and model assets. They can also take specific inputs and provide specific outputs for those users who are integrating the services inside their applications.

Standard web services, like all web services, are identified by their name and version. Additionally, a standard web service is also defined by any Python or R code, models, and any necessary model assets. When deploying a standard web service, you should also define the required inputs and any output the application developers use to integrate the service in their applications.

While R code can come in several forms, the code and models for Python services comes in the form of a function handle. 

A full code example for deploying Python web services can be [found in this Quickstart](quickstart-deploy-python-web-service.md). A snippet of the deploy function can be [seen here](#deploy-example).

<a name="realtime"></a>

### Realtime web services

Once you've built a predictive model, in many cases the next step is to operationalize the model. That is to generate predictions from the pre-trained model in real time. In this scenario, where new data often become available one row at a time, latency becomes the critical metric. It is important to respond with the single prediction (or score) as quickly as possible.

Realtime web services offer even lower latency and better load to produce results faster and score more models in parallel. The improved performance boost comes from the fact that these web services do not rely on an interpreter at consumption time even though the services use the objects created by the model. Therefore, no additional resources or time is spent spinning up a session for each call. Additionally, since the model is cached in memory, it is only loaded once. 

Additionally, you do not need to specify inputs or outputs for realtime web services. Data.frame inputs and outputs are assumed automatically.

In Python, this type of web takes only models created with **supported functions** **and does not support arbitrary code.

The following functions are supported in a Python realtime service: 
+ These [revoscalepy package](../../python-reference/revoscalepy/revoscalepy-package.md) functions: rx_logit, rx_lin_mod, rx_btrees, rx_dtree, and rx_dforest 

+ These [MicrosoftML package](../../python-reference/microsoftml/microsoftml-package.md) functions for machine learning and transform tasks: rx_fast_trees, rx_fast_forest, rx_logistic_regression, rx_oneclass_svm, rx_neural_network, rx_fast_linear, featurize_text, concat, categorical, categorical_hash, featurize_image, get_sentiment, load_image, resize_image, extract_pixels, select_columns, and drop_columns.

A code example for deploying realtime services can be [found later in this article](#deploy-example). 


While R models are automatically serialized for realtime services, in Python you must manually serialize the model before deploying a realtime service. Use the [rx_serialize_model function](../../python-reference/revoscalepy/rx-serialize-model.md) from the [revoscalepy package](../../python-reference/revoscalepy/revoscalepy-package.md) installed with Machine Learning Server. Other serialization functions are not supported.

More details about **R realtime services** and the supported functions in R are covered in [this article](../how-to-deploy-web-service-publish-manage-in-r.md#realtime).


<a name="publishService"></a>

## Deploy web services

To deploy your analytics, you must publish them as web services in Machine Learning Server. Once hosted on Machine Learning Server, you can update and manage them. They can also be consumed by other users. Both deploy and publish are used synonymously.

After you've authenticated, you can publish as a web service. Publishing returns a [service object](../../python-reference/azureml-model-management-sdk/service.md) containing the client stub for consuming that service.

<a name="deploy-example"></a>

**Example: publish a standard service**

```Python
# Publish a standard service called 'cars_model'
# Give it a version of '1.0'
# Assign the service to 'service' variable
service = client.service(cars_model)\
        .version("1.0")\
        .code_fn(manualTransmission, init)\
        .inputs(hp=float, wt=float)\
        .outputs(answer=pd.DataFrame)\
        .models(cars_model=cars_model)\
        .description('My first python model')\
        .artifacts(['answer.csv', 'image.png'])\
        .deploy()
```

**Example: publish a realtime service**

```Python
# Publish a realtime service 'kyphosisService' version 'v1.0'
# Assign service to 'realtimeApi' variable

# serialize the model using revoscalepy.rx_serialize_model
from revoscalepy import rx_serialize_model
s_model = rx_serialize_model(model, realtime_scoring_only=True)

# To publish the realtime service, 
# we first initiate a 'realtimeDefinition' object.
# Then keep annotate the object with other parameters
webserv = client.realtime_service(linear_model) \
        .version('1.0') \
        .serialized_model(s_model) \
        .description("this is a realtime model") \
        .deploy()
```

Learn how to get a [list of all services](how-to-consume-web-services.md), retrieve a [web service object](how-to-consume-web-services.md) for consumption, and [share services](how-to-consume-web-services.md) with others.

### Publishing new versions

You can keep different versions of a web service. Update the version each time you use '.deploy'. [Learn more about versioning](#versioning).

<a name="data-types"></a>

### Input and output data types

The following table lists [the supported data types](../../python-reference/azureml-model-management-sdk/mlserver.md#deployservice) for the input and output schemas of Python web services.

|I/O data types|Write as|
|--------|-----|
|numeric|float|
|integer|int|
|logical|bool|
|character or vector|str|
|array|numpy.array |
|matrix|numpy.matrix |
|data.frame|numpy.DataFrame|

<a name="updateService"></a>

## Update web services

To change a web service after you've published it without changing its name or version, use '.redeploy' instead of '.deploy' for the service object. Then, specify the changes, such as the code, model, description, inputs, or outputs. When you update a service, it overwrites that named version and returns a [service object](../../python-reference/azureml-model-management-sdk/service.md) containing the client stub for consuming that service.

In this example, we update the service to add a description useful to people who might consume this service and use a new code function, 'manualTransmission2'.

```Python
# Redeploy this standard service 'car_model' version '1.0'
# Using a new function for the model and updated description
service = client.service(cars_model)\
        .version("1.0")\
        .code_fn(manualTransmission2, init)\
        .inputs(hp=float, wt=float)\
        .outputs(answer=pd.DataFrame)\
        .models(cars_model=cars_model)\
        .description('Predict transmission type (automatic or manual)')\
        .artifacts(['answer.csv', 'image.png'])\
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
# -- Now delete cars_model v1.0
client.delete_service(cars_model, version = "1.0")
```

If it is successful, the success status  _"True"_  is returned. If it fails, then execution stops and an error message is produced.


## Permissions on web services

By default, any authenticated Machine Learning Server user can:
+ Update and delete web services they have published
+ Retrieve any web service object for consumption
+ Retrieve a list of any or all web services

By default, all web service operations are available to authenticated users. Destructive tasks, such as deleting a web service, are available only to the user who initially created the service.  However, your administrator can also [assign role-based authorization](../configure-roles.md) to further control the permissions around web services. Ask your administrator for details on your role.

<a name="versioning"></a>

## Version your services

Every time a web service is published, a version is assigned to the web service. Versioning enables users to better manage the release of their web services and helps the people consuming your service to find it easily. 

At publish time, you can specify an alphanumeric string that is meaningful to those who consume the service. For example, you could use '2.0', 'v1.0.0', 'v1.0.0-alpha', or 'test-1'. Meaningful versions are helpful when you intend to share services with others. We highly a **consistent and meaningful versioning convention** across your organization or team such as semantic versioning. Learn more about semantic versioning here: http://semver.org/.

If you do not specify a version, a globally unique identifier (GUID) is automatically assigned. These GUID numbers are long making them harder to remember and use. 


## Use session snapshots

Create a snapshot of a Python session to store its environment within a web service so it can be reproduced at consume time. Session snapshots are useful when you need environment preconfigured with certain libraries, objects, models, files, and artifacts. Snapshots save the whole workspace and working directory. 

When publishing a service, you can only use a session snapshot that you've created. No one else can use your snapshots to publish a service.

While session snapshots can also be used when publishing a web service for environment dependencies, it may have an impact on the performance of the consumption time.  For optimal performance, consider the size of the snapshot carefully and ensure that you keep only those workspace objects you need and purge the rest. In a session, you can use the Python 'del' function or [the `deleteWorkspaceObject` API request](https://microsoft.github.io/deployr-api-docs/#delete-workspace-object) to remove unnecessary objects. 

```python
#Create a snapshot of the current session.
response = client.create_snapshot(session_id, deployrclient.models.CreateSnapshotRequest("Iris Snapshot"), headers)

#Return the snapshot ID for reference when you publish later.
response.snapshot_id

#If you forget the ID, list every snapshot to get the ID again.
for snapshot in client.list_snapshots(headers):
    print(snapshot)
```