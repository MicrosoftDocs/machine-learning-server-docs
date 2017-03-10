---

# required metadata
title: "Getting Started for DeployR Administrators"
description: "Getting started for DeployR Administrators: high level introdution to DeployR for the server administrator"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "02/08/2017"
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
ms.technology: "deployr"
ms.custom: ""

---

# Configuring: Get Started for Administrators

**Applies to:  Microsoft R Server 9.0.1 & 9.1**

>To benefit from Microsoft R Server’s deployment and operationalization features, you can configure R Server after installation to act as a deployment server and host analytic web services as well as to execute code remotely. For a general introduction to R Server for operationalization, read the [About](about.md) topic.

This guide is for system administrators of the operationalization feature in R Server. If you are responsible for creating or maintaining an evaluation or a production deployment of the R Server with the operationalization feature, then this guide is for you.

As an administrator, your key responsibilities are to ensure configuration for the operationalization feature is properly provisioned and configured to meet the demands of your user community. In this context, the following policies are of central importance:

-   Server [security policies](#security-policies), which include user authentication and authorization
-   Server [R package management policies](#r-package-policies)
-   Server [runtime policies](#runtime-policies), which affect availability, scalability, and throughput

Whenever your policies fail to deliver the expected runtime behavior or performance, you'll need to troubleshoot your deployment. For that we provide [diagnostic tools](admin-diagnostics.md) and numerous recommendations.


## Setup R Server for Operationalization

To benefit from Microsoft R Server’s web service deployment and remote execution features, you must first [configure R Server](configuration-initial.md) after installation to act as a deployment server and host analytic web services. 

All configurations have at least a single web node and single compute node:

+ A **web node** acts as an HTTP REST endpoint with which users can interact directly to make API calls. The web node accesses data in the database, and send jobs to the compute node.

+ A **compute node** is used to execute R code as a session or service. Each compute node has its own pool of R shells.

The simplest configuration is a single web node and compute node on a single machine, called a **one-box configuration**.  You can also install multiple components on multiple machines, which is referred to as an  **enterprise configuration**.

[Learn more on how to configure for operationalization.](configuration-initial.md) 


## Security Policies

User access to the R Server and the operationalization services offered on its [API](api.md) are entirely under your control as the server administrator. R Server's operationalization feature offers seamless integration with popular enterprise security solutions like Active Directory LDAP or Azure Active Directory. You can configure R Server to [authenticate](security-authentication.md) using these methods to establish a trust relationship between your user community and the operationalization engine for R Server. Your users can then supply simple `username` and `password` credentials in order to verify their identity. [A token will be issued to an authenticated user.](security-access-tokens.md)

However, authentication is only one part of the full set of [security](security.md) features, which includes full [HTTPS/SSL encryption](security-https.md) support and [CORS support](security-cors.md). 

## R Package Policies

The primary function of the operationalization feature is to support the execution of R code on behalf of client applications. One of your key objectives as an administrator is to ensure a reliable, consistent execution environment for that code.

The R code developed and deployed by data scientists within your community will frequently depend on one or more R packages. Those R packages may be hosted on [CRAN](http://cran.r-project.org/), [MRAN](http://mran.microsoft.com), [github](https://github.com/), in your own local CRAN repository or elsewhere.

Making sure that these R package dependencies are available to the code executing on R Server's operationalization feature requires active participation from you, the administrator. There are several R package management policies you can adopt for your deployment, which are detailed in this [R Package Management guide](package-management.md).

## Runtime Policies

The operationalization feature supports a wide range of runtime policies that affect many aspects of the server runtime environment. As an administrator, you can select the preferred policies that best reflect the needs of your user community.

### General

The external configuration file, `appsettings.json` defines a number of policies for the services. There is one `appsettings.json` file on each web node and on each compute node. This file contains a wide range of policy configuration options for that node. The location of this file depends on the R Server version, operating system, and the node. Learn more in this article: ["Editing the `appsettings.json` configuration file for R Server"](admin-configuration-file.md).
 
### Asynchronous Batch Sizes

Your users can perform speedy real-time and batch scoring. To reduce the risk of resource exhaustion by a single user, you can set the maximum number of operations that a single caller can execute in parallel during a specific asynchronous batch job. 

This value is defined in `"MaxNumberOfThreadsPerBatchExecution"`  property in the `appsettings.json` on the web node. If you have multiple web nodes, we recommend you set the same values on every machine. 

### Availability

The operationalization feature consists of a number of web and compute nodes that combine to deliver the full capabilities of this R operationalization server. Each component can be configured for Active-Active High Availability (HA) in order to deliver a robust, reliable runtime environment.

You can configure R Server to use multiple Web Nodes for Active-Active backup / recovery using a load balancer.

For data storage high availability, you can leverage the high availability capabilities found in enterprise grade databases (SQL Server or PostgreSQL). Learn how to use one of those databases, [here](configure-remote-database.md).

![High Availability](../media/o16n/admin-HA.png)


<!--For a discussion of the available server, grid, and database HA policy options, see the [DeployR High Availability Guide](deployr-admin-configure-high-availability.md).-->

### Scalability & Throughput

In the context of a discussion on runtime policies, the topics of scalability and throughput are closely related. Some of the most common questions that arise when planning the configuration and provisioning of R Server for operationalization are:

-   How many users can I support?
-   How many web and compute nodes do I need?
-   What throughput can I expect?

The answer to these questions will ultimately depend on the configuration and size of the configuration and node resources allocated to your deployment.

To evaluate and simulate the capacity of a configuration, use the [Evaluate Capacity tool](admin-evaluate-capacity.md). You can also [adjust the pool size](admin-evaluate-capacity.md#pool) of available R shells for concurrent operations.
<!--For detailed information and recommendations on tuning the server and grid for optimal throughput, read the [DeployR Scale & Throughput Guide](deployr-admin-scale-and-throughput.md).-->

## Troubleshooting

There is no doubt that, as an administrator, you've experienced failures with servers, networks, and systems—most probably at the very inopportune times. Likewise, your chosen runtime policies may sometime fail to deliver the runtime behavior or performance needed by your community of users.

When those failures occur in the operationalization environment, we recommend you first turn to the [diagnostic testing tool](admin-diagnostics.md) to attempt to identify the underlying cause of the problem.

Beyond the diagnostics tool, the [Troubleshooting](admin-diagnostics.md#troubleshooting) documentation offers suggestions and recommendations for common problems with known solutions.

## More Resources

This section provides a quick summary of useful links for administrators working with R Server's operationalization feature.

>Use the table of contents to find all of the guides and documentation needed by the administrator.

**Key Documents**
-   [About Operationalization](about.md)
-   [Configuration](configuration-initial.md)
-   [Security](security.md)
-   [R Package Management](package-management.md)
-   [Diagnostic Testing & Troubleshooting](admin-diagnostics.md)
-   [Capacity Evaluatation](admin-evaluate-capacity.md)

**Other Getting Started Guides**
-   [Application Developers](app-developer-get-started.md)
-   [Data Scientists](data-scientist-get-started.md)

**Support Channel**
-   [Microsoft R Server Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)
