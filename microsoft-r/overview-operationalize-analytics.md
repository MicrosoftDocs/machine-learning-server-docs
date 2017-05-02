---

# required metadata
title: "Introduction to Microsoft R Server and R Client"
description: "Learn about Microsoft R features and components in R Server, R Client, R Open."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "01/05/2017"
ms.topic: "get-started-article"
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
  - r-client
  - r-server
ms.custom: ""

---

# Operationalizing Analytics

Being able to operationalize your analytics is a central capability in R Server, and one of the key reasons you would choose R Server over R Client. Formerly known as DeployR, this capability for operationalizing your code is now fully integrated into R Server. After installing R Server on select platforms (availability on all platforms is still pending), you'll have everything you need to [configure R Server to host R analytics web services and remote R sessions](operationalize/configuration-initial.md).  For details on which platforms, see [Supported platforms](rserver-install-supported-platforms.md).

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization who can then, in turn, consume the analytics. Microsoft R Server provides the operationalizing tools to deploy R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. R Server turns your R scripts into analytics web services, so R code can be easily executed by applications running on a secure server.

An operationalized R Server offers the ability to host and bundle R analytics into web services with minimal code changes. R Server accepts interactive commands through [mrsdeploy functions](mrsdeploy/mrsdeploy.md) for remote execution and web service deployment. Data scientists can use `mrsdeploy` functions  on the command line. Application developers can write code to instrument equivalent operations and integrate web services into their applications using [easy-to-consume Swagger-based APIs](operationalize/api.md) in any programming language.

You can configure R Server to operationalize analytics [on a single machine](operationalize/configuration-initial.md#onebox). It can also be [scaled](operationalize/configure-enterprise.md) for business-critical applications with multiple web and compute nodes on clustered servers for load balancing. This gives you the ability to pipeline data streams that are subsequently transformed, analyzed, and visualized into an R analytics web service.

In a Windows environment, multi-server topologies are supported through Windows clustering methodologies. Compute nodes can be made highly available using Windows server failover clusters in Active-Active mode. Web nodes can be scaled out using Windows network load balancing. R Server also supports production-grade workloads and seamless integration with popular [enterprise security solutions](operationalize/security.md).

For more information and next steps, see the articles on [the introduction to operationalizing analytics](operationalize/about.md) and [configuring R Server to operationalize](operationalize/configuration-initial.md).

> [!NOTE]
> In this context, clustered topologies are composed of standalone servers, not nodes in Hadoop or cloud services in Azure. The ability to use R Server to operationalize analytics is available in many, but not all, of the supported platforms. For the most up-to-date list, see [supported R Server platforms](rserver-install-supported-platforms.md).

## See Also

[What's new in R Server](rserver-whats-new.md)
