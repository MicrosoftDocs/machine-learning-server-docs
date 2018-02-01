---

# required metadata
title: "Manage and declare compute node URIs - Machine Learning Server "
description: "Declare the compute node URIs so that Machine Learning Server web nodes know to which compute nodes they can send requests."
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
ms.suite: "machine-learning"
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# Manage and declare compute nodes

**Applies to:  Machine Learning Server, Microsoft R Server**

In order for the Machine Learning Server web nodes to know to which compute nodes it can send requests, you must maintain a complete list of compute node URIs. This list of compute node URIs is shared across all web nodes automatically and is managed through the Admin CLI (v9.3) or, for earlier versions, in the Administration Utility.

R Server 9.1 administrators must declare URIs manually for each web node in their respective appsettings.json file. 

>[!Important]
>1. If the ['owner' role is defined](configure-roles.md), then the administrator must belong to the 'Owner' role in order to manage compute nodes. 
>
>2. If you declared URIs in R Server and have upgraded to Machine Learning Server, the URIs are copied from the old appsettings.json to the database so they can be shared across all web nodes. If you remove a URI with the utility, it is deleted from the appsettings.json file as well for consistency.


## Machine Learning Server 9.3

In Machine Learning Server 9.3, you can use `admin` extension of the Azure Command Line Interface ([Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)) to set up and manage your configuration, including the declaration and management of compute nodes with your web nodes.

>[!NOTE]
>- You must first [set up your compute nodes](configure-machine-learning-server-enterprise.md) before doing anything else with the `admin` extension of the CLI.
>- You do not need an Azure subscription to use this CLI. It is installed as part of Machine Learning Server and runs locally.  

1. On the machine hosting the node, launch a command line window or terminal  with administrator (Windows) or root/sudo (Linux) privileges.

1. If you are not yet authenticated in the CLI, do so now. This is an administrator task only, so you must have the Owner role to declare or manage URIs. The account name is `admin` unless LDAP or AAD is configured.
   ```
   az login —-mls

   # Use the following if you need help with logins.
   az login --mls --help
   ```

1. Use the CLI to declare the IP address of each compute node you configured. You can specify a single URI, several URIs, or even an IP range:
   ```
   az ml admin compute-node-uri add --uri <uris>
   ```

   For multiple compute nodes, separate each URI with a comma. The following example shows a single URI and a range of IPs (1.0.1.1, 1.0.1.2, 1.0.2.1, 1.0.2.2, 1.0.3.1, 1.0.3.2): <br/>http://1.1.1.1:12805, http://1.0.1-3.1-2:12805
 
## Machine Learning Server 9.2

To declare or manage URIs in Machine Learning Server 9.2:

1. Log in to the machine on which one of your web nodes is installed.

1. [Launch the administration utility](configure-admin-cli-launch.md) with administrator privileges (Windows) or root/sudo privileges (Linux).

1. From the main menu, choose the option **Manage compute nodes**.

1. From the submenu, choose **Add URIs** to declare one or more compute node URIs.
   
1. When prompted, enter the IP address of each compute node you configured. You can specify a specific URI or  specify IP ranges. For multiple compute nodes, separate each URI with a comma. 

   For example: http://1.1.1.1:12805, http://1.0.1-3.1-2:12805

   In this example, the range represents six IP values: 1.0.1.1, 1.0.1.2, 1.0.2.1, 1.0.2.2, 1.0.3.1,  1.0.3.2.

1. You can also choose to remove URIs or view the list of URIs. 

1. Return the main menu of the utility.
