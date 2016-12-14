---

# required metadata
title: "Getting Started for DeployR Administrators"
description: "Getting started for DeployR Administrators: high level introdution to DeployR for the server administrator"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/20/2016"
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

# Getting Started - Administrators

**Applies to:  Microsoft R Server 9.0.1**

This guide is for system administrators of R Server. If you are responsible for creating or maintaining an evaluation or a production deployment of the R Server with the operationalization feature, then this guide is for you.

As an administrator, your key responsibilities are to ensure configuration for the operationalization feature is properly provisioned and configured to meet the demands of your user community. In this context, the following policies are of central importance:

-   Server [security policies](#security-policies), which include user authentication and authorization
-   Server [R package management policies](#r-package-policies)
-   Server [runtime policies](#runtime-policies), which affect availability, scalability, and throughput

Whenever your policies fail to deliver the expected runtime behavior or performance, you'll need to troubleshoot your deployment. For that we provide [diagnostic tools](admin-utility.md#test) and numerous recommendations.

But first, you must [configure R Server for operationalization](configuration-initial.md). 

>For a general introduction to R Server for operationalization, read the [About](about.md) topic.

## Security Policies

User access to the R Server and the operationalization services offered on its [API](api.md) are entirely under your control as the server administrator. R Server's operationalization feature offers seamless integration with popular enterprise security solutions like Active Directory LDAP or Azure Active Directory. You can configure R Server to [authenticate](security-authentication.md) using these methods to establish a trust relationship between your user community and the operationalization engine for R Server. Your users can then supply simple `username` and `password` credentials in order to verify their identity. [A token will be issued to an authenticated user.](security-access-tokens.md)

However, authentication is only one part of the full set of [security](security.md) features, which includes full [HTTPS/SSL encryption](security-https.md) support and [CORS support](security-cors.md). 

## R Package Policies

The primary function of the operationalization feature is to support the execution of R code on behalf of client applications. One of your key objectives as an administrator is to ensure a reliable, consistent execution environment for that code.

The R code developed and deployed by data scientists within your community will frequently depend on one or more R packages. Those R packages may be hosted on [CRAN](http://cran.r-project.org/), [MRAN](http://mran.microsoft.com), [github](https://github.com/), in your own local CRAN repository or elsewhere.

Making sure that these R package dependencies are available to the code executing on R Server's operationalization feature requires active participation from you, the administrator. There are several R package management policies you can adopt for your deployment, which are detailed in this [R Package Management guide](package-management.md).

## Runtime Policies

The the operationalization feature supports a wide range of runtime policies that affect many aspects of the server runtime environment. As an administrator, you can select the preferred policies that best reflect the needs of your user community.

<!--### General-->

The external configuration file, `appsettings.json` defines a number of policies for the services. There is one `appsettings.json` file on each web node and on each compute node. This file contains a wide range of policy configuration options for that node, including:

-   Global settings such as server name, declared compute nodes, authentication properties, remote database connection information, and so on
-   Runtime policies governing pool size

On Windows, `appsettings.json` is located here  where `<MRS_home>` is the path to the Microsoft R Server installation directory. To find this path, enter `normalizePath(R.home())` in your R console:

   + Web node, find it under: `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\` 
   
   + Compute node, find it under: `<MRS_home>\deployr\Microsoft.DeployR.Server.BackEnd\`  

On Linux, `appsettings.json` is located here:

   + Web node, find it under: `/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/` 
   
   + Compute node, find it under: `/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.BackEnd/` 
  

<!--### Availability-->

<!--The DeployR product consists of a number of software components that combine to deliver the full capabilities of the R Integration Server: the server, the grid and the database. Each component can be configured for High Availability (HA) in order to deliver a robust, reliable runtime environment.

For a discussion of the available server, grid, and database HA policy options, see the [DeployR High Availability Guide](deployr-admin-configure-high-availability.md).-->

<!--### Scalability & Throughput-->

<!--In the context of a discussion on runtime policies, the topics of scalability and throughput are closely related. Some of the most common questions that arise when planning the configuration and provisioning of R Server for operationalization are:

-   How many users can I support?
-   How many web and compute nodes do I need?
-   What throughput can I expect?

The answer to these questions will ultimately depend on the configuration and size of the configuration and node resources allocated to your deployment.

For detailed information and recommendations on tuning the server and grid for optimal throughput, read the [DeployR Scale & Throughput Guide](deployr-admin-scale-and-throughput.md).-->

## Troubleshooting

There is no doubt that, as an administrator, you've experienced failures with servers, networks, and systems—most probably at the very inopportune times. Likewise, your chosen runtime policies may sometime fail to deliver the runtime behavior or performance needed by your community of users.

When those failures occur in the operationalization environment, we recommend you first turn to the [diagnostic testing tool](admin-utility.md#test) to attempt to identify the underlying cause of the problem.

<!--Beyond the diagnostics tool, the [Troubleshooting](deployr-admin-diagnostics-troubleshooting.md#troubleshooting) documentation offers suggestions and recommendations for common problems with known solutions.-->

## More Resources

This section provides a quick summary of useful links for administrators working with R Server's operationalization feature.

>Use the table of contents to find all of the guides and documentation needed by the administrator.

**Key Documents**
-   [About Operationalization](about.md)
-   [Configuration](configuration-initial.md)
-   [Security](security.md)
-   [R Package Management](package-management.md)
<!---   [Scale & Throughput](deployr-admin-scale-and-throughput.md)-->
-   [Diagnostic Testing & Troubleshooting](admin-utility.md#test)

**Other Getting Started Guides**
-   [Application Developers](app-developer-get-started.md)
-   [Data Scientists](deployr-data-scientist-getting-started.md)

**Support Channel**
-   [Microsoft R Server Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)
