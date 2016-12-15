---

# required metadata
title: "Getting Started for DeployR Administrators"
description: "Getting started for DeployR Administrators: high level introduction to DeployR for the server administrator"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/06/2016"
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

**Applies to: DeployR 8.x**

>Looking for the new documentation for the operationalization feature in Microsoft R Server 9.0.x ? [Start here](operationalize/about.md).

This guide is for system administrators of DeployR, the *R Integration Server*. If you are responsible for creating or maintaining an evaluation or a production deployment of the DeployR server, then this guide is for you.

As an administrator, your key responsibilities are to ensure the DeployR server is properly provisioned and configured to meet the demands of your user community. In this context, the following policies are of central importance:

-   Server [security policies](#security-policies), which include user authentication and authorization
-   Server [R package management policies](#r-package-policies)
-   Server [runtime policies](#runtime-policies), which affect availability, scalability, and throughput

Whenever your policies fail to deliver the expected runtime behavior or performance, you'll need to [troubleshoot](#troubleshooting) your deployment. For that we provide diagnostic tools and numerous recommendations.

But first, you must install the server. Comprehensive installation guides are available [here](deployr-installation.md).

>For a general introduction to DeployR, read the [About DeployR](deployr-about.md) document.

## Security Policies

User access to the DeployR server and the services offered on its [API](deployr-api-reference.md) are entirely under your control as the server administrator. The principal method of granting access to DeployR is to use the [Administration Console](deployr-admin-console/deployr-admin-console-about.md) to create [user accounts](deployr-admin-console/deployr-admin-console-user-accounts.md) for each member of your community.

The creation of user accounts establishes a trust relationship between your user community and the DeployR server. Your users can then supply simple `username` and `password` credentials in order to verify their identity to the server. You can grant additional [permissions](deployr-admin-console/deployr-admin-console-permissions-with-roles.md) on a user-by-user basis to expose further functionality and data on the server.

However, basic user authentication and authorization are only one small part of the full set of [DeployR security](deployr-admin-security/deployr-security-authentication.md) features, which includes full HTTPS/SSL encryption support, IP filters, and `password` format and auto-locking policies. DeployR Enterprise also offers seamless integration with popular enterprise security solutions.

The full set of DeployR security features available to you as a system administrator are detailed in this [Security guide](deployr-admin-security/deployr-security.md).

## R Package Policies

The primary function of the DeployR server is to support the execution of R code on behalf of client applications. One of your key objectives as a DeployR administrator is to ensure a reliable, consistent execution environment for that code.

The R code developed and deployed by [data scientists](deployr-data-scientist-getting-started.md) within your community will frequently depend on one or more R packages. Those R packages may be hosted on [CRAN](http://cran.r-project.org/), [MRAN](http://mran.microsoft.com), [github](https://github.com/), in your own local CRAN repository or elsewhere.

Making sure that these R package dependencies are available to the code executing on the DeployR server requires active participation from you, the administrator. There are two R package management policies you can adopt for your deployment, which are detailed in this [R Package Management guide](deployr-admin-r-package-management.md).

## Runtime Policies

The DeployR server supports a wide range of runtime policies that affect many aspects of the server runtime environment. As an administrator, you can select the preferred policies that best reflect the needs of your user community.

### General

The [Administration Console](deployr-admin-console/deployr-admin-console-about.md) offers a [Server Policies](deployr-admin-console/deployr-admin-managing-server-policies.md) tab within which can be found a wide range of policy configuration options, including:

-   Global settings such as server name, default server boundary, and so on
-   Project persistence policies governing resource usage (sizes and autosave)
-   Runtime policies governing authenticated, asynchronous, and anonymous operations
-   Runtime policies governing concurrent operation limits, file upload limits, and event stream access

The full set of general policy options available with the **Server Policies** tab are detailed in this [online help topic](deployr-admin-console/deployr-admin-managing-server-policies.md).

### Availability

The DeployR product consists of a number of software components that combine to deliver the full capabilities of the R Integration Server: the server, the grid and the database. Each component can be configured for High Availability (HA) in order to deliver a robust, reliable runtime environment.

For a discussion of the available server, grid, and database HA policy options, see the [DeployR High Availability Guide](deployr-admin-configure-high-availability.md).

### Scalability & Throughput

In the context of a discussion on DeployR server runtime policies, the topics of scalability and throughput are closely related. Some of the most common questions that arise when planning the configuration and provisioning of the DeployR server and grid are:

-   How many users can I support?
-   How many grid nodes do I need?
-   What throughput can I expect?

The answer to these questions will ultimately depend on the configuration and size of the server and grid resources allocated to your deployment.

For detailed information and recommendations on tuning the server and grid for optimal throughput, read the [DeployR Scale & Throughput Guide](deployr-admin-scale-and-throughput.md).

## Big Data Policies

There may be times when your DeployR user community needs access to genuinely large data files, or big data.

When such files are stored in the DeployR Repository or at any network-accessible URI, the R code executing on the DeployR server can load the desired file data on-demand. However, physically moving big data is expensive both in terms of bandwidth and throughput.

To alleviate this overhead, the DeployR server supports a set of NFS-mounted directories dedicated to managing large data files. We refer to these directories as 'big data' external directories. As an administrator, you can enable this service by:

1.  [Configuring](deployr-admin-manage-big-data.md#setting-up-nfs-setup) the big data directories within your deployment.

2.  Informing your DeployR users that they must [use the R function, `deployrExternal`](deployr-admin-manage-big-data.md#adding-files-to-external-directories) in their R code to reference big data files within these directories.

For the complete configuration and usage documentation, read the guide "[Managing External Directories for Big Data](deployr-admin-manage-big-data.md)".

## Troubleshooting

There is no doubt that, as an administrator, you've experienced failures with servers, networks, and systems—most probably at the very inopportune times. Likewise, your chosen runtime policies may sometime fail to deliver the runtime behavior or performance needed by your community of users.

When those failures occur in the DeployR environment, we recommend you first turn to the [DeployR diagnostic testing tool](deployr-admin-diagnostics-troubleshooting.md) to attempt to identify the underlying cause of the problem.

Beyond the diagnostics tool, the [Troubleshooting](deployr-admin-diagnostics-troubleshooting.md#troubleshooting) documentation offers suggestions and recommendations for common problems with known solutions.


## More Resources

This section provides a quick summary of useful links for administrators working with DeployR.

>Use the table of contents to find all of the guides and documentation needed by the administrator.

**Key Documents**
-   [About DeployR](deployr-about.md)
-   [Installation & Configuration](deployr-installation.md)
-   [Security](deployr-admin-security/deployr-security.md)
-   [R Package Management](deployr-admin-r-package-management.md)
-   [Scale & Throughput](deployr-admin-scale-and-throughput.md)
-   [Diagnostic Testing & Troubleshooting](deployr-admin-diagnostics-troubleshooting.md)

**Other Getting Started Guides**
-   [Application Developers](deployr-application-developer-getting-started.md)
-   [Data Scientists](deployr-data-scientist-getting-started.md)

**Support Channel**
-   [Microsoft R Server (and DeployR) Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)
