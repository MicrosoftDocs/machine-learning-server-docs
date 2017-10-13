---

# required metadata
title: "Update DeployR after Java update - DeployR 8.x "
description: "Update DeployR after Java update"
keywords: "DeployR, JRE, JDK, Java, Update"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "11/21/2016"
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

# Updating DeployR Configuration after Java Version Update

**Applies to: DeployR 8.x**   (See [comparison between 8.x and 9.x](../whats-new-in-r-server.md#8vs9))

>Looking for docs for Microsoft R Server 9? [Start here](../what-is-operationalization.md).

This topic describes how to update the configuration of DeployR (Apache Tomcat, etc. ) after a Java JRE/JDK.


**To update the configuration for the new Java version:**

1. Open a command prompt/terminal window with administrator/root/sudo privileges:

1. [Stop DeployR](deployr-common-administration-tasks.md#startstop).

1. Go to the Apache Tomcat directory. For example, on Windows the default location is `C:\Program Files\Microsoft\DeployR-8.0.5\Apache_Tomcat\bin`.

1. At the prompt, enter the following command to start the utility:
   ```NA
   tomcat7w.exe //MS//Apache-Tomcat-for-DeployR-8.0.5
   ```

1. In the dialog, enter the new Java version to be used for DeployR.
 
1. Click **OK** to save these settings.

1. [Restart DeployR](deployr-common-administration-tasks.md#startstop).
 

