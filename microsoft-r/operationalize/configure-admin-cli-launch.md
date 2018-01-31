---

# required metadata
title: "Administration Utility - Machine Learning Server "
description: "configure Machine Learning Server for operationalization, set passwords, restart nodes, update ports, run diagnostics, and encrypt credentials."
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

# Administration utility for operationalizing analytics

**Applies to:  Machine Learning Server, Microsoft R Server 9.x**

This article describes how to launch the tool used to manage the web and compute nodes. These nodes enable you to operationalize your analytics with Machine Learning Server.

In Machine Learning Server 9.3, this man
Use the utility to:
+ [Configure server for operationalization](configure-start-for-administrators.md#configure-server-for-operationalization) front-ends and back-ends     
+ [Set a local admin password](#admin-password)     
+ [Stop and restart](#startstop) web and compute node services     
+ [Update the service ports](#ports)     
+ [Run diagnostic tests](configure-run-diagnostics.md)     
+ [Encrypt credentials](#encrypt)     
+ [Evaluate the configuration's capacity](configure-evaluate-capacity.md)     
+ [Manage compute nodes](#uris)     
+ [Learn about command-line switches to this utility script](#switch)     

<a name="launch"></a>

## Launch the Administrator Utility

These instructions describe how to launch the Administrator Utility.

**On Windows:**

You can launch the administration utility AS AN ADMINISTRATOR (right-click) using the shortcut in the **Start** menu called **Administration Utility**.
>[!Warning]
>If your organization has the default powershell execution policy of "Restricted" (common for Windows 10 and Windows Server 2012), you may have issues running the administration utility using the shortcut. In that case, either use the alternate option detailed in the next bullet or you can change the execution policy to "Unrestricted." Read this article on powershell execution policies for details.

Alternately, open a command-line window with administrator privileges and enter the following commands:

|Version|Commands|
|----|------------|
|9.2.1|cd \<server_home><br/>dotnet Microsoft.MLServer.Utils.AdminUtil\Microsoft.MLServer.Utils.AdminUtil.dll|
|9.1.0|cd \<server_home><br/>dotnet Microsoft.RServer.Utils.AdminUtil\Microsoft.RServer.Utils.AdminUtil.dll|
|9.0.1|cd \<server_home><br/>dotnet Microsoft.DeployR.Utils.AdminUtil\Microsoft.DeployR.Utils.AdminUtil.dll|

where '\<server_home>' is the [path to the installation directory](../operationalize/configure-find-admin-configuration-file.md).  

**On Linux:**

Launch the administration utility script with root or sudo privileges with the following commands:

|Version|Commands|
|----|------------|
|9.2.1|cd \<server_home><br/>sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll|
|9.1.0|cd \<server_home><br/>sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll|
|9.0.1|cd \<server_home><br/>sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll|

where '\<server_home>' is the [path to the installation directory](../operationalize/configure-find-admin-configuration-file.md).
