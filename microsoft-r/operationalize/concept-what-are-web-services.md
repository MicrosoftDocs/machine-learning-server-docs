---

# required metadata
title: "What is a web service - Machine Learning Server "
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

# What are web services in Machine Learning Server?

**Applies to: Machine Learning Server**

In a nutshell, your R and Python code and models can be deployed as web services. Exposed as web services hosted in Machine Learning Server, these models and code can be accessed and consumed in R, Python, programmatically using REST APIs, or using Swagger generated client libraries. Web services can be deployed from one platform and consumed on another. They can be consumed synchronously, in realtime, or in batch mode.

Make it possible for users a chance to use your code and predictive models by deploying them as web services. Web services  facilitate the consumption and integration of the operationalized models and code they contain.  

Once you've built a predictive model, in many cases the next step is to operationalize the model. That is to generate predictions from the pre-trained model on demand. In this scenario, where new data often become available one row at a time, latency becomes the critical metric. It is important to respond with the single prediction (or score) as quickly as possible.

**Requirement!** Before you can deploy and work with web services, you must have access to a Machine Learning Server instance [configured to host web services](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization). 

There are two types of web services: standard and realtime. 

<a name="standard"></a>

## Standard web services

These web services offer fast execution and scoring of arbitrary Python or R code and models. They can contain code, models, and model assets. They can also take specific inputs and provide specific outputs for those users who are integrating the services inside their applications.

Standard web services, like all web services, are identified by their name and version. Additionally, they can also be defined by any Python or R code, models, and any necessary model assets. When deploying a standard web service, you should also define the required inputs and any output the application developers use to integrate the service in their applications.

**See a standard web service deployment example: [R](../operationalize/quickstart-publish-r-web-service.md)  |  [Python](../operationalize/python/quickstart-deploy-python-web-service.md)**

<a name="realtime"></a>

## Realtime web services

Realtime web services do not support arbitrary code and only accept models created with the supported functions from packages installed with the product. See the following sections for the list of supported functions by language and package.

Realtime web services offer even lower latency to produce results faster and score more models in parallel. The improved performance boost comes from the fact that these web services do not depend on an interpreter at consumption time even though the services use the objects created by the model. Therefore, fewer additional resources and less time is spent spinning up a session for each call. Additionally, the model is only loaded once in the compute node and can be scored multiple times.

For realtime services, you do **not** need to specify:
+ inputs and outputs (dataframes are assumed)
+ code (only serialized models are supported)

**See realtime web service deployment examples: [R](how-to-deploy-web-service-publish-manage-in-r.md#realtime-example)  |  [Python](python/how-to-deploy-manage-web-services.md#realtime-example)**

<a name=r></a>

### Supported R functions for realtime

A model object created with these supported functions:

|R package|Supported functions|
|-------------|--------------------|
|[RevoScaleR](../r-reference/revoscaler/revoscaler.md)|rxBTrees, rxDTree, rxDForest, rxLogit, rxLinMod|
|[MicrosoftML](../r-reference/microsoftml/microsoftml-package.md)|Machine learning and transform tasks:<br/>rxFastTrees, rxFastForest, rxLogisticRegression, rxOneClassSvm, rxNeuralNet, rxFastLinear, featurizeText, concat, categorical, categoricalHash, selectFeatures, featurizeImage, getSentiment, loadimage, resizeImage, extractPixels, selectColumns, and dropColumns<br><br>While mlTransform featurization is supported in realtime scoring, R transforms are not supported. Instead, use sp_execute_external_script.|
<a name="inputdf"></a>
There are additional restrictions on the input dataframe format for microsoftml models:

1. The dataframe must have the same number of columns as the formula specified for the model.

1. The dataframe must be in the exact same order as the formula specified for the model.
  
1. The columns must be of the same data type as the training data. Type casting is not possible.

<a name=python></a>

### Supported Python functions for realtime

|Python package|Supported functions|
|-------------|--------------------|
|[revoscalepy](../python-reference/revoscalepy/revoscalepy-package.md)|rx_btrees, rx_dforest, rx_dtree, rx_logit, rx_lin_mod|
|[microsoftml](../python-reference/microsoftml/microsoftml-package.md)|Machine learning and transform tasks:<br/>categorical, categorical_hash, concat, extract_pixels, featurize_text, featurize_image, get_sentiment, rx_fast_trees, rx_fast_forest, rx_fast_linear, rx_logistic_regression, rx_neural_network, rx_oneclass_svm, load_image, resize_image, select_columns, and drop_columns.<br/><br/>See the preceding [input dataframe format restrictions](#inputdf).|



<a name="versioning"></a>

## Versioning

Every time a web service is published, a version is assigned to the web service. Versioning enables users to better manage the release of their web services and helps the people consuming your service to find it easily. 

At publish time, specify an alphanumeric string that is meaningful to those users who consume the service. For example, you could use '2.0', 'v1.0.0', 'v1.0.0-alpha', or 'test-1'. Meaningful versions are helpful when you intend to share services with others. We highly a **consistent and meaningful versioning convention** across your organization or team such as semantic versioning. Learn more about semantic versioning here: http://semver.org/.

If you do not specify a version, a globally unique identifier (GUID) is automatically assigned. These GUID numbers are long making them harder to remember and use. 

<a name="consume"></a>

## Who consumes web services

After a web service has been published, authenticated users can consume that web service on various platforms and in various languages.  You can consume directly in R or Python, using APIs, or in your preferred language via Swagger.

You can make it easy for others to find your web services by providing them with the name and version of the web service.

+ **Data scientists** who want to explore and consume the services directly [in R](../operationalize/how-to-consume-web-service-interact-in-r.md#consume-service) and [in Python](../operationalize/python/how-to-consume-web-services.md#consume-service).

+ **Quality engineers** who want to bring the models in these web services into validation and monitoring cycles.

+ **Application developers** who want to call and integrate a web service into their applications. Developers can generate client libraries for integration using the Swagger-based JSON file generated during service deployment. Read "[How to integrate web services and authentication into your application](how-to-build-api-clients-from-swagger-for-app-integration.md)" for more details.  Services can also be consumed using the [RESTful APIs](concept-api.md) that provide direct programmatic access to a service's lifecycle.


## How are web services consumed

Web services can be consumed using one of these approaches:

|Approach|Description|
|---|---|
|Request Response|The service is consumed directly using a single synchronous consumption call.<br/>Learn how [in R](../operationalize/how-to-consume-web-service-interact-in-r.md) \| [in&nbsp;Python](../operationalize/python/how-to-consume-web-services.md)
|Asynchronous Batch|Users send a single asynchronous request to the server who in turn makes multiple service calls on their behalf.<br/>Learn how [in R](../operationalize/how-to-consume-web-service-asynchronously-batch.md)|



<a name=permissions></a>

## Permissions

By default, any authenticated Machine Learning Server user can:
+ Publish a new service
+ Update and delete web services they have published
+ Retrieve any web service object for consumption
+ Retrieve a list of any or all web services

Destructive tasks, such as deleting a web service, are available only to the user who initially created the service.  However, your administrator can also [assign role-based authorization](configure-roles.md) to further control the permissions around web services. When you list services, you can see your role for each one of them.

## See also

In R:
 + [Deploy and manage web services in R](../operationalize/how-to-deploy-web-service-publish-manage-in-r.md)     
 + [List, get, and consume web services in R](../operationalize/how-to-consume-web-service-interact-in-r.md)    

 + [Asynchronous web service consumption via batch](../operationalize/how-to-consume-web-service-asynchronously-batch.md)    

In Python:
 + [Deploy and manage web services in Python](../operationalize/python/how-to-deploy-manage-web-services.md)    

 + [List, get, and consume web services in Python](../operationalize/python/how-to-consume-web-services.md)    