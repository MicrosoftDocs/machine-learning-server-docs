---

# required metadata
title: "Machine Learning Server in the Cloud (Virtual Machines)"
description: "Machine Learning Server in the Cloud: Azure"
keywords: "Machine Learning Server, R Server, azure, virtual machine"
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "05/02/2018"
ms.topic: "conceptual"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Machine Learning Server in the Cloud

[Machine Learning Server](../what-is-microsoft-r-server.md), formerly known as Microsoft R Server, is a broadly deployable enterprise-class analytics platform for R and Python for on premises computing. 

It's also available in the cloud on Azure virtual machines (VM), and through Azure services that have embedded the R and Python libraries. This article explains your options for using Machine Learning Server in Azure.

## Azure virtual machine with Machine Learning Server

You can provision an Azure virtual machine, running either a Windows or Linux operating system, that already has Machine Learning Server with both R and Python support. 

The easiest way to provision the VM is one-click install using an ARM template. If you have an Azure subscription, you can use an ARM template that performs the following tasks: provisions a virtual machine, installs Machine Learning Server, opens port 12800, and configures the server for operationalization. 

Follow these links to get started:

+ [One-click installation of an auto-scale environment to operationalize your R analytics](https://blogs.msdn.microsoft).
+ [Machine Learning Server ARM templates on Github](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates)
com/mlserver/2017/07/07/set-up-an-auto-scale-environment-to-operationalize-your-r-analytics-with-just-one-click/)
+ [SQL Server Machine Learning Server as preconfigured Azure virtual machine on Windows](https://docs.microsoft.com/sql/advanced-analytics/r/provision-the-r-server-only-sql-server-2016-enterprise-vm-on-azure)
+ [Machine Learning Server as preconfigured Azure virtual machine on Linux or Windows](machine-learning-server-azure-vm-on-linux.md). The core engine is configured, but operationalization is not. For instructions, see [Operationalize analytics with Machine Learning Server](../what-is-operationalization.md).


## Data science VM

Azure also offers a data science VM that has a full toolset, where the R and Python packages in Machine Learning Server are just one component. For more information, see [Machine Learning Server on the Microsoft Data Science Virtual Machine](r-server-vm-data-science.md).

## Azure services

The following Azure services include R and Python packages:

+ [R Server on Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview)
