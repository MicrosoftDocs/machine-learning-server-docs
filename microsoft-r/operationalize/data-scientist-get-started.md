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

Now that you've learned about [R Server's operationalization feature](about.md), we can dig into how data scientists can deploy,  consume, and share web services with R Server in order to operationalize their R analytics.

Data scientists work locally with [Microsoft R Client](../r-client-get-started.md) in their preferred R IDE and favorite version control tools to build scripts and models. Using the `mrsdeploy` package shipped with Microsoft R Client and R Server, the data scientist can develop, test, and ultimately deploy these R analytics as web services in your production environment. 

An R Server web service is a stateless execution in an R shell on the compute node. Each web service is uniquely defined by a `name` and `version`. You can use the functions in [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) to gain access a service's lifecycle from an R script. Similarly, a set of [RESTful APIs](https://microsoft.github.io/deployr-api-docs/9.0.1/#services-management-apis) are available to provide programmatic access to a service's lifecycle directly. 

Once deployed, the web service can be (1) consumed directly in R by a data scientist and/or (2) [integrated into an application by an application developer using a .JSON file and swagger](app-developer-get-started.md).

![Operationalization Engine](../media/o16n/data-scientist-easy-deploy.png) 

## What You'll Need

You'll develop your R analytics locally with R Client, deploy them to Microsoft R Server as web services, and then consume or share it from R Server.

**On the local client**, you'll need to [install R Client](../r-client-get-started.md) first.  You'll also need to [configure the R IDE](https://msdn.microsoft.com/en-us/microsoft-r/r-client-get-started#step-2-configure-your-ide) of your choice, such as R Tools for Visual Studio, to run Microsoft R Client.  Once you have this set up, you can develop your R analytics in your local R IDE using the functions in [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) that was installed with Microsoft R Client (and R Server). 

**On the remote server**, you'll need an installed instance of [Microsoft R Server](../rserver.md) with its [operationalization feature configured](configuration-initial.md). Once R Server is configured for operationalization, you'll be able to connect to it from your local machine, deploy your models and other analytics to Microsoft R Server as web services, and finally consume or share those services from R Server. 

## Example: Deploy a Model as a Service

This example walks you through the deployment of a simple model as a web service hosted in R Server.

>[!IMPORTANT]
> This example assumes you have configured an R Integrated Development Environment (IDE) to work with [Microsoft R Client](../r-client-get-started.md). It also assumes you have [authenticated access](security-authentication.md) to an instance of Microsoft R Server with its [operationalization feature configured](configuration-initial.md).

### Run Prediction Locally

1. Download the sample files (a .R script and .rds model and .rds data file) to a local directory, such as `C:/temp/example`.

   @@ CAN WE PROVIDE FILES FOR DOWNLOAD?
   
   @@ WHERE DO THEY GET THE MODEL AND DATA FROM?

1. Launch your R IDE. In our example, we are using R Tools | Visual Studio (RTVS), but any popular R IDE should work.
   
   @SCREEN [RTVS with R Client installed]

1. Set the working directory to the local folder in which you saved scripts, models, data files and so on. 

   In our example, we executed the following commands:
   ```
   ######################################################
   #     SET WORKING DIRECTORY TO SCRIPT LOCATION       #
   ######################################################
   # Replace `C:/temp/example` with path to your files 

   setwd("C:/temp/example”)
   ```

1. Load the R script into the IDE so that you can examine the script contents. In our example, we loaded `RServer_FlightPredictionDemo.r` 
   
   @SCREEN [RTVS SHOWING SCRIPT CODE  RServer_FlightPredictionDemo.r]

1. Run code to load the model, associate the model with the training data’s column info, and then wrap the prediction with a scoring function (`flightPrediction`) for easy prediction, and finally test the function to verify that it works as expected. Note that the function’s input data is a batch data set for scoring.  

   In our example, we executed the following commands:
   ```
   ######################################################
   #        CREATE AND TEST A SCORING FUNCTION          #
   ######################################################
    
   # Load the predictive model 
   logitModel <- readRDS('logitModel.rds')
 
   # Create a list to pass the data colum info together with the model object
   trainingData <- readRDS('trainingDF.rds')
   colInfo <- rxCreateColInfo(trainingData)
 
   modelInfo <- list(predictiveModel = logitModel, colInfo = colInfo)
 
   # Wrap up the prediction to a function for easy consumption
   flightPrediction <- function(flightData) {
      data <- rxImport(flightData, colInfo = modelInfo$colInfo)
      rxPredict(modelInfo$predictiveModel, data, type = "response")
   }
 
   # Test if the function works as expected
   testData <- readRDS('testDF.rds')
   flightPrediction(testData)
   ```

1. Examine the results of the locally executed code. In this example, the results show 900 predictions for each row of the data set.
   
   @SCREEN [RTVS SHOWING RESULTS]

1. From your local R IDE, log into Microsoft R Server **with your credentials** using one of the authentication functions found [the `mrsdeploy` package](../mrsdeploy/mrsdeploy.md) (`remoteLogin`, `remoteLoginAAD`, or `remoteLoginAD`).  These are functions from the `mrsdeploy` package. In our example, we use Azure Active Directory for authentication. 

   In our example, we executed the following commands:
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

   @SCREEN [RTVS PROOF OF SUCCESSFUL CONNECTION TO MRS.]

   Now, you are successfully connected to the remote R Server.

1. To publish a web service, you'll need to copy the model, scripts, data and so on to Microsoft R Server. 

   @@ HOW DO THE FILES END UP ON R SERVER SO THEY CAN RUN AND TEST IT THERE? 

   @@ WHERE EXACTLY DO THE FILES NEED TO BE?

1. Publish the model as a web service using the `publishService()` function from the `mrsdeploy` package.  In order to publish it, you'll need to specify:
   + A name for the service 
   + The data file and model 
   + Any inputs 
   + The output that application developers will need to integrate in their applications. 
   + A version number for the service, if you'll be sharing it with others.   

   @@ SHOULD WE COVER VERSIONING AT THIS POINT? EACH TIME YOU TRY THIS CODE, USE A UNIQUE VERSION NUMBER. 

   @@ DO WE ADD A WARNING NOTE HERE SAYING THAT YOU CANNOT PUBLISH FROM A REMOTE SESSION AND POINT TO THAT REMOTE EXECUTION DOCUMENTATION HERE?

   In our example, we executed the following commands to publish a web service called `FlightPredictionService` using the model `modelInfo` and a scoring function called `flightPrediction`. It takes as an input ???@@. The scoring function returned ???.@@
   ```
   ######################################################
   #          PUBLISH MODEL AS A SERVICE                #
   ######################################################
   
   api <- publishService(
     "FlightPredictionService",
     code = flightPrediction,
     model = modelInfo,
     inputs = list(newflightdata = "data.frame"), 
     outputs = list(answer = "data.frame"), 
     v = 'v1.0.0'
   )
   ``` 

   With this line of code, you have now deployed and hosted a web service in a remote instance of Microsoft R server.

1. @WHAT IS THIS?

   ```
   api$capabilities()
   ``` 

1. Right from your R prompt, verify that the published service returns the same results when you run it against the R Server production environment. Run the scoring code.
 
   In our example, we executed the following commands:
   ```
   ######################################################
   #        RUN SCORING FUNCTION ON R SERVER            #
   ######################################################
   
   result <- api$flightPrediction(testData)
   print(result$output("answer"))
   ``` 

   In our example, we observe the same 900+ prediction results as we did when it was locally executed.

   @SCREEN [RTVS SHOWING SAME 900+ RESULTS.]

   >[!NOTE]
   >As long as the package versions are the same on R Server as they are locally, you should get the same results.

1. You can now collaborate and hand off your predictive web service to **other authenticated users of R Server**, such as:

   + Other data scientists, who can consume this service in R as long as they know the service name and version. They might want to further test the service before it is shared with a wider audience. 

   + Application developers, who can consume the service if you send them the Swagger-based JSON file produced when the web service was published. Using this JSON file and by specifying the required inputs to the service, the developer can integrate it into his or her application.  [Learn more.](app-developer-get-started.md)