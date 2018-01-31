---

# required metadata
title: "Launch the Administration CLI - Machine Learning Server "
description: "Launch the Admin CLI and utility used to manage the operationalization configuration for Machine Learning Server"
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

# Launch the Admin CLI to manage the operationalization configuration for Machine Learning Server

**Applies to:  Machine Learning Server, Microsoft R Server**

This article describes how to launch the tool used to manage the web and compute nodes. These nodes enable you to operationalize your analytics with Machine Learning Server.

+ In **Machine Learning Server 9.3**, this management tool is called the Administration Command Line Interface (Admin CLI).

+ In earlier version, this tool is called the Administration Utility.

With this tool, you can:
+ [Configure server for operationalization](configure-start-for-administrators.md#configure-server-for-operationalization) front-ends and back-ends     
+ [Set a local admin password](#admin-password)     
+ [Stop and restart](#startstop) web and compute node services     
+ [Update the service ports](#ports)     
+ [Run diagnostic tests](configure-run-diagnostics.md)     
+ [Encrypt credentials](#encrypt)     
+ [Evaluate the configuration's capacity](configure-evaluate-capacity.md)     
+ [Manage compute nodes](#uris)     
+ [Learn about command-line switches to this utility script](#switch)     


<a name="93"></a>

## Launch the Admin CLI in v9.3

@Heidi

## Launch the utility in v9.2

These instructions describe how to launch the Administrator Utility on Machine Learning Server 9.2.

**To launch the 9.2 Administration Utility:**

+ **On Windows,** right-click the "Administration Utility" program icon in the Start menu, and then click "Run as Administrator".
  >[!Note]
  >If the default powershell execution policy for your organization is "Restricted", the shortcut may not work. To get around this issue, either change the execution policy to "Unrestricted", or run the following in a command-line window with administrator privileges:<br/>`cd C:\Program Files\Microsoft\ML Server\R_SERVER\o16n`<br/>`dotnet Microsoft.MLServer.Utils.AdminUtil\Microsoft.MLServer.Utils.AdminUtil.dll` 

+ **On Linux,** run the following commands with `root` or `sudo` privileges:
  ```
  cd /opt/microsoft/mlserver/9.2.1/o16n
  sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll
  ``` 
 
## Launch the utility in v9.1

These instructions describe how to launch the Administrator Utility on Machine Learning Server 9.1.

**To launch the 9.1 Administration Utility:**

+ **On Windows,** right-click the "Administration Utility" program icon in the Start menu, and then click "Run as Administrator".
  >[!Note]
  >If the default powershell execution policy for your organization is "Restricted", the shortcut may not work. To get around this issue, either change the execution policy to "Unrestricted", or run the following in a command-line window with administrator privileges:<br/>`cd <r-home>\deployr`<br/>`dotnet Microsoft.RServer.Utils.AdminUtil\Microsoft.RServer.Utils.AdminUtil.dll`<br/>Find r-home by running `normalizePath(R.home())` in the R console.

+ **On Linux,** run the following commands with `root` or `sudo` privileges:
  ```
  cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0/
  sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll
  ``` 
 
## Launch the utility in v9.0

These instructions describe how to launch the Administrator Utility on Machine Learning Server 9.1.

**To launch the 9.1 Administration Utility:**

+ **On Windows,** right-click the "Administration Utility" program icon in the Start menu, and then click "Run as Administrator".
  >[!Note]
  >If the default powershell execution policy for your organization is "Restricted", the shortcut may not work. To get around this issue, either change the execution policy to "Unrestricted", or run the following in a command-line window with administrator privileges:<br/>`cd <r-home>\deployr`<br/>`dotnet Microsoft.DeployR.Utils.AdminUtil\Microsoft.DeployR.Utils.AdminUtil.dll`<br/>Find r-home by running `normalizePath(R.home())` in the R console.

+ **On Linux,** run the following commands with `root` or `sudo` privileges:
  ```
  cd /usr/lib64/microsoft-r/rserver/o16n/9.0.1/
  sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll
  ``` 