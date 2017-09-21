---

# required metadata
title: "How to use Machine Learning Server on Linux VM in Azure (Virtual Machine) - Machine Learning Server | Microsoft Docs"
description: "Learn how to work with Machine Learning Server / R Server on Linux by using a virtual machine in Azure."
keywords: "Machine Learning Server, R Server, linux, virtual machine"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
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
  - r-server
#ms.custom: ""

---

# Machine Learning Server for Linux 9.2:  Virtual Machine on Azure

>Looking for an older version? See here: [R Server 9.1](r-server-vm-azure-linux.md), [ R Server 9.0.1](r-server-vm-azure-linux-9-0-1.md), or [R Server 8.0.5](r-server-vm-azure-linux-8-0-5.md). 

Machine Learning Server, formerly known as R Server, is the most broadly deployable enterprise-class analytics platform for R and Python available today. This virtual machine (VM) includes the Machine Learning Server version 9.2.1 for Linux (CentOS/RedHat version 7.2 or Ubuntu version 16.04). More information on this version can be found at: https://msdn.microsoft.com/en-us/microsoft-r/rserver-install-linux-server. 

This VM also includes the custom [R packages](../r-reference/introducing-r-server-r-package-reference.md) and [Python libraries](../python-reference/introducing-python-package-reference.md) installed with the product that offer machine learning algorithms, R and Python helpers for deploying analytics, and portable, scalable, and distributable data analysis functions.

>[!Tip]
>We recommend that you use the new version of the Azure portal, and the Azure Marketplace. Some images are not available when browsing the Azure Gallery on the classic portal.

## Provision the Machine Learning Server Virtual Machine

If you are new to using Azure VMs, we recommend that you review [this article](https://azure.microsoft.com/en-us/documentation/services/virtual-machines/linux/) for more information about using the portal and configuring a virtual machine.

**To create the Machine Learning Server on Linux VM:**

1. Go to the Azure Portal: http://portal.azure.com.

1. Click **Virtual Machines** in the left menu.

1. Click **Add**.

1. In the search box, enter: "Machine Learning Server".

   A list of virtual machines matching this string appears.

1. Choose the Linux version from the list. 

1. Accept the terms and get started by clicking **Create**. 

1. Use the onscreen prompts and fields to configure your Machine Learning Server VM. 
   >- You need an Azure subscription to create the VM.
   >- If you are unfamiliar with the Virtual Machines on Azure, [learn more about the process here.](https://azure.microsoft.com/en-us/documentation/services/virtual-machines/linux/)

1. After the VM is deployed and running, [connect](#connect) to the VM to begin interacting with Machine Learning Server. 

1. At this point, you can also: 
    + [Install an R IDE](#ride) or a Python interpreter.

    + Configure Machine Learning Server to [operationalize your analytics](#o16n) so it acts as a deployment server and host analytic web services. 

<a name="connect"></a>

## Connect to the Virtual Machine

Many people have success connecting using an open-source SSH client. 

Use the public IP address listed in the properties page of your VM in the Azure portal to connect.

After launching, you'll be in your root directory in a terminal window. 

## Launch Machine Learning Server

To start Machine Learning Server, simple type `R` at the command prompt. The Machine Learning Server console session starts.

<a name="ride"></a>

## Configure an R IDE

With Machine Learning Server installed, you can configure your favorite R integrated development environment (IDE) to point to the Machine Learning Server R executable. This way, whenever you execute your R code, you do so using Machine Learning Server and benefit from its proprietary packages.  Machine Learning Server works well with popular IDEs such as [RStudio](https://www.rstudio.com) Desktop or Server. 

#### Configure RStudio for Machine Learning Server
  1. Launch RStudio.
  1. [Update the path to R](https://support.rstudio.com/hc/en-us/articles/200486138-Using-Different-Versions-of-R).
  1. When you launch RStudio, Machine Learning Server is now the default R engine.

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
+ [Creating Azure VMs with Azure Resource Manager PowerShell cmdlets](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/05/05/creating-azure-vms-with-arm-powershell-cmdlets.aspx)

<a name="o16n"></a>

## Operationalize R and Python Analytics with Machine Learning Server on the VM

In order to operationalize your analytics with Machine Learning Server, you can [configure Machine Learning Server](operationalize-r-server-one-box-config.md) after installation to act as a deployment server and host analytic web services. 


## Access Data in an Azure Storage Account

When you need to use data from your Azure storage account, there are several options for accessing or moving the data:

+ Copy the data from your storage account to the local file system using a utility, such as [AzCopy.](https://azure.microsoft.com/en-us/documentation/articles/storage-use-azcopy/)

+ Add the files to a file share on your storage account and then mount the file share as a network drive on your VM. For more information, see [Mounting Azure files.](https://azure.microsoft.com/en-us/documentation/articles/storage-how-to-use-files-linux/)


## Resources & Documentation

Additional documentation about Machine Learning can be found on this documentation site using the table of contents on your left.

See these additional resources to learn about R in general:
+ [DataCamp](http://www.datacamp.com/): A free introductory and intermediate course in R, and a course on working with big data using Revolution R

+ [Stack Overflow](http://www.stackoverflow.com/): A good resource for R and Python programming and ML tools questions

+ [Cross Validated](https://stats.stackexchange.com/): A site for questions about statistical issues in machine learning

+ [R Help mailing list](https://www.r-project.org/mail.html): The list and its archives offer a good resource of historical information

+ [MRAN website](https://mran.microsoft.com/documents/getting-started/): Many other R resources.