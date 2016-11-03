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

# Configure and Administer R Server for Operationalization

Learn more about configuring and administering the deployment server.
+ [Configuration scenarios](configurations.md)

 + Article on “Configuration Scenarios”: includes a technical architecture/config diagram of front-end/backend with authentication; introduces one-box vs enterprise ready config)  [SME: DANIEL, EFRAT]
 + Configuration Guide for Windows, including these topics:
     + Basic Setup for Operationalization (one-box; not enterprise-ready; small scale prototyping/POC, try it out)  [SME: Nick]
     + Enterprise Ready Setup for Operationalization (multiple front and backends; scale backend, active directory, LDAPs, SSL/Certificates, remote postgres (linux) or SQL database(windows))  [SME: DANIEL]
     + On cloud configuration
     + General post install steps (firewall rules)
 + Configuration Guide for Linux (like for Windows)
 + Cloud configuration
 + Enterprise Grade Security:
     + Authentication 
     + Authorization
     + CORS
     + Connection Security (HTTPS/SSL)

### Enterprise-grade Security
Learn about security options for DeployR, including:
+ [Authentication](security-authentication.md)
+ [Enabling HTTPS](security-https.md)
+ [Cross-Origin Resource Sharing](security-cors.md)

 + Administration Section:
     + Inspect Server & Log files
     + Stop & Restart
     + Configure a Database
     + ession management
     + Web service monitoring
    ???

