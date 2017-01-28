---

# required metadata
title: "Operationalization: Get Started for Data Scientists | Microsoft R Docs"
description: "Operationalization of R Analytics with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "1/25/2017"
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
ms.technology: 
  - deployr
  - r-server
ms.custom: ""
---

# Get Started for Data Scientists

Now that you've learned about [R Server's operationalization feature](about.md), we can dig into how data scientists can deploy,  consume, and share web services in order to operationalize their R analytics.

Data scientists work locally with [Microsoft R Client](../r-client-get-started.md) in their preferred R IDE and favorite version control tools to build scripts and models. Using the `mrsdeploy` package that ships with Microsoft R Client and R Server, the data scientist can develop, test, and ultimately deploy these R analytics as web services in your production environment. 

An R Server web service is an R code execution on the [operationalization compute node](configuration-initial.md). Each web service is uniquely defined by a `name` and `version`. You can use the functions in [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) to gain access a service's lifecycle from an R script. This package is installed with Microsoft R Client as well as Microsoft R Server.  The `mrsdeploy` package provides functions for publishing and managing a web service backed by an R code block or script that you provide. The package also provides functions for establishing a [remote execution](remote-execution.md) session in a console application.  [Learn more about this package](../mrsdeploy/mrsdeploy.md)  Similarly, a set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/9.0.1/#services-management-apis) are available to provide direct programmatic access to a service's lifecycle directly. 

Once deployed, the web service can be: 
+ Consumed directly in R by another data scientist, for testing purposes for example 
+ [Integrated into an application by an application developer](app-developer-get-started.md)  using the  Swagger-based .JSON file produced when the web service was published. 

![Operationalization Engine](../media/o16n/data-scientist-easy-deploy.png) 

## What You'll Need

You'll develop your R analytics locally with R Client, deploy them to Microsoft R Server as web services, and then consume or share them.

**On the local client**, you'll need to [install R Client](../r-client-get-started.md) first.  You'll also need to [configure the R IDE](https://msdn.microsoft.com/en-us/microsoft-r/r-client-get-started#step-2-configure-your-ide) of your choice, such as R Tools for Visual Studio, to run Microsoft R Client.  Once you have this set up, you can develop your R analytics in your local R IDE using the functions in [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) that was installed with Microsoft R Client (and R Server). 

**On the remote server**, you'll need access to an instance of [Microsoft R Server](../rserver.md) with its [operationalization feature configured](configuration-initial.md). Once R Server is configured for operationalization, you'll be able to connect to it from your local machine, deploy your models and other analytics to Microsoft R Server as web services, and finally consume or share those services. 

## Example: Deploy a model as a service

This example walks you through the deployment of a simple model as a web service hosted in R Server.

>[!IMPORTANT]
> This example assumes you have configured an R Integrated Development Environment (IDE) to work with [Microsoft R Client](../r-client-get-started.md). It also assumes you have [authenticated access](security-authentication.md) to an instance of Microsoft R Server with its [operationalization feature configured](configuration-initial.md).

### Step 1: Modeling locally

1. Launch your R IDE. 

   <!--@In our example, we are using R Tools | Visual Studio (RTVS), but any popular R IDE should work.-->
   
   <!--@SCREEN [RTVS with R Client installed]-->

1. Set the working directory to the local folder in which you saved the scripts, models, data files and so on. 

   In our example, we executed the following commands:
   ```
   ######################################################
   #     SET WORKING DIRECTORY TO SCRIPT LOCATION       #
   ######################################################
   # Replace `C:/temp/example` with path to your files 

   setwd("C:/temp/example”)
   ```

1. Run the R code to create the model.  

   In our example, the model, `mt-model`, is creating using the dataset `mtcars`, which is a built-in data frame in R.

   ```
   ######################################################
   #    CREATE AND TEST A LOGISTIC REGRESSION MODEL     #
   ######################################################
    
   # logistic regression vehicle transmission to estimate 
   # the probability of a vehicle being fitted with a 
   # manual transmission base on horsepower (hp) and weight (wt)

   # create glm model with mtcars dataset
   mt-model <- glm(formula = am ~ hp + wt, data = mtcars, family = binomial)

   # wrap prediction in scoring function
   manualTransmission <- function(hp, wt) {
     newdata <- data.frame(hp = hp, wt = wt)
     predict(mt-model, newdata, type = "response")
   }
   
   # test scoring function by printing results
   print(manualTransmission(120, 2.8)) # 0.6418125
   ```

1. Examine the results of the locally executed code. 

   <!--In this example, the results show @.-->
   
   <!--@SCREEN [RTVS SHOWING RESULTS]-->

### Step 2: Publishing the model on R Server as a web service

1. From your local R IDE, log into Microsoft R Server **with your credentials** using the appropriate authentication function from [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) (`remoteLogin`, `remoteLoginAAD`, or `remoteLoginAD`).  Ask your administrator for authentication details if you do not have any.

   In our example, we used Azure Active Directory for authentication.

   ```
   ######################################################
   #          LOG INTO MICROSOFT R SERVER               #
   ######################################################
   
   remoteLoginAAD(
       "https://rserver.contoso.com:12800", 
       authuri = "https://login.windows.net", 
       tenantid = "microsoft.com", 
       clientid = "5599bff3-2ec2-4975-9068-28acf86a3b6f", 
       resource = "b9667d00-1c06-4b9d-a94f-06ecb71822b0", 
       session = FALSE 
       )
   ``` 

   <!--@SCREEN [RTVS PROOF OF SUCCESSFUL CONNECTION TO MRS.]-->

   Now, you are successfully connected to the remote R Server.

1. Publish the model as a web service to R Server using the `publishService()` function from the `mrsdeploy` package.  Learn more about managing and publishing web services using this package [in this article](../mrsdeploy/mrsdeploy-websrv-vignette.md).

   To publish it, you'll need to specify:
   + A name for the service 
   + The code and the model 
   + Any inputs 
   + The output that application developers will need to integrate in their applications. 
   + A version number for the service, if you'll be sharing it with others.   

   <!--@ SHOULD WE COVER VERSIONING AT THIS POINT? EACH TIME YOU TRY THIS CODE, USE A UNIQUE VERSION NUMBER. -->

   <!-- @ DO WE ADD A WARNING NOTE HERE SAYING THAT YOU CANNOT PUBLISH FROM A REMOTE SESSION AND POINT TO THAT REMOTE EXECUTION DOCUMENTATION HERE?-->

   In our example, we executed these commands to publish a web service called `mtService` using the model called `mt-model` and a scoring function called `manualTransmission`. As an input, it takes a list of vehicle horsepower and vehicle weight represented as R numerica. As an output, a percentage as an R numeric for the probability each vehicle has of being fitted with a manual transmission.

   ```
   ##########################################################
   #              PUBLISH MODEL AS A SERVICE                #
   ##########################################################
   
   # load object `mt-model` into the global environment 
   # of your local R session
   status <- getRemoteObject(c("model")) 

   # return objects in current session. 
   # verify if model loaded
   ls()

   # publish as service called `mtService` with version `v1.0.0`
   # and assign it to the variable `api`
   api <- publishService(
     mtService,
     code = manualTransmission,
     model = mt-model,
     inputs = list(hp = "numeric", wt = "numeric"),
     outputs = list(answer = "numeric"),
     v = "v1.0.0"
   )
   ``` 

   With this line of code, we've now deployed version `v1.0.0` of web service `mtService` in the remote instance of Microsoft R server.

1. Right from your R prompt, run the scoring code to verify that the published service returns the same results when you run it against the R Server production environment. 
 
   In our example, we executed the following commands:
   ```
   ##########################################################
   #           RUN SCORING FUNCTION ON R SERVER             #
   ##########################################################
   
   # Print the service
   api
   
   # Test consume service by calling the function,
   # `manualTransmission`, contained in the service
   result <- api$manualTransmission(120, 2.8)

   # Print response output named `answer`
   print(result$output("answer")) # 0.6418125
   ``` 

   In our example, we observe the same results as we did when it was locally executed.

   >[!NOTE]
   >As long as the package versions are the same on R Server as they are locally, you should get the same results.

<a name="share"></a>

### Step 3: Sharing the service with others

You can now collaborate and hand off your predictive web service to **other authenticated users of R Server**, such as:

   + Other data scientists, who can consume this service in R as long as they know the service name and version. They might want to further test the service before it is shared with a wider audience. 

   + Application developers, who can consume the service if you send them the Swagger-based JSON file produced when the web service was published. Using this JSON file and by specifying the required inputs to the service, the developer can integrate it into his or her application.  [Learn more.](app-developer-get-started.md)


<!--## Example: Deploy an R script as a service-->