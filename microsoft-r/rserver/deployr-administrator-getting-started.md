---

# required metadata
title: "Getting Started for DeployR Administrators"
description: "Getting started for DeployR Administrators: high level introdution to DeployR for the server administrator"
keywords: ""
author: "jmartens"
manager: "Paulette.McKay"
ms.date: "03/17/2016"
ms.topic: "get-started-article"
ms.prod: "deployr"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---



# Getting Started - Administrators

## Introduction

This guide is for system administrators of DeployR, the *R Integration Server*. If you are responsible for creating or maintaining an evaluation or a production deployment of the DeployR server, then this guide is for you.

As an administrator, your key responsibilities are to ensure the DeployR server is properly provisioned and configured to meet the demands of your user community. In this context, the following policies are of central importance:

-   Server [security policies](#security), which include user authentication and authorization
-   Server [R package management policies](#package)
-   Server [runtime policies](#runtime), which affect availability, scalability, and throughput

Whenever your policies fail to deliver the expected runtime behavior or performance, you'll need to [troubleshoot](#trouble) your deployment. For that we provide diagnostic tools and numerous recommendations.

But first, you must install the server. A comprehensive installation guide with instructions for Linux, Windows and OS X deployments is available [here](deployr-installing-configuring.md).

>[!NOTE]
>For a general introduction to DeployR, read the [About DeployR](deployr-about.md) document.

## Security Policies

User access to the DeployR server and the services offered on it's [API](https://deployr.revolutionanalytics.com/documents/dev/api-doc/) are entirely under your control as the server administrator. The principal method of granting access to DeployR is to use the [Administration Console](https://deployr.revolutionanalytics.com/documents/help/admin-console) to create [user accounts](https://deployr.revolutionanalytics.com/documents/help/admin-console/#Topics/user-intro.htm) for each member of your community.

The creation of user accounts establishes a trust relationship between your user community and the DeployR server. Your users can then supply simple `username` and `password` credentials in order to verify their identity to the server. You can grant additional [permissions](https://deployr.revolutionanalytics.com/documents/help/admin-console/#Topics/role-intro.htm) on a user-by-user basis to expose further functionality and data on the server.

However, basic user authentication and authorization are only one small part of the full set of [DeployR security](deployr-admin-security.md) features, which includes full HTTPS/SSL encryption support, IP filters, and `password` format and auto-locking policies. [DeployR Enterprise edition](https://deployr.revolutionanalytics.com/download) also offers seamless integration with popular enterprise security solutions.

The full set of DeployR security features available to you as a system administrator are detailed in this [Security guide](deployr-admin-security.md).

## R Package Policies

The primary function of the DeployR server is to support the execution of R code on behalf of client applications. One of your key objectives as a DeployR administrator is to ensure a reliable, consistent execution environment for that code.

The R code developed and deployed by [data scientists](deployr-data-scientist-getting-started.md) within your community will frequently depend on one or more R packages. Those R packages may be hosted on [CRAN](http://cran.r-project.org/), [MRAN](http://go.microsoft.com/fwlink/?LinkID=698301), [github](https://github.com/), in your own local CRAN repository or elsewhere.

Making sure that these R package dependencies are available to the code executing on the DeployR server requires active participation from you, the administrator. There are two R package management policies you can adopt for your deployment, which are detailed in this [R Package Management guide](deployr-admin-r-package-management.md).

## Runtime Policies

The DeployR server supports a wide range of runtime policies that affect many aspects of the server runtime environment. As an administrator, you can select the preferred policies that best reflect the needs of your user community.

### General

The [Administration Console](https://deployr.revolutionanalytics.com/documents/help/admin-console) offers a [Server Policies](https://deployr.revolutionanalytics.com/documents/help/admin-console/#Topics/policies-intro.htm) tab within which can be found a wide range of policy configuration options, including:

-   Global settings such as server name, default server boundary, and so on
-   Project persistence policies governing resource usage (sizes and autosave)
-   Runtime policies governing authenticated, asynchronous, and anonymous operations
-   Runtime policies governing concurrent operation limits, file upload limits, and event stream access

The full set of general policy options available with the **Server Policies** tab are detailed in this [online help topic](https://deployr.revolutionanalytics.com/documents/help/admin-console/#Topics/policies-intro.htm).

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

2.  Informing your DeployR users that they must [use the R function, `deployrExternal`](deployr-admin-manage-big-data.md#external-directory-structure) in their R code to reference big data files within these directories.

For the complete configuration and usage documentation, read the guide "[Managing External Directories for Big Data](deployr-admin-manage-big-data.md)".

## Troubleshooting

There is no doubt that, as an administrator, you've experienced failures with servers, networks, and systems—most probably at the very inopportune times. Likewise, your chosen runtime policies may sometime fail to deliver the runtime behavior or performance needed by your community of users.

When those failures occur in the DeployR environment, we recommend you first turn to the [DeployR diagnostic testing tool](deployr-admin-diagnostics-troubleshooting.md) to attempt to identify the underlying cause of the problem.

Beyond the diagnostics tool, the [Troubleshooting](deployr-admin-diagnostics-troubleshooting.md#troubleshooting) documentation offers suggestions and recommendations for common problems with known solutions.

## Further Reading

This section provides a quick summary of useful links for administrators working with DeployR.

### DeployR Introduction & Download

-   [About DeployR](deployr-about.md)
-   [DeployR Server Download](https://deployr.revolutionanalytics.com/download/)
-   [Installation & Configuration](deployr-installing-configuring.md)

### Administrator Guides

-   [Getting Started for Administrators](deployr-administrator-getting-started.md)
-   [Security](deployr-admin-security.md)
-   [R Package Management](deployr-admin-r-package-management.md)
-   [Administration Console Help](https://deployr.revolutionanalytics.com/documents/help/admin-console)
-   [Server, Grid & Database High Availability](deployr-admin-configure-high-availability.md)
-   [Scale & Throughput](deployr-admin-scale-and-throughput.md)
-   [Managing External Directories for Big Data](deployr-admin-manage-big-data.md)
-   [Diagnostic Testing & Troubleshooting](deployr-admin-diagnostics-troubleshooting.md)
-   [Upgrading & Migrating Data](deployr-installing-configuring.md#upgrading-deployr)
-   [Common DeployR Administration Tasks](deployr-common-administration-tasks.md)
-   [Reinstalling Microsoft R Server, Revolution R Open, or R](deployr-admin-configure-reinstall-r.md)
-   [Using Hadoop Impersonation and DeployR](deployr-admin-hadoop-impersonation.md)

### Other Getting Started Guides

-   [For Data Scientists](deployr-data-scientist-getting-started.md)
-   [For Application Developers](deployr-application-developer-getting-started.md)

### Support Channels

-   [DeployR Forum](http://go.microsoft.com/fwlink/?LinkID=708535)


