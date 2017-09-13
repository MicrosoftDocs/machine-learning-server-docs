---

# required metadata
title: "Quickstart for deploying Python models as web services - Machine Learning Server | Microsoft Docs"
description: "How to deploy an R model as a service"
keywords: "quickstart, Machine Learning Server, deploy python models"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "9/25/2017"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: "r"
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""

---
# Deploy a Python model as a web service with azureml-model-management-sdk

**Applies to: Microsoft Learning Server 9.2**

## Objective

Learn how to deploy an Python model as a web service with Machine Learning Server. Data scientists work locally in their preferred Python IDE and favorite version control tools to build scripts and models. Using the [azureml-model-management-sdk Python package](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) that ships with Machine Learning Server, you can develop, test, and ultimately deploy these Python analytics as web services in your production environment. 

In Machine Learning Server, a web service is a Python code execution on the [compute node](../configure-start-for-administrators.md#configure-server-for-operationalization). Each web service is uniquely defined by a `name` and `version`. You can use the functions in [the azureml-model-management-sdk Python library ](../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) to gain access a service's lifecycle from a Python script. A set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/#services-management-apis) are also available to provide direct programmatic access to a service's lifecycle directly. 

## Time estimate

If you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have the following ready:

+ An instance of [Anaconda and the azureml-model-management-sdk package installed](../../install/python-libraries-interpreter.md) on your local machine.    

+ An instance of [Machine Learning Server ](../../what-is-machine-learning-server.md) installed that has been [configured to operationalize analytics](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization).

+ The connection details to that instance of Machine Learning Server. Contact your administrator for any missing connection details. After [connecting to Machine Learning Server](../../operationalize/python/how-to-authenticate-in-python.md) in Python, deploy your analytics as web services so others can consume them. 


## Example code

The example for this quickstart is stored in a Jupyter notebook. This notebook format allows you to not only see the code alongside detailed explanations, but also allows you to try out the code.

This example walks through the deployment of a Python model as a web service hosted in Machine Learning Server. We will build a simple linear model using the [rx_lin_mod](../../python-reference/revoscalepy/rx-lin-mod.md) function from the [revoscalepy package](../../python-reference/revoscalepy/revoscalepy-package.md) installed with Machine Learning Server or [locally](../../install/python-libraries-interpreter.md). This package requires a connection to Machine Learning Server.  

>[!IMPORTANT]
>Be sure to replace with the correct login details for your configuration. Connecting to Machine Learning Server using the azureml-model-management-sdk library is covered [in this article](../../operationalize/python/how-to-authenticate-in-python.md).

[**Download the Jupyter notebook**]().


## A. Model locally
Now let's dive into this example down. Let's start by creating the model locally, then publish it, and then share it with other authenticated users.

1. Launch your R IDE or Rgui so you can start entering R code. 

1. If you have R Server 9.0.1, load the mrsdeploy package. In R Server 9.1, this package is preloaded for you.

   ```R
   library(mrsdeploy)
   ```

1. Create a GLM model called `carsModel` using the dataset `mtcars`, which is a built-in data frame in R. Using horsepower (hp) and weight (wt), this model estimates the probability that a vehicle has been fitted with a manual transmission.

   ```R
   # Create glm model with `mtcars` dataset
   carsModel <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)

   # Produce a prediction function that can use the model
   manualTransmission <- function(hp, wt) {
     newdata <- data.frame(hp = hp, wt = wt)
     predict(carsModel, newdata, type = "response")
   }
   
   # test function locally by printing results
   print(manualTransmission(120, 2.8)) # 0.6418125
   ```

1. Examine the results of the locally executed code. You can compare these results to the results of the web service later.

<br> 

## B. Publish model as a web service

1. From your local R IDE, log in to Microsoft R Server **with your credentials**. Use the appropriate authentication function from [the mrsdeploy package](how-to-connect-log-in-with-mrsdeploy.md) (`remoteLogin` or `remoteLoginAAD`) for your authentication method.  

   For simplicity, the following code uses the basic local 'admin' account for authentication with the `remoteLogin` function and `session = false` so that no remote R session is started.  Learn more about authenticating with Active Directory LDAP or Azure Active Directory in the article "[Connecting to R Server from mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md)."

   >[!IMPORTANT]
   >Be sure to replace the remoteLogin() function with the [correct login details for your configuration](how-to-connect-log-in-with-mrsdeploy.md).

   ```R  
   # Use `remoteLogin` to authenticate with R Server using 
   # the local admin account. Use session = false so no 
   # remote R session started
   remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)
   ``` 

   Now, you are successfully connected to the remote R Server.

1. Publish the model as a web service to R Server using [the publishService() function](how-to-deploy-web-service-publish-manage-in-r.md) from the mrsdeploy package. 

   In this example, you publish a web service called `"mtService"` using the model `carsModel` and the function `manualTransmission`. As an input, the service takes a list of vehicle horsepower and vehicle weight represented as an R numerical. As an output, a percentage as an R numeric for the probability each vehicle has of being fitted with a manual transmission. 


   When publishing a service, specify its name and version, the R code, the inputs, and the outputs needed for application integration as well as other parameters. 

   >[!NOTE]
   >To publish a web service while in a remote R session, carefully [review these guidelines](../../r/how-to-execute-code-remotely.md#publish-remote-session). 

   ```R
   api <- publishService(
     "mtService",
     code = manualTransmission,
     model = carsModel,
     inputs = list(hp = "numeric", wt = "numeric"),
     outputs = list(answer = "numeric"),
     v = "v1.0.0"
   )
   ```

<br> 

## C. Consume the service in R to test

Consume the service in R directly after publishing it to verify that the results are as expected.

```R
# Print capabilities that define the service holdings: service 
# name, version, descriptions, inputs, outputs, and the 
# name of the function to be consumed
print(api$capabilities())
   
# Consume service by calling function, `manualTransmission`
# contained in this service
result <- api$manualTransmission(120, 2.8)

# Print response output named `answer`
print(result$output("answer")) # 0.6418125   
``` 

The results should match the results obtained when the model was run locally earlier.
As long as the package versions are the same on R Server as they are locally, you should get the same results. You can check for differences using [a remote session "diff report."](../../r/how-to-execute-code-remotely.md#diff) 

>[!WARNING]
>If you get an alphanumeric error code, such as `Message: b55088c4-e563-459a-8c41-dd2c625e891d`, when consuming a service, search for that code in the [compute node's log file](configure-run-diagnostics.md#logs) to reveal the full error message. 

## D. Get the Swagger-based JSON file

Anyone can test and consume the service using its auto-generated Swagger-based JSON file. This Swagger-based JSON file is specific to a given version of a service. You can easily get this file during the same authenticated session in which you published the service. It can be downloaded to the local file system. You can get this Swagger file as long as the web service exists as described in the article "[How to interact with and consume web services in R](how-to-consume-web-service-interact-in-r.md)." 

In this example, we executed these commands to download the Swagger-based JSON file:

```R
swagger <- api$swagger()
cat(swagger, file = "swagger.json", append = FALSE) 
``` 

>[!NOTE]
>[Learn how to get and share](how-to-consume-web-service-interact-in-r.md#data-scientists-share) this Swagger-based JSON file after the session ends.



## Next steps

After it has been deployed, the web service can be: 
+ [Consumed directly in R by another data scientist](how-to-consume-web-service-interact-in-r.md#data-scientists-share), for testing purposes for example 

+ [Integrated into an application by an application developer](how-to-build-api-clients-from-swagger-for-app-integration.md)  using the  Swagger-based .JSON file produced when the web service was published. 



## How to execute R code remotely

You can use Microsoft R Client to run your R code locally and from R Client you can connect remotely to R Server to run your code there. You can easily switch between the local context and the remote context using pause() and resume() functions.  Learn more in this article, [Remote Execution in Microsoft R Server](../../r/how-to-execute-code-remotely.md).

Requirements for remote execution include:

+ Configure an R Integrated Development Environment (IDE) to work with [Microsoft R Client](../../r-client-get-started.md). 
+ Obtain [authenticated access](configure-authentication.md) to an instance of Microsoft R Server with its [operationalization feature configured](../configure-start-for-administrators.md#configure-server-for-operationalization).

## More resources

This section provides a quick summary of useful links for data scientists operationalizing R analytics with R Server.

>Use the table of contents to find all the guides and documentation needed by the administrator.

**Key Documents**
 + [About Operationalization](../../what-is-operationalization.md)    
 + [Functions in mrsdeploy package](../../r-reference/mrsdeploy/mrsdeploy-package.md)    
 + [Connecting to R Server from mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md)    
 + [Working with web services in R](how-to-deploy-web-service-publish-manage-in-r.md)    
 + [Asynchronous batch execution of web services in R](how-to-consume-web-service-asynchronously-batch.md)    
 + [Execute on a remote Microsoft R Server](../../r/how-to-execute-code-remotely.md)    

**Other Getting Started Guides**
 + [How to integrate web services and authentication into your application](how-to-build-api-clients-from-swagger-for-app-integration.md)    
 + [Administrators](configure-start-for-administrators.md)    

**Support Channel**
 + [User Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)