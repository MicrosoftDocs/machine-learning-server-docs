---

# required metadata
title: "DeployR Client Library Tutorial | DeployR 8.x"
description: "DeployR Client Library Tutorial"
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

# Client Library Tutorial

**Applies to: DeployR 8.x**   (See [comparison between 8.x and 9.x](../rserver-whats-new.md#8vs9))

>Looking for docs for Microsoft R Server 9? [Start here](../deployr-repository-manager/about.md).

## Introduction

The DeployR API exposes a wide range of R analytics services to client application developers. These services are exposed using standards based JSON and are delivered as Web services over HTTP(s). This standards based approach makes it possible to integrate DeployR services within just about any client application environment.

To further simplify the integration of DeployR services within client applications several client libraries are provided for Java, JavaScript and .NET developers. These native client libraries provide a number of significant advantages over working directly with the raw API, including simplified service calls, encoding of call parameter data, and automatic handling of response markup on the API.

>**Try Out Our Examples!** Explore the client library examples for [Java, ](https://github.com/Microsoft/java-example-client-basics) [Javascript,](https://github.com/Microsoft/js-client-library/releases) and [.NET.](https://github.com/Microsoft/dotnet-client-library) Find them under the `examples` directory of each Github repository. Additionally, find more comprehensive examples for [Java](https://github.com/microsoft/?utf8=%E2%9C%93&query=java-example) and [JavaScript](https://github.com/microsoft/?utf8=✓&query=js-example).

>Check out the [*RBroker Framework*](deployr-tools-and-samples.md) for a simple yet powerful alternative to working with the client libraries. The framework handles a lot of the complexity in building real world client applications so you don't have to.

### API Overview

This section briefly introduces the top-level R analytics services exposed on the DeployR API:

-   **User Services @ /r/user/***

    Providing the basic authentication mechanisms for end-users and client applications that need to avail of [*authenticated services*](#authenticated-services) on the API.

-   **Project Services @ /r/project/***

    Providing [*authenticated services*](#authenticated-services) related to stateful, and optionally persistent, R session environments and analytics Web service execution.

-   **Background Job Services @ /r/job/***

    Providing [*authenticated services*](#authenticated-services) related to persistent R session environments for scheduled or periodic analytics Web service executions.

-   **Repository Services @ /r/repository/***

    Providing [*authenticated services*](#authenticated-services) related to R script, model and data file persistence plus *authenticated* and [*anonymous services*](#anonymous-services) related to analytics Web service execution.

All services on the DeployR API are documented in detail in the [API Reference Guide](deployr-api-reference.md).

### Hello World Example

The following code snippets provide the ubiquituous "Hello World" example for the client libraries, demonstrating the basic programming model used when working with the client libraries in Java and JavaScript, and C\# respectively.

**Java:**

    // 1. Establish a connection to DeployR.
    //
    // This example assumes the DeployR server is running on localhost.
    //
    String deployrEndpoint = "http://localhost:<PORT>/deployr";
    RClient rClient = RClientFactory.createClient(deployrEndpoint);

    //
    // 2. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R.
    //
    RScriptExecution exec =
        rClient.executeScript("regression", "demo", "george", null);

    //
    // 3. Retrieve the analytics Web service execution results.
    //
    // An RScriptExecution provides a client application access to
    // results including R console output, generated plots or files
    // and DeployR-encoded R object workspace data.
    //

    String console = exec.about().console;
    List<RProjectResult> plots = exec.about().results;
    List<RProjectFile> files = exec.about().artifacts;
    List<RData> objects = exec.about().workspaceObjects;

**JavaScript:**

    // 1. Establish a connection to DeployR.
    //
    // This example assumes the DeployR server is running on localhost.
    //
    deployr.configure({ host: 'http://localhost:<PORT>' });

    //
    // 2. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R.
    //
    deployr.io('/r/repository/script/execute')
      .data({ author: 'george', directory: 'demo', filename: 'regression' })
      .end(function(res) {
         //
         // 3. Retrieve the analytics Web service execution results.
         //
         var exec = res.get('execution');
         var workspace = res.get('workspace');
         var console = exec.console;
         var plots = exec.results;
         var files = exec.artifacts;
         var objects = workspace.objects;
      });

**C#:**

    // 1. Establish a connection to DeployR.
    //
    // This example assumes the DeployR server is running on localhost.
    //
    String deployrEndpoint = "http://localhost:8050/deployr";
    RClient rClient = RClientFactory.createClient(deployrEndpoint);

    //
    // 2. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R.
    //
    RScriptExecution exec = rClient.executeScript("regression", "demo", "george", "");

    //
    // 3. Retrieve the analytics Web service execution results.
    //
    // An RScriptExecution provides a client application access to
    // results including R console output, generated plots or files
    // and DeployR-encoded R object workspace data.
    //

    String console = exec.about().console;
    List<RProjectResult> plots = exec.about().results;
    List<RProjectFile> files = exec.about().artifacts;
    List<RData> objects = exec.about().workspaceObjects;

>**Try Out Our Examples!** After downloading the client library, you can find basic examples that complement this tutorial under the `rbroker/examples` directory. Additionally, find more comprehensive examples for [Java](https://github.com/microsoft/?utf8=%E2%9C%93&query=java-example) and [JavaScript](https://github.com/microsoft/?utf8=✓&query=js-example).

## Getting Connected

The first step for any client application developer using the client libraries is to establish a connection with the DeployR server. A connection is established as follows:

**Java:**

    // 1. Establish a connection to DeployR.
    //
    // The RClientFactory is provided to simplify the establishment and
    // eventual release of RClient connections.
    //
    // This example assumes the DeployR server is running on localhost.
    //
    String deployrEndpoint = "http://localhost:<PORT>/deployr";
    RClient rClient = RClientFactory.createClient(deployrEndpoint);

**JavaScript:**

    // 1. Establish a connection to DeployR.
    //
    // deployr.configure( { host: '' } ) is provided to simplify the establishment
    // of client connections.
    //
    // This example assumes the DeployR server is running on http://localhost:<PORT>.

    //
    // Browser - Same Origin does not need the deployr.configure({ host: '' }) step.
    // Browser - Cross-origin resource sharing (CORS) requests do.
    //
    deployr.configure( { host: 'http://DIFFERENT_DOMAIN:<PORT>', cors: true });

    //
    // Node.js -  No CORS is needed in Node.js
    //

**C#:**

    #!/usr/bin/env node
    deployr.configure( { host: 'http://localhost:<PORT>' });
    deployr.configure( { host: 'http://DIFFERENT_DOMAIN:<PORT>' });

    // 1. Establish a connection to DeployR.
    //
    // The RClientFactory is provided to simplify the establishment and
    // eventual release of RClient connections.
    //
    // This example assumes the DeployR server is running on localhost.
    //
    String deployrEndpoint = "http://localhost:8050/deployr";
    RClient rClient = RClientFactory.createClient(deployrEndpoint);

## Authentication

Once a connection to the DeployR server has been established the next step for a client application developer is to decide if end-users or the application itself needs access to [*authenticated services*](#api-overview) on the API.

If *authenticated project*, *background job* or *repository* management services are needed by the application then the application must first authenticate. If an application only uses [*anonymous services*](#anonymous-services) then the application can operate *anonymously*, without ever authenticating.

The following code snippets demonstrate how to authenticate using the client libraries:

**Java:**

    // 1. Authenticate an end-user or client application.
    //
    // The RBasicAuthentication supports basic username/password authentication.
    // The RUser returned represents an authenticated end-user or application.
    //
    RAuthentication authToken = new RBasicAuthentication("george", "s3cret");
    RUser rUser = rClient.login(authToken);

**JavaScript:**

    // Same Origin Request
    //
    // 1. Authenticate an end-user or client application.
    //
    // deployr.io('/r/user/login') supports basic username/password authentication.
    // The JSON returned represents an authenticated end-user or application.
    //
    deployr.io('/r/user/login')
      .data({ username: 'george', password: 's3cret' })
      .end(function(res) {
           var rUser = res.deployr.response.user;
      });

    //
    // Cross-origin resource sharing (CORS) Request
    //
    deployr.configure( { host: 'http://dhost:<PORT>', cors: true })
      .io('/r/user/login')
      .data({ username: 'george', password: 's3cret' })
      .end(function(res) {
           var rUser = res.deployr.response.user;
      });

**C#:**

    // 1. Authenticate an end-user or client application.
    //
    // The RBasicAuthentication supports basic username/password authentication.
    // The RUser returned represents an authenticated end-user or application.
    //
    RAuthentication authToken = new RBasicAuthentication("george", "s3cret");
    RUser rUser = rClient.login(authToken);

>Authenticated users not only have access to *authenticated services* on the API, they also have much broader access to R scripts, models and data files stored in the repository compared to *anonymous* users.

## Authenticated Services

An *authenticated* user or simply an authenticated client application has access to the full range of *authenticated services* offered on the DeployR API. These services are categorized as follows:

1.  [Authenticated Project Services](#project-services)
2.  [Background Job Services](#background-job-services)
3.  [Repository Management Services](#repository-services)

The following sections introduce the services themselves and demonstrate how the client libraries make these services available.

<a name="authenticated-projects"></a>
### Project Services

A project is simply a DeployR-managed R session. Any project created by an authenticated user is referred to as an [Authenticated Project](deployr-api-reference.md#authenticated-projects). There are three types of *authenticated project*:

1.  Temporary Project - a stateful, transient R session offering unrestricted API access that lives only for the duration of the current user HTTP session or until explicitly closed.

2.  User Blackbox Project - a stateful, transient R session offering restricted API access that lives only for the duration of the current user HTTP session.

3.  Persistent Project - a stateful, persistent R session offering unrestricted API access can can live indefiitely, across multiple user HTTP sessions.

Each type of *authenticated project* is provided to support distinct workflows within client applications.

Project Services are most easily understood when considered as a collection of related services:

1.  Project Creation Services
2.  Project Execution Services
3.  Project Workspace Services
4.  Project Directory Services
5.  Project Package Services

The following sections demonstrate working with some of the key features associated with each family of services within the client libraries.

#### 1. Project Creation Services

These services support the creation of *authenticated projects*. The following code snippets demonstrate some of the ways the client libraries make these services available.

**Java:**

    //
    // 1. Create a temporary project.
    //
    RProject blac = rUser.createProject();

    //
    // 2. Create a user blackbox project.
    //
    ProjectCreationOptions options = new ProjectCreationOptions();
    options.blackbox = true;
    RProject blackbox  = rUser.createProject(options);

    //
    // 3. Create a persistent project.
    //
    // Simply naming a project created by an authenticated user causes
    // that project to become a persistent project.
    //
    RProject persistent = rUser.createProject("Demo Project",
                                "Sample persistent project");

    //
    // 4. Create a pool of temporary projects.
    //
    // Project pools can be very convenient when a client application needs
    // to service multiple concurrent users or requests. The _options_ parameter
    // can be used to custom initialize all projects in the pool.
    //
    List<RProject> projectPool = rUser.createProjectPool(poolSize, options);

**JavaScript:**

    // 1. Create a temporary project.
    //
    deployr.io('/r/project/create')  
      .end(function(res) {
           var project = res.deployr.response.project;
      });

    //
    // 2. Create a user blackbox project.
    //
    deployr.io('/r/project/create')  
      .data({ blackbox: true })
      .end(function(res) {
           var blackbox = res.deployr.response.project;
      });

    //
    // 3. Create a persistent project.
    //
    // Simply naming a project created by an authenticated user causes
    // that project to become a persistent project.
    //
    deployr.io('/r/project/create')  
      .data({
        projectname: 'Demo Project',
        projectdesrc: 'Sample persistent project'
      })
      .end(function(res) {
           var persistent = res.deployr.response.project;
      });

    //
    // 4. Create a pool of temporary projects.
    //
    // Project pools can be very convenient when a client application needs
    // to service multiple concurrent users or requests. The _options_ parameter
    // can be used to custom initialize all projects in the pool.
    //
    deployr.io('/r/project/pool')  
      .data({ poolsize: poolsize }) // additional options
      .end(function(res) {
           var projectPool = res.deployr.response.projects;
      });

**C#:**

    // 1. Create a temporary project.
    //
    RProject blac = rUser.createProject();

    //
    // 2. Create a user blackbox project.
    //
    ProjectCreationOptions options = new ProjectCreationOptions();
    options.blackbox = true;
    RProject blackbox  = rUser.createProject(options);

    //
    // 3. Create a persistent project.
    //
    // Simply naming a project created by an authenticated user causes
    // that project to become a persistent project.
    //
    RProject persistent = rUser.createProject("Demo Project",
                                "Sample persistent project");

    //
    // 4. Create a pool of temporary projects.
    //
    // Project pools can be very convenient when a client application needs
    // to service multiple concurrent users or requests. The _options_ parameter
    // can be used to custom initialize all projects in the pool.
    //
    List<RProject> projectPool = rUser.createProjectPool(poolSize, options);

#### 2. Project Execution Services

These services support the execution of analytics Web services on *authenticated projects*. The following code snippets demonstrate some of the ways the client libraries make these services available.


**Java:**

    // 1. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R.
    //
    RProjectExecution exec =
        rProject.executeScript("regression", "demo", "george", null);

    //
    // 2. Execute an analytics Web service based on an arbitrary block
    // of R code: [codeBlock]
    //
    RProjectExecution exec = rProject.executeCode(codeBlock);

    //
    // 3. Execute an analytics Web service based on a URL-addressable
    // R script: [regressionURL]
    //
    RProjectExecution exec = rProject.executeExternal(regressionURL);

    //
    // 4. Retrieve the analytics Web service execution results.
    //
    // Regardless of what style of execution service is used the RProjectExecution
    // response provides access to results including R console output, generated
    // plots or files and DeployR-encoded R object workspace data.
    //
    String console = exec.about().console;
    List<RProjectResult> plots = exec.about().results;
    List<RProjectFile> files = exec.about().artifacts;
    List<RData> objects = exec.about().workspaceObjects;

**JavaScript:**

    // 1. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R.
    //
    deployr.io('/r/project/execute/script')  
      .data({
        filename: 'regression',
        directory: 'demo',
        author: 'george',
        project: rProject
      })
      .end(function(res) {
         var exec = res.deployr.response.execution;
      });

    //
    // 2. Execute an analytics Web service based on an arbitrary block
    // of R code: [codeBlock]
    //
    deployr.io('/r/project/execute/code')  
      .data({ code: codeBlock, project: rProject })
      .end(function(res) {
         var exec = res.deployr.response.execution;
      });

    //
    // 3. Execute an analytics Web service based on a URL-addressable
    // R script: [regressionURL]
    //
    deployr.io('/r/project/execute/code')  
      .data({ project: rProject, externalsource: regressionURL })
      .end(function(res) {
         var exec = res.deployr.response.execution;
      });

    //
    // 4. Retrieve the analytics Web service execution results.
    //
    // Regardless of what style of execution service is used /r/project/execution/*
    // responses provide access to results including R console output, generated
    // plots or files and DeployR-encoded R object workspace data.
    //
    var about = res.deplor.response;
    var console = about.execution.console;
    var plots = about.execution.results;
    var files = about.execution.artifacts;
    var objects = about.workspace.objects;

**C#:**

    // 1. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R.
    //
    ProjectExecutionOptions options = new ProjectExecutionOptions();
    RProjectExecution exec = rProject.executeScript("regression", "demo", "george", options);

    //
    // 2. Execute an analytics Web service based on an arbitrary block
    // of R code: [codeBlock]
    //
    exec = rProject.executeCode(codeBlock);

    //
    // 3. Execute an analytics Web service based on a URL-addressable
    // R script: [regressionURL]
    //
    exec = rProject.executeExternal(regressionURL, options);

    //
    // 4. Retrieve the analytics Web service execution results.
    //
    // Regardless of what style of execution service is used the RProjectExecution
    // response provides access to results including R console output, generated
    // plots or files and DeployR-encoded R object workspace data.
    //
    String console = exec.about().console;
    List<RProjectResult> plots = exec.about().results;
    List<RProjectFile> files = exec.about().artifacts;
    List<RData> objects = exec.about().workspaceObjects;

>All executions services support a standard execution model defined on the DeployR API.

#### 3. Project Workspace Services

These services support the manipulation and management of R workspace objects within *authenticated projects*. The following code snippets demonstrate some of the ways the client libraries make these services available.


**Java:**

    //
    // 1. Create R object data in the workspace by executing an arbitrary
    // block of R code.
    //
    RProjectExecution exec = rProject.executeCode("x <- rnorm(5)");

    //
    // 2. Create R object data in the workspace by pushing DeployR-encoded data
    // from the client application.
    //
    // 2.1 Prepare sample R object vector data.
    // 2.2 Use RDataFactory to encode the sample R object vector data.
    // 2.3 Push encoded R object into the workspace.
    //
    List<Double> vectorValues = Arrays.asList(10, 11, 12, 13, 14);
    RData encodedVector =
        RDataFactory.createNumericVector("samplev", vectorValues);
    rProject.pushObject(encodedVector);

    //
    // 3. Retrieve the DeployR-encoding of an R object from the workspace.
    //
    RData encodedVector = rProject.getObject("samplev");

    //
    // 4. Store R object data from the workspace as a binary object file (.rData)
    // in the repository.
    //
    RRepositoryFile repositoryFile =
        rProject.storeObject("samplev", "Object data description.",
                                         true, null, false, false);


**JavaScript:**

    // **Sequential execution while remaining asynchronous**

    // --------------------------------------------------------------------------
    // 1. Create R object data in the workspace by executing an arbitrary
    // block of R code.
    // --------------------------------------------------------------------------
    deployr.io('/r/project/execute/code')  
      .data({ project: rProject, code: 'x <- rnorm(5)' })
      .end()
      // --------------------------------------------------------------------------
      // 2. Create R object data in the workspace by pushing DeployR-encoded data
      // from the client application.
      //
      // 2.1 Prepare sample R object vector data.
      // 2.2 Use RDataFactory to encode the sample R object vector data.
      // 2.3 Push encoded R object into the workspace.
      // --------------------------------------------------------------------------
      .io('/r/project/workspace/push')  
      .data({ project: rProject })
      .rinput({
         type: deployr.RDataType.RNUMERIC_VECTOR,
         name: 'samplev',
         value: [10, 11, 12, 13, 14]
      })
      .end()
      // --------------------------------------------------------------------------
      // 3. Retrieve the DeployR-encoding of an R object from the workspace.
      // --------------------------------------------------------------------------
      .io('/r/project/workspace/get')  
      .data({ project: rProject, name: 'samplev' })
      .end()
      // --------------------------------------------------------------------------
      // 4. Store R object data from the workspace as a binary object file (.rData)
      // in the repository.
      // --------------------------------------------------------------------------
      .io('/r/project/directory/store')  
      .data({
        filename: 'samplev',
        desrc: 'Object data description.',
        project: rProject,
        newversion: true
      })
      .end();

**C#:**

    //
    // 1. Create R object data in the workspace by executing an arbitrary
    // block of R code.
    //
    RProjectExecution exec = rProject.executeCode("x <- rnorm(5)");

    //
    // 2. Create R object data in the workspace by pushing DeployR-encoded data
    // from the client application.
    //
    // 2.1 Prepare sample R object vector data.
    // 2.2 Use RDataFactory to encode the sample R object vector data.
    // 2.3 Push encoded R object into the workspace.
    //
    List<Double?> vectorValues = new List<Double?>(){10, 11, 12, 13, 14};
    RData encodedVector = RDataFactory.createNumericVector("samplev", vectorValues);
    rProject.pushObject(encodedVector);

    //
    // 3. Retrieve the DeployR-encoding of an R object from the workspace.
    //
    encodedVector = rProject.getObject("samplev");

    //
    // 4. Store R object data from the workspace as a binary object file (.rData)
    // in the repository.
    //
    RRepositoryFile repositoryFile = rProject.storeObject("samplev", "Object data description.",
                                         true, false, false, null);

#### 4. Project Directory Services

These services support the manipulation and management of R working directory files within *authenticated projects*. The following code snippets demonstrate some of the ways the client libraries make these services available.


**Java:**

    //
    // 1. Transfer a url-addressable file into the working directory.
    //
    // Demonstrates transferring a file from fileURL location into the working
    // directory. In this example, the name of the file created in the working
    // directory is data.csv.
    //
    DirectoryUploadOptions options = new DirectoryUploadOptions();
    options.filename = "data.csv";
    RProjectFile file = rProject.transferFile(fileURL, options);

    //
    // 2. Load a repository-managed file into the working directory.
    //
    RRepositoryFile repoFile =
        rUser.fetchFile("data.csv", "demo", "george", null);
    RProjectFile projectFile = rProject.loadFile(repoFile);

**JavaScript:**

    //
    // 1. Transfer a url-addressable file into the working directory.
    //
    deployr.io('/r/project/directory/transfer')  
      .data({ project: rProject, url: fileURL }) // additional options
      .end();

    //
    // 2. Load a repository-managed file into the working directory.
    //
    deployr.io('/r/repository/file/fetch')  
      .data({ filename: 'data.csv', directory: 'demo', author: 'george' })
      .end()
      .io('/r/project/directory/load')  
      .data({ project: rProject, filename: 'data.csv' })
      .end();

**C#:**

    //
    // 1. Transfer a url-addressable file into the working directory.
    //
    DirectoryUploadOptions options = new DirectoryUploadOptions();
    RProjectFile file = rProject.transferFile(fileURL, options);

    //
    // 2. Load a repository-managed file into the working directory.
    //
    RRepositoryFile repoFile = rUser.fetchFile("data.csv", "demo", "george", "");
    RProjectFile projectFile = rProject.loadFile(repoFile);

#### 5. Project Package Services

These services support the manipulation and management of R packages within *authenticated projects*. The following code snippets demonstrate some of the ways the client libraries make these services available.


**Java:**

    //
    // 1. List R packages attached on the project.
    //
    List<RProjectPackage> pkgs = rProject.listPackages(false);

    //
    // 2. Attach an R package to the project.
    //
    List<RProjectPackage> pkgs = rProject.attachPackage("ggplot2", "???");

**JavaScript:**

    //
    // 1. List R packages attached on the project.
    //
    deployr.io('/r/project/package/list')
      .data({ project: rProject })
      .end(function(res) {
         var pkgs = res.deployr.response.packages;
      });

    //
    // 2. Attach an R package to the project.
    //
    deployr.io('/r/project/package/attach')
      .data({ project: rProject, name: 'ggplot2' })
      .end(function(res) {
         var pkgs = res.deployr.response.packages;
      });

**C#:**

    //
    // 1. List R packages attached on the project.
    //
    List<RProjectPackage> pkgs = rProject.listPackages(false);

    //
    // 2. Attach an R package to the project.
    //
    pkgs = rProject.attachPackage("ggplot2", "???");

### Background Job Services

A background job is simply a request to execute an R analytics Web services in the background, possibly at some future time and date. By default, the result of that execution will be stored as a *persistent project* on behalf of the *authenticated* user making the request.

>By default, each background job execution stores it's results in a *persistent project*. Persistent projects are discussed in the [*Authenticated Project Service*](#project-services) section.

Each job moves through a simple lifecyle on the server, starting with submission, followed by queueing, running and eventually reaching a completion state indicating success or failure. The status of each background job can be queried, jobs can be cancelled and when a job completes the *authenticated* user that sumitted the job can collect the results of the execution.

The following code snippets demonstrate some of the ways the client libraries make these services available.

**Java:**

    //
    // 1. Submit a background job for scheduled execution of an analytics Web
    // service based on a repository-managed R script: /george/demo/regression.R.
    //
    // Note: the options parameter is optional but can be used on any job
    // execution call to:
    //
    // (a) Schedule the current job for execution at some future time/date.
    // (b) Set scheduling priority level for the current job.
    // (c) Request the automatic storage of specific job results to the
    // repository, such as binary R object or file data.
    //
    JobExecutionOptions options = new JobExecutionOptions();
    options.priority = JobExecutionOptions.HIGH_PRIORITY;

    RJob job =
        rUser.submitJobScript("Sample Job", "Sample Description",
                              "regression", "demo", "george", null, options);

    //
    // 2. Submit a background job for scheduled execution of an analytics Web
    // service based on an arbitrary block of R code: [codeBlock]
    //
    RJob job = rUser.submitJobCode("Sample Job", "Sample description.",
                                                    codeBlock, options);

    //
    // 3. Submit a background job for scheduled execution of an analytics Web
    // service based on a URL-addressable R script: [regressionURL]
    //
    RJob job = rUser.submitJobExternal("Sample Job", "Sample description.",
                                                    regressionURL, options);

    //
    // 4. Query that execution status of a background job.
    //
    RJobDetails details = job.query();

    if(details.status.equals(RJob.COMPLETED)) {

        //
        // Retrieve the results of the background job execution, typically
        // found within the associated RProject.
        //
        RProject project = rUser.getProject(details.project);
    }

**JavaScript:**

    //
    // 1. Submit a background job for scheduled execution of an analytics Web
    // service based on a repository-managed R script: /george/demo/regression.R.
    //
    // Note: the options parameter is optional but can be used on any job
    // execution call to:
    //
    // (a) Schedule the current job for execution at some future time/date.
    // (b) Set scheduling priority level for the current job.
    // (c) Request the automatic storage of specific job results to the
    // repository, such as binary R object or file data.
    //
    deployr.io('/r/job/submit')
      .data({
         rscriptauthor: 'george'
         rscriptdirectory: 'demo',
         name: 'regression',
         priority: 'high' // `low` (default), `medium` or `high`
      })
      .end(function(res) {
         var job = res.deployr.response.job;
      });

    //
    // 2. Submit a background job for scheduled execution of an analytics Web
    // service based on an arbitrary block of R code: [codeBlock]
    //
    deployr.io('/r/job/submit')
      .data({ name: 'Sample Job', descr: 'Sample description.', code: codeBlock })
      .end(function(res) {
         var job = res.deployr.response.job;
      });

    //
    // 3. Submit a background job for scheduled execution of an analytics Web
    // service based on a URL-addressable R script: [regressionURL]
    //
    deployr.io('/r/job/submit')
      .data({
         name: 'Sample Job',
         descr: 'Sample description.',
         externalsource: regressionURL
      })
      .end(function(res) {
         var job = res.deployr.response.job;
      });

    //
    // 4. Query that execution status of a background job.
    //
    deployr.io('/r/job/query')
      .data({ job: rJob })
      .end(function(res) {
         var details = res.deployr.response.job;

         if (details.status === deployr.RJob.COMPLETED) {
            //
            // Retrieve the results of the background job execution, typically
            // found within the associated RProject.
            //
            deployr.io('/r/project/about')
              .data({ project: details.project })
              .end();
         }
      });

**C#:**

    //
    // 1. Submit a background job for scheduled execution of an analytics Web
    // service based on a repository-managed R script: /george/demo/regression.R.
    //
    // Note: the options parameter is optional but can be used on any job
    // execution call to:
    //
    // (a) Schedule the current job for execution at some future time/date.
    // (b) Set scheduling priority level for the current job.
    // (c) Request the automatic storage of specific job results to the
    // repository, such as binary R object or file data.
    //
    JobExecutionOptions options = new JobExecutionOptions();
    options.priority = JobExecutionOptions.HIGH_PRIORITY;

    RJob job = rUser.submitJobScript("Sample Job", "Sample Description",
                                "regression", "demo", "george", null, options);

    //
    // 2. Submit a background job for immediate execution of an analytics Web
    // service based on an arbitrary block of R code: [codeBlock]
    //
    job = rUser.submitJobCode("Sample Job", "Sample description.", codeBlock, options);

    //
    // 3. Submit a background job for immediate execution of an analytics Web
    // service based on a URL-addressable R script: [regressionURL]
    //
    job = rUser.submitJobExternal("Sample Job", "Sample description.", regressionURL, options);

    //
    // 4. Query that execution status of a background job.
    //
    RJobDetails details = job.query();

    if(details.status == RJob.COMPLETED) {

        //
        // Retrieve the results of the background job execution, typically
        // found within the associated RProject.
        //
        RProject project = rUser.getProject(details.project);
    }
    }

>All executions services support the standard execution model defined on the DeployR API.

### Repository Services

The [Repository Manager](../deployr-repository-manager/deployr-repository-manager-about.md) is a tool, delivered as an easy-to-use Web interface, that serves as a bridge between the R scripts, models, and data created in existing analytics tools and the deployment of that work into the DeployR repository to support the development of client applications and integrations.

That tool uses the full range of *repository services* on the DeployR API to deliver it's many *file* and *directory* related functionalities. Your own applications can also leverage these sames services as needed. The following code snippets demonstrate some of the ways the client libraries make these services available.


**Java:**

    //
    // 1. List repository-managed directories belonging to the authenticated
    // user.
    //
    List<RRepositoryDirectory> dirs = rUser.listDirectories();

    //
    // 2. List repository-managed directories along with file listings
    // per directory belonging to authenticated user.
    //
    List<RRepositoryDirectory> dirs =
        rUser.listDirectories(true, false, false, false);
    for(RRepositoryDirectory dir : dirs) {
        List<RRepositoryFile> files = dir.about().files;
    }

    //
    // 3. List repository-managed files, independent of directories, belonging
    // to the authenticated user.
    //
    List<RRepositoryFile> files = rUser.listFiles();
    for(RRepositoryFile file : files) {
        URL fileURL = file.download();
    }

    //
    // 4. List repository-managed files, independent of directories, belonging
    // to the authenticated user and also include files shared or published by
    // other users.
    //
    List<RRepositoryFile> repoFiles = rUser.listFiles(false, true, true);

    //
    // 5. Upload a file to the repository on behalf of the authenticated
    // user.
    //
    InputStream fis = new FileInputStream(new File("report.pdf"));
    RepoUploadOptions options = new RepoUploadOptions();
    options.filename = "report.pdf";
    options.directory = "examples";
    options.descr = "Quarterly report.";
    RRepositoryFile repoFile = rUser.uploadFile(fis, options);


**JavaScript:**

    //
    // 1. List repository-managed directories belonging to the authenticated user.
    //
    deployr.io('/r/repository/directory/list')
      .end(function(res) {
         var dirs = res.deployr.repository.directories;
      });

    //
    // 2. List repository-managed directories along with file listings
    // per directory belonging to authenticated user.
    //
    deployr.io('/r/repository/directory/list')
      .data({ userfiles: true })
      .end(function(res) {
         var dirs = res.deployr.repository.directories;
         dirs.forEach(function(dir) {
            var files = dir.files;
         });
      });

    //
    // 3. List repository-managed files, independent of directories, belonging
    // to the authenticated user.
    //
    deployr.io('/r/repository/file/list')
      .end(function(res) {
         var files = res.deployr.repository.files;
         files.forEach(function(file) {
             deployr.io('/r/repository/file/download')
               .data({ filename: file.filename })
               .end();
         });
      });

    //
    // 4. List repository-managed files, independent of directories, belonging
    // to the authenticated user and also include files shared or published by
    // other users.
    //
    deployr.io('/r/repository/file/list')
      .data({ shared: true, published: true })
      .end(function(res) {
        var repoFiles = res.deployr.repository.files;
      });

    //
    // Browser
    //
    // 5. Upload a file to the repository on behalf of the authenticated user.
    //
    // Assuming element on page:
    // `<input id="report-pdf" type="file">`
    //
    var file = document.getElementById('report-pdf').files[0];

    deployr.io('/r/repository/file/upload')
      .data({
         filename: 'report.pdf',
         directory: 'examples',
         descr: 'Quarterly report.'
      })
      .attach(file)
      .end(function(res) {
        var repoFile = res.deployr.repository.file;
      });

    //
    // Node.js
    //
    // 5. Upload a file to the repository on behalf of the authenticated user.
    //

    #!/usr/bin/env node
    deployr.io('/r/repository/file/upload')
      .data({
         filename: 'report.pdf',
         directory: 'examples',
         descr: 'Quarterly report.'
      })
      .attach('report.pdf')
      .end(function(res) {
        var repoFile = res.deployr.repository.file;
      });

**C#:**

    //
    // 1. List repository-managed directories belonging to the authenticated
    // user.
    //
    List<RRepositoryDirectory> dirs = rUser.listDirectories();

    //
    // 2. List repository-managed directories along with file listings
    // per directory belonging to authenticated user.
    //
    dirs = rUser.listDirectories(true, false, false, false, "");
    foreach(RRepositoryDirectory dir in dirs)
    {
        List<RRepositoryFile> rfiles = dir.about().files;
    }

    //
    // 3. List repository-managed files, independent of directories, belonging
    // to the authenticated user.
    //
    List<RRepositoryFile> files = rUser.listFiles();
    foreach(RRepositoryFile file in files)
    {
        String fileURL = file.download();
    }

    //
    // 4. List repository-managed files, independent of directories, belonging
    // to the authenticated user and also include files shared or published by
    // other users.
    //
    List<RRepositoryFile> repoFiles = rUser.listFiles(false, true, true);

    //
    // 5. Upload a file to the repository on behalf of the authenticated
    // user.
    //
    String fileName = "report.pdf";
    RepoUploadOptions options = new RepoUploadOptions();
    options.filename = "report.pdf";
    options.directory = "examples";
    options.descr = "Quarterly report.";
    RRepositoryFile repoFile = rUser.uploadFile(fileName, options);

>See the [Working with the Repository APIs](deployr-api-reference.md#repository) chapter for detailed information regarding working with repository-managed files and directories.

## Anonymous Services

An *anonymous* user, being any user that has not authenticated with the DeployR server, only has access to *anonymous services* offered on the DeployR API. There is just one single services category for *anonymous services*, which is [Anonymous Project Services](#anonymous-services)

A project is simply a DeployR-managed R session. Any project created by an anonymous user is known as an *anonymous project*. In practice, anonymous projects are automatically created by DeployR on behalf of anonymous users each time they execute an analytics Web service.

There are two types of *anonymous project*:

1.  Stateless Project - a stateless, transient R session that lives only for the duration of the analytics Web service execution.
2.  HTTP Blackbox Project - a stateful, transient R session that lives only for the duration of the current user HTTP session.

>While *anonymous project services* are provided primarily for *anonymous* users, these same service calls are also available to *authenticated* users.

The following code snippets demonstrate how the client libraries make these services available.


**Java:**

    //
    // 1. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R using a Stateless Project.
    //
    RScriptExecution exec =
        rClient.executeScript("regression", "demo", "george", null);

    //
    // 2. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R using a HTTP Blackbox Project.
    //
    AnonymousProjectExecutionOptions options =
        new AnonymousProjectExecutionOptions();
    options.blackbox = true;
    RScriptExecution exec =
        rClient.executeScript("regression", "demo", "george", null, options);

    //
    // 3. Execute an analytics Web service based on a URL-addressable
    // R script: [regressionURL] using a Stateless Project.
    //
    // Note: while this style of execution uses a Stateless Project it is only
    // available to authenticated users that have been granted POWER_USER
    // permissions by the DeployR administrator.
    //
    RScriptExecution exec = rClient.executeExternal(regressionURL, options);

**JavaScript:**

    //
    // 1. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R using a Stateless Project.
    //
    deployr.io('/r/repository/script/execute')
      .data({ author: 'george', directory: 'demo', filename: 'regression' })
      .end(function(res) {
        var exec = res.deployr.repository.execution;
      });

    //
    // 2. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R using a HTTP Blackbox Project.
    //
    deployr.io('/r/repository/script/execute')
      .data({
         author: 'george',
         directory: 'demo',
         filename: 'regression',
         blackbox: true
      })
      .end(function(res) {
        var exec = res.deployr.repository.execution;
      });

    //
    // 3. Execute an analytics Web service based on a URL-addressable
    // R script: [regressionURL] using a Stateless Project.
    //
    // Note: while this style of execution uses a Stateless Project it is only
    // available to authenticated users that have been granted POWER_USER
    // permissions by the DeployR administrator.
    //
    deployr.io('/r/repository/script/execute')
      .data({ externalsource: regressionURL })    
      .end(function(res) {
        var exec = res.deployr.repository.execution;
      });

**C#:**

    //
    // 1. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R using a Stateless Project.
    //
    RScriptExecution exec =
        rClient.executeScript("regression", "demo", "george", "");

    //
    // 2. Execute an analytics Web service based on a repository-managed
    // R script: /george/demo/regression.R using a HTTP Blackbox Project.
    //
    AnonymousProjectExecutionOptions options =
        new AnonymousProjectExecutionOptions();
    options.blackbox = true;
    exec = rClient.executeScript("regression", "demo", "george", "", options);

    //
    // 3. Execute an analytics Web service based on a URL-addressable
    // R script: [regressionURL] using a Stateless Project.
    //
    // Note: while this style of execution uses a Stateless Project it is only
    // available to authenticated users that have been granted POWER_USER
    // permissions by the DeployR administrator.
    //
    exec = rClient.executeExternal(regressionURL, options);

>See the [Anonymous Projects](deployr-api-reference.md#r-for-application-developers) section for further details.

## Standard Execution Model

The DeployR API supports a standard set of parameters across all execution APIs which are commonly know as the standard execution model. A summary of those execution APIs are shown here:

-   [/r/project/execute/code](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-project-execute-code)
-   [/r/project/execute/script](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-project-execute-script)
-   [/r/repository/script/execute](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-repository-script-execute)
-   [/r/repository/script/render](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-repository-script-render)
-   [/r/job/submit](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-job-submit)
-   [/r/job/schedule](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-job-schedule)

Conceptually this standard set of parameters can be categorized into three groups: *pre-execution*, *on-execution* and *post-execution* parameters.

### Pre-Execution Parameters

The *pre-execution* parameters allow the caller to *pre-heat* the R session with DeployR-encoded R data, binary R object data, and file data taken from a number of different sources. The following code snippets demonstrate how to use these parameters using the client libraries:


**Java:**

    //
    // ProjectExecutionOptions: used to pass standard execution model
    // parameters on execution calls. All fields are optional.
    //
    ProjectExecutionOptions options = new ProjectExecutionOptions();

    //
    // Parameter: inputs
    // Allows the caller to pass DeployR-encoded R object values as inputs.
    // These inputs are turned into R objects in the workspace before the execution begins.
    //
    RData rNum = RDataFactory.createNumeric("num", 10.0);
    Date date = new GregorianCalendar(2015, 3, 2).getTime();
    RDate rDate = RDataFactory.createDate("date", date, "yyyy-MM-dd");
    List<RData> rinputs = Arrays.asList(rNum, rDate);
    options.rinputs = rinputs;

    //
    // Parameter: csvinputs
    // Allows the caller to pass R object primitive values as comma-separated
    // name/value pairs. These inputs are turned into R object string literals in the
    // workspace before the execution begins.
    //
    options.csvinputs = "num,10.0,expired,false";

    //
    // Parameter: preloadobject[name | directory | author | (optional) version]
    // Allows the caller to load one or more binary R objects (.rData) from the
    // repository into the workspace before the execution begins.
    //
    // Specify comma-separated values on the filename, directory and author fields
    // on your ProjectPreloadOptions instance if you want to load two or more binary R objects.
    //
    ProjectPreloadOptions preloadWorkspace = new ProjectPreloadOptions();
    preloadWorkspace.filename = "model.R";
    preloadWorkspace.directory = "example";
    preloadWorkspace.author = "testuser";
    options.preloadWorkspace = preloadWorkspace;

    //
    // Parameter: preloadfile[name | directory | author | (optional) version]
    // Allows the caller to load one or more files from the repository into the
    // working directory before the execution begins.
    //
    // Specify comma-separated values on the filename, directory and author fields
    // on your ProjectPreloadOptions instance if you want to load two or more files.
    //
    ProjectPreloadOptions preloadDirectory = new ProjectPreloadOptions();
    preloadDirectory.filename = "model.R";
    preloadDirectory.directory = "example";
    preloadDirectory.author = "testuser";
    options.preloadDirectory = preloadDirectory;

    //
    // Parameter: preloadbydirectory
    // Allows the caller to load all files within one or more repository-managed
    // directories into the working directory before the execution begins.
    //
    // Specify comma-separated value on the preloadByDirectory field on your
    // ProjectExecutionOptions instance if you want to load the files found
    // within two or more directories.
    //
    preloadDirectory.preloadByDirectory = "production-models";

    //
    // Parameter: adopt[directory | workspace | packages]
    // Allow the caller to load a pre-existing project workspace, project working
    // directory and/or project package dependencies before the execution begins.
    //
    // Specify the same project identifier on all adoption option fields to adopt
    // everything from a single DeployR project. Omit the project identifier on
    // any of the adoption option fields if that data is not required on your
    // execution. Specify multiple different project identifiers on the adoption
    // option fields if you require data from multiple projects on your execution.
    //
    ProjectAdoptionOptions adoptionOptions = new ProjectAdoptionOptions();
    adoptionOptions.adoptDirectory = "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca";
    adoptionOptions.adoptWorkspace = null;
    adoptionOptions.adoptPackages = "PROJECT-4f9fe8bf-9425-4f2c-aa15-5d94818988f9";
    options.adoptionOptions = adoptionOptions;

**JavaScript:**

    //
    // All options are supiled in a simple object literal hash and passed to the
    // request's `.data(options)` function.
    //

    //
    // ProjectExecutionOptions: used to pass standard execution model
    // parameters on execution calls. All fields are optional.
    //
    var options = {};

    //
    // Parameter: rinputs
    // Allows the caller to pass DeployR-encoded R object values as inputs.
    // These inputs are turned into R objects in the workspace before the execution
    // begins.
    //
    var RIn = deployr.RInput;
    var rinputs = [ RIn.numeric('num', 10.0), RIn.date('date', new Date()) ];

    deployr.io(api)
      .rinputs(rinputs)

    //
    // Parameter: csvinputs
    // Allows the caller to pass R object primitive values as comma-separated
    // name/value pairs. These inputs are turned into R object string literals in
    // the workspace before the execution begins.
    //
    options.csvinputs = 'num,10.0,expired,false';

    //
    // Preload Object Parameters:
    //   - preloadobjectname
    //   - preloadobjectdirectory
    //   - preloadobjectauthor
    //   - preloadobjectversion (optional)
    //
    // Allows the caller to load one or more binary R objects (.rData) from the
    // repository into the workspace before the execution begins.
    //
    // Specify comma-separated values on the filename, directory and author fields
    // on your `Project Preload Options` if you want to load two or more binary
    // R objects.
    //
    options.preloadobjectfilename = 'model.R';
    options.preloadobjectdirectory = 'example';
    options.preloadobjectauthor = 'testuser';

    //
    // Preload File Parameters:
    //   - preloadfilename
    //   - preloadfiledirectory
    //   - preloadfileauthor
    //   - preloadfileversion (optional)
    // Allows the caller to load one or more files from the repository into the
    // working directory before the execution begins.
    //
    // Specify comma-separated values on the filename, directory and author fields
    // on your `Project Preload Options` if you want to load two or more files.
    //
    options.preloadfilename = 'model.R';
    options.preloadfiledirectory = 'example';
    options.preloadfileauthor = 'testuser';

    //
    // Parameter: preloadbydirectory
    // Allows the caller to load all files within one or more repository-managed
    // directories into the working directory before the execution begins.
    //
    // Specify comma-separated value on the preloadByDirectory field on your
    // `Project Execution Options` if you want to load the files found within two
    // or more directories.
    //
    options.preloadbydirectory = 'production-models';

    //
    // Adopt Parameters:
    //   - adoptdirectory
    //   - adoptworkspace
    //   - adpotpackages
    //
    // Allow the caller to load a pre-existing project workspace, project working
    // directory and/or project package dependencies before the execution begins.
    //
    // Specify the same project identifier on all adoption option fields to adopt
    // everything from a single DeployR project. Omit the project identifier on
    // any of the adoption option fields if that data is not required on your
    // execution. Specify multiple different project identifiers on the adoption
    // option fields if you require data from multiple projects on your execution.
    //
    options.adoptdirectory = 'PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca';
    options.adoptworkspace = null;
    options.adoptpackages = 'PROJECT-4f9fe8bf-9425-4f2c-aa15-5d94818988f9';

**C#:**

    //
    // ProjectExecutionOptions: used to pass standard execution model
    // parameters on execution calls. All fields are optional.
    //
    ProjectExecutionOptions options = new ProjectExecutionOptions();

    //
    // Parameter: inputs
    // Allows the caller to pass DeployR-encoded R object values as inputs.
    // These inputs are turned into R objects in the workspace before the execution begins.
    //
    RData rNum = RDataFactory.createNumeric("num", 10.0);
    DateTime dateval = new DateTime(2014, 3, 2);
    RDate rDate = RDataFactory.createDate("date", dateval, "yyyy-MM-dd");
    options.rinputs.Add(rNum);
    options.rinputs.Add(rDate);

    //
    // Parameter: csvrinputs
    // Allows the caller to pass R object primitive values as comma-separated
    // name/value pairs. These inputs are turned into R object string literals in the
    // workspace before the execution begins.
    //
    options.csvrinputs = "num,10.0,expired,false";

    //
    // Parameter: preloadobject[name | directory | author | (optional) version]
    // Allows the caller to load one or more binary R objects (.rData) from the
    // repository into the workspace before the execution begins.
    //
    // Specify comma-separated values on the filename, directory and author fields
    // on your ProjectPreloadOptions instance if you want to load two or more binary R objects.
    //
    ProjectPreloadOptions preloadWorkspace = new ProjectPreloadOptions();
    preloadWorkspace.filename = "model.rData";
    preloadWorkspace.directory = "example";
    preloadWorkspace.author = "testuser";
    options.preloadWorkspace = preloadWorkspace;

    //
    // Parameter: preloadfile[name | directory | author | (optional) version]
    // Allows the caller to load one or more files from the repository into the
    // working directory before the execution begins.
    //
    // Specify comma-separated values on the filename, directory and author fields
    // on your ProjectPreloadOptions instance if you want to load two or more files.
    //
    ProjectPreloadOptions preloadDirectory = new ProjectPreloadOptions();
    preloadDirectory.filename = "model.rData";
    preloadDirectory.directory = "example";
    preloadDirectory.author = "testuser";
    options.preloadDirectory = preloadDirectory;


    //
    // Parameter: adopt[directory | workspace | packages]
    // Allow the caller to load a pre-existing project workspace, project working
    // directory and/or project package dependencies before the execution begins.
    //
    // Specify the same project identifier on all adoption option fields to adopt
    // everything from a single DeployR project. Omit the project identifier on
    // any of the adoption option fields if that data is not required on your
    // execution. Specify multiple different project identifiers on the adoption
    // option fields if you require data from multiple projects on your execution.
    //
    ProjectAdoptionOptions adoptionOptions = new ProjectAdoptionOptions();
    adoptionOptions.adoptDirectory = "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca";
    adoptionOptions.adoptWorkspace = null;
    adoptionOptions.adoptPackages = "PROJECT-4f9fe8bf-9425-4f2c-aa15-5d94818988f9";
    options.adoptionOptions = adoptionOptions;

### On-Execution Parameters

The *on-execution* parameters allow the caller to control certain aspects of the R session environment itself. The following code snippets demonstrate how to use these parameters using the client libraries:


**Java:**

    //
    // ProjectExecutionOptions: used to pass standard execution model
    // parameters on execution calls. All fields are optional.
    //
    ProjectExecutionOptions options = new ProjectExecutionOptions();

    //
    // Parameter: echooff
    // Controls whether R commands executing on the call will appear in
    // the console output returned on the response markup. R commands
    // are echoed to console output by default.
    //
    options.echooff = true | false;

    //
    // Parameter: consoleoff
    // Controls whether console output will be returned on the response markup.
    // Console output is returned by default.
    //
    // For efficiency reasons, enabling consoleoff is recommended if the
    // expected volume of console output generated on a given execution
    // is high and that output is not a required return value on your call.
    //
    options.consoleoff = true | false;

    //
    // Parameter: enableConsoleEvents
    // Controls whether console events are delivered in realtime on the event
    // stream for the current execution. Console events are disabled by default.
    //
    // This parameter should only be enabled in environments where the rate
    // of executions is low, such as when end-users are manually testing code
    // executions. This parameter must be disabled when you are operating in
    // a high-volume transactional environment in order to avoid degrading the
    // server's runtime performance.
    //
    options.enableConsoleEvents = true | false;

    //
    // Parameter: graphics | graphicsWidth | graphicsHeight
    // Controls which graphics device is used (png, svg) on the execution and
    // the default demensions for images on that device.
    //
    options.graphics = "svg";

**JavaScript:**

    //
    // All options are supiled in a simple object literal hash and passed to the
    // request's `.data(options)` function.
    //

    //
    // ProjectExecutionOptions: used to pass standard execution model
    // parameters on execution calls. All fields are optional.
    //
    var options = {};

    //
    // Parameter: echooff
    // Controls whether R commands executing on the call will appear in
    // the console output returned on the response markup. R commands
    // are echoed to console output by default.
    //
    options.echooff = true | false;

    //
    // Parameter: consoleoff
    // Controls whether console output will be returned on the response markup.
    // Console output is returned by default.
    //
    // For efficiency reasons, enabling consoleoff is recommended if the
    // expected volume of console output generated on a given execution
    // is high and that output is not a required return value on your call.
    //
    options.consoleoff = true | false;

    //
    // Parameter: enableConsoleEvents
    // Controls whether console events are delivered in realtime on the event
    // stream for the current execution. Console events are disabled by default.
    //
    // This parameter should only be enabled in environments where the rate
    // of executions is low, such as when end-users are manually testing code
    // executions. This parameter must be disabled when you are operating in
    // a high-volume transactional environment in order to avoid degrading the
    // server's runtime performance.
    //
    options.enableConsoleEvents = true | false;

    //
    // Parameter: graphics | graphicsWidth | graphicsHeight
    // Controls which graphics device is used (png, svg) on the execution and
    // the default demensions for images on that device.
    //
    options.graphics = 'svg';

**C#:**

    //
    // ProjectExecutionOptions: used to pass standard execution model
    // parameters on execution calls. All fields are optional.
    //
    ProjectExecutionOptions options = new ProjectExecutionOptions();

    //
    // Parameter: echooff
    // Controls whether R commands executing on the call will appear in
    // the console output returned on the response markup. R commands
    // are echoed to console output by default.
    //
    options.echooff = true | false;

    //
    // Parameter: consoleoff
    // Controls whether console output will be returned on the response markup.
    // Console output is returned by default.
    //
    // For efficiency reasons, enabling consoleoff is recommended if the
    // expected volume of console output generated on a given execution
    // is high and that output is not a required return value on your call.
    //
    options.consoleoff = true | false;

    //
    // Parameter: enableConsoleEvents
    // Controls whether console events are delivered in realtime on the event
    // stream for the current execution. Console events are disabled by default.
    //
    // This parameter should only be enabled in environments where the rate
    // of executions is low, such as when end-users are manually testing code
    // executions. This parameter must be disabled when you are operating in
    // a high-volume transactional environment in order to avoid degrading the
    // server's runtime performance.
    //
    options.enableConsoleEvents = true | false;

    //
    // Parameter: graphicsDevice | graphicsWidth | graphicsHeight
    // Controls which graphics device is used (png, svg) on the execution and
    // the default demensions for images on that device.
    //
    options.graphicsDevice = "svg";

### Post-Execution Parameters

The *post-execution* parameters allow the caller to retrieve data from the R session on the response markup and/or store data from the R session into the repository. It is important to note that the *post-execution* parameters that support storing data into the repository are only available to *authenticated* users on these calls. The following code snippets demonstrate how to use these parameters using the client libraries:


**Java:**

    //
    // ProjectExecutionOptions: used to pass standard execution model
    // parameters on execution calls. All fields are optional.
    //
    ProjectExecutionOptions options = new ProjectExecutionOptions();

    //
    // Parameter: storeobject
    // Allows the caller to specify a comma-separated list of workspace
    // objects to be stored in the repository after the execution completes.
    //
    // The storedirectory parameter can be used in conjunction with the
    // storeobject parameter. It allows the caller to specify a target
    // repository directory for the stored object or objects.
    //
    // The storepublic parameter can be used in conjunction with the
    // storeobject parameter. When enabled, the stored object file in the
    // DeployR repository will automatically be assigned public access.
    //
    // The storenewversion parameter can be used in conjunction with the
    // storeobject parameter. When enabled, a new version of the stored
    // object file will be created within the DeployR repository, ensuring
    // old versions of the same file will be preserved.
    //
    // Note, an object called "mtcars" will be saved as a binary R object
    // file called mtcars.R within the DeployR repository.
    //
    ProjectStorageOptions storageOptions = new ProjectStorageOptions();
    storageOptions.objects = "mtcars";
    storageOptions.directory = "examples";
    storageOptions.published = true;
    storageOptions.newversion = false;
    options.storageOptions = storageOptions;

    //
    // Parameter: storeworkspace
    // Allows the caller to store the entire workspace in the repository
    // after the execution completes.
    //
    // The storedirectory parameter can be used in conjunction with the
    // storeworkspace parameter. It allows the caller to specify a target
    // repository directory for the stored workspace.
    //
    // The storepublic parameter can be used in conjunction with the
    // storeworkspace parameter. When enabled, the stored workspace file in
    // the DeployR repository will automatically be assigned public access.
    //
    // The storenewversion parameter can be used in conjunction with the
    // storeworkspace parameter. When enabled, a new version of the stored
    // workspace file will be created within the DeployR repository, ensuring
    // old versions of the same file will be preserved.
    //
    // Note, the workspace will be saved as a binary R object file using the
    // name specified on the storeworkspace parameter.
    //
    ProjectStorageOptions storageOptions = new ProjectStorageOptions();
    storageOptions.workspace = "workspace-Mar18-2015.R";
    storageOptions.directory = "examples";
    storageOptions.published = false;
    storageOptions.newversion = true;
    options.storageOptions = storageOptions;

    //
    // Parameter: storefile
    // Allows the caller to specify a comma-separated list of working
    // directory files to be stored in the repository after the execution
    // completes.
    //
    // The storedirectory parameter can be used in conjunction with the
    // storefile parameter. It allows the caller to specify a target
    // repository directory for the stored file or files.
    //
    // The storepublic parameter can be used in conjunction with the
    // storedirectory parameter. When enabled, the stored directory file
    // in the DeployR repository will automatically be assigned public access.
    //
    // The storenewversion parameter can be used in conjunction with the
    // storedirectory parameter. When enabled, a new version of the stored
    // directory file will be created within the DeployR repository, ensuring
    // old versions of the same file will be preserved.
    //
    // Note, each files indicated on the storefile parameter will be will
    // be saved using the name indicated within the DeployR repository.
    //
    ProjectStorageOptions storageOptions = new ProjectStorageOptions();
    storageOptions.files = "data.csv, scores.rData";
    storageOptions.directory = "examples";
    storageOptions.published = true;
    storageOptions.newversion = true;
    options.storageOptions = storageOptions;

    //
    // Parameter: infinity | nan | encodeDataFramePrimitiveAsVector
    // Allow the caller to control how DeployR-encoded R object data is
    // encoded in the response markkup.
    //
    // Infinity is encoded as 0x7ff0000000000000L by default.
    // Nan is encoded as null by default.
    // Primitive within dataframes are encoded as primitives by default.
    //
    options.infinity = ;
    options.nan = null;
    options.encodeDataFramePrimitiveAsVector = true;

**JavaScript:**

    //
    // All options are supiled in a simple object literal hash and passed to the
    // request's `.data(options)` function.
    //

    //
    // ProjectExecutionOptions: used to pass standard execution model
    // parameters on execution calls. All fields are optional.
    //
    var options = {};

    //
    // Store Object Parameters:
    //   - robjects
    //   - storedirectory
    //   - storepublished
    //   - storenewversion
    //
    // Allows the caller to specify a comma-separated list of workspace
    // objects to be stored in the repository after the execution completes.
    //
    // The `storedirectory` parameter can be used in conjunction with the
    // `storeobject` parameter. It allows the caller to specify a target
    // repository directory for the stored object or objects.
    //
    // The `storepublic` parameter can be used in conjunction with the
    // storeobject parameter. When enabled, the stored object file in the
    // DeployR repository will automatically be assigned public access.
    //
    // The `storenewversion` parameter can be used in conjunction with the
    // `storeobject` parameter. When enabled, a new version of the stored
    // object file will be created within the DeployR repository, ensuring
    // old versions of the same file will be preserved.
    //
    // Note, an object called "mtcars" will be saved as a binary R object
    // file called mtcars.R within the DeployR repository.
    //
    options.robjects = 'mtcars';
    options.storedirectory = 'examples';
    options.storepublished = true;
    options.storenewversion = false;

    //
    // Store Workspace Parameters:
    //   - storeworkspace
    //   - storedirectory
    //   - storepublished
    //   - storenewversion
    //
    // Allows the caller to store the entire workspace in the repository
    // after the execution completes.
    //
    // The `storedirectory` parameters can be used in conjunction with the
    // `storeworkspace` parameter. It allows the caller to specify a target
    // repository directory for the stored workspace.
    //
    // The `storepublic` parameters can be used in conjunction with the
    // `storeworkspace` parameter. When enabled, the stored workspace file in
    // the DeployR repository will automatically be assigned public access.
    //
    // The `storenewversion` parameters can be used in conjunction with the
    // `storeworkspace` parameter. When enabled, a new version of the stored
    // workspace file will be created within the DeployR repository, ensuring
    // old versions of the same file will be preserved.
    //
    // Note, the workspace will be saved as a binary R object file using the
    // name specified on the storeworkspace parameter.
    //
    options.storeworkspace = 'workspace-Mar18-2015.R';
    options.storedirectory = 'examples';
    options.storepublished = false;
    options.storenewversion = true;

    //
    // Store File Parameters:
    //   - storefile
    //   - storedirectory
    //   - storepublished
    //   - storenewversion
    //
    // Allows the caller to specify a comma-separated list of working
    // directory files to be stored in the repository after the execution
    // completes.
    //
    // The `storedirectory` parameter can be used in conjunction with the
    // storefile parameter. It allows the caller to specify a target
    // repository directory for the stored file or files.
    //
    // The `storepublic` parameters can be used in conjunction with the
    // storedirectory parameter. When enabled, the stored directory file
    // in the DeployR repository will automatically be assigned public access.
    //
    // The `storenewversion` parameters can be used in conjunction with the
    // storedirectory parameter. When enabled, a new version of the stored
    // directory file will be created within the DeployR repository, ensuring
    // old versions of the same file will be preserved.
    //
    // Note, each files indicated on the `store file` parameters will be will
    // be saved using the name indicated within the DeployR repository.
    //
    options.storefile = 'data.csv, scores.rData';
    options.storedirectory = 'examples';
    options.storepublished = true;
    options.storenewversion = true;

    //
    // Parameter: infinity | nan | encodeDataFramePrimitiveAsVector
    // Allow the caller to control how DeployR-encoded R object data is
    // encoded in the response markkup.
    //
    // Infinity is encoded as 0x7ff0000000000000L by default.
    // Nan is encoded as null by default.
    // Primitive within dataframes are encoded as primitives by default.
    //
    options.infinity = ;
    options.nan = null;
    options.encodeDataFramePrimitiveAsVector = true;

**C#:**

    //
    // ProjectExecutionOptions: used to pass standard execution model
    // parameters on execution calls. All fields are optional.
    //
    ProjectExecutionOptions options = new ProjectExecutionOptions();

    //
    // Parameter: storeobject
    // Allows the caller to specify a comma-separated list of workspace
    // objects to be stored in the repository after the execution completes.
    //
    // The storedirectory parameter can be used in conjunction with the
    // storeobject parameter. It allows the caller to specify a target
    // repository directory for the stored object or objects.
    //
    // The storepublic parameter can be used in conjunction with the
    // storeobject parameter. When enabled, the stored object file in the
    // DeployR repository will automatically be assigned public access.
    //
    // The storenewversion parameter can be used in conjunction with the
    // storeobject parameter. When enabled, a new version of the stored
    // object file will be created within the DeployR repository, ensuring
    // old versions of the same file will be preserved.
    //
    // Note, an object called "mtcars" will be saved as a binary R object
    // file called mtcars.rData within the DeployR repository.
    //
    ProjectStorageOptions storageOptions = new ProjectStorageOptions();
    storageOptions.objects = "mtcars";
    storageOptions.directory = "examples";
    storageOptions.published = true;
    storageOptions.newVersion = false;
    options.storageOptions = storageOptions;

    //
    // Parameter: storeworkspace
    // Allows the caller to store the entire workspace in the repository
    // after the execution completes.
    //
    // The storedirectory parameter can be used in conjunction with the
    // storeworkspace parameter. It allows the caller to specify a target
    // repository directory for the stored workspace.
    //
    // The storepublic parameter can be used in conjunction with the
    // storeworkspace parameter. When enabled, the stored workspace file in
    // the DeployR repository will automatically be assigned public access.
    //
    // The storenewversion parameter can be used in conjunction with the
    // storeworkspace parameter. When enabled, a new version of the stored
    // workspace file will be created within the DeployR repository, ensuring
    // old versions of the same file will be preserved.
    //
    // Note, the workspace will be saved as a binary R object file using the
    // name specified on the storeworkspace parameter.
    //
    storageOptions.workspace = "workspace-Mar18-2015.rData";
    storageOptions.directory = "examples";
    storageOptions.published = false;
    storageOptions.newVersion = true;
    options.storageOptions = storageOptions;

    //
    // Parameter: storefile
    // Allows the caller to specify a comma-separated list of working
    // directory files to be stored in the repository after the execution
    // completes.
    //
    // The storedirectory parameter can be used in conjunction with the
    // storefile parameter. It allows the caller to specify a target
    // repository directory for the stored file or files.
    //
    // The storepublic parameter can be used in conjunction with the
    // storedirectory parameter. When enabled, the stored directory file
    // in the DeployR repository will automatically be assigned public access.
    //
    // The storenewversion parameter can be used in conjunction with the
    // storedirectory parameter. When enabled, a new version of the stored
    // directory file will be created within the DeployR repository, ensuring
    // old versions of the same file will be preserved.
    //
    // Note, each files indicated on the storefile parameter will be will
    // be saved using the name indicated within the DeployR repository.
    //
    storageOptions.files = "data.csv, scores.rData";
    storageOptions.directory = "examples";
    storageOptions.published = true;
    storageOptions.newVersion = true;
    options.storageOptions = storageOptions;

    //
    // Parameter: infinity | nan | encodeDataFramePrimitiveAsVector
    // Allow the caller to control how DeployR-encoded R object data is
    // encoded in the response markkup.
    //
    // Infinity is encoded as 0x7ff0000000000000L by default.
    // Nan is encoded as null by default.
    // Primitive within dataframes are encoded as primitives by default.
    //
    options.infinity = "";
    options.nan = null;
    options.encodeDataFramePrimitiveAsVector = true;

## R Object Data Encoding

DeployR-specific encodings are used to encode R object data passing into and out of the DeployR server. Each of the client libraries provide support that simplifies the task of encoding R object data for sending to the DeployR server.

Encoded R object data can be sent on the *inputs* parameter on the following calls:

-   [/r/project/execute/code](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-project-execute-code)
-   [/r/project/execute/script](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-project-execute-script)
-   [/r/repository/script/execute](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-repository-script-execute)
-   [/r/repository/script/render](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-repository-script-render)
-   [/r/job/submit](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-job-submit)
-   [/r/job/schedule](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-job-schedule)
-   [/r/project/workspace/push](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-project-workspace-push)

See the [Standard Execution Model](#standard-execution-model) section of this documentation for details describing how DeployR-encoded R object data can be sent using the *inputs* parameter on these calls.

DeployR-specific encodings are provided for the following classes of R object:

-   character, integer, numeric and logical
-   Date, POSIXct and POSIXlt
-   vector, matrix
-   ordered, factor
-   list, data.frame.

The following code snippets demonstrate the mechanism for creating these types of encodings using the client libraries. The following code snippets demonstrate the mechanism for creating these types of encodings using the client libraries.


**Java:**

    //
    // 1. Encode an R logical value.
    //
    RBoolean bool = RDataFactory.createBoolean("bool", true);

    //
    // 2. Encode an R numeric value.
    //
    RNumeric num = RDataFactory.createNumeric("num", 10.0);

    //
    // 3. Encode an R literal value.
    //
    RString str = RDataFactory.createString("str", "hello");


    //
    // 4. Encode an R date.
    //
    Date dateval = new GregorianCalendar(2014, 3, 2).getTime();
    RDate date = RDataFactory.createDate("date", dateval, "yyyy-MM-dd");

    //
    // 5. Encode an R logical vector.
    //
    List<Boolean> bvv = new ArrayList<Boolean>();
    bvv.add(true);
    bvv.add(false);
    RBooleanVector boolvec = RDataFactory.createBooleanVector("boolvec", bvv);

    //
    // 6. Encode an R numeric vector.
    //
    List<Double> nvv = new ArrayList<Double>();
    nvv.add(10.0);
    nvv.add(20.0);
    RNumericVector numvec = RDataFactory.createNumericVector("numvec", nvv);

    //
    // 7. Encode an R literal vector.
    //
    List<String> svv = new ArrayList<String>();
    svv.add("hello");
    svv.add("world");
    RStringVector strvec = RDataFactory.createStringVector("strvec", svv);

    //
    // 8. Encode an R date vector.
    //
    List<Date> dvv = new ArrayList<Date>();
    dvv.add(new GregorianCalendar(2014, 10, 10).getTime());
    dvv.add(new GregorianCalendar(2014, 11, 11).getTime());
    RStringVector datevec =
        RDataFactory.createStringVector("datevec", dvv, "yyyy-MM-dd");

    //
    // 9. Encode an R POSIX date vector.
    //
    List<Date> dvv = new ArrayList<Date>();
    dvv.add(new GregorianCalendar(2014, 10, 10, 10, 30, 30).getTime());
    dvv.add(new GregorianCalendar(2014, 11, 11, 10, 30, 30).getTime());
    RStringVector posixvec =
        RDataFactory.createStringVector("posixvec", dvv, "yyyy-MM-dd HH:mm:ss z");

    //
    // 10. Encode an R logical matrix.
    //
    List<Boolean> bml = new ArrayList<Boolean>();
    bml.add(new Boolean(true));
    bml.add(new Boolean(true));
    List<List<Boolean>> bmv = new ArrayList<List<Boolean>>();
    bmv.add(bml);
    RBooleanMatrix boolmat = RDataFactory.createBooleanMatrix("boolmat", bmv);

    //
    // 11. Encode an R numeric matrix.
    //
    List<Double> nml = new ArrayList<Double>();
    nml.add(new Double(10.0));
    nml.add(new Double(20.0));
    List<List<Double>> nmv = new ArrayList<List<Double>>();
    nmv.add(nml);
    RNumericMatrix nummat = RDataFactory.createNumericMatrix("nummat", nmv);

    //
    // 12. Encode an R literal matrix.
    //
    List<String> sml = new ArrayList<String>();
    sml.add(new String("hello"));
    sml.add(new String("world"));
    List<List<String>> smv = new ArrayList<List<String>>();
    smv.add(sml);
    RStringMatrix strmat = RDataFactory.createStringMatrix("strmat", smv);

    //
    // 13. Encode an R list.
    //
    List<RData> lv = new ArrayList<RData>();
    lv.add(RDataFactory.createNumeric("n1", 10.0));
    lv.add(RDataFactory.createNumeric("n2", 10.0));
    RList numlist = RDataFactory.createList("numlist", lv);

    //
    // 14. Encode an R data.frame.
    //
    List<Double> rnvv = new ArrayList();
    rnvval.add(new Double(1.0));
    rnvval.add(new Double(2.0));
    RNumericVector rnv  = RDataFactory.createNumericVector("rnv", rnvval);
    List<Date> rdvval = new ArrayList<Date>();
    rdvval.add(new Date());
    rdvval.add(new Date());
    RDateVector rdv =
        RDataFactory.createDateVector("rdv", rdvval, "yyyy-MM-dd");
    List<RData> dfv = new ArrayList<RData>();
    dfv.add(rnv);
    dfv.add(rdv);
    List<RData> df = RDataFactory.createDataFrame("numdf", dfv);

    //
    // 15. Encode an R factor.
    //
    List<String> rfval = new ArrayList<String>();
    rfval.add("a");
    rfval.add("b");
    rfval.add("d");
    rfval.add("e");
    RFactor factor = RDataFactory.createFactor(name, rfval, false);

**JavaScript:**

    var RInput = deployr.RInput;

    //
    // 1. Encode an R logical value.
    //
    var bool = RInput.logical('bool', true);

    // --or--
    deployr.io(...)
      .logical('bool', true)

    //
    // 2. Encode an R numeric value.
    //
    var num = RInput.numeric('num', 10.0);

    // --or--
    deployr.io(...)
      .numeric('num', 10.0)

    //
    // 3. Encode an R literal value.
    //
    var str = RInput.character('str', 'hello');

    // --or--
    deployr.io(...)
      .character('str', 'hello')

    //
    // 4. Encode an R date.
    //
    var dateval = new Date(2014, 3, 2);
    var date = RInput.date('date', dateval);

    // --or--
    deployr.io(...)
      .date('date', dateval)

    //
    // 5. Encode an R logical vector.
    //
    var bvv = [];
    bvv.push(RInput.logical('item1', true));
    bvv.push(RInput.logical('item1', true));
    var boolvec = RInput.logicalVector('boolvec', bvv);

    // --or--
    deployr.io(...)
      .logicalVector('boolvec', bvv)

    //
    // 6. Encode an R numeric vector.
    //
    var nvv = [];
    nvv.push(RInput.numeric('item1', 10.0));
    nvv.push(RInput.numeric('item1', 20.0));
    var numvec = RInput.numericVector('numvec', nvv);

    // --or--
    deployr.io(...)
      .numericVector('numvec', nvv)

    //
    // 7. Encode an R literal vector.
    //
    var svv = [];
    svv.push(RInput.character('item1', 'hello'));
    svv.push(RInput.character('item2', 'world'));
    var strvec = RInput.characterVector('strvec', svv);

    // --or--
    deployr.io(...)
      .characterVector('strvec', svv)

    //
    // 8. Encode an R date vector.
    //
    var dvv = [];
    dvv.push(RInput.date('item1', new Date()));
    dvv.push(RInput.date('item2', new Date()));
    var datevec = RInput.dateVector('dvv', dvv);

    // --or--
    deployr.io(...)
      .dateVector('dvv', dvv)

    //
    // 9. Encode an R POSIX date vector.
    //
    var dvv = [];
    dvv.push(RInput.posix('item1', new Date()));
    dvv.push(RInput.posix('item2', new Date()));
    var posixvec = RInput.posixVector('posixvec', dvv);

    // --or--
    deployr.io(...)
      .posixVector('posixvec', dvv)

    //
    // 10. Encode an R logical matrix.
    //
    var bml = [];
    var bmv = [];
    bml.push(RInput.logical('item1', true));
    bml.push(RInput.logical('item2', true));
    bmv.push(bml);
    var boolmat = RInput.logicalMatrix('boolmat', bmv)

    // --or--
    deployr.io(...)
      .logicalMatrix('boolmat', bmv)

    //
    // 11. Encode an R numeric matrix.
    //
    var nml = [];
    var nmv = [];
    nml.push(RInput.numeric('item1', 10.0));
    nml.push(RInput.numeric('item2', 20.0));
    nmv.push(nml);
    var nummat = RInput.numericMatrix('nummat', nmv)

    // --or--
    deployr.io(...)
      .numericMatrix('nummat', nmv)

    //
    // 12. Encode an R literal matrix.
    //
    var sml = [];
    var smv = [];
    sml.push(RInput.character('item1', 'hello'));
    sml.push(RInput.character('item2', 'world'));
    smv.push(sml);
    var strmat = RInput.characterMatrix('strmat', smv)

    // --or--
    deployr.io(...)
      .characterMatrix('strmat', smv)

    //
    // 13. Encode an R list.
    //
    var lv = [];
    lv.push(RInput.numeric('n1', 10.0));
    lv.push(RInput.numeric('n2', 10.0));

    // --or--
    deployr.io(...)
      .list('numlist', lv)

    //
    // 14. Encode an R data.frame.
    //
    var rnval = [];
    rnval.push(RInput.numeric('item1', 1.0));
    rnval.push(RInput.numeric('item2', 2.0));
    var rnv  = RInput.numericVector('rnv', rnvval);
    var rdvval = [];
    rdvval.push(RInput.date('item3', new Date()));
    rdvval.push(RInput.date('item4', new Date()));
    var rdv = RInput.dateVector('rdv', rdvval);
    var dfv = [];
    dfv.push(rnv);
    dfv.push(rdf);
    var df = RInput.dataframe('numdef', dfv);

    // --or--
    deployr.io(...)
      .dataframe('numdf', dfv)

    //
    // 15. Encode an R factor.
    //
    var rfval = [];
    rfval.push('a');
    rfval.push('b');
    rfval.push('d');
    rfval.push('e');
    var factor = RInput.factor('rfval', rfval);

    // --or--
    deployr.io(...)
      .factor('rfval', rfval);

**C#:**

    //
    // 1. Encode an R logical value.
    //
    RBoolean rBool = RDataFactory.createBoolean("bool", true);

    //
    // 2. Encode an R numeric value.
    //
    RNumeric rNum = RDataFactory.createNumeric("num", 10.0);

    //
    // 3. Encode an R literal value.
    //
    RString rString = RDataFactory.createString("str", "hello");


    //
    // 4. Encode an R date.
    //
    DateTime dateval = new DateTime(2014, 3, 2);
    RDate rDate = RDataFactory.createDate("date", dateval, "yyyy-MM-dd");

    //
    // 5. Encode an R logical vector.
    //
    List<Boolean?> boolVector = new List<Boolean?>();
    boolVector.Add(true);
    boolVector.Add(false);
    RBooleanVector rBoolVector = RDataFactory.createBooleanVector("boolvec", boolVector);

    //
    // 6. Encode an R numeric vector.
    //
    List<Double?> numVector = new List<Double?>();
    numVector.Add(10.0);
    numVector.Add(20.0);
    RNumericVector rNumericVector = RDataFactory.createNumericVector("numvec", numVector);

    //
    // 7. Encode an R literal vector.
    //
    List<String> strVector = new List<String>();
    strVector.Add("hello");
    strVector.Add("world");
    RStringVector rStringVector = RDataFactory.createStringVector("strvec", strVector);

    //
    // 8. Encode an R date vector.
    //
    List<String> dateVector = new List<String>();
    dateVector.Add(new DateTime(2014, 10, 10).ToString());
    dateVector.Add(new DateTime(2014, 11, 11).ToString());
    RDateVector rDateVector =
        RDataFactory.createDateVector("datevec", dateVector, "yyyy-MM-dd");


    //
    // 9. Encode an R logical matrix.
    //
    List<Boolean?> boolVec = new List<Boolean?>();
    boolVec.Add(true);
    boolVec.Add(true);
    List<List<Boolean?>> boolMatrix = new List<List<Boolean?>>();
    boolMatrix.Add(boolVec);
    RBooleanMatrix rBoolMatrix = RDataFactory.createBooleanMatrix("boolmat", boolMatrix);

    //
    // 10. Encode an R numeric matrix.
    //
    List<Double?> numVec = new List<Double?>();
    numVec.Add(10.0);
    numVec.Add(20.0);
    List<List<Double?>> numMatrix = new List<List<Double?>>();
    numMatrix.Add(numVec);
    RNumericMatrix rNumMatrix = RDataFactory.createNumericMatrix("nummat", numMatrix);

    //
    // 11. Encode an R literal matrix.
    //
    List<String> strVec = new List<String>();
    strVec.Add("hello");
    strVec.Add("world");
    List<List<String>> strMatrix = new List<List<String>>();
    strMatrix.Add(strVec);
    RStringMatrix rStrMatrix = RDataFactory.createStringMatrix("strmat", strMatrix);

    //
    // 12. Encode an R list.
    //
    List<RData> listRData = new List<RData>();
    listRData.Add(RDataFactory.createNumeric("n1", 10.0));
    listRData.Add(RDataFactory.createNumeric("n2", 20.0));
    RList numlist = RDataFactory.createList("numlist", listRData);

    //
    // 13. Encode an R data.frame.
    //
    List<Double?> numVect = new List<Double?>();
    numVect.Add(1.0);
    numVect.Add(2.0);
    RNumericVector rNumVector = RDataFactory.createNumericVector("rnv", numVect);
    List<String> dateVec = new List<String>();
    dateVec.Add(new DateTime(2014,1,2).ToString());
    dateVec.Add(new DateTime(2014,1,3).ToString());
    RDateVector rDateVect = RDataFactory.createDateVector("rdv", dateVec, "yyyy-MM-dd");
    List<RData> dfVector = new List<RData>();
    dfVector.Add(rNumVector);
    dfVector.Add(rDateVector);
    RDataFrame rDataFrame = RDataFactory.createDataFrame("numdf", dfVector);

    //
    // 15. Encode an R factor.
    //
    List<String> factorVector = new List<String>();
    factorVector.Add("a");
    factorVector.Add("b");
    factorVector.Add("d");
    factorVector.Add("e");
    RFactor rFactor = RDataFactory.createFactor("myfactor", factorVector);

>See the [Web Service API Data Encodings](deployr-api-reference.md#encoding) section for further details.

## R Object Data Decoding

DeployR-specific encodings are used to encode R object data passing into and out of the DeployR server. Each of the client libraries provide support that simplifies the task of decoding R object data being returned from the DeployR server.

One or more R objects can be returned as DeployR-encoded objects in the response markup on any of the following execution calls:

-   [/r/project/execute/code](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-project-execute-code)
-   [/r/project/execute/script](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-project-execute-script)
-   [/r/repository/script/execute](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-repository-script-execute)
-   [/r/repository/script/render](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-repository-script-render)

The following code snippets demonstrate the mechanism for requesting DeployR-encoded objects to be returned on these calls:


**Java:**

    //
    // ProjectExecutionOptions: use the _robjects_ parameter to request
    // one or more R objects to be returned as DeployR-encoded objects
    // on the response markup.
    //
    // In this example, following the execution the "mtcars" and the
    // "score" workspace objects will be returned on the call.
    //
    ProjectExecutionOptions options = new ProjectExecutionOptions();
    options.routputs = Arrays.asList("mtcars", "score");

**JavaScript:**

    //
    // ProjectExecutionOptions: use the `robjects` or `robject` parameter to request
    // one or more R objects to be returned as DeployR-encoded objects on the
    // response markup.
    //
    // In this example, following the execution the "mtcars" and the "score"
    // workspace objects will be returned on the call.
    //
    deployr.io(api)
     .routputs([ 'mtcars', 'score' ])

    // --- or ---

    deployr.io(api)
     .routput('mtcars')
     .routput('score')

**C#:**

    //
    // ProjectExecutionOptions: use the _robjects_ parameter to request
    // one or more R objects to be returned as DeployR-encoded objects
    // on the response markup.
    //
    // In this example, following the execution the "mtcars" and the
    // "score" workspace objects will be returned on the call.
    //
    ProjectExecutionOptions options = new ProjectExecutionOptions();
    options.routputs.Add("mtcars");
    options.routputs.Add("score");

When working with temporary or persistent DeployR projects R objects can also be returned as DeployR-encoded objects in the response markup on the following workspace call:

-   [/r/project/workspace/get](https://microsoft.github.io/deployr-api-docs/8.0.5/#r-project-workspace-get)

The following code snippet demonstrates the mechanism for requesting DeployR-encoded objects to be returned on this call:


**Java:**

    //
    // RProject.getObject(String objectName)
    // RProject.getObject(String objectName, boolean encodeDataFramePrimitiveAsVector)
    // RProject.getObject(List<String> objectNames)
    // RProject.getObject(List<String> objectNames, boolean encodeDataFramePrimitiveAsVector)
    //
    // In this example, following the execution the "mtcars" and the
    // "score" workspace objects will be returned on the call.
    //
    List<String> objectNames = Arrays.asList("mtcars", "score");
    List<RData> rDataList = rProject.getObject(objectNames, true);

**JavaScript:**

    //
    //
    // var obj = response.workspace(objectName);
    //
    // -- or --
    //
    // var workspace = response.workspace();
    //
    // In this example, following the execution the "mtcars" and the
    // "score" workspace objects will be returned on the call.
    //

**C#:**

    deployr.io(api)
      .end(function(response) {
         var mtcars = response.workspace('mtcars');
         var score = response.workspace('score');
       });

    //
    // RProject.getObject(String objectName)
    // RProject.getObject(String objectName, boolean encodeDataFramePrimitiveAsVector)
    // RProject.getObjects(List<String> objectNames, boolean encodeDataFramePrimitiveAsVector)
    //
    // In this example, following the execution the "mtcars" and the
    // "score" workspace objects will be returned on the call.
    //
    RData rDataMtcars = rProject.getObject("mtcars", true);
    RData rDataScore = rProject.getObject("score", true);

Encodings are provided for the following classes of R object:

-   character, integer, numeric and logical
-   Date, POSIXct and POSIXlt
-   vector, matrix
-   ordered, factor
-   list, data.frame.

The method for decoding these DeployR-encoded objects within a client application depends on the specific client library being used. The following code snippets demonstrate the mechanism for decoding R objects in order to retrieve their values using the client libraries:


**Java:**

    //
    // Java Client Library R Object Decoding
    //
    // The Java client library always returns DeployR-encoded objects
    // as an instance of com.revo.deployr.client.data.RData.
    //
    // To decode an instance of RData in order to retrieve the value
    // of the object you must determine the concrete implementation
    // class of the RData instance. The concrete class provides a
    // getValue() method with an appropriate return type.
    //
    // There are two approaches available to Java client developers.
    //

    //
    // (1) Explicit Casting of RData to Concrete Class
    //
    // This approach can be used if you know the underlying
    // concrete class for your instance of RData ahead of time.
    //
    // The set of available concrete classes for RData are defined
    // in the com.revo.deployr.client.data package.
    //
    RData rData = rProject.getObject("mtcars");
    RDataFrame mtcars = (RDataFrame) rData;
    List<RData> values = mtcars.getValue();

    //
    // (2) Runtime Detection of RData underlying Concrete Class
    //
    // This approach can be used if you are working with multiple
    // objects or if the underlying concrete class for each
    // instance of RData is simply unknown ahead of time.
    //
    // The set of available concrete classes for RData are defined
    // in the com.revo.deployr.client.data package.
    //
    List<String> objectNames = Arrays.asList("mtcars", "score");
    List<RData> rDataList = rProject.getObjects(objectNames);

    for(RData rData : rDataList) {

      if(rData instanceof RDataFrame) {
        List<RData> values = ((RDataFrame) rData).getValue();
        processDataFrame(rData.getName(), values);
      } else
      if(rData instanceof RNumeric) {
        double value = ((RNumeric) rData).getValue();
        processNumeric(rData.getName(), value);
      }
      // else..if..etc...

    }

**JavaScript:**

    //
    // JavaScript Client Library R Object Decoding
    //
    // The Java client library always returns DeployR-encoded objects as object
    // literals.
    //
    // There are two approaches available to JavaScript client developers.
    //

    //
    // (1) Explicit "Casting" of `R Data`
    //
    // This approach can be used if you know the underlying type of the RData on the
    // response ahead of time.
    //
    var mtcars = response.workspace('mtcars'); // dataframe
    var value = mtcars.value;

    //
    // (2) Runtime Detection of `R Data`
    //
    // This approach can be used if you are working with multiple objects or if the
    // RData is simply unknown ahead of time on the response.
    //
    var objects = response.workspace();

    for (var obj in objects) {
      if (obj.type === 'dataframe') {
         processDataFrame(obj.name, obj.value);
      } else if (obj.type === 'numeric') {
         processNumeric(obj.name, obj.value);
      }
      // else..if..etc...
    }

**C#:**

    //
    // .NET Client Library R Object Decoding
    //
    // The .NET client library always returns DeployR-encoded objects
    // as an instance of RData.
    //
    // To decode an instance of RData in order to retrieve the value
    // of the object you must determine the concrete implementation
    // class of the RData instance. The concrete class provides a
    // Value method with an appropriate return type.
    //
    // There are two approaches available to .NET client developers.
    //

    //
    // (1) Explicit Casting of RData to Concrete Class
    //
    // This approach can be used if you know the underlying
    // concrete class for your instance of RData ahead of time.
    //
    //
    RData rData = rProject.getObject("mtcars");
    RDataFrame mtcars = (RDataFrame) rData;
    List<RData> values = (List<RData>)mtcars.Value;

    //
    // (2) Runtime Detection of RData underlying Concrete Class
    //
    // This approach can be used if you are working with multiple
    // objects or if the underlying concrete class for each
    // instance of RData is simply unknown ahead of time.
    //
    //
    List<String> objectNames = new List<String>{"mtcars", "score"};
    List<RData> rDataList = rProject.getObjects(objectNames, true);

    foreach(RData rDataVal in rDataList)
    {

        if(rDataVal.GetType() == typeof(RDataFrame))
        {
            List<RData> vals = (List<RData>)rDataVal.Value;
            //write your own function to parse a dataframe
            processDataFrame(rDataVal.Name, vals);
        }
        else if(rDataVal.GetType() == typeof(RNumeric))
        {
            Double value = (Double)rDataVal.Value;
        }
        // else..if..etc...
    }
