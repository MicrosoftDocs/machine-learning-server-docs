---

# required metadata
title: "DeployR API Reference Guide"
description: "DeployR API Reference Guide"
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "05/05/2016"
ms.topic: "article"
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
# DeployR API Reference Guide

## Overview
The DeployR API exposes the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications. This API is exposed by the DeployR server, a standards-based server technology capable of scaling to meet the needs of enterprise-grade deployments.

With the advent of DeployR, the full statistics, analytics and visualization capabilities of R can now be directly leveraged inside Web, desktop and mobile applications.

### R for Application Developers

**R Analytics Web Services**

While data scientists can work with R directly in a console window or IDE, application developers need a different set of tools to leverage R inside applications. The DeployR API exposes R analytics Web services, making the full capabilities of R available to application developers on a simple yet powerful Web services API.

**Analytics Web Service Integration**

As an application developer integrating with DeployR-managed analytics Web services, typically your interest is in executing R code, not writing it. Data scientists with R programming skills write R code. With one-click in the *DeployR Repository Manager* this R code can be turned into a DeployR-managed analytics Web service. Once R code is exposed by DeployR as a service, an application can make API calls to pass inputs to the service, execute the service and retrieve outputs from the service. Those outputs can include R object data, R graphics output such as plots and charts, and any file data written to the working directory associated the current R session.

Each time a service is executed on the API, the service makes use of an R session that is managed by DeployR as a ***project*** on behalf of the application. Depending on the nature and requirements of your application you can choose to execute services on *anonymous projects* or on *authenticated projects*.

**[Anonymous Projects](#anonymous-projects)**

1.  ***Stateless projects*** are useful when an application needs an R session to make a one-off request. It may help to think of a stateless project as a disposable R session, that is used once and then discarded. There are many types of application workflows that can benefit from working with stateless projects.
2.  ***HTTP blackbox projects*** make stateful R sessions available to *anonymous* users. HTTP blackbox projects are useful when an application needs to maintain the same R session for the duration of an anonymous user's HTTP session.

**[Authenticated Projects](#authenticated-projects)**

1.  ***Temporary projects*** are useful when an application needs to maintain the same R session for the duration of an *authenticated* user's session within the application.

2.  ***User blackbox projects*** are a special type of temporary project that limits API access on the underlying R session. User blackbox projects are most useful when an application developer wants to associate a temporary project with a user without granting that user unfettered access to the underlying R session.

3.  ***Persistent projects*** are useful when an application needs to maintain the same R session across multiple user sessions within the application. Each time a user executes R code on their persistent project the accumulated R objects in the workspace as well as files in the working directory from previous service executions are available. A persistent project remains available indefinitely to the user until they decide to explicitly delete it.

To simplify integration of R analytics Web services using the DeployR API, we provide several client libraries, which are currently available for Java, JavaScript and .NET developers. A major benefit of using these client libraries is that they simplify making calls, encoding data, and handling response markup on the API.

### Users on the API

**Authenticated Users**

One of the first steps for most typical applications using this API is to provide a mechanism for users to authenticate with the DeployR server by signing-in and signing-out of the application.

To sign-in a user must provide username and password credentials. These credentials then need to be verified by the DeployR server using the [/r/user/login](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#userlogin) call. Credentials are matched against user account data stored in the DeployR database or against user account data stored in LDAP, Active Directory or PAM directory services.

If these credentials are verified by the DeployR server, then we say that the user is an *authenticated* user. An *authenticated* user is granted access to the full API, allowing the user to work on [projects](#projects-on-the-api), submit or schedule [jobs](#jobs-on-the-api) and work with [repository-managed](#repository-on-the-api) files and scripts.

Refer to the section [Working with User APIs](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#workingusers) for a detailed description of all user-related APIs.

**Pre-Authenticated Users**

There are situations where a user may have been reliably authenticated by some external system such as Siteminder prior to accessing a DeployR-enabled Web application. These types of pre-authentication scenarios are now supported by the server. Consequently, DeployR-enabled Web applications should omit sign-in and sign-out logic when *pre-authenticated* users are expected.

**Anonymous Users**

While many applications require the security and controls associated with *authenticated* users there are cases when an application may want to offer specific services to users without ever establishing a formal verification of the user's identity.

In such situations we say that the user is an *anonymous* user. Typically an *anonymous* user is an unauthenticated visitor to a DeployR-enabled Web application.

*Anonymous* users are only granted access to a single API call, [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute). Refer to section [Working with Repository Script APIs](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscripts) for more details.

### Projects on the API

A *project* on the DeployR API represents an R session.

Most R users are accustomed to working with R interactively in an R console window. In this environment users can input commands to manipulate, analyze, visualize and interpret object and file data in the R session. The set of objects in the R session are collectively known as the *workspace*. The set of files in the R session are collectively known as the *working directory*. The R session environment also supports libraries, which allows the functionality found in R packages to be loaded by the user on-demand.

The DeployR environment supports these same set of functionalities by introducing the concept of *projects* on the API. As with working in an R console window, all operations on project APIs are synchronous, where requests are processed serially and blocked until completion.

DeployR supports a number of different types of projects, each of which is designed to support distinct workflows within client applications. The following sections discuss the different types of projects available:

#### Anonymous Projects

An *anonymous project* is a project created by an *anonymous user*. There are two types of *anonymous* project:

1.  Stateless Project

2.  HTTP Blackbox Project

*Stateless and HTTP Blackbox* projects can be created using the following API calls:

1.  [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute) | Executes a repository-managed script on an anonymous project
2.  [/r/repository/script/render](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptrender) | Executes a repository-managed script on an anonymous project and renders outputs to HTML

>*Anonymous* users are not permitted to work directly with the [Project APIs](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#overviewprojects), those APIs are only available to *authenticated* users.

**Stateless Projects**

Stateless projects are useful when an application needs to make a one-off request on an R session. Once the service request has responded with the results of the execution the R session is shutdown and permanently deleted by the server.

**HTTP Blackbox Projects**

With the introduction of HTTP *blackbox* projects, stateful R sessions are now available to *anonymous* users. HTTP blackbox projects are useful when an application needs to maintain the same R session for the duration of an *anonymous* user's HTTP session. Once the HTTP session expires or the server detects that the *anonymous* user has been idle (default: 15 minutes idle timeout) the HTTP blackbox project and all associated state are permanently deleted by the server. There can only be one HTTP blackbox project live on a HTTP session at any given time.

To execute an R script on a HTTP blackbox project, enable the ***blackbox*** parameter on the following calls:

1.  [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute) | Executes a repository-managed script on an anonymous project

2.  [/r/repository/script/render](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptrender) | Executes a repository-managed script on an anonymous project and renders outputs to HTML

There are two types of data that can be returned when executing an R script on a HTTP blackbox project:

1.  Unnamed plots generated by the R graphics device on the execution are returned as results

2.  Named workspace objects identified on the *robjects* parameter can be returned as DeployR-encoded R objects

All files in a HTTP blackbox project's working directory are completely hidden from the user.

These calls also support a ***recycle*** parameter that can be used when working with HTTP blackbox projects. When this parameter is enabled the underlying R session is recycled before the execution occurs. Recycling an R session deletes all R objects from the workspace and all files from the working directory. The ability to *recycle* a HTTP blackbox project gives an *anonymous* user control over the R session lifecycle.

To interrupt an execution on the HTTP blackbox project on the current HTTP session use the following call:

1.  [/r/repository/script/interrupt](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptinterrupt) | Interrupts an execution on a HTTP blackbox project

#### Authenticated Projects

An *authenticated project* is a project created by an authenticated user. There are three types of authenticated project:

1.  Temporary Project

2.  User Blackbox Project

3.  Persistent Project

*Authenticated projects* can be created using the following API calls:

1.  [/r/project/create](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectcreate) | Creates a new temporary, user blackbox or persistent project

2.  [/r/project/pool](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectpool) | Creates a pool of temporary projects

*Authenticated* users own projects. Each *authenticated* user can create zero, one or more projects. Each project is allocated its own workspace and working directory on the server and maintains its own set of R package dependencies along with a full R command history. A user can execute R code on a project using the [/r/project/execute/code](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutecode) and [/r/project/execute/script](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutescript) calls and retrieve the R command history for the project using the [/r/project/execute/history](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutehistory) call.

>Given an *authenticated* user can do everything an *anonymous* user can do on the API it is possible for an *authenticated* user to create and work with *anonymous* projects as described in the section [Anonymous Projects](#anonymous-projects).

**Temporary Projects**

Temporary projects are useful when an application needs to maintain the same R session for the duration of an *authenticated* user's session within the application. Once the user makes an explicit call on [/r/project/close](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectclose) or simply signs-out of the application the temporary project and all associated state are permanently deleted by the server.

A user can create a temporary project by omitting the name parameter on the [/r/project/create](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectcreate) call or they can create a pool of temporary projects using the [/r/project/pool call](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectpool).


>All unnamed projects are temporary projects.

**User Blackbox Projects**

A user blackbox project is a special type of temporary project that limits API access on the underlying R session. User blackbox projects are most useful when an application developer wants to associate a temporary project with a user without granting that user unfettered access to the underlying R session.

There are two types of data that can be returned when executing an R script on a user blackbox project:

1.  Unnamed plots generated by the R graphics device on the execution are returned as results

2.  Named workspace objects identified on the robjects parameter can be returned as DeployR-encoded R objects

All files in a user blackbox project's working directory are completely hidden from the user.

User blackbox projects can be created using the blackbox parameter on the following API calls:

1.  [/r/project/create](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectcreate) | Creates a new project

2.  [/r/project/pool](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectpool) | Creates a pool of temporary projects

User blackbox projects permit only the following subset of project-related API calls:

-  [/r/project/execute/script ](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutescript)| Executes a script on project

-  [/r/project/execute/interrupt](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecuteinterrupt) | Interrupts a code execution on project

-  [/r/project/ping](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectping) | Pings a project

-  [/r/project/recycle](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectrecycle) | Recycles a project R session

-  [/r/project/close](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectclose) | Closes a project

This set of API calls on user blackbox projects are collectively known as the *User Blackbox API Controls*.

A user blackbox project also ensure that none of the R code from the R scripts that are executed on that project is ever returned in the response markup.

**Persistent Projects**

Persistent projects are useful when an application needs to maintain the same R session across multiple user sessions within the application. A persistent project is stored indefinitely in the server unless it is explicitly deleted by the user. Whenever users return to the server their own persistent projects are available so that, users can pick up where they last left off. In this way, persistent projects can be developed over days, weeks, months even years.

The server stores the following state for each persistent project:

- Project workspace.

- Project working directory.

- Project package dependencies.

- Project R command history and associated results.

An *authenticated* user can create a persistent project by specifying a value for the *name* parameter on the /r/project/create call. Alternatively, if a user is working on a temporary project then that project can become persistent once the user makes a call on [/r/project/save](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectsave) which has the effect of naming the project.

All *named* projects are *persistent* projects.

Refer to the section [Working with Projects](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#workingprojects) for a detailed description of all *authenticated project* related APIs.

#### Project Ownership & Collaboration

As noted, each *authenticated* user can create zero, one or more projects. Each newly created project is by default privately owned, and visible only to it's creator. Such user-based privacy is a central aspect of the DeployR security model. However, there are scenarios in which a more flexible access model for projects would benefit users by facilitating collaboration among them.

**Persistent Project Read-Only Collaboration**

A project owner may wish to share a project by granting read-only access to fellow *authenticated* users on the same DeployR server. Once read-only access has been granted it facilitates discussions, demonstrations and so on.

To facilitate these kinds of workflows the [/r/project/about/update](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectaboutupdate) call allows users to toggle the read-only "shared" access controls on any of their private projects. Using this call to enable shared/read-only access on a project makes the project visible to all authenticated users. Using this call to disable shared/read-only access on a project makes the project invisible to all but the project owner(s).

**Persistent Project Read-Write Collaboration**

At times a project owner may also wish to grant read-write access on a project to fellow *authenticated* users sharing the same DeployR server so they can actively collaborate on the development of a project. This type of project collaboration would be beneficial, for example, as part of a planned multistage development or simply to permit work on the project to continue when the original project owner goes on vacation.

To facilitate these kinds of workflows a project owner can now grant authorship rights on a project to one or more *authenticated* users using the [/r/project/grant](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectgrant) call. Once a user has been granted authorship rights on a project they effectively co-own the project and can view, modify and even delete the project. Where mutiple users have been granted authorship rights on a project the server ensures that only one user can be actively working on the project at any one time.

>Deleting a project with multiple authors simply revokes authorship rights to the project for the caller. After the call, all other authors on the project still have full access to the project. If the last remaining author deletes a project, then the project is permanently deleted.

### Jobs on the API

Working with R interactively in an R console window or working with projects on the API are both examples of synchronous working environments where a user can make a request that will be blocked until processing completes and an appropriate response is generated and returned. When working with R interactively, the response is displayed as output in the R console window. When working with projects on the API, the response is well-formed markup on the response stream.

However, there are times when it can be advantageous to permit users to make requests without requiring that they wait for responses. For example, consider long-running operations that could take hours or even days to complete.

The DeployR environment supports these types of long-running operations by introducing the concept of *jobs* on the API. DeployR managed jobs support the execution of commands in the background on behalf of users.

Refer to the section [Working with Jobs](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#workingjobs) for a detailed description of all job related APIs.

### Repository on the API

The DeployR environment offers *authenticated* users versioned file storage by introducing the concept of a repository on the API. The repository provides a persistent store for user files of any type, including binary object files, plot image files, data files, simple text files and files containing blocks of R code which are referred to as [repository-managed scripts](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscripts).

Because the repository supports a versioned file system a full version history for each file is maintained and any version of a file can be retrieved upon request.

**Repository-Managed Files**

Each user has access to a private repository store. Each file placed in that store will be maintained indefinitely by the server unless it is explicitly deleted by the user.

Repository-managed files can be easily loaded by users directly into [anonymous projects](#anonymous projects), [authenticated projects](#authenticated-projects) as well as into jobs. For example, a binary object file in the repository can be loaded directly into a project workspace using the [/r/project/workspace/load](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectworkspaceload) call. A data file can be loaded directly into a project working directory using the [/r/project/directory/load](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectdirectoryload) call.

Conversely, objects in a project workspace and files in a project working directory can be stored directly to the repository using the [/r/project/workspace/store](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectworkspacestore) and [/r/project/directory/store](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectdirectorystore) calls respectively.

Repository-managed files can also be loaded directly into the workspace or working directory ahead of executing code on [anonymous projects](#anonymous-projects) or an *asynchronous job*.

Refer to the section [Working with Repository Files](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryfiles) for further details.

**Repository File Ownership & Collaboration**

By default, files in a user's private repository are visible only to that user. Such user-based privacy is a central aspect of the DeployR security model. However, there are situations that can benefit from a more flexible access model for repository-managed files.

**Repository File Read-Only Collaboration**

A user may wish to share a file in their private repository by granting read-only access to fellow *authenticated* and/or *anonymous* users. Once read-only access has been granted to users they can download and view the file. In the case of a repository-managed script users can also execute the file.

To facilitate these kinds of workflows the [/r/repository/file/update](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryfileupdate) call allows users to toggle the read-only access controls on any of their repository-managed files.

Using this call to enable *shared* access on a file makes the file visible to *authenticated* users. Using this call to enable *published* access on a file makes the file visible to *authenticated* and *anonymous* users.

**Repository File Read-Write Collaboration**

At times the owner of a repository-managed file may also wish to grant read-write access on a file so that fellow *authenticated* users who are sharing the same DeployR server can actively collaborate on the development of a file. For example, consider a situation where the owner of a repository-managed script wishes to collaborate more closely on it's design and implementation with a select group of *authenticated* users.

To facilitate this kind of collaborative workflow a user can now grant authorship rights on a repository file to one or more *authenticated* users using the [/r/repository/file/grant](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryfilegrant) call. Once a user has been granted authorship rights on a repository-managed file the user now co-owns the file and has the ability to download, modify and even delete the file.

>[!IMPORTANT]
>When modifying a repository-managed file with multiple authors it is the responsibility of the users in the collaborative workflow to ensure the overall consistency of the file.
>Deleting a repository-managed file with multiple authors simply revokes authorship rights to the file for the caller. After the call all other authors on the file will still have full access to the file. If the last remaining author deletes a repository-managed file, then the file is permanently deleted.

**Repository-Managed Scripts**

Repository-managed scripts are a special type of repository-managed file. Any file with a .r or a .R extension will be identified by the server as a repository-managed script.

These scripts are essentially blocks of R code with well-defined inputs and outputs. While scripts are technically also repository-managed files, they are designed to be exposed as an executable on the API.

Scripts can be created, managed and deployed using the standard Repository APIs or directly within the* DeployR Repository Manager*. Refer to the [Repository Manager Help](rserver/deployr-repository-manager/deployr-repository-manager-about.md) for further details.

*Authenticated* users can execute scripts within the context of any project using the /r/project/execute/script call. Both *authenticated* and *anonymous* users can execute scripts within the context of [anonymous projects](#anonymous-projects) using the [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute) and [/r/repository/script/render](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptrender) calls.

Refer to the section [Working with Repository Scripts](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscripts) for a detailed description of repository-managed script-specific APIs.

>Repository-managed scripts are files in the repository so all API calls described in the section [Working with Repository Files](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryfiles) are available to create and manage repository-managed scripts.

**Repository-Managed Shell Scripts**

The Repository Shell Script APIs provide some shell script-specific functionality for repository-managed shell scripts.

Shell scripts can be .sh, .csh, .bash, or .bat files.

While shell scripts are technically also repository-managed files, shell scripts differ from other repository-managed files as they are executable on the DeployR server.

Due to the special security concerns associated with excuting shell scripts on the DeployR server only shell scripts owned by ADMINISTRATOR users can be executed on this API call. Any attempt to execute a shell script stored in the repository that is not owned by an ADMINISTRATOR user will be rejected.

Access to repository-managed shell scripts is controlled by the standard set of private, restricted, shared and public DeployR repository access controls on a file-by-file basis.

Refer to the section [Working with Repository Shell Scripts](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryshell) for a detailed description of repository-managed shell script-specific APIs.

### Event Stream

The event stream API is unique within the DeployR API as it supports push notifications from the DeployR server to client applications. Notifications correspond to discrete events that occur within the DeployR server. Rather than periodically polling the server for updates a client application can simply subscribe once to the event stream and then receive event notifications pushed by the server.

Refer to the section [Working with Repository Shell Scripts](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryshell) for a detailed description of repository-managed shell script-specific APIs.

## DeployR Web Services API Overview

While it is not necessary to understand the internal architecture of the DeployR server in order to use this API the following overview is provided in order to lend context to server administrators intending to support the API and to client application developers intending to use the API.

![](media/deployr-api-reference-guide/deployr-api-reference-guide-1.png)

**R User Web, Desktop, Mobile Apps**

The R user web, desktop and mobile apps represent client applications built using the DeployR API. The API is a fully standardized Web services interface using JSON and XML over HTTP. This means that any piece of software that is both capable of connecting to the server and parsing either JSON or XML can become a client.

To make life easier for the client developers using this API, DeployR also provides several client libraries. These client libraries, currently available for Java, JavaScript and .NET developers, simplify making calls, encoding data and handling response markup on the API.

**DeployR Public API**

This document describes the DeployR Public API and the complete set of API services provided for [users](#users-on-the-api), [projects](#projects-on-the-api), [jobs](#jobs-on-the-api), [repository-managed](#repository-on-the-api) files and scripts and the [event stream](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#workingeventstream). Using this API directly or by taking advantage of the DeployR client libraries developers can integrate R-based analytics into their client applications.

**DeployR Administration Console**

The DeployR Administration Console is a browser-based administrators tool used to customize the deployment configuration for the server, tune server runtime behaviors, and facilitate the integration of client applications on the API. Refer to the [Administration Console documentation](rserver/deployr-admin-console/deployr-admin-console-about.md) for further details.

**DeployR Grid Management Framework**

The grid management framework provides load balancing capabilities for intensive R-compute environments. This framework manages a scalable network of collaborating nodes where each node on the grid contributes its own resources (processor, memory and disk). Each node can be leveraged by the server to execute R analyses on behalf of client applications.  For further details, refer to section [Managing the Grid](rserver/deployr-admin-console/deployr-admin-managing-the-grid.md).

**Spring 3 Framework & J2EE Container**

The Spring Framework is a lightweight, modular framework for building enterprise Java applications. The DeployR server builds on top of the Spring Framework in order to deliver advanced R-based analytics services on-demand in a secure, robust manner.

**Database**

The DeployR server leverages a database to manage the reliable persistence of all user, project and repository data. 

### Architecture Overview

The DeployR API exposes a set of services via a Web service interface using HTTP(S). You can send HTTP GET and POST requests to invoke services on the API. In a nutshell:

- HTTP GET method is expected when retrieving data on the API

- HTTP POST method is required when submitting, updating or deleting data on the API

Most API calls respond with well-formed markup. On such calls you can request the response markup in either JSON or XML format. Some API calls return binary data such as image plots, structured data like CSV files or gzip compressed archives. In all cases the type of data being returned will always be indicated by the *Content-Type* header on the response.

Additionally, each request will return a meaningful HTTP status code on the response. For a list of response codes that can be indicated on this API and that should be handled by all applications, refer to section [API Response Codes](#api-response-code-overview).

### API Parameter Overview

For each service call, there is a documented list of required and optional parameters. Parameter values are expected to be UTF-8 compliant and URL-encoded.

The DeployR API currently supports both JSON and XML formats for data exchange. Consequently, each method requires the use of the ***format*** parameter in order to specify how the request and response data on the call will be encoded.

While all parameters are specified as a name/value pair, some parameter values require complex data. Whenever complex data is required, a JSON/XML schema defines how these values are to be encoded. For more information on the schema definitions and specific examples of relevant service calls, refer to section [Web Service API Data Encodings](#encode-r-object-data-for-use-on-the-api).

### API Response Code Overview

Each DeployR API call will respond with a meaningful HTTP status code. Applications using this API should be implemented to handle each of these status codes as appropriate.

**HTTP Success Codes**

-  **200** OK

**HTTP Error Codes**

-  **400** Bad Request: invalid data on call

-  **401** Unauthorized Access: caller has insufficient privileges

-  **403** Forbidden Access: caller is unauthorized

-  **405** HTTP Method Disallowed: disallowed HTTP method on call

-  **409** Conflict: project is currently busy on call, concurrent call rejected

-  **503** Service Temporarily Unavailable: HTTP session temporarily invalidated

**API Call Status & Error Codes**

When an API call returns an HTTP 200 status code the application should still test the ***success*** property in the response markup on each call to determine whether the DeployR server was able to successfully execute the service on behalf of the caller.

If the ***success*** property in the response markup indicates failure (with a value of **false**), then the application can inspect the ***error*** and ***errorCode*** properties in the response markup to determine the underlying cause of that failure.

The ***error*** property provides a plain text message describing the underlying failure. The ***errorCode*** property indicates the nature of the underlying error. Possible values for the ***errorCode*** property are shown here:

-  **900** General Server Error: runtime error.

-  **910** Grid Resource Error: maximum number of concurrent live projects exceeded for authenticated user

-  **911** Grid Resource Error: maximum number of concurrent live projects exceeded for anonymous user

-  **912** Grid Resource Error: maximum number of concurrent live jobs exceeded for authenticated user

-  **913** Grid Resource Error: grid resources temporarily at exhaustion.

-  **914** Grid Resource Error: grid runtime boundary limit exceeded.

-  **915** Grid Resource Error: grid node unresponsive.

-  **916** Grid Resource Error: grid node R session unresponsive.

-  **917** Grid Resource Error: grid node version incompatible with server.

-  **940** Authentication Error: username/password credentials provided are invalid.

-  **941** Authentication Error: user has insufficient privileges to login on the API.

-  **942** Authentication Error: another user has already authentciated on the current HTTP session.

-  **943** Authentication Error: user account has been disabled by the system administrator

-  **944** Authentication Error: user account has been temporarily locked by the system administrator

-  **945** Authentication Error: user account password has expired, requires reset

To understand how grid resource errors occur, refer to the sections [Managing the Grid](rserver/deployr-admin-console/deployr-admin-managing-the-grid.md) and [Managing Server Policies](rserver/deployr-admin-console/deployr-admin-managing-server-policies.md).

*Sample API (JSON) response markup indicating error on call:*

	{
	    "deployr": {
	        "response": {
	            "call": "/r/user/login",
	            "success": false,
	            "error": "Bad credentials",
	            "errorCode": 900,
	            "httpcookie": "2606B516198D7B2F62B43ADBA15F29C2"
	        }
	    }
	}

### Explore the API

To help developers familiarize themselves with the full set of APIs DeployR ships with a Web-based API Explorer tool. This tool allows developers to explore the DeployR API in an interactive manner.

Refer to the documentation on the [API Explorer Tool](rserver/deployr-api-explorer-tool.md) for more details.

### API Call Overview

Each of the following *"Working with"* sections in the API Reference detail the individual API calls by category. For each call described in this reference guide, you can review the following information:

-  Call REST Endpoint

-  Call Description

-  Call HTTP Method

-  Call Request Format

-  Call Parameter List

-  Example Call Parameters

-  Example Call Response Markup (JSON)

-  Example Call Response Markup (XML)

## Encode R Object Data for use on the API

DeployR-specific encodings are used to encode R object data for use on the API.

Encoded object data can be returned from the server in the response markup on the following calls:

-  [/r/project/workspace/list](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectworkspacelist)

-  [/r/project/workspace/get](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectworkspaceget)

-  [/r/project/execute/code](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutecode)

-  [/r/project/execute/script](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutescript)

-  [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute)

Encoded object data can also be sent to the server as parameter values on the following calls:

-  [/r/project/workspace/push](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectworkspacepush)

-  [/r/project/execute/script](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutescript)

-  [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute)

Each encoded object is defined by the following properties:

- *name* - the name of the encoded object

- *type* - the DeployR-specific encoding type for the encoded object

- *rclass* - the underlying R class for the encoded object

- *value* - the data encapsulated by the encoded object

When encoded objects are returned from the server in the response markup the purpose of the type property is to indicate to developers how to decode the encapsulated data for use within a client application. For example, an encoded vector object returned on the [/r/project/workspace/get](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectworkspaceget) call will appear in the response markup as follows:

	{
		"name": "sample_vector",
		"type": "vector",
		"rclass": "character",
		"value": [
			"a",
			"b",
			"c"
		]
	}

In the preceding example, the encoded vector, named *sample_vector* , indicates a *type* property with a value of *vector*. This informs the client application that the *value* property on this encoded object will contain an array of values. Those values will match the values found in the vector object in the workspace.

When encoded objects are sent as parameters to the server, the client must name each object, and then provide values for the *type* and *value* properties for each of those objects. The *rclass* property is determined automatically by the server and need not be specified by the client application. For example, passing both an encoded logical and an encoded matrix as *inputs* on the [/r/project/workspace/push](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectworkspacepush) call is achieved as follows:

	{
		"sample_logical" : {
			"type": "primitive",
			"value" : true
		},
		"sample_matrix": {
			"type": "matrix",
			"value": [
				[
					1,
					3,
					12
				],
				[
					2,
					11,
					13
				]
			]
		}
	}

>In the case of primitive encodings the type property is optional and can therefore be omitted from the markup being sent to the server.

**DeployR Encoding Types**

The complete list of supported DeployR-encoding types is shown here. As indicated, these encodings can encode a wide range of different classes of R object:

- ***primitive*** type encodes R objects with class: character, integer, numeric and logical.

- ***date*** type encodes R objects with class: Date, POSIXct and POSIXlt.

- ***vector*** type encodes R objects with class: vector.

- ***matrix*** type encodes R objects with class: matrix.

- ***factor*** type encodes R objects with class: ordered and factor.

- ***list*** type encodes R objects with class: list.

- ***dataframe*** type encodes R objects with class: data.frame.

**Sample DeployR Encodings**

The following R code is provided for demonstration purposes. The code creates a wide range of R objects. After the following code, you will find sample JSON and XML response markup that demonstrates how each of the objects generated here is encoded on the API.

	# primitive character
	x_character <- "c"
	# primitive integer
	x_integer <- as.integer(10)

	# primitive numeric (double)
	x_numeric <- 5

	# logical
	x_logical <- T

	# Date
	x_date <- Sys.Date()

	# "POSIXct" "POSIXt"
	x_posixct <- Sys.time()

	# vector (character)
	x_character_vector <- c("a","b","c")

	# vector (integer)
	x_integer_vector <- as.integer(c(10:25))

	# vector (numeric)
	x_numeric_vector <- as.double(c(10:25))

	# matrix
	x_matrix <-  matrix(c(1,2,3, 11,12,13), nrow = 2, ncol=3)

	# ordered factor
	x_ordered_factor <- ordered(substring("statistics", 1:10, 1:10), levels=c("s","t","a","c","i"))

	# unordered factor
	x_unordered_factor <- factor(substring("statistics", 1:10, 1:10), levels=c("s","t","a","c","i"))

	#list
	x_list <- list(c(10:15),c(40:45))

	# data frame
	x_dataframe <- data.frame(c(10:15),c(40:45))

**JSON Encodings on API Response Markup**

The following response markup contains annotations that describe each of the supported DeployR R object encodings:

	{
		"deployr": {
			"response": {
				"success": true,
				"workspace": {
			  "objects" : [
			//
			//  DeployR Primitive Encodings 
			//

					{
						"name": "x_character",
						"type": "primitive",
						"rclass": "character",
						"value": "c"
					},

					{
						"name": "x_integer",
						"type": "primitive",
						"rclass": "integer",
						"value": 10
					},

					{
						"name": "x_numeric",
						"type": "primitive",
						"rclass": "numeric",
						"value": 5
					},

					{
						"name": "x_logical",
						"type": "primitive",
						"rclass": "logical",
						"value": true
					},


			//
			//  DeployR Date Encodings 
			//

					{
						"name": "x_date",
						"type": "date",
						"rclass": "date",
						"value": "2011-10-04",
						"format": "yyyy-MM-dd"
					},

					{
						"name": "x_posixct",
						"type": "date",
						"rclass": "POSIXct",
						"value": "2011-10-05",
						"format": "yyyy-MM-dd HH:mm:ss Z"
					},


			//
			//  DeployR Vector Encodings 
			//

					{
						"name": "x_character_vector",
						"type": "vector",
						"rclass": "character",
						"value": [
							"a",
							"b",
							"c"
						]
					},

					{
						"name": "x_integer_vector",
						"type": "vector",
						"rclass": "integer",
						"value": [
							10,
							11,
							12,
							13,
							14,
							15
						]
					},

					{
						"name": "x_numeric_vector",
						"type": "vector",
						"rclass": "numeric",
						"value": [
							10,
							11,
							12,
							13,
							14,
							15
						]
					},


			//
			//  DeployR Matrix Encodings 
			//

					{
						"name": "x_matrix",
						"type": "matrix",
						"rclass": "matrix",
						"value": [
							[
								1,
								3,
								12
							],
							[
								2,
								11,
								13
							]
						]
					},


			//
			//  DeployR Factor Encodings 
			//

					{
						"name": "x_ordered_factor",
						"type": "factor",
						"rclass": "ordered",
						"value": [
							"s",
							"t",
							"a",
							"t",
							"i",
							"s",
							"t",
							"i",
							"c",
							"s"
						],
						"labels": [
							"s",
							"t",
							"a",
							"c",
							"i"
						],
						"ordered": true
					},

					{
						"name": "x_unordered_factor",
						"type": "factor",
						"rclass": "factor",
						"value": [
							"s",
							"t",
							"a",
							"t",
							"i",
							"s",
							"t",
							"i",
							"c",
							"s"
						],
						"labels": [
							"s",
							"t",
							"a",
							"c",
							"i"
						],
						"ordered": false
					},


			//
			//  DeployR List Encodings 
			//

					{
						"name": "x_list",
						"type": "list",
						"rclass": "list",
						"value": [
							{   "name" : "1",
								"value": [
									10,
									11,
									12,
									13,
									14,
									15
								],
								"type": "vector",
								"rclass": "numeric"
							},
							{   "name" : "2",
								"value": [
									40,
									41,
									42,
									43,
									44,
									45
								],
								"type": "vector",
								"rclass": "numeric"
							}
						]
					},


			//
			//  DeployR Dataframe Encodings 
			//

					{
						"name": "x_dataframe",
						"type": "dataframe",
						"rclass": "data.frame",
						"value": [
							{
								"type": "vector",
								"name": "c.10.15.",
								"rclass": "numeric",
								"value": [
									10,
									11,
									12,
									13,
									14,
									15
								]
							},
							{
								"type": "vector",
								"name": "c.40.45.",
								"rclass": "numeric",
								"value": [
									40,
									41,
									42,
									43,
									44,
									45
								]
							}
						]
					}

				  ]
			},
				"call": "/r/project/workspace/get",
				"project": {
					"project": "PROJECT-00c512b6-84a4-45ff-b94a-134cca8e0584",
					"descr": null,
					"owner": "testuser",
					"live": true,
					"origin": "Project original.",
					"name": null,
					"projectcookie": null,
					"longdescr": null,
					"ispublic": false,
					"lastmodified": "Wed, 5 Oct 2011 06:31:37 +0000"
				}
			}
		}
	}

**XML Encodings on API Response Markup**

The following response markup contains annotations that describe each of the supported DeployR R object encodings:

	<deployr>
	  <response>
		<success>true</success>
		<workspace>
		  <objects>
		  //
		  //  DeployR Primitive Encodings 
		  //

		  <object>
			<name>x_character</name>
			<type>primitive</type>
			<rclass>character</rclass>
			<value>c</value>
		  </object>

		  <object>
			<name>x_integer</name>
			<type>primitive</type>
			<rclass>integer</rclass>
			<value>10</value>
		  </object>

		  <object>
			<name>x_numeric</name>
			<type>primitive</type>
			<rclass>numeric</rclass>
			<value>5.0</value>
		  </object>

		  <object>
			<name>x_logical</name>
			<type>primitive</type>
			<rclass>logical</rclass>
			<value>true</value>
		  </object>


		  //
		  //  DeployR Date Encodings 
		  //

		  <object>
			<name>x_date</name>
			<type>date</type>
			<rclass>date</rclass>
			<value>2011-10-04</value>
			<format>yyyy-MM-dd</format>
		  </object>

		  <object>
			<name>x_posixct</name>
			<type>date</type>
			<rclass>POSIXct</rclass>
			<value>2011-10-05</value>
		<format>yyyy-MM-dd HH:mm:ss Z</format>
		  </object>


		  //
		  //  DeployR Vector Encodings 
		  //

		  <object>
			<name>x_character_vector</name>
			<type>vector</type>
			<rclass>character</rclass>
			<values>
			  <value>a</value>
			  <value>b</value>
			  <value>c</value>
			</values>
		  </object>

		  <object>
			<name>x_integer_vector</name>
			<type>vector</type>
			<rclass>integer</rclass>
			<values>
			  <value>10</value>
			  <value>11</value>
			  <value>12</value>
			  <value>13</value>
			  <value>14</value>
			  <value>15</value>
			</values>
		  </object>

		  <object>
			<name>x_numeric_vector</name>
			<type>vector</type>
			<rclass>numeric</rclass>
			<values>
			  <value>10</value>
			  <value>11</value>
			  <value>12</value>
			  <value>13</value>
			  <value>14</value>
			  <value>15</value>
			</values>
		  </object>


		  //
		  //  DeployR Matrix Encodings 
		  //

		  <object>
			<name>x_matrix</name>
			<type>matrix</type>
			<rclass>matrix</rclass>
			<values>
			  <value>1.0, 3.0, 12.0</value>
			  <value>2.0, 11.0, 13.0</value>
			</values>
		  </object>


		  //
		  //  DeployR Factor Encodings 
		  //

		  <object>
			<name>x_ordered_factor</name>
			<type>factor</type>
			<rclass>ordered</rclass>
			<ordered>true</ordered>
			<values>
			  <value>s</value>
			  <value>t</value>
			  <value>a</value>
			  <value>t</value>
			  <value>i</value>
			  <value>s</value>
			  <value>t</value>
			  <value>i</value>
			  <value>c</value>
			  <value>s</value>
			</values>
			<labels>
			  <label>s</label>
			  <label>t</label>
			  <label>a</label>
			  <label>c</label>
			  <label>i</label>
			</labels>
		  </object>

		  <object>
			<name>x_unordered_factor</name>
			<type>factor</type>
			<rclass>factor</rclass>
			<values>
			  <value>s</value>
			  <value>t</value>
			  <value>a</value>
			  <value>t</value>
			  <value>i</value>
			  <value>s</value>
			  <value>t</value>
			  <value>i</value>
			  <value>c</value>
			  <value>s</value>
			</values>
			<labels>
			  <label>s</label>
			  <label>t</label>
			  <label>a</label>
			  <label>c</label>
			  <label>i</label>
			</labels>
			<ordered/>
		  </object>


		  //
		  //  DeployR List Encodings 
		  //

		  <object>
			<type>list</type>
			<rclass>list</rclass>
			<values>
			  <value>
				<name>1</name>
				<type>vector</type>
				<rclass>numeric</rclass>
				<values>
				  <value>10</value>
				  <value>11</value>
				  <value>12</value>
				  <value>13</value>
				  <value>14</value>
				  <value>15</value>
				</values>
			  </value>
			  <value>
				<name>2</name>
				<type>vector</type>
				<rclass>numeric</rclass>
				<values>
				  <value>40</value>
				  <value>41</value>
				  <value>42</value>
				  <value>43</value>
				  <value>44</value>
				  <value>45</value>
				</values>
			  </value>
			</values>
			<name>x_list</name>
		  </object>

		  //
		  //  DeployR Dataframe Encodings 
		  //

		  <object>
			<name>x_dataframe</name>
			<type>dataframe</type>
			<rclass>data.frame</rclass>
			<values>
			  <value>
				<name>c.10.15.</name>
				<type>vector</type>
				<rclass>numeric</rclass>
				<values>
				  <value>10</value>
				  <value>11</value>
				  <value>12</value>
				  <value>13</value>
				  <value>14</value>
				  <value>15</value>
				</values>
			  </value>
			  <value>
				<name>c.40.45.</name>
				<type>vector</type>
				<rclass>numeric</rclass>
				<values>
				  <value>40</value>
				  <value>41</value>
				  <value>42</value>
				  <value>43</value>
				  <value>44</value>
				  <value>45</value>
				</values>
			  </value>
			</values>
		  </object>

		  </objects>
		</workspace>
		<call>/r/project/workspace/get</call>
		<project>
		  <projectPROJECT-00c512b6-84a4-45ff-b94a-134cca8e0584</project>
		  <name/>
		  <descr/>
		  <longdescr/>
		  <live>true</live>
		  <ispublic/>
		  <projectcookie/>
		  <origin>Project original.</origin>
		  <owner>testuser</owner>
		  <lastmodified>Wed, 5 Oct 2011 06:39:52 +0000</lastmodified>
		</project>
	  </response>
	</deployr>

**Sample JSON Encodings for Input Parameters on API**

	{
		"x_character": {
			"type": "primitive",
			"value": "c"
		},
		"x_integer": {
			"type": "primitive",
			"value": 10
		},
		"x_double": {
			"type": "primitive",
			"value": 5.5
		},
		"x_logical": {
			"type": "primitive",
			"value": true
		},
		"x_date": {
			"type": "date",
			"value": "2011-10-04",
			"format": "yyyy-MM-dd"
		},
		"x_posixct": {
			"type": "date",
			"value": "2011-10-05 12:13:14 -0800",
			"format": "yyyy-MM-dd HH:mm:ss Z"
		},
		"x_character_vector": {
			"type": "vector",
			"value": [
				"a",
				"b",
				"c"
			]
		},
		"x_integer_vector": {
			"type": "vector",
			"value": [
				10,
				11,
				12
			]
		},
		"x_double_vector": {
			"type": "vector",
			"value": [
				10,
				11.1,
				12.2
			]
		},
		"x_matrix": {
			"type": "matrix",
			"value": [
				[
					1,
					3,
					12
				],
				[
					2,
					11,
					13
				]
			]
		},
		"x_ordered_factor": {
			"type": "factor",
			"ordered": true,
			"value": [
				"s",
				"t",
				"a",
				"t",
				"i",
				"s",
				"t",
				"i",
				"c",
				"s"
			],
			"labels": [
				"s",
				"t",
				"a",
				"c",
				"i"
			]
		},
		"x_unordered_factor": {
			"type": "factor",
			"ordered": false,
			"value": [
				"s",
				"t",
				"a",
				"t",
				"i",
				"s",
				"t",
				"i",
				"c",
				"s"
			],
			"labels": [
				"s",
				"t",
				"a",
				"c",
				"i"
			]
		},
		"x_list": {
			"type": "list",
			"value": [
				{
					"name": "first",
					"value": [
						10,
						11,
						12
					],
					"type": "vector"
				},
				{
					"name": "second",
					"value": [
						40,
						41,
						42
					],
					"type": "vector"
				}
			]
		},
		"x_dataframe": {
			"type": "dataframe",
			"value": [
				{
					"name": "first",
					"value": [
						10,
						11,
						12
					],
					"type": "vector"
				},
				{
					"name": "second",
					"value": [
						40,
						41,
						42
					],
					"type": "vector"
				}
			]
		}
	}

**Sample XML Encodings for Input Parameters on API**

	<inputs>
		<object>
			<name>x_character</name>
			<type>character</type>
			<value>c</value>
		</object>
		<object>
			<name>x_integer</name>
			<type>numeric</type>
			<value>10</value>
		</object>
		<object>
			<name>x_double</name>
			<type>numeric</type>
			<value>5.5</value>
		</object>
		<object>
			<name>x_logical</name>
			<type>logical</type>
			<value>true</value>
		</object>
		<object>
			<name>x_date</name>
			<type>date</type>
			<rclass>date</rclass>
			<value>2011-10-04</value>
			<format>yyyy-MM-dd</format>
		</object>
		<object>
			<name>x_posixct</name>
			<type>date</type>
			<rclass>POSIXct</rclass>
			<value>2011-10-05 12:13:14 -0800</value>
			<format>yyyy-MM-dd HH:mm:ss Z</format>
		</object>
		<object>
			<name>x_character_vector</name>
			<type>vector</type>
			<rclass>numeric</rclass>
			<values>
				<value>a</value>
				<value>b</value>
				<value>c</value>
			</values>
		</object>
		<object>
			<name>x_numeric_vector</name>
			<type>vector</type>
			<rclass>numeric</rclass>
			<values>
				<value>10</value>
				<value>11</value>
				<value>12</value>
			</values>
		</object>
		<object>
			<name>x_matrix</name>
			<type>matrix</type>
			<rclass>numeric</rclass>
			<values>
				<value>10,110</value>
				<value>11,111</value>
				<value>12,122</value>
			</values>
		</object>
		<object>
			<name>x_ordered_factor</name>
			<type>factor</type>
			<ordered>true</ordered>
			<values>
				<value>s</value>
				<value>t</value>
				<value>a</value>
				<value>t</value>
				<value>i</value>
				<value>s</value>
				<value>t</value>
				<value>i</value>
				<value>c</value>
				<value>s</value>
			</values>
			<labels>
				<label>s</label>
				<label>t</label>
				<label>a</label>
				<label>c</label>
				<label>i</label>
			</labels>
		</object>
		<object>
			<name>x_unordered_factor</name>
			<type>factor</type>
			<ordered>false</ordered>
			<values>
				<value>s</value>
				<value>t</value>
				<value>a</value>
				<value>t</value>
				<value>i</value>
				<value>s</value>
				<value>t</value>
				<value>i</value>
				<value>c</value>
				<value>s</value>
			</values>
			<labels>
				<label>s</label>
				<label>t</label>
				<label>a</label>
				<label>c</label>
				<label>i</label>
			</labels>
		</object>
		<object>
			<name>x_list</name>
			<type>list</type>
			<values>
				<value>
					<name>first</name>
					<type>vector</type>
					<rclass>numeric</rclass>
					<values>
						<value>10</value>
						<value>11</value>
						<value>12</value>
					</values>
				</value>
				<value>
					<name>second</name>
					<type>vector</type>
					<rclass>numeric</rclass>
					<values>
						<value>40</value>
						<value>41</value>
						<value>42</value>
					</values>
				</value>
			</values>
		</object>
		<object>
			<name>x_dataframe</name>
			<type>dataframe</type>
			<values>
				<value>
					<name>first</name>
					<type>vector</type>
					<rclass>numeric</rclass>
					<values>
						<value>10</value>
						<value>11</value>
						<value>12</value>
					</values>
				</value>
				<value>
					<name>second</name>
					<type>vector</type>
					<rclass>numeric</rclass>
					<values>
						<value>40</value>
						<value>41</value>
						<value>42</value>
					</values>
				</value>
			</values>
		</object>
	</inputs>

**DeployR Encodings Made Easy**

To simplify life for those client developers using The DeployR API, we provide several client libraries for Java, JavaScript and .NET developers. A major benefit of using these client libraries is that they greatly simplify the creation of DeployR-encoded object data inputs to the server and the parsing of DeployR-encoded object data as outputs from the server.

## API Change History
 
### DeployR 7.4.1

**Standard Execution Model Changes**

The DeployR API standard execution model has been updated. A new on-execution parameter has been added:

-  artifactsoff - (optional) when enabled, artfiacts generated in the working directory are neither cached to the database nor reported in the response markup

This change applies across all execution APIs:

-  [/r/project/execute/code](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutecode)

-  [/r/project/execute/script](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutescript)

-  [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute)

-  [/r/repository/script/render](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptrender)

-  [/r/job/submit](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#jobsubmit)

-  [/r/job/schedule](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#jobschedule)

### DeployR 7.4

#### Grid Cluster Targeted Executions

Grid node "cluster" names, used to denote the runtime characteristics of a node, can be assigned by the DeployR admin, using the [Administration Console](rserver/deployr-admin-console/deployr-admin-console-about.md), to individual nodes or groups of nodes on the DeployR grid, for example, "hi-mem" or "hi-cpu". This feature is **DeployR Enterprise** only.

By identifying a value on a new *cluster* parameter client applications can request tasks be executed on nodes within a specific cluster on the grid on the following calls:

-  [/r/project/create](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectcreate)

-  [/r/project/pool](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectpool)

-  [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute)

-  [/r/repository/script/render](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptrender)

-  [/r/job/submit](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#jobsubmit)

-  [/r/job/schedule](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#jobschedule)

Cluster names are case-inssensitive so "Hi-Mem" and "hi-mem" indicate the same cluster. If the cluster indicated on any of these calls fails to match a actual cluster name assigned by the admin on the DeployR grid the call will be rejected.


#### Project Phantom Executions

Phantom executions are a special type of execution on a project that avoid per-execution meta-data being created by the server in the database. This helps avoid database resource exhaustion on the server when high-throughput, long-lived project pools are in use, such as pools used by the RBroker Framework.
Client applications can request tasks be executed as phantom executions by enabling the value of the new phantom parameter on the following calls:

-  [/r/project/execute/code](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutecode)

-  [/r/project/execute/script](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutescript)

> A phantom execution will not appear in the project history returned by the /r/project/execute/history call.

#### External Repository Support

DeployR has always provided [API support](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#workingrepository) for working with repository-managed files and scripts that have been uploaded into the DeployR Default Repository . DeployR 7.4 server introduces support for a new repository store known as the DeployR External Repository .

Unlike the default repository which is backed by the MongoDB database, the new external repository is simply a directory on disk from where the DeployR server can list and load files and execute scripts.

Managing files in the external repository is as simple as creating, editing, copying, moving and deleting files on disk. All interactions with the external repository on the API are read-only, so files and scripts can be listed, loaded and even executed but not modified in any way on the API.

>The DeployR External Repository can be used to provide a seamless bridge between existing file control systems, such as git and svn, and the DeployR server.

Listing external repository-managed files and scripts is supported using a new external parameter on the following set of APIs:

-  [/r/repository/file/list](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryfilelist)

-  [/r/repository/script/list](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptlist)

-  [/r/repository/directory/list](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositorydirectorylist)

Loading and executing external repository-managed files and scripts use the existing set of filename , directory , and author parameters on the following calls:

-  [/r/project/execute/code](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutecode)

-  [/r/project/execute/script](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutescript)

-  [/r/project/directory/load](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectdirectoryload)

-  [/r/project/workspace/load](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectworkspaceload)

-  [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute)

-  [/r/repository/script/render](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptrender)

-  [/r/job/submit](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#jobsubmit)

-  [/r/job/schedule](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#jobschedule)

The base directory for the external repository can be found here on DeployR server:

	$DEPLOYR_HOME/deployr/external/repository

Each user can maintain private, shared and public files in the external repository at the following locations respectively:

	// Private external repository files.
	$DEPLOYR_HOME/deployr/external/repository/{username}
	// Shared external repository files.
	$DEPLOYR_HOME/deployr/external/repository/shared/{username}
	
	// Public external repository files.
	$DEPLOYR_HOME/deployr/external/repository/public/{username}
	
	// Note, sub-directories are supported by the external repository,
	// for example, the following files are all privately owned by testuser:
	$DEPLOYR_HOME/deployr/external/repository/testuser/plot.R
	$DEPLOYR_HOME/deployr/external/repository/testuser/demo/score.R
	$DEPLOYR_HOME/deployr/external/repository/testuser/demo/model.rData

A small number of sample files are deployed to the external repository following each new DeployR 7.4 installation and you may try out the external repository using new support found in the latest [API Explorer](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#explorer).

For more information about working with the new DeployR External Repository please post your questions to the [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr).

#### Default and External Repository Filters

Each of the following APIs have been updated to support more sophisticated filters on results:

-  [/r/repository/file/list](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryfilelist)

-  [/r/repository/directory/list](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositorydirectorylist)

Using the *directory* and *categoryFilter* independently or together the results returned on these calls can be filtered to specific subsets of available files, for example:

**(1) Show me all of the data files across all of my directories in the default repository**

	categoryFilter=data

**(2) Show me all of the files in the tutorial directory in the default repository**

	directory=tutorial

**(3) Show me just binary R files within the example-fraud-score directory in the default repository**
	
	directory=example-fraud-score
	categoryFilter=R

**(4) Show me just R scripts within my private demo directory in the external repository**

	directory=external:root:demo
	categoryFilter=script

**(5) Show me just R scripts within testuser's public plot directory in the external repository**

	directory=external:public:testuser:plot
	categoryFilter=script


#### Repository Shell Script Execution

The following new Repository Shell API has been added:

1.  [/r/repository/shell/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryshellexecute) | Executes a repository-managed shell script on the DeployR server.

For more information about working with this new API please post your questions to the [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr).


### DeployR 7.3

#### User Grid Resource Release API

The following new User API has been added:

1.  [/r/user/release](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#userrelease) | Releases server-wide grid resources held by the currently autheneticated user

#### Standard Execution Model Changes

The DeployR API [standard execution model](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#standardexecutionmodel) has been updated. A new pre-execution parameter has been added:

-  preloadbydirectory - (optional) comma-separated list of repository directory names from which to load the complete set of files in each directory into the working directory prior to execution

Also, a new on-execution parameter has been added:

-  enableConsoleEvents - (optional) when enabled R console events are delivered on the event stream for the current execution

The introduction of the enableConsoleEvents is a breaking change for those using the [event stream](#event-stream). Prior versions of DeployR enabled R console events by default and there was no way on the API to disable these events. As of 7.3, R console events on the event stream are disabled by default and if required, must be explicitely requested by enabling this new parameter.

These two changes apply across all execution APIs:

-  [/r/project/execute/code](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutecode)

-  [/r/project/execute/script](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectexecutescript)

-  [/r/repository/script/execute](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptexecute)

-  [/r/repository/script/render](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#repositoryscriptrender)

-  [/r/job/submit](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#jobsubmit)

-  [/r/job/schedule](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#jobschedule)

The new *preloadbydirectory* parameter is also supported on the following APIs:

-  [/r/project/create](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectcreate)

-  [/r/project/pool](https://deployr.revolutionanalytics.com/documents/dev/api-doc/guide/single.html#projectpool)
