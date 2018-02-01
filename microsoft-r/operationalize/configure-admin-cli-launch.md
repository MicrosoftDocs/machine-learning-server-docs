---

# required metadata
title: "Launch the Administration CLI - Machine Learning Server "
description: "Launch the Admin CLI and utility used to manage the operationalization configuration for Machine Learning Server"
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

# Launch the administration tool/CLI to manage the operationalization configuration 

**Applies to:  Machine Learning Server, Microsoft R Server**

This article describes how to launch the tool used to manage the web and compute nodes for Machine Learning Server. These nodes enable you to operationalize your analytics with Machine Learning Server.

The tool to use depends on your version:
+ In Machine Learning Server 9.3, you can use `admin` extension of the Azure Command Line Interface ([Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)) to set up and manage your Machine Learning Server node configuration, including stopping and starting services.

  >[!Important]
  >- You must first [set up your nodes](configure-machine-learning-server-one-box.md) before running any other `admin` extension commands in the CLI.
  >- You do not need an Azure subscription to use this CLI. It is installed as part of Machine Learning Server and runs locally.  

+ In versions 9.0 - 9.2, this tool is called the **Administration Utility**.

With this tool, you can:
+ [Configure web and compute nodes to enable operationalizing with Machine Learning Server](configure-start-for-administrators.md#configure-server-for-operationalization)    
+ [Set a local admin password](configure-admin-cli-local-password.md)  
+ [Stop and restart nodes](configure-admin-cli-stop-start.md) 
+ [Update the node ports](configure-admin-cli-ports.md)     
+ [Run diagnostic tests](configure-run-diagnostics.md)     
+ [Encrypt credentials](configure-admin-cli-encrypt-credentials.md)     
+ [Evaluate the configuration's capacity](configure-evaluate-capacity.md)     
+ [Manage compute nodes](configure-admin-cli-compute-uris.md)     
+ [Command-line switches to administration utility](#switch)     


<a name="93"></a>

## Azure CLI for administration in Machine Learning Server 9.3

Once Machine Learning Server has been installed, you can start configuring nodes for operationalization and managing that configuration. 

1. Launch a DOS command line, powershell window, or terminal window with administrator privileges. 

1. Call the help function to verify that Administration CLI is working properly. At the commmand prompt, enter:
   ```
   az ml admin --help
   ```

## Administration utility in Machine Learning Server 9.2

These instructions describe how to launch the Administrator Utility on Machine Learning Server 9.2.

To launch the 9.2 Administration Utility:

+ **On Windows,** right-click the "Administration Utility" program icon in the Start menu, and then click "Run as Administrator".
  >[!Note]
  >If the default powershell execution policy for your organization is "Restricted", the shortcut may not work. To get around this issue, either change the execution policy to "Unrestricted", or run the following in a command-line window with administrator privileges:<br/>`cd C:\Program Files\Microsoft\ML Server\R_SERVER\o16n`<br/>`dotnet Microsoft.MLServer.Utils.AdminUtil\Microsoft.MLServer.Utils.AdminUtil.dll` 

+ **On Linux,** run the following commands with `root` or `sudo` privileges:
  ```
  cd /opt/microsoft/mlserver/9.2.1/o16n
  sudo dotnet Microsoft.MLServer.Utils.AdminUtil/Microsoft.MLServer.Utils.AdminUtil.dll
  ``` 
 
## Administration utility in R Server 9.1

These instructions describe how to launch the Administrator Utility on Machine Learning Server 9.1.

To launch the 9.1 Administration Utility:

+ **On Windows,** right-click the "Administration Utility" program icon in the Start menu, and then click "Run as Administrator".

  >[!Note]
  >If the default powershell execution policy for your organization is "Restricted", the shortcut may not work. To get around this issue, either change the execution policy to "Unrestricted", or run the following in a command-line window with administrator privileges:<br/>`cd <r-home>\deployr`<br/>`dotnet Microsoft.RServer.Utils.AdminUtil\Microsoft.RServer.Utils.AdminUtil.dll`<br/>Find r-home by running `normalizePath(R.home())` in the R console.

+ **On Linux,** run the following commands with `root` or `sudo` privileges:
  ```
  cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0/
  sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll
  ``` 
 
## Administration utility in R Server 9.0

These instructions describe how to launch the Administrator Utility on Machine Learning Server 9.1.

To launch the 9.1 Administration Utility:

+ **On Windows,** right-click the "Administration Utility" program icon in the Start menu, and then click "Run as Administrator".
  >[!Note]
  >If the default powershell execution policy for your organization is "Restricted", the shortcut may not work. To get around this issue, either change the execution policy to "Unrestricted", or run the following in a command-line window with administrator privileges:<br/>`cd <r-home>\deployr`<br/>`dotnet Microsoft.DeployR.Utils.AdminUtil\Microsoft.DeployR.Utils.AdminUtil.dll`<br/>Find r-home by running `normalizePath(R.home())` in the R console.

+ **On Linux,** run the following commands with `root` or `sudo` privileges:
  ```
  cd /usr/lib64/microsoft-r/rserver/o16n/9.0.1/
  sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll
  ``` 


<br/><a name="switch"></a>

## Command-line switches

The following command-line switches are available for the administration utility installed with R Server 9.0 - 9.1 and Machine Learning Server 9.2.1.

|Switch|Description|Introduced in version|
|----|-----|:---:|
|-silentoneboxinstall password uris <br/><br/>-silentinstall  password uris|Sets up a [one-box configuration](configure-start-for-administrators.md#configure-server-for-operationalization) silently, sets an admin password, and in 9.2 allows you to [specify compute node URIs](configure-admin-cli-compute-uris.md) or IP ranges. A password is required. For example:<br/><br/>-silentinstall myPass123 http://1.1.1.1:12805,http://1.0.1.1-3:12805 |9.1, <br/>URIs in 9.2|
|-silentwebnodeinstall password uris|Configures a [web node](configure-start-for-administrators.md#configure-server-for-operationalization) silently, sets an admin password, and in 9.2 allows you to [specify compute node URIs](configure-admin-cli-compute-uris.md) or IP ranges. A password is required. For example:<br/><br/>-silentwebnodeinstall myPass123 http://1.1.1.1:12805,http://1.0.1.1-3:12805 |9.1, <br/><br/>URIs in 9.2|
|-silentcomputenodeinstall|Configures a [compute node](configure-start-for-administrators.md#configure-server-for-operationalization) silently.  For example:<br/><br/>-silentcomputenodeinstall|9.1|
|-setpassword password|Sets the password. Cannot be used <br/> if LDAP or AAD was configured.  For example:<br/><br/>-setpassword myPass123|9.1|
|-preparedbmigration filePath|Migrates the data from current database to a different database schema. Takes the [path to the web node’s appsetting.json file](../operationalize/configure-find-admin-configuration-file.md) as an argument. This is uncommonly needed as a step when upgrading. For example:<br/><br/>-preparedbmigration \<web-node-dir>/appsettings.json|9.1|
|-encryptsecret Secret CertificateStoreName CertificateStoreLocation CertificateSubjectName|Silently [encrypts secrets](configure-admin-cli-encrypt-credentials.md).  For example:<br/><br/>-encryptsecret&nbsp;theSecret&nbsp;Store&nbsp;Location Subject|9.1|
