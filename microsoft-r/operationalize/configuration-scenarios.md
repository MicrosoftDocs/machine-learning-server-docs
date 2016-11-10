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

In order to operationalize your R analytics, you must configure Microsoft R Server after installation to enable the operationalization feature.

The most simple configuration involves a single front-end and back-end for operationalization, called a **one-box configuration**. They are defined as follows.
+ A **front-end** acts as a http REST endpoint with which DeployR users can interact directly to make API calls. It can also access data in the database, and send jobs to be computed on the back-end. 
+ A **back-end** serves is used to execute R code as a session or service.

In addition to the one-box configuration, you can also install multiple components on multiple machines. This is called an **enterprise** configuration. 

<a name="onebox"></a>
## The Basic One-Box Configuration

With one-box configurations, as the name suggests, everything runs on a single machine and set-up is a breeze. This configuration is useful when you want to explore what it is to operationalize R analytics using R Server. It is perfect for testing, proof-of-concepts, and small-scale prototyping, but might not be appropriate for production usage. 

This configuration includes an operationalization front-end and back-end on the same machine. It also relies on the default local SQLite database. 

[Learn how to setup R Server for operationalization with a _one-box configuration_](configuration-initial.md#onebox).

![One-box configuration](../media/o16n/setup-onebox.jpeg)


<a name="enterprise"></a>
## The Enterprise Configuration

With enterprise configurations, you can work with your production-grade data within a scalable, multi-machine setup, and benefit from enterprise-grade security. 

This configuration includes one or more operationalization front-ends and back-ends on a group of machines. Front-ends and back-ends can be scaled independently.  

Additionally, you can configure DeployR to authenticate against [Active Directory (LDAP) or Azure Active Directory](security-authentication.md). 

When you have multiple front-ends, you must also [configure a remote database](configure-remote-database.md) (SQL Server or PostgreSQL) in place of the default SQLite database in order to share data among front-end services.
 

[Learn how to setup R Server for operationalization withan _enterprise configuration_](configuration-initial.md#enterprise).

> With this configuration, we strongly recommend that you [configure DeployR for HTTPS](security-https.md).  

![Enterprise Configuration](../media/o16n/setup-enterprise.jpeg)

