---

# required metadata
title: "Provision version 8.0.3 R Server (standalone) SQL Server 2016 Enterprise feature"
description: "Provision the R Server 8.0.3 Only SQL Server 2016 Enterprise VM on Azure"
keywords: 
author: "chuckheinzelman"
ms.author: "charlhe"
manager: "cgronlun"
ms.date: 12/20/2016
ms.topic: "how-to"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology:
#ms.custom: ""
---

# Provision version 8.0.3 R Server (standalone) SQL Server 2016 Enterprise feature

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

Applies To: Azure Virtual Machines, provisioned as the 8.0.3 version of "R Server Only SQL Server 2016 Enterprise"

If you already have an existing 8.0.3 version of "R Server Only SQL Server 2016 Enterprise" Virtual Machine on Azure, these instructions apply to your VM. 

A newer version (9.0.1) is the current VM image on Azure. You can no longer obtain the previous version (8.0.3). If installation and provisioning is still ahead of you, we recommend that you get the newest version. For instructions, see [Provision the R Server Only SQL Server 2016 Enterprise VM on Azure](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm).

## Provision the R Server 8.0.3 Virtual Machine

Version 8.0.3 includes DeployR Enterprise, for deploying R analytics inside applications and backend systems. For more information, see [About Deploy R](../deployr/deployr-about.md).

As a prerequisite, review this article for more information about using the portal and configuring a virtual machine. [Virtual Machines - Getting started](/azure/virtual-machines)

To create the R Server VM from the Microsoft Azure Marketplace

1. Click **Virtual machines**, and in the search box, type *R Server*.
2. Select **R Server Only SQL Server 2016 Enterprise**
3. Continue to provision the virtual machine as described in this article: [Create your first Windows virtual machine in the Azure portal](https://github.com/rgl/azure-content/blob/master/articles/virtual-machines/virtual-machines-windows-hero-tutorial.md)
4. After the VM has been created and is running, click the **Connect** button to open a connection and log into the new machine.
5. When you connect, **Server Manager** is opened by default, but no additional server configuration is required. Close **Server Manager** to get to the desktop, and proceed with the next steps: 
6. Installing additional R tools or development tools
7. Configuring DeployR

To locate the R Server VM in the Azure Classic Portal

1. Click **Virtual Machines**, and then click **NEW**.
2. In the **New** pane, **Compute** and **Virtual Machine** should already be selected.
3. Click **From Gallery** to locate the VM image. You can type *R Server* in the search box, or click **Microsoft** and then scroll down until you see **R Server Only SQL Server 2016 Enterprise**.

## Install R Tools

By default, Microsoft R Server includes all the R tools installed with a base installation of R, including RTerm and RGui. A shortcut to RGui has been added to the desktop, if you want to get started using R right away.

However, you might wish to install additional R tools, such as RStudio or Microsoft R Client. See the following links for download locations and instructions:

+ [Microsoft R Client](../r-client/what-is-microsoft-r-client.md)
+ [RStudio for Windows](https://www.rstudio.com)

After you have installed these tools, be sure to point your tools to the R Server libraries.

## Using DeployR on the Virtual Machine

Some additional steps are required to use the DeployR instance that is installed on this VM.

To configure the DeployR instance:

On the VM, open the **DeployR Administrator Utility**.
Set the DeployR administrator password as described here: [Post Installation Steps](../deployr/deployr-install-on-windows.md#post-installation-steps)
Set the DeployR Web context. For more information, see [DeployR Admin Installation in the Cloud](../deployr/deployr-admin-install-in-cloud.md)
Open the appropriate ports on the VM as described here: [Configuring Azure Endpoints](../deployr/deployr-admin-install-in-cloud.md#configuring-azure-endpoints)
Update the Windows Firewall as described here: [Updating the firewall](../deployr/deployr-admin-install-in-cloud.md#updating-the-firewall)

## Accessing Data in an Azure Storage Account

When you need to use data from your Azure storage account, there are several options for accessing or moving the data:

+ Copy the data from your storage account to the local file system using a utility, such as [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy#copy-files-in-azure-file-storage-with-azcopy-preview-version-only).
+ Add the files to a file share on your storage account and then mount the file share as a network drive on your VM. For more information, see [Mounting Azure files](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).

  ## Resources

See these additional resources to learn about R in general:

+ [DataCamp](https://www.datacamp.com/) Provides a free introductory and intermediate course in R, and a course on working with big data using Revolution R.
+ [Stack Overflow](http://stackoverflow.com/) A good resource for R programming and ML tools questions.
+ [Cross Validated](https://stats.stackexchange.com/) Site for questions about statistical issues in machine learning.
+ [R Help mailing list archives](https://www.r-project.org/mail.html) Good resource of historical information.
+ [MRAN website](https://mran.microsoft.com/documents/getting-started/) Many other R resources.

## See Also

[SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services)