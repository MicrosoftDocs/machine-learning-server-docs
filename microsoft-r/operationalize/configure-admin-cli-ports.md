---

# required metadata
title: "Update the port values - Machine Learning Server"
description: "You can update the ports numbers for the web node, compute node, or deployr-rserve."
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

# Update port values

**Applies to:  Machine Learning Server, Microsoft R Server**

You can update the ports numbers for the web node, compute node, or [deployr-rserve](https://github.com/Microsoft/deployr-rserve) (a forked version of RServe).

**To update ports in Machine Learning Server 9.3:**
@Heidi

**To update ports in earlier versions:**

1. Log in to the machine on which your web node or compute node is installed.

1. [Launch the administration utility](configure-admin-cli-launch.md) with administrator privileges (Windows) or root/sudo privileges (Linux).

1. From the main menu, choose the option **Change service ports**.

1. From the submenu, choose the port you want to update.

1. Enter the port number. 

>The port number will be updated the next time the [service is restarted](configure-admin-cli-stop-start.md).
