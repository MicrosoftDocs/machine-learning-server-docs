---

# required metadata
title: "Quick Start for Microsoft R Client"
description: "Microsoft R Client quickstart"
keywords: "R Client, quickstart, Microsoft R Client, Introduction, Get Started with R Client"
author: "j-martens"
manager: "jhubbard"
ms.date: "5/10/2017"
ms.topic: "get-started-article"
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
ms.technology: "r-client"
ms.custom: ""

---
# Deploy an R Model as a web service

**Applies to: Microsoft R Server 9.x**

>[!NOTE]
> This code can also be run on Microsoft R Client if you connect remotely to R Server.

## Objective

Learn how to publish an R model as a web service with Microsoft R Server. Data scientists work locally with [Microsoft R Client](../r-client-get-started.md) in their preferred R IDE and favorite version control tools to build scripts and models. Using the `mrsdeploy` package that ships with Microsoft R Client and R Server, you can develop, test, and ultimately deploy these R analytics as web services in your production environment. 

An R Server web service is an R code execution on the [operationalization compute node](configuration-initial.md). Each web service is uniquely defined by a `name` and `version`. You can use the functions in [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) to gain access a service's lifecycle from an R script. A set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/#services-management-apis) are also available to provide direct programmatic access to a service's lifecycle directly. 

## Time estimate

If you have completed the prerequisites, this task will take approximately *10* minutes to complete.

## Prerequisites

Before you begin this QuickStart, have the following ready:

+ An instance of [Microsoft R Client installed](../r-client-get-started.md) on your local machine. You can optionally configure an R IDE of your choice, such as R Tools for Visual Studio, to run Microsoft R Client.   

+ An instance of [Microsoft R Server installed](../rserver.md) that has been [configured to operationalize analytics](configuration-initial.md).

+ The connection details and access to that instance of Microsoft R Server. Please contact your administrator for any missing connection details. After R Server is configured properly, you can [connect to it from your local machine](../operationalize/mrsdeploy-connection.md) in R to deploy your models and other analytics as web services so they can be consumed. 


## Example code

This article walks through the deployment of a simple R model as a web service hosted in R Server.  Here is the entire R code for the example that we'll walkthrough in the sections that follow.

>[!IMPORTANT]
>Be sure to replace the `remoteLogin()` function below with the correct login details for your configuration. Connecting to R Server using the `mrsdeploy` package is covered [in this article](mrsdeploy-connection.md).

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
#            Log into Microsoft R Server                 #
##########################################################
   
# Use `remoteLogin` to authenticate with R Server using 
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

# Publish as service using `publishService()` function from 
# `mrsdeploy` package. Name service "mtService" and provide
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

1. If you have R Server 9.0.1, load the `mrsdeploy` package. In R Server 9.1, this package is preloaded for you.

   ```R
   library(mrsdeploy)
   ```

1. Create a GLM model called `carsModel` using the dataset `mtcars`, which is a built-in data frame in R. This model estimates the probability of a vehicle being fitted with a manual transmission based on horsepower (hp) and weight (wt)

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

1. Examine the results of the locally executed code. You'll compare these to the results of the web service later.

<br> 

## B. Publish model as a web service

1. From your local R IDE, log into Microsoft R Server **with your credentials** using the appropriate authentication function from [the `mrsdeploy` package](../operationalize/mrsdeploy-connection.md) (`remoteLogin` or `remoteLoginAAD`).  

   For simplicity, the code below uses the basic local `admin` account for authentication with the `remoteLogin` function and `session = false` so that no remote R session is started.  Learn more about authenticating with Active Directory LDAP or Azure Active directory, the authentication functions, and their arguments in the article: ["Connecting to R Server from mrsdeploy"](../operationalize/mrsdeploy-connection.md).


   >[!IMPORTANT]
   >Be sure to replace the `remoteLogin()` function below with the [correct login details for your configuration](mrsdeploy-connection.md).

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

1. Publish the model as a web service to R Server using [the `publishService()` function](../operationalize/data-scientist-manage-services.md) from the `mrsdeploy` package. 

   In this example, you publish a web service called `"mtService"` using the model `carsModel` and the function `manualTransmission`. As an input, the service takes a list of vehicle horsepower and vehicle weight represented as an R numerical. As an output, a percentage as an R numeric for the probability each vehicle has of being fitted with a manual transmission. 


   When publishing, you must specify, among other parameters, a service name and version, the R code, the inputs, as well as the outputs that application developers will need to integrate in their applications. 

   >[!NOTE]
   >To publish a web service while in a remote R session, carefully [review these guidelines](remote-execution.md#publish-remote-session). 

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

## C. Test the service by consuming it in R

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
As long as the package versions are the same on R Server as they are locally, you should get the same results. You can check for differences using [a remote session "diff report"](remote-execution.md#diff). 

>[!WARNING]
>If you get an alphanumeric error message similar to `Message: b55088c4-e563-459a-8c41-dd2c625e891d` when consuming a web service, use that string to find the full error message text in the [compute node's log file](admin-diagnostics.md#logs). 

## D. Get the Swagger-based JSON file

During the authenticated session in which you published the service, you can download the Swagger-based JSON file specific to this service so that you or other authenticated users can test and consume the service. This Swagger-based JSON file is generated when the service was published. It will be downloaded to the local file system. 

In this example, we executed these commands to download the Swagger-based JSON file:

```R
swagger <- api$swagger()
cat(swagger, file = "swagger.json", append = FALSE) 
``` 

>[!NOTE]
>See [the next section](#share) for the code to get this Swagger-based JSON file after the session ends.



## Next steps

After it has been deployed, the web service can be: 
+ [Consumed directly in R by another data scientist](data-scientist-manage-services.md#data-scientists-share), for testing purposes for example 
+ [Integrated into an application by an application developer](app-developer-get-started.md)  using the  Swagger-based .JSON file produced when the web service was published. 



## How to execute R code remotely

You can use Microsoft R Client to run your R code locally and from R Client you can connect remotely to R Server to run your code there. You can easily switch between the local context and the remote context using `pause()` and `resume()` functions.  Learn more in this article, [Remote Execution in Microsoft R Server](remote-execution.md).

Requirements for remote execution include:

+ You must configure an R Integrated Development Environment (IDE) to work with [Microsoft R Client](../r-client-get-started.md). 
+ You must also have [authenticated access](security-authentication.md) to an instance of Microsoft R Server with its [operationalization feature configured](configuration-initial.md).

## More resources

This section provides a quick summary of useful links for data scientists operationalizing R analytics with R Server.

>Use the table of contents to find all of the guides and documentation needed by the administrator.

**Key Documents**
+ [About Operationalization](about.md)
+ [Functions in mrsdeploy package](../mrsdeploy/mrsdeploy.md)
+ [Connecting to R Server from mrsdeploy](../operationalize/mrsdeploy-connection.md)
+ [Working with web services in R](../operationalize/data-scientist-manage-services.md)
+ [Asynchronous batch execution of web services in R](../operationalize/data-scientist-batch-mode.md)
+ [Execute on a remote Microsoft R Server](remote-execution.md)

**Other Getting Started Guides**
+ [Application Developers](app-developer-get-started.md)
+ [Administrators](admin-get-started.md)

**Support Channel**
+ [Microsoft R Server Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)