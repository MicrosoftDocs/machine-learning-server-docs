---

# required metadata
title: "Operationalization with R Server"
description: "Operationalization of R Analytics with Microsoft R Server"
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

# Deployment Server Configuration Scenarios

If you want to operationalize your R analytics, you must configure Microsoft R Server after installation to act as a deployment server that hosts analytic web services.

There are essentially two types of configurations:

1. The "One-box configuration", which is commonly used for testing

1. The "Enterprise-ready configuration", which is essential for production grade environments

<a name="onebox"></a>
## The One-Box Basic Configuration

With one-box configurations, as the name suggests, everything runs on a single machine and set-up is a breeze. This configuration is useful when you want to explore what it is to operationalize R analytics using R Server. It is perfect for testing, proof-of-concepts, and small-scale prototyping, but is not appropriate for production usage. 

This configuration includes an operationalization front-end and back-end on the same machine. It relies on the default local SQLite database. The front-end acts as a front-end interface with which DeployR users can interact directly to make API calls, access data in the database, and send jobs to be computed on the back-end. The back-end serves directly in support of the front-end services and is used to execute R code as a session or service, for computation, and manages stateful R shells.

[Learn how to setup R Server for operationalization with a _one-box configuration_](configuration-initial.md#onebox).

![One-box configuration](../media/o16n/setup-onebox.jpeg)


<a name="enterpriseready"></a>
## The Enterprise-Ready Configuration

With enterprise-ready configurations, you can work with your production-grade data within a scalable, multi-machine setup, and benefit from enterprise-grade security. 

This configuration includes one or more operationalization front-ends and back-ends on a group of machines. The front-end acts as a front-end interface with which DeployR users can interact directly to make API calls, access data in the database, and send jobs to be computed on the back-end. The back-end serves directly in support of the front-end services and is used to execute R code as a session or service, for computation, and manages stateful R shells. Front-ends and back-ends can be scaled independently.  You can also [configure a remote database](configure-remote-database.md) (SQL Server or PostgreSQL) in place of the default SQLite database. 

[Learn how to setup R Server for operationalization withan _enterprise-ready configuration_](configuration-initial.md#enterpriseready).

> With this configuration, we strongly recommend that you [configure DeployR for HTTPS](security-https.md).  

![Enterprise-Ready Configuration](../media/o16n/setup-enterprise-ready.jpeg)

