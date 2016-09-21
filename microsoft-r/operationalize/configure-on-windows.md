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

# Configuring R Server for Operationalization on Windows

After installing Microsoft R Server, you can configure the product for the operationalization of R analytics. 

## Setting up for a single box configuration

With this configuration, everything runs on a single machine. This configuration is perfect for testing, proof-of-concepts, and small-scale prototyping, but is not appropriate for production usage. [Learn more.](configurations.md)

**To configure on a single machine:**

1. If R Server (Standalone) is not yet installed, install it now.

1. Install the operationalization front-end and back-end onto the same machine as follows:
    1. Find this script XXXX
    1. Launch it.
    1. Choose Front-end. Go.
    1. Choose Back-end. Go.

1. Set the password for built-in local `administrator` account as follows:
    1. Find admin utility.
    1. Launch it.
    1. Choose menu option.
    1. Enter password for the  `administrator` account.
    1. Confirm password. 
    1. Exit the utility.

1. Run the diagnostic script to test that the configuration is working.

You are now ready to begin operationalizating your R analytics with R Server.

## Setting up for an enterprise-ready configuration

This type of configuration is highly recommended for production usage. It offers a scalable architecture, enterprise-grade security, and even a remote SQL or PostgreSQL database. [Learn more.](configurations.md)

**To configure R Server for production usage:**

1. Determine the number of front-ends, back-ends, and machines you'll want to have in your configuration.

1. On each machine, you'll include in your configuration, install R Server (Standalone).

1. 