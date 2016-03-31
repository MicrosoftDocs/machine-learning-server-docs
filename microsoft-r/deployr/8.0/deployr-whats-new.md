# DeployR Release Notes

## What's New in DeployR

This topic presents the changes and new features in DeployR version 8.0.0 as well as several prior releases.

-   [Release 8.0.0](#v0800)
-   [Release 7.4.1](#v0741)
-   [Release 7.4.0](#v0740)
-   [Release 7.3](#v0730)
-   [Release 7.2](#v0720) (DeployR Enterprise Only)
-   [Release 7.1](#v0710) (DeployR Enterprise Only)
-   [Release 7.0](#v0700) (DeployR Enterprise Only)

### Version 8.0.0

> DeployR 8.0.0 is now available for [download](https://deployr.revolutionanalytics.com/download).

DeployR 8.0.0 has been rebranded for distribution via Microsoft Corporation.

## Past Releases

For historical purposes, we've included the list of changes and additions from several releases prior to DeployR 8.0.0.

### Version 7.4.1

The following list highlights the major changes and improvements to DeployR 7.4.1.

-   DeployR Enterprise now supports for RRE 7.4.1.
-   DeployR Open now supports R 3.2.x and RRO 3.2.x.
-   DeployR is now Java 8 compatible.
-   Windows 10 is now an experimental platform for DeployR Open.
-   New [Data Scientist Getting Started](https://deployr.revolutionanalytics.com/documents/getting-started/data-scientist/) guide walks data scientists through their role and resources
-   New [Application Developer Getting Started](https://deployr.revolutionanalytics.com/documents/getting-started/application-developer/) guide walks application developers through their role and resources
-   New [Administrator Getting Started](https://deployr.revolutionanalytics.com/documents/getting-started/administrator/) guide walks DeployR admins through their role and resources
-   New [Writing Portable R Code](https://deployr.revolutionanalytics.com/documents/dev/scientist-portable-code/) guide was added for data scientists.
-   New R package: `deployrUtils`. The goal of deployrUtils is to solve several R portability issues that arise when Data Scientists develop R analytics for use in their local R environment and in the DeployR server environment. [Install from GitHub](https://github.com/deployr/deployrUtils/releases).

### Version 7.4.0

The following list highlights the major changes and improvements to DeployR 7.4.0.

-   New [R Session Process Controls](https://deployr.revolutionanalytics.com/documents/admin/security/#processcontrols) introduce fine-grain file system access controls for R sessions on Linux platforms.
-   New [DeployR Command Line Tool (CLI)](https://github.com/deployr/deployr-cli) with initial support for running DeployR-enabled example applications.
-   New and updated Web service API support as detailed in the [API Reference Guide change history](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/74changehistory.html).
-   Updated [Java, JavaScript and .NET RBroker Framework](https://github.com/deployr?query=rbroker) with additional features, patches and updated change history.
-   Updated [Java, JavaScript and .NET client libraries](https://github.com/deployr?query=client) with additional features, patches and updated change history.
-   Updated user account password security with automatic [bcrypt hashing](https://en.wikipedia.org/wiki/Bcrypt), which is resistent to rainbow table and brute-force search attacks.

### Version 7.3

The following list highlights the major changes and improvements to DeployR 7.3.

-   Support for [DeployR Enterprise and DeployR Open](https://deployr.revolutionanalytics.com/download).
-   Support for this new DeployR website providing documentation and downloads.
-   New [RBroker Framework](https://deployr.revolutionanalytics.com/docanddown/#rbroker) providing a simple yet powerful API that supports the rapid integration of on-demand R analytics inside any JavaScript, Java, or .NET application.
-   New and updated API support as detailed in the [API Reference Guide change history](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/73changehistory.html).
-   New user account password and user account locking policies as detailed in the [Security Guide](https://deployr.revolutionanalytics.com/documents/admin/security/).
-   New Administrator Diagnostic Tools that the `admin` can use to diagnose the DeployR environment to help identify unresponsive components, including the server itself, grid nodes and the database.
-   New [Server Log Events](https://deployr.revolutionanalytics.com/documents/admin/common/#logs) are now captured in the DeployR server log files. These log events provide a permanent record of API call/response events, authentication events, HTTP session events, and grid R session events.

### Version 7.2 (Enterprise Only)

-   Works with Microsoft R Server 7.2
-   A diagnostic script, which outputs basic details about your environment, identifies any unresponsive components, such as the server itself, grid nodes, databases, and so on, and gathers essential log and configuration files.

### Version 7.1 (Enterprise Only)

-   With this release, RevoDeployR is now called DeployR.

-   We’ve also introduced the DeployR Repository Manager, which is a tool that simplifies the task of managing repository files. Authenticated DeployR users can use the Repository Manager to manage repository files (R scripts, data files, and so on) as well as interact with their R scripts in a live debugging environment. Note that the script management functionality formerly found in the RevoDeployR Management Console is now contained in the Repository Manager. This tool can be accessed through the DeployR landing page.

-   The Management Console has been renamed to the Administration Console. Other changes to that console include:
    -   The Administration Console can only be accessed by the admin user.
    -   All R script management functionality, other than import and export of R scripts, has been moved to the Repository Manager.
    -   The user testmanager and the role SCRIPT\_MANAGER were obsoleted and removed when script management functionality was moved to the DeployR Repository Manager.
-   There is new and updated API support, including new directory support on the Repository APIs. Refer to the **Change History** section in the **API Reference Guide** for that release for details.

### Version 7.0 (Enterprise Only)

This release contains new features and improvements, including:

-   New and updated API support including:
    -   Support for user blackbox projects, which are a new type of secure temporary project for authenticated users
    -   Support for HTTP blackbox projects, which are a new type of secure, stateful project for anonymous users
    -   Support for creating pools of temporary projects on the `/r/project/pool` API
    -   New standardized set of parameters across all execution APIs
    -   New R script execution chaining support on all script execution APIs
    -   New role-based restricted access control for files on the Repository APIs
    -   For JavaScript developers, server-side events pushed on new `/r/event/stream` API
    -   For an overview of additions and updates to the API for this release please refer to the section **API Change History** on the documentation landing page for that release (`http://SERVER:PORT/revolution/docs/documentation/`).
-   New Event Stream Console, which is a browser-based console window for viewing `/r/event/stream` events. This console is integrated into the management console, the API Explorer tool, and the JavaScript sample applications delivered with RevoDeployR.
-   The management console was updated to include:
    -   The creation and use of custom roles to restrict access to R scripts and event streams
    -   Private, Restricted, Shared and Public access controls for R Scripts
    -   Validation of grid node configurations upon creation, update, and import
    -   New event stream access policies under **Server Policies**


