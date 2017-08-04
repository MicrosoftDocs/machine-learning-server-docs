---

# required metadata
title: "DeployR Administration Console Help | DeployR 8.x"
description: "Intro to DeployR Administration Console Help"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "03/17/2016"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "deployr"
#ms.custom: ""

---

# About DeployR Administration Console

<!--**THIS TOPIC APPLIES TO:** ![](./media/deployr-admin-console-about/checkmark.jpeg) DeployR for Microsoft R Server 8.0.5 ![](./media/deployr-admin-console-about/checkmark.jpeg) DeployR Enterprise and Open (8.0.0)-->

The DeployR Administration Console, which is delivered with DeployR, is an easy-to-use web interface that facilitates the proper management and administration of your DeployR deployment. Accordingly, the following functions are supported in the console:

-   The creation and management of [user accounts](deployr-admin-console-user-accounts.md)
-   The creation and management of [roles](deployr-admin-console-permissions-with-roles.md), which are used to grant users permissions and to restrict access to R scripts
-   The import and export of [R scripts](deployr-admin-console-managing-r-scripts.md)
-   The creation and management of [R boundaries](deployr-admin-managing-r-boundaries.md), which are used to constrain runtime resource usage
-   The creation and management of [IP filters](deployr-admin-managing-access-with-ip-filters.md)
-   The backup and restore of [database contents](deployr-admin-console-database.md)
-   The management of node resources on the DeployR [grid](deployr-admin-managing-the-grid.md)
-   The management of DeployR [server policies](deployr-admin-managing-server-policies.md)
-   The [monitoring of events on the grid](deployr-admin-monitoring-events.md)

## Who Can Access the Administration Console

Only the `admin` user account, representing the administrator, has access to this console and can:

- Create and manage user accounts, roles, R boundaries, and IP filters
- Import and export scripts for preserving backups or for the purposes of migration
- Manage DeployR grid node resources and the server policies

## How to Access the Administration Console

>You cannot log in to DeployR from multiple accounts using a single brand of browser program. To use two or more accounts concurrently, you'll need to log in to each one in a separate brand of browser. 

**To access and log in to the Administration Console after installation:**

1.  In a browser window, enter the Administration Console’s URL:

		http://<DEPLOYR-IP-ADDRESS>:<PORT>/deployr/administration
	where `<DEPLOYR_SERVER_IP>` is the IP address of the DeployR machine and where `<PORT>` is the port number used during installation. 

2.  Click **Administration Console Log In** in the upper right to log in.  If this is your first time using the Administration Console, try one of the [preconfigured users](deployr-admin-console-user-accounts.md#preconfigured-user-accounts).

3. Click **Log In**.

<br>
**To log out of the Administration Console:**

+ Click **Log Out** in the upper right corner of the console.
 
>See the [Scale & Throughput Guide](deployr-admin-scale-and-throughput.md) for help in planning the provisioning of server and grid capacity.
