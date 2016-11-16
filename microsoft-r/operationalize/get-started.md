---

# required metadata
title: "About DeployR"
description: "DeployR introduction and overview."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/06/2016"
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

# Start Operationalizing Your R Analytics

 + Description of Operationalizing R Analytics (What it is)  [SME: CARL]
 + Workflow Scenario – develop/test/production  [SME: CARL]
 + Add workflow graphic made clickable (Pedro/Carl to design – include remote execution on this too) [SME: CARL]
 + Best practices  [SME: CARL]
 + Use White Paper/Operationalization guide written by Carl for inspiration

----------

A new introductory document, About DeployR, is needed. It would present a high level graphic illustrating a cycle of Develop – Test – Deploy/Manage. Then it should drill down on each of these environments.
 
Development Environment
 
Test Environment
 
Production Environment
 
It would also describe DeployR as having a web node and compute node. The front end consists of APIs, domain services (users, files). It will be independently scalable. The compute node consists of RServe sessions and Shells (R sessions).
 
Answer the question: What do I need it for? What problems are we trying to solve?
 
What are the pain points and how are they solved by DeployR.
 

As a data scientist, I can deploy R analytics (scripts, models) to the production environment.
As a data scientist, I can build R scripts, models and do the version controls in my favorite IDE (R Studio, RTVS) with my version control tools (Git, SVN, VSO). [Note: currently R Studio supported/integrated with Git and SVN)]
As a data scientist, after developed the scripts/model locally, I can test them in the production environment easily (for example, with a simple R function to run them remotely against DeployR production server).
As a data scientist, I can collaborate with App developers by sharing my scripts as APIs (Using API tools such as Swagger) with the definitions of the inputs for my scripts.  
After the testing, I committed the changes, tagged them in the version control tools and release (copy) them to a release folder which will store the R analytics for production usage. [We need to further understanding if this is the common usage of Git as the respository of production assets.]
 As the intelligent app developer, I learnt from the data scientist about the R analytics he/she deployed. I can run/test them in a test harness and then integrate them into my app.
If implemented, also document the one click deployment from RTVS/R Studio. deploy R analytics (scripts, models) to the production environment by a button click or right click on the R assets in RTVS/RStudio