---

# required metadata
title: "How to use R Server on Linux VM in Azure | Microsoft Azure"
description: "Learn how to work with R Server on Linux by using a virtual machine in Azure."
keywords: "R Server, linux, virtual machine"
author: "j-martens"
manager: "paulette.mckay"
ms.date: "08/24/2016"
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

# Frequently asked questions when using Microsoft R Server on a Linux VM on Azure

Microsoft R Server is the most broadly deployable enterprise-class analytics platform for R available today. This VM includes the Microsoft R Server 2016 (version 8.0.5) for Linux (CentOS version 6.8). More information on this version can be found at <https://msdn.microsoft.com/en-us/microsoft-r/rserver-install-linux-server>.

As part of Microsoft R Server, this VM also includes [DeployR Enterprise](deployr-about.md) for deploying R analytics inside applications and backend systems. 

## Provisioning the R Server Virtual Machine

If you are new to using Azure VMs, we recommend that you see this article for more information about using the portal and configuring a virtual machine. Virtual Machines - Getting Started

To create the R Server VM from the Microsoft Azure Marketplace:
1. Go to https://azure.microsoft.com/en-us/marketplace/
1. https://azure.microsoft.com/en-us/services/virtual-machines/
1. Click Virtual machines, and in the search box, type R Server.
1. Select R Server Only SQL Server 2016 Enterprise
1. Continue to provision the virtual machine as described in this article: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/

After the VM has been created and is running, click the **Connect** button to open a connection and log into the new machine.
When you connect, Server Manager is opened by default, but no additional server configuration is required. Close Server Manager to get to the desktop, and proceed with the next steps:
Installing additional R tools or development tools
Configuring DeployR
To locate the R Server VM in the Azure Classic Portal
Click Virtual Machines, and then click NEW.
In the New pane, Compute and Virtual Machine should already be selected.
Click From Gallery to locate the VM image. You can type R Server in the search box, or click Microsoft and then scroll down until you see R Server Only SQL Server 2016 Enterprise.

## Connecting to the Virtual Machine

Many people have success connecting using an open-source SSH client like PuTTY or Cygwin. These can be found in various places by searching with Bing.com or other engines for SSH clients. You can get the public IP address from the properties page of your VM in the Azure portal.

After launching, you will be in a terminal window in your `/home` directory. 

To start Microsoft R Server, simple type `R` at the command prompt. The R Server console session will start.

## R IDEs for Microsoft R Server on Linux

With Microsoft R Server installed, you can configure your favorite R integrated development environment (IDE) to point to the Microsoft R Server R executable. This way, whenever you execute your R code, you'll do so using Microsoft R Server and benefit from its proprietary packages.  R Server works well popular IDEs such as [RStudio](https://www.rstudio.com) Desktop or Server. 

### Configuring RStudio for Microsoft R Server
  1. Launch RStudio.
  1. [Update the path to R](https://support.rstudio.com/hc/en-us/articles/200486138-Using-Different-Versions-of-R).
     1. From the **Tools** menu, choose **Global Options**.
     1. In the  **General** tab, update the path to R to point to R executable for R Server.
  1. When you launch RStudio, Microsoft R Server will now be the default R engine.

### Opening Ports needed to use RStudio Server

RStudio Server uses port 8787. The default configuration for the Azure VM does not open this port. To do that, you will need to go to the Azure Portal and elect the proper Network Security Group. Select the All Settings option and choose Inbound security rules. Add a new rule for RStudio. Name the rule, choose Any for the Protocol, and add port 8787 to the destination port range. Click OK to save your changes. You should now be able to access RStudio using a browser.

### Assigning a Fully Qualified Domain Name to the VM for accessing RStudio Server

No cloud service is created to contain the public resources for the VM so there is no fully qualified domain name assigned to the dynamic public IP by default. One can be created and added to the image after deployment using the Azure PowerShell. The format of the hostname will be ````domainnamelabel; region;.cloudapp.azure.com````. 

Here's an example of how to add a public hostname using PowerShell for a VM named ‘rrecloudvm’ with resource group ‘rrecloudrg’ and desired hostname of ‘rrecloud’.

```
PS C:\\Users\\juser> Select-AzureSubscription -SubscriptionName "Visual Studio Ultimate with MSDN" –Current

PS C:\\Users\\juser> Switch-AzureMode -Name AzureResourceManager

PS C:\\Users\\juser> New-AzurePublicIpAddress -Name rrecloudvm -ResourceGroupName rrecloudrg -Location "South Central US" -DomainNameLabel rrecloud -AllocationMethod Dynamic
```

After adding access to port TCP/8787 to the inbound security rules, RStudio Server can be accessed at <http://rrecloud.southcentralus.cloudapp.azure.com:8787/>

Some related resources are:

+ <https://azure.microsoft.com/en-gb/documentation/articles/virtual-machines-azurerm-versus-azuresm/>
+ <http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/05/05/creating-azure-vms-with-arm-powershell-cmdlets.aspx>

## Using DeployR on the Virtual Machine

To configure the DeployR instance:

1. On the VM, open the DeployR administrator utility.

   1. [Set the DeployR administrator password](deployr-install-on-linux.md#postinstall).

   1. [Set the DeployR Web context](deployr-admin-install-in-cloud.md#enabling-deployr-on-azure).

1. [Open the appropriate ports on the VM](deployr-admin-install-in-cloud.md#configuring-azure-endpoints).

## Accessing Data in an Azure Storage Account

When you need to use data from your Azure storage account, there are several options for accessing or moving the data:

+ Copy the data from your storage account to the local file system using a utility, such as[AzCopy.](https://azure.microsoft.com/en-us/documentation/articles/storage-use-azcopy/)

+ Add the files to a file share on your storage account and then mount the file share as a network drive on your VM. For more information, see [Mounting Azure files.](https://azure.microsoft.com/en-us/documentation/articles/storage-how-to-use-files-linux/)

## Using Data in an Azure Data Lake Storage (ADLS) account

You can read data from ADLS storage using ScaleR, if you reference the storage account the same way that you would an HDFS file system, by using webHDFS. For more information, see this [setup guide](http://go.microsoft.com/fwlink/?LinkId=723452).

## Resources
Additional documentation about Microsoft R can be found in this MSDN library using the table of contents on your left.

See these additional resources to learn about R in general:
+ [DataCamp](http://www.datacamp.com/): A free introductory and intermediate course in R, and a course on working with big data using Revolution R
+ [Stack Overflow](http://www.stackoverflow.com/): A good resource for R programming and ML tools questions
+ [Cross Validated](https://stats.stackexchange.com/): A site for questions about statistical issues in machine learning
+ [R Help mailing list](https://www.r-project.org/mail.html): The list and its archives offers a good resource of historical information
+ [MRAN website](https://mran.microsoft.com/documents/getting-started/): Many other R resources.