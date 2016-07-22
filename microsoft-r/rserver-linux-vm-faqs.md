---

# required metadata
title: "Frequently asked questions when using R Server on Linux VM in Azure | Microsoft Azure"
description: "Learn how to work with R Server on Linux by using a virtual machine in Azure."
keywords: "R Server, linux, virtual machine"
author: "jeffstokes72"
manager: "paulettm"
ms.date: "07/12/2016"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: "cgronlun"
ms.suite: ""
ms.tgt_pltfrm: "na"
ms.technology: "r-server"
ms.custom: ""

---

# Frequently asked questions when using Microsoft R Server installed on a Linux VM on Azure

## Question: What is Microsoft R Server on Linux VM?

**Answer:** Microsoft R Server is the most broadly deployable enterprise-class analytics platform for R available today. This VM includes the Microsoft R Server 2016 (version 8.0.5) for Linux (CentOS version 6.8). More information on this version can be found at <https://msdn.microsoft.com/en-us/microsoft-r/rserver-install-linux-server>.

This VM also includes DeployR Enterprise for deploying R analytics inside applications and backend systems. More information on Deploy R can be found at <https://msdn.microsoft.com/en-us/microsoft-r/deployr-about>.

## Question: How do I connect to the Virtual Machine?

**Answer:** Many people have success connecting using an open-source SSH client like PuTTY or Cygwin. These can be found in various places by searching with Bing.com or other engines for SSH clients. You can get the public IP address from the properties page of your VM in the Azure portal.

## Question: I have launched my Virtual Machine, now what?

**Answer:** After launching, you will be in a terminal window in your /home directory. To start Microsoft R Server, simple type R at the command prompt. The R Server console session will start.

## Question: Is there an IDE on Linux?

**Answer:** There is no IDE provided for Microsoft R Server for Linux. RStudio is a popular third party IDE, which can be downloaded directly from the RStudio site. Instructions for downloading and installing are at RStudio.com. The single user desktop version is at <https://www.rstudio.com/products/rstudio/download/> and the Server version is available at <https://www.rstudio.com/products/rstudio/download-server/>

## Question: How do I open the ports needed to use RStudio Server?

**Answer:** RStudio Server uses port 8787. The default configuration for the Azure VM does not open this port. To do that, you will need to go to the Azure Portal and elect the proper Network Security Group. Select the All Settings option and choose Inbound security rules. Add a new rule for RStudio. Name the rule, choose Any for the Protocol, and add port 8787 to the destination port range. Click OK to save your changes. You should now be able to access RStudio using a browser.

## Question: How do I assign a fully qualified domain name to the VM for accessing RStudio Server?

**Answer:** No cloud service is created to contain the public resources for the VM so there is no fully qualified domain name assigned to the dynamic public IP by default. One can be created and added to the image after deployment using the Azure PowerShell. The format of the hostname will be ````domainnamelabel; region;.cloudapp.azure.com````. Below is an example of how to add a public hostname using PowerShell for a VM named ‘rrecloudvm’ with resource group ‘rrecloudrg’ and desired hostname of ‘rrecloud’.

PS C:\\Users\\juser> Select-AzureSubscription -SubscriptionName "Visual Studio Ultimate with MSDN" –Current

PS C:\\Users\\juser> Switch-AzureMode -Name AzureResourceManager

PS C:\\Users\\juser> New-AzurePublicIpAddress -Name rrecloudvm -ResourceGroupName rrecloudrg -Location "South Central US" -DomainNameLabel rrecloud -AllocationMethod Dynamic

After adding access to port TCP/8787 to the inbound security rules, RStudio Server can be accessed at <http://rrecloud.southcentralus.cloudapp.azure.com:8787/>

Some useful resources for this problem are here:

<https://azure.microsoft.com/en-gb/documentation/articles/virtual-machines-azurerm-versus-azuresm/>

<http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/05/05/creating-azure-vms-with-arm-powershell-cmdlets.aspx>

## Question: Where is the documentation to use Microsoft R Server on Linux?

**Answer:** The documentation for R Server can be found on MSDN at <https://msdn.microsoft.com/library/mt674634.aspx>

## Question: What other steps do I need to follow to use the DeployR instance on this VM?

**Answer:** To configure the DeployR instance, do the following:

1. Run the DeployR Administrator Utility and do the following:

   1. [Set the DeployR administrator password](deployr-install-on-linux.md#postinstall).

   1. [Set the DeployR Web context](deployr-admin-install-in-cloud.md#enabling-deployr-on-azure).

1. [Open the appropriate ports on the VM](deployr-admin-install-in-cloud.md#configuring-azure-endpoints).

## Question: How can I access data in my Azure Storage Account from the VM?

**Answer:** There are a couple of ways. One is to copy the data from your storage account to the local file system using a utility such as AzCopy: [https://azure.microsoft.com/en-us/documentation/articles/storage-use-azcopy/\#copy-files-in-azure-file-storage-with-azcopy-preview-version-only](https://azure.microsoft.com/en-us/documentation/articles/storage-use-azcopy/)

Another way is through the use of Azure Files to add a file share on your storage account and then mount the file share on your VM. For more information on this approach see: <https://azure.microsoft.com/en-us/documentation/articles/storage-how-to-use-files-linux/>

## Question: Can I access data in my Azure Data Lake Storage (ADLS) account from the VM?

**Answer:** Yes, your ADLS storage can be accessed in ScaleR as an HDFS file system through use of webHDFS. A setup guide is available here: <http://go.microsoft.com/fwlink/?LinkId=723452>