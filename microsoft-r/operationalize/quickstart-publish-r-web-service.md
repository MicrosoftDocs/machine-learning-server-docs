---

# required metadata
title: "Quickstart: Deploy R models as web services with mrsdeploy - Machine Learning Server "
description: "How to deploy an R model as a service"
keywords: "quickstart, Machine Learning Server, Microsoft R Server, deploy r models"
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "get-started-article"
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
# Deploy an R Model as a web service with mrsdeploy

**Applies to: Microsoft R Client 3.x, Machine Learning Server, Microsoft R Server 9.x**

## Objective

Learn how to publish an R model as a web service with Machine Learning Server, formerly known as Microsoft R Server. Data scientists work locally with [Microsoft R Client](../r-client-get-started.md) in their preferred R IDE and favorite version control tools to build scripts and models. Using the mrsdeploy package that ships with Microsoft R Client and Machine Learning Server, you can develop, test, and ultimately deploy these R analytics as web services in your production environment. 

An Machine Learning Server R web service is an R code execution on the [operationalization compute node](configure-start-for-administrators.md#configure-server-for-operationalization). Each web service is uniquely defined by a `name` and `version`. You can use the functions in [the mrsdeploy package](../r-reference/mrsdeploy/mrsdeploy-package.md) to gain access a service's lifecycle from an R script. A set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/#services-management-apis) are also available to provide direct programmatic access to a service's lifecycle directly. 

## Time estimate

If you have completed the prerequisites, this task takes approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have the following ready:

+ An instance of [Microsoft R Client installed](../r-client-get-started.md) on your local machine. You can optionally configure an R IDE of your choice, such as R Tools for Visual Studio, to run Microsoft R Client.   

+ An instance of [Machine Learning Server installed](../what-is-machine-learning-server.md) that has been [configured to operationalize analytics](configure-start-for-administrators.md#configure-server-for-operationalization). 

  For your convenience, [Azure Resource Management (ARM) templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#template-deployment) are available to quickly deploy and configure the server for operationalization in Azure.

+ The connection details to that instance of Machine Learning Server. Contact your administrator for any missing connection details. After [connecting to Machine Learning Server](how-to-connect-log-in-with-mrsdeploy.md) in R, deploy your analytics as web services so others can consume them. 


## Example code

This article walks through the deployment of a simple R model as a web service hosted in Machine Learning Server.  Here is the entire R code for the example that we walk through in the sections that follow.

>[!IMPORTANT]
>Be sure to replace the remoteLogin() function with the correct login details for your configuration. Connecting to Machine Learning Server using the mrsdeploy package is covered [in this article](how-to-connect-log-in-with-mrsdeploy.md).

```r
##########################################################
#       Create & Test a Logistic Regression Model        #
##########################################################
    
# Use logistic regression equation of vehicle transmission 
# in the data set mtcars to estimate the probability of 
# a vehicle being fitted with a manual transmission 
# based on horsepower (hp) and weight (wt)

# If on R Server 9.0, load mrsdeploy package now
library(mrsdeploy)

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
#            Log into Server                 #
##########################################################
   
# Use `remoteLogin` to authenticate with Server using 
# the local admin account. Use session = false so no 
# remote R session started
remoteLogin("http://localhost:12800", 
            username = "admin", 
            password = "{{YOUR_PASSWORD}}",
            session = FALSE)

##########################################################
#             Publish Model as a Service                 #
##########################################################

# Generate a unique serviceName for demos 
# and assign to variable serviceName
serviceName <- paste0("mtService", round(as.numeric(Sys.time()), 0))

# Publish as service using publishService() function from 
# mrsdeploy package. Name service "mtService" and provide
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
#         Get Service-specific Swagger File in R         #
##########################################################
   
# During this authenticated session, download the  
# Swagger-based JSON file that defines this service
swagger <- api$swagger()
cat(swagger, file = "swagger.json", append = FALSE)

# Now share this Swagger-based JSON so others can consume it
```


## A. Model locally
Now let's dive into this example down. Let's start by creating the model locally, then publish it, and then share it with other authenticated users.

1. Launch your R IDE or Rgui so you can start entering R code. 

1. If you have R Server 9.0.1, load the mrsdeploy package. In later releases, this package is preloaded for you.

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

1. From your local R IDE, log in to Machine Learning Server **with your credentials**. Use the appropriate authentication function from [the mrsdeploy package](how-to-connect-log-in-with-mrsdeploy.md) (`remoteLogin` or `remoteLoginAAD`) for your authentication method.  

   For simplicity, the following code uses the basic local 'admin' account for authentication with the `remoteLogin` function and `session = false` so that no remote R session is started.  Learn more about authenticating with Active Directory LDAP or Azure Active Directory in the article "[Connecting to Machine Learning Server from mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md)."

   >[!IMPORTANT]
   >Be sure to replace the remoteLogin() function with the [correct login details for your configuration](how-to-connect-log-in-with-mrsdeploy.md).

   ```R  
   # Use `remoteLogin` to authenticate with Server using 
   # the local admin account. Use session = false so no 
   # remote R session started
   remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)
   ``` 

   Now, you are successfully connected to the remote Machine Learning Server.

1. Publish the model as a web service to Machine Learning Server using [the publishService() function](how-to-deploy-web-service-publish-manage-in-r.md) from the mrsdeploy package. 

   In this example, you publish a web service called `"mtService"` using the model `carsModel` and the function `manualTransmission`. As an input, the service takes a list of vehicle horsepower and vehicle weight represented as an R numerical. As an output, a percentage as an R numeric for the probability each vehicle has of being fitted with a manual transmission. 


   When publishing a service, specify its name and version, the R code, the inputs, and the outputs needed for application integration as well as other parameters. 

   >[!NOTE]
   >To publish a web service while in a remote R session, carefully [review these guidelines](../r/how-to-execute-code-remotely.md#publish-remote-session). 

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
As long as the package versions are the same on Machine Learning Server as they are locally, you should get the same results. You can check for differences using [a remote session "diff report."](../r/how-to-execute-code-remotely.md#diff) 

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

You can use Microsoft R Client to run your R code locally and from R Client you can connect remotely to Machine Learning Server to run your code there. You can easily switch between the local context and the remote context using pause() and resume() functions.  Learn more in this article, [Remote Execution in Microsoft Machine Learning Server](../r/how-to-execute-code-remotely.md).

Requirements for remote execution include:

+ Configure an R Integrated Development Environment (IDE) to work with [Microsoft R Client](../r-client-get-started.md). 
+ Obtain [authenticated access](configure-authentication.md) to an instance of Machine Learning Server with its [operationalization feature configured](configure-start-for-administrators.md#configure-server-for-operationalization).

## More resources

This section provides a quick summary of useful links for data scientists operationalizing R analytics with Machine Learning Server.

 + [About Operationalization](../what-is-operationalization.md)    
 + [Functions in mrsdeploy package](../r-reference/mrsdeploy/mrsdeploy-package.md)    
 + [Connecting to Machine Learning Server from mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md)    
 + [Working with web services in R](how-to-deploy-web-service-publish-manage-in-r.md)    
 + [Asynchronous batch execution of web services in R](how-to-consume-web-service-asynchronously-batch.md)    
 + [Execute on a remote Machine Learning Server](../r/how-to-execute-code-remotely.md)    
 + [How to integrate web services and authentication into your application](how-to-build-api-clients-from-swagger-for-app-integration.md)    
 + [Get started for administrators](configure-start-for-administrators.md)    
 + [User Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)