---

# required metadata
title: "How to use R Server on Linux VM in Azure | Microsoft Azure"
description: "Learn how to work with R Server on Linux by using a virtual machine in Azure."
keywords: "R Server, linux, virtual machine"
author: "j-martens"
manager: "jhubbard"
ms.date: "4/26/2017"
ms.topic: "article"
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
  - r-server
ms.custom: ""

---

# Microsoft R Server for Linux 9.1:  Virtual Machine on Azure

>Looking for an older version? See here: [9.0.1](install/r-server-vm-azure-linux-9-0-1.md) or [8.0.5](install/r-server-vm-azure-linux-8-0-5.md). 

Microsoft R Server is the most broadly deployable enterprise-class analytics platform for R available today. This virtual machine (VM) includes the Microsoft R Server version 9.1) for Linux (CentOS version 7.2 or Ubuntu version 16.04). More information on this version can be found at: https://msdn.microsoft.com/en-us/microsoft-r/rserver-install-linux-server. 

This VM also includes:

+ `mrsdeploy`: a new R package for deploying R analytics inside applications and backend systems. [Learn more...](r-reference/mrsdeploy/mrsdeploy-package.md)

+ `MicrosoftML`:  a new 'Microsoft Machine Learning algorithm' R package that includes functions for incorporating machine learning into R code or a script that executes on R Server for Windows, R Client for Windows, and SQL Server R Services. Availability for Linux, Hadoop, and Azure HDInsight is projected for the first quarter of 2017. [Learn more...](r-reference/microsoftml/microsoftml-package.md)

## Provision the R Server Virtual Machine

If you are new to using Azure VMs, we recommend that you review [this article](https://azure.microsoft.com/en-us/documentation/services/virtual-machines/linux/) for more information about using the portal and configuring a virtual machine.

**To create the Microsoft R Server on Linux VM:**

1. Go to the [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/).

1. In the search box, type **Microsoft R Server on Linux**.

1. Select **Microsoft R Server on Linux**. 

1. Click **Create Virtual Machine**. 

1. Accept the terms and get started by clicking **Create**. 

1. Use the onscreen prompts and fields to configure your R Server VM. 
   >- You'll need an Azure subscription to create the VM.
   >- If you are unfamiliar with the Virtual Machines on Azure, [learn more about the process here.](https://azure.microsoft.com/en-us/documentation/services/virtual-machines/linux/)

1. After the VM is deployed and running, [connect](#connect) to the VM to begin interacting with R Server. 

1. At this point, you can also: 
    + [Install an R IDE](#ride)
    + Configure R Server to [operationalize your analytics](#o16n) so it acts as a deployment server and host analytic web services. 

<a name="connect"></a>

## Connect to the Virtual Machine

Many people have success connecting using an open-source SSH client. 

Use the public IP address listed in the properties page of your VM in the Azure portal to connect.

After launching, you'll be in your root directory in a terminal window. 

## Launch Microsoft R Server

To start Microsoft R Server, simple type `R` at the command prompt. The R Server console session starts.

<a name="ride"></a>

## Configure an R IDE

With Microsoft R Server installed, you can configure your favorite R integrated development environment (IDE) to point to the Microsoft R Server R executable. This way, whenever you execute your R code, you'll do so using Microsoft R Server and benefit from its proprietary packages.  R Server works well popular IDEs such as [RStudio](https://www.rstudio.com) Desktop or Server. 

#### Configure RStudio for Microsoft R Server
  1. Launch RStudio.
  1. [Update the path to R](https://support.rstudio.com/hc/en-us/articles/200486138-Using-Different-Versions-of-R).
  1. When you launch RStudio, Microsoft R Server is now the default R engine.

#### Open Ports needed to Use RStudio Server

RStudio Server uses port 8787. The default configuration for the Azure VM does not open this port. To do that, you must go to the Azure Portal and elect the proper Network Security Group. Select the **All Settings** option and choose **Inbound security rules**. Add a new rule for RStudio. Name the rule, choose **Any** for the Protocol, and add port 8787 to the destination port range. Click **OK** to save your changes. You should now be able to access RStudio using a browser.

#### Assign a Fully Qualified Domain Name to the VM for Accessing RStudio Server

No cloud service is created to contain the public resources for the VM so there is no fully qualified domain name assigned to the dynamic public IP by default. One can be created and added to the image after deployment using the Azure PowerShell. The format of the hostname is ````domainnamelabel; region;.cloudapp.azure.com````. 

For example, to add a public hostname using PowerShell for a VM named `rservercloudvm` with resource group `rservercloudrg` and desired hostname of `rservercloud`.

```
PS C:\\Users\\juser> Select-AzureSubscription -SubscriptionName "Visual Studio Ultimate with MSDN" –Current

PS C:\\Users\\juser> Switch-AzureMode -Name AzureResourceManager

PS C:\\Users\\juser> New-AzurePublicIpAddress -Name rservercloudvm -ResourceGroupName rservercloudrg -Location "South Central US" -DomainNameLabel rservercloud -AllocationMethod Dynamic
```

After adding access to port TCP/8787 to the inbound security rules, RStudio Server can be accessed at <http://rservercloud.southcentralus.cloudapp.azure.com:8787/>

Some related articles are:

+ [Azure Compute, Network, and Storage Providers for Windows applications under Azure Resource Manager deployment model](https://azure.microsoft.com/en-gb/documentation/articles/virtual-machines-azurerm-versus-azuresm/)
+ [Creating Azure VMs with ARM PowerShell cmdlets](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/05/05/creating-azure-vms-with-arm-powershell-cmdlets.aspx)

<a name="o16n"></a>

## Operationalize R Analytics with R Server on the VM

In order to operationalize your analytics with Microsoft R Server, you can [configure R Server](install/operationalize-r-server-one-box-config.md) after installation to act as a deployment server and host analytic web services. 


## Access Data in an Azure Storage Account

When you need to use data from your Azure storage account, there are several options for accessing or moving the data:

+ Copy the data from your storage account to the local file system using a utility, such as [AzCopy.](https://azure.microsoft.com/en-us/documentation/articles/storage-use-azcopy/)

+ Add the files to a file share on your storage account and then mount the file share as a network drive on your VM. For more information, see [Mounting Azure files.](https://azure.microsoft.com/en-us/documentation/articles/storage-how-to-use-files-linux/)


## Resources & Documentation

Additional documentation about Microsoft R can be found in this MSDN library using the table of contents on your left.

See these additional resources to learn about R in general:
+ [DataCamp](http://www.datacamp.com/): A free introductory and intermediate course in R, and a course on working with big data using Revolution R

+ [Stack Overflow](http://www.stackoverflow.com/): A good resource for R programming and ML tools questions

+ [Cross Validated](https://stats.stackexchange.com/): A site for questions about statistical issues in machine learning

+ [R Help mailing list](https://www.r-project.org/mail.html): The list and its archives offers a good resource of historical information

+ [MRAN website](https://mran.microsoft.com/documents/getting-started/): Many other R resources.