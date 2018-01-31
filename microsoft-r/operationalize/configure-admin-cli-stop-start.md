---

# required metadata
title: "Stop or start nodes- Machine Learning Server "
description: "Stop or start the web or compute nodes for Machine Learning Server operationalization"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "9/25/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""
---

# Stop or start web or compute nodes for Machine Learning Server

**Applies to:  Machine Learning Server, Microsoft R Server**

You can start or stop all operationalization-related nodes on the machine at once.

<a name="93"></a>

## Stop or start nodes in Machine Learning Server 9.3

@Heidi

## Stop or start nodes in earlier versions

**To stop or start a web node or compute node:**

1. [Launch the administration utility.](configure-admin-cli-launch.md)

1. From the main menu, choose the option **Stop and start services**.

1. From the submenu, choose which nodes to start or stop.

>[!Warning]
>If the following error occurs when attempting to start a web node "Microsoft.AspNetCore.Server.Kestrel.Internal.Networking.UvException: Error -97 EAFNOSUPPORT address family not supported", then disable IPv6 on your operating system.