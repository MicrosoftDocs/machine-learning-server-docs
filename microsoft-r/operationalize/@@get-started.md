---

# required metadata
title: "Operationalization of R Analytics with Microsoft R Server | Microsoft Docs"
description: "Operationalization of R Analytics with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/08/2016"
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
<!--
# Start Operationalizing Your R Analytics

A new introductory document, About DeployR, is needed. It would present a high level graphic illustrating a cycle of Develop – Test – Deploy/Manage. Then it should drill down on each of these environments.
 
Develop > Test > Deploy 



# NOTES FROM VIDEO WITH CARL EFRAT

## Example: Consume a Web Service in R

This example walks you through the consumption of a web service hosted in R Server.

>[!IMPORTANT]
> This example assumes you have configured an R Integrated Development Environment (IDE) to work with [Microsoft R Client](../r-client-get-started.md). It also assumes you have [authenticated access](security-authentication.md) to an instance of Microsoft R Server with its [operationalization feature configured](configuration-initial.md).

We have completed the demo of the first experience. Now let’s demo the 2nd experience: how other data scientists can consume this web service. 


## Consume Web Services Inside R
## this is a separate guide under DS node
Data scientists can also explore and consume Web services directly in R using some of the functions in the `mrsdeploy` package installed with Microsoft R Server and R Client. 

A web service is @@@  It can be a model or any function R code.  A web service might contain not only the model, but also the prediction script used to create it. Web services are versioned and authors can roll back to any older version at any time. R Server can serve as a central repository for model and host all web services. 

The data scientist consuming this web service might want to test it immediately after publishing to see it works as expected. They may want to interact with the web service to test it, retrain the model, and iterate upon it.  Or, the data scientist consuming the service might be someone other than the person who created the web service.  It might be someone who wants to reuse the functions inside the service.

Authorized users can access these web services and consume them inside R or directly in their applications leading to model reuse as well as bringing these models into validation and monitoring cycles by quality engineers.

Use the example from Carl's demo and then carry that example over into the app dev section.

"Thinking about the scenario that as a data scientist in QA team, after a model is deployed into production, I can validate and monitor its performance with the new production data, from R!"

--------carl script
Assume now I am another data scientist from QA team. I am going to verify the Flight Prediction web service just published by model team.

First, I use listServices function to list all available services.
<Action: highlight ‘listServices’ code and press “Ctrl+Enter” to run them>

I can explore and choose the one that I am interested. I find out the one just published. I choose it. Then I use getService function to consume the functions of that service. 
<Action: highlight ‘Get the services’ code and press “Ctrl+Enter” to run them>

I can use the new flight data to verify and monitor the deployed models. 
<Action: highlight ‘Consume the Service’ code and press “Ctrl+Enter” to run them>


You can see the 600 prediction results with the new data. The whole process is in R, so it is easy and nature for me. Consuming these services in R is a unique capability of Microsoft R Sever. It enables a set of exciting scenarios. For example, data scientists can deploy not only models, but any arbitrary R scripts/functions, they can share the useful functions for other data scientist to quickly consume in R!
--------end carl script

@@@mention Remote Execution in get started (point to MRC doc) & Consuming Services in R



## OTHER STUFF

### TESTING
testing is done by publishing in your dev sandbox without a version and consuming in R to see that it matches what you developed locally. You can use the functions to update and using package functions and eventually when satisfied you can publish with a version for consumption by others. 


-----------
other topic ideas: MAKE SEPARATE TOPIC ON UPDATING:

VERSIONING WEB SERVICES

HOW TO PUBLISH A DEV VERSION AND HOW TO UPDATE.

HOW TO UPDATE JUST A MODEL WITHOUT TOUCHING INPUT AND OUTPUT PARAMETERS

HOW TO JUST UPDATE THE CODE

HOW TO UPDATE EXISTING SERVICE
-----------


## Execute R Remotely
## this is a separate guide under Microsoft R client node (it requires MRC to use it)
Requires MRS AND MRC to use it. 

Use MRC with O16N enabled MRS to host remote R Sessions and operationalize R scripts.

Solves this:

“I can offload the function execution for heavy processing to a chunky server”
“I can validate my scripts against production environment before deployment”


Screenshot from slide 12. 

You can only use remote execution with MRC if it is connected to a version of Microsoft R Server with operationalization configured. You'll need MRS with O16N enabled to benefit from remote execution. 

composable tool for services or whatever. 

+ Built-in remote execute functions in R Client/R Server through the `mrsdeploy` package.

+ "Generate Diff" report to reconcile local and remote  [make easy to compare local and remote packages]

+ Execute .R script or interactive R commands

+ Results come back to local
+ Generate working snapshots for resume and reuse
+ IDE agnostic

USE WHAT IS THERE (OPERATIONALIZE/REMOTE-EXECUTION) AND ADD SOME CHARTS


Snapshot functions are very useful for remote execution scenarios. It can save the whole workspace and working directory so that you can pick up from exactly where you left last time. Thank about saving and loading a game.

It can also be used when publish a web service to help you handle the environment dependencies. But it might impact the performance of the Request-Response time. For optimal performance, consider the size of the snapshot carefully. Before creating a snapshot, ensure that keep only those workspace objects you need and purge the rest.  And, in the event that you only need a single object, consider passing that object alone itself instead of using a snapshot.

Include cheat sheets slide 13



**ADD code samples** with a scenario.



@@ADD TO TABLE OF CONTENTS

### Data Scientists
#### [Get started for data scientists](operationalize/data-scientist-get-started.md)
#### [Consuming Services in R](operationalize/api.md)
#### [API access token management](operationalize/security-access-tokens.md)

## [Remote execution](operationalize/remote-execution.md)


Under MRC Release Notes: 
Not a feature list


Data Scientist
1. o16N must already be configured by their admin
2. they need connection details from admin
3. develop locally with R client

-->