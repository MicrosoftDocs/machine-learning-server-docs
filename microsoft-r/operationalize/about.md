---

# required metadata
title: "Operationalize R Analytics with R Server | Microsoft R Server Docs"
description: "Operationalize R Analytics with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/10/2017"
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
  - deployr
  - r-server
ms.custom: ""

---

# About operationalizing analytics with R Server

**Applies to:  Microsoft R Server 9.x**  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (Find "DeployR 8.x" docs [here](../deployr-about.md).)

Operationalization refers to the process of publishing R models and code to Microsoft R Server in the form of web services and the consumption of these services within client applications to affect business results.

Today, more and more businesses are adopting advanced analytics for mission critical decision making in areas such as fraud detection, healthcare and manufacturing. Typically, data scientists first build the predictive models, and only then can businesses deploy those models in a production environment and consume them for predictive actions. 

Being able to operationalize your analytics is a central capability in R Server. Formerly known as DeployR, this capability for operationalizing your code is fully integrated into R Server. After installing R Server on select platforms, you'll have everything you need to [configure R Server to host R analytics web services and remote R sessions](admin-get-started.md).  For details on which platforms, see [Supported platforms](../rserver-install-supported-platforms.md).

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization who can then, in turn, consume the analytics. Microsoft R Server provides the operationalizing tools to deploy R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. R Server turns your R scripts into analytics web services, so R code can be easily executed by applications running on a secure server.

>[!Important]
>R Server 9.x is **not backwards compatible** with DeployR 8.x. There is no migration path as the APIs are completely new and the data stored in the database is structured differently. Check out [this table](../rserver-whats-new.md#8vs9) to see  the main differences between Microsoft R Server 9.x configured to operationalize analytics and the add-on DeployR 8.0.5 which was available in R Server 8.0.5.

## Solving long development lifecycles

R is a great modeling tool, but **the challenge lies in how to effectively operationalize R**. Traditionally, this has not been an easy process (slow innovation and error-prone) and it can take months to rewrite these models before you can use them. 

![Engine](../media/o16n/about-traditional-challenge.png) 

Introducing Microsoft R Server, the deployment engine for your advanced R analytics. Regardless of the source, language or method, you can simplify, deploy, and realize the promise and power of advanced analytics.

## What you get

After you [configure R Server to operationalize](admin-get-started.md), you can: 

||Key Features|
|-|-|
|![1](../media/o16n/about-1.png)|● Data scientists turn R analytics into Web services with one line of code<br>● Developers use Swagger-based [REST APIs](api.md) that are [easy to consume](app-developer-get-started.md) <br>&nbsp; &nbsp; with any programming languages including R|
|![2](../media/o16n/about-2.png)|● Model in one platform, then deploy and score web services in another platform:<br>&nbsp; &nbsp; [Windows, SQL, Linux/Hadoop](admin-get-started.md) <br>● Model on-premises, then score your data in the cloud, or vice versa <br>● With Microsoft R Server, it is easier/faster to use the power of R in production<br>&nbsp; &nbsp; to unlock insights hidden in your data |
|![3](../media/o16n/about-3.png)|● Perform fast scoring: real time & batch <br>● Scale to a grid for powerful computing with load balancing<br>● Use [diagnostic](admin-diagnostics.md) and [capacity evaluation](admin-evaluate-capacity.md) tools|
|![4](../media/o16n/about-4.png)|● Integrate with [enterprise authentication (AD/LDAP or Azure AD)](security-authentication.md)<br>● Connect securely: [HTTPS with SSL/TLS 1.2](security-https.md)<br>● Enterprise grade high availability|

An operationalized R Server offers the ability to host and bundle R analytics into web services with minimal code changes. R Server accepts interactive commands through [mrsdeploy functions](../mrsdeploy/mrsdeploy.md) for remote execution and web service deployment. Data scientists can use `mrsdeploy` functions  on the command line. Application developers can write code to instrument equivalent operations and integrate web services into their applications using [easy-to-consume Swagger-based APIs](api.md) in any programming language.

## Actors in Operationalizing Analytics

Microsoft R Server offers the **best-in-class deployment** experience for the administrator, data scientists, and application developers alike. 

![Personas](../media/o16n/about-personas.png)

+ **Administrators** The [easy configuration](admin-get-started.md) of Microsoft R Server for operationalizing analytics includes enterprise grade security and reliability on many platforms. It scales for business-critical applications and offers support for production-grade workloads and high availability. [Diagnostic](admin-diagnostics.md) and [capacity evaluation](admin-evaluate-capacity.md) tools are provided to help you tune and manage. R Server's engine seamlessly integrates with popular enterprise security solutions such as [LDAP/Active Directory and Azure Active Directory](security-authentication.md). Connections can also be secured [SSL/TLS 1.2](security-https.md). 

+ **Data Scientists** In a single line of code, data scientists can deploy  models or any arbitrary R code as analytic web services. 

+ **Application Developers** Using their favorite development environment, application developers can easily integrate those web services into their apps using Swagger-based [REST APIs](api.md) with any programming languages including R. They can perform fast scoring: real time & batch. 

<br>

<div align=center><iframe width="560" height="315" src="https://www.youtube.com/embed/1Nvs6QShWqY" frameborder="0" allowfullscreen></iframe></div>

## Configuration

You can configure R Server to operationalize analytics [on a single machine](configuration-initial.md#onebox) or be [scaled across multiple machines](configure-enterprise.md) for business-critical applications with multiple web and compute nodes on clustered servers for load balancing. This gives you the ability to pipeline data streams that are subsequently transformed, analyzed, and visualized into an R analytics web service.

In a Windows environment, multi-server topologies are supported through Windows clustering methodologies. Compute nodes can be made highly available using Windows server failover clusters in Active-Active mode. Web nodes can be scaled out using Windows network load balancing. R Server also supports production-grade workloads and seamless integration with popular [enterprise security solutions](admin-get-started.md#security). In this context, clustered topologies are composed of standalone servers, not nodes in Hadoop or cloud services in Azure.

The ability to use R Server to operationalize analytics is available in many, but not all, of the supported platforms. For the most up-to-date list, see [supported R Server platforms](../rserver-install-supported-platforms.md).

## Next steps

Now that you know about R Server's capability to operationalize your analytics, explore these articles to learn more.

+ [What's new in R Server](../rserver-whats-new.md)
+ [Administrator Get Started](admin-get-started.md)
+ [Data Scientist Get Started](data-scientist-get-started.md)
+ [Application Developer Get Started](app-developer-get-started.md)