---

# required metadata
title: "Stop or start web and compute nodes on Machine Learning Server "
description: "Stop or start the web or compute nodes for Machine Learning Server operationalization"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
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

# Stop or start web and compute nodes

**Applies to:  Machine Learning Server, Microsoft R Server**

You can start or stop all operationalization-related web and compute nodes on the Machine Learning Server machine at once.

## Machine Learning Server 9.3

In Machine Learning Server 9.3, you can use `admin` extension of the [Azure Command Line Interface](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) (**Azure CLI**) to manage your configuration, including stopping and starting services.

1. On the machine hosting the node, launch a command line window or terminal  with administrator (Windows) or root/sudo (Linux) privileges.

1. Run the command to either stop or start a node.
   ```
   # Start nodes on a machine
   az ml admin node start --computenode --webnode

   # Stop nodes on a machine
   az ml admin node stop --computenode --webnode

   # Get help on other permutations
   az ml admin node start --help
   az ml admin node stop --help
   ```

## Earlier versions

To stop or start a web node or compute node:

1. [Launch the administration utility.](configure-admin-cli-launch.md)

1. From the main menu, choose the option **Stop and start services**.

1. From the submenu, choose which nodes to start or stop.

>[!Warning]
>If the following error occurs when attempting to start a web node "Microsoft.AspNetCore.Server.Kestrel.Internal.Networking.UvException: Error -97 EAFNOSUPPORT address family not supported", then disable IPv6 on your operating system.