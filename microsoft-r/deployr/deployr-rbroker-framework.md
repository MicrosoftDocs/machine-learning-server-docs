---

# required metadata
title: "DeployR RBroker Framework Tutorial - DeployR 8.x "
description: "The tutorial for DeployR's RBroker framework"
keywords: "DeployR, tutorial, RBroker, framework"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "05/06/2016"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "deployr"
#ms.custom: ""

---


# RBroker Framework Tutorial

**Applies to: DeployR 8.x**   (See [comparison between 8.x and 9.x](../whats-new-in-r-server.md#8vs9))

>Looking for docs for Microsoft R Server 9? [Start here](../what-is-operationalization.md).

## Introduction

The RBroker Framework is the simplest way to integrate DeployR-enabled analytics Web services inside any Java, JavaScript or .NET application. This document introduces the basic building blocks exposed by the framework, explains the programming model encouraged by the framework, and demonstrates the framework in action using language-specific code samples.

>The RBroker Framework is designed to support transactional, on-demand analytics, where each invocation on an analytics Web service is a standalone operation that executes on a stateless R session environment. If your application requires a long-lived stateful R session environment, then please see the [DeployR Client Libraries](deployr-tools-and-samples.md), which offer support for stateful operations on DeployR-managed Projects.

**Try Out Our Examples!** Explore the RBroker Framework examples for [Java,](https://github.com/deployr/java-example-rbroker-basics) [Javascript,](https://github.com/deployr/js-rbroker-framework) and [.NET.](https://github.com/deployr/dotnet-rbroker-framework) Find them under the `examples` directory of each Github repository. Additional sample applications are also [available on GitHub.](http://github.com/deployr?query=example)

### Basic Building Blocks

The RBroker Framework defines three simple building blocks:

1.  **RBroker**: an RTask execution engine
2.  **RTask**: an executable representing any analytics Web service
3.  **RTaskResult**: a collection of result data for an instance of RTask

These building blocks are described in further detail in the following sections.

### Building Block 1: RBroker

At its most fundamental level, an RBroker is an RTask execution engine, exposing a simple interface to be used by client application developers to submit RTask for execution. However, an RBroker engine handles more than simple execution. Specifically, an RBroker engine is responsible for the following:

1.  Queuing RTask requests on behalf of client applications
2.  Acquiring a dedicated R session for the purposes of executing each RTask
3.  Initiating optional R session pre-initialization for each RTask
4.  Launching the execution of each RTask
5.  Handling execution success or failure responses for each RTask
6.  Returning RTaskResult result data for each RTask to client applications

An RBroker takes on these responsibilities so that client application developers don't have to.

### Building Block 2: RTask

An RTask is an executable description of any DeployR-enabled analytics Web service.

There are three distinct styles of analytics Web service available to client application developers.
They are based on:

1.  Repository-managed R scripts
2.  Arbitrary blocks of R code
3.  URL-addressable R scripts

Each style of analytics Web service is supported when instantiating instances of RTask. To execute any RTask, submit the RTask to an instance of RBroker.

### Building Block 3: RTaskResult

An RTaskResult provides the client application access to RTask result data. Such result data may include:

1.  RTask generated R console output
2.  RTask generated graphics device plots
3.  RTask generated working directory files
4.  RTask generated R object workspace data

RTaskResult data can be read, displayed, processed, stored, or further distributed by client applications.

### Basic Programming Model

The basic programming model when working with the RBroker framework is as follows:

1.  Create an instance of RBroker within your application.

2.  Create one or more instances of RTask within your application.

3.  Submit these RTasks to your instance of RBroker for execution.

4.  Integrate the results of your RTasks returned by RBroker within your application.

As this basic programming model indicates, when using the RBroker framework, your application logic can focus entirely on defining your R analytics tasks and integrating your R analytics task results. The inherent complexity of managing both client-side DeployR API calls and server-side R session lifecycles is handled entirely by RBroker, which greatly simplifies client application development.

### Hello World Example

The following code snippets provide the ubiquituous "Hello World" example for the RBroker framework. Each snippet provides a brief demonstration of how the RBroker framework basic programming model is accomplished in Java, JavaScript and C\# code respectively.


**Java:**

    //
    // 1. Create an instance of RBroker.
    //
    // Typically, a client application will require only a single
    // instance of RBroker.
    //
    // An RBrokerFactory is provided to facilitate the creation of
    // RBroker instances.
    //
    RBroker rBroker =
        RBrokerFactory.discreteTaskBroker(brokerCfg);

    //
    // 2. Define an instance of RTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    RTask rTask = RTaskFactory.discreteTask("regression",
                                            "demo",
                                            "george",
                                            version,
                                            taskOptions);


    //
    // 3. Submit the instance of RTask to RBroker for execution.
    //
    // The RBroker.submit() call is non-blocking so an instance
    // of RTaskToken is returned immediately.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

    //
    // 4. Retrieve the RTask result.
    // 
    // An RTaskResult provides a client application access to RTask
    // results, including R console output, generated plots or files,
    // and DeployR-encoded R object workspace data.
    //
    // While this example demonstrates a synchronous approach to RTaskResult
    // handling, the RBRoker framework also supports an asynchronous model.
    // The asynchronous model is recommended for all but the most simple integrations.
    //
    RTaskResult rTaskResult = rTaskToken.getResult();

**JavaScript:**

    // Browser --> window.rbroker
    <script src="rbroker.js"></script>

    // Node.js
    #!/bin/env node
    var rbroker = require('rbroker');

    //
    // 1. Create an instance of RBroker.
    //
    // Typically, a client application will require only a single
    // instance of RBroker.
    //
    // An RBrokerFactory is provided to facilitate the creation of
    // RBroker instances.
    //
    var dsBroker = rbroker.discreteTaskBroker(brokerCfg);

    //
    // 2. Define an instance of RTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    var rTask = rbroker.discreteTask({ 
        filename: 'regression',
        directory: 'demo',
        author: 'george',
        version: version
        // more options...
    });

    //
    // 3. Submit the instance of RTask to RBroker for execution.
    //
    // The RBroker.submit() call is non-blocking so an instance
    // of RTaskToken is returned immediately.
    //
    var rTaskToken = dsBroker.submit(rTask);

    //
    // 4. Retrieve the RTask result.
    // 
    // An RTaskResult provides a client application access to RTask
    // results, including R console output, generated plots or files,
    // and DeployR-encoded R object workspace data.
    //
    rTasktoken.complete(function(rTask, rTaskResult) {
       // result...
    });  

    //
    // Or combine Steps (3 and 4) above into a single chain using `.complete()`
    //
    dsBroker.submit(rTask)
       .complete(function(rTask, rTaskResult) {
          // result...
       });

**C#:**

    //
    // 1. Create an instance of RBroker.
    //
    // Typically, a client application will require only a single
    // instance of RBroker.
    //
    // An RBrokerFactory is provided to facilitate the creation of
    // RBroker instances.
    //
    RBroker rBroker = RBrokerFactory.discreteTaskBroker(brokerCfg);

    //
    // 2. Define an instance of RTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    DiscreteTaskOptions taskOptions = new DiscreteTaskOptions();
    RTask rTask = RTaskFactory.discreteTask("regression",
                                            "demo",
                                            "george",
                                            "",
                                            taskOptions);


    //
    // 3. Submit the instance of RTask to RBroker for execution.
    //
    // The RBroker.submit() call is non-blocking so an instance
    // of RTaskToken is returned immediately.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

    //
    // 4. Retrieve the RTask result.
    // 
    // An RTaskResult provides a client application access to RTask
    // results, including R console output, generated plots or files,
    // and DeployR-encoded R object workspace data.
    //
    // While this example demonstrates a synchronous approach to RTaskResult
    // handling, the RBRoker framework also supports an asynchronous model.
    // The asynchronous model is recommended for all but the most simple integrations.
    //
    RTaskResult rTaskResult = rTaskToken.getResult();

**Try Out Our Examples!** After downloading the RBroker Framework, you can find basic examples that complement this tutorial under the `deployr/examples` directory. Additionally, find more comprehensive examples for [Java](https://github.com/microsoft/?utf8=%E2%9C%93&query=java-example) and [JavaScript](https://github.com/microsoft/?utf8=✓&query=js-example).

## RBroker Runtime Options

>The RBroker Framework defines three simple building blocks (RBroker, RTask and RTaskResult), which can be instantiated in a number of ways to allow client application solutions to leverage distinct runtime characteristics from the framework.

Currently, there are three RBroker runtimes available. These runtimes are identified as:

1.  [Discrete Task Runtime](#discrete-task-runtime)
2.  [Pooled Task Runtime](#pooled-task-runtime)
3.  [Background Task Runtime](#background-task-runtime)

>When contemplating any RBroker-based solution, a developer must first decide which runtime to use.

The following sections detail the characteristics of each RBroker runtime and provide guidance to help developers make the correct runtime selection based on their specific needs.

### Discrete Task Runtime

The Discrete Task Runtime acquires DeployR grid resources per RTask on-demand, which has a number of important consequences at runtime. These consequences are:

1.  If an appropriate slot on the grid can be acquired by the RBroker, then an RTask will be executed.

2.  If an appropriate slot on the grid can not be acquired by the the RBroker, then the RTask result will indicate an `RGridException` failure.

3.  Any RTask indicating an `RGridException` failure can be resubmitted to the RBroker.

    >It is the responsibility of the client application developer to decide whether such RTasks should be resubmitted to RBroker, logged by the application, and/or reported to the end-user.

4.  This runtime supports a `maxConcurrency` configuration property that limits the maximum number of RTasks that the RBroker will attempt to execute in parallel. All RTasks submitted in excess of this limit are automatically queued by the RBroker, which ensures that no more than `maxConcurrency` RTasks are executing on the runtime at any one time.

5.  Tuning the `maxConcurrency` property to reflect the real-world limits determined by grid resources provisioned by the DeployR system administrator is an important step when deploying solutions on the Discrete Task Runtime.

    >See the [Grid Resource Management](#gridprimer) section of this tutorial for guidance when setting `maxConcurrency` for your RBroker instance.

### Discrete Task Runtime Suitability

This type of runtime is well-suited to all forms of RBroker prototyping as well as for public facing production deployments. If you anticipate anonymous users making use of DeployR analytics Web services, then this runtime is for you.

**Tip!** If that does not sound like a suitable RBroker runtime for your client application, then consider the [Pooled Task Runtime](#pooled-task-runtime) or the [Background Task Runtime](#background-task-runtime).

### Discrete Task Runtime Programming Model

The Discrete Task Runtime is supported by the DiscreteTaskBroker. This broker executes instances of DiscreteTask. The R session environment allocated on the grid per DiscreteTask can be pre-initialized using DiscreteTaskOptions.


**Java:**

    //
    // 1. Create an instance of DiscreteTaskBroker.
    //
    RBroker rBroker =
        RBrokerFactory.discreteTaskBroker(brokerCfg);

    //
    // 2.1 Create an instance of DiscreteTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    RTask rTask = RTaskFactory.discreteTask("regression",
                                            "demo",
                                            "george",
                                            version,
                                            discreteTaskOptions);

    //
    // 2.2 Create an instance of DiscreteTask.
    //
    // RTasks describing analytics Web services based on arbitrary
    // blocks of R code are not supported by DiscreteTaskBroker.
    //

    //
    // 2.3. Create an instance of DiscreteTask.
    //
    // This RTask describes an analytics Web service based on a
    // URL-addressable R script: [regressionURL]
    //
    // This type of RTask is available only when executing on
    // behalf of an authenticated user with POWER_USER permissions.
    //
    RTask rTask = RTaskFactory.discreteTask(regressionURL,
                                            discreteTaskOptions);

    //
    // 3. Submit instance of RTask to RBroker for execution.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

**JavaScript:**

    // Browser --> window.rbroker
    <script src="rbroker.js"></script>

    // Node.js
    #!/bin/env node
    var rbroker = require('rbroker');

    //
    // 1. Create an instance of DiscreteTaskBroker.
    //
    var dsBroker = rbroker.discreteTaskBroker(brokerCfg);

    //
    // 2.1 Create an instance of DiscreteTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    var rTask = rbroker.discreteTask({ 
        filename: 'regression',
        directory: 'demo',
        author: 'george',
        version: version
        // more discrete-task-options...
    });

    //
    // 2.2 Create an instance of DiscreteTask.
    //
    // RTasks describing analytics Web services based on arbitrary
    // blocks of R code are not supported by DiscreteTaskBroker.
    //
    var rTask = rbroker.discreteTask({ 
      code: codeBlock 
      // more pooled-task-options...
    });

    //
    // 2.3. Create an instance of DiscreteTask.
    //
    // This RTask describes an analytics Web service based on a
    // URL-addressable R script: [regressionURL]
    //
    // This type of RTask is available only when executing on
    // behalf of an authenticated user with POWER_USER permissions.
    //
    var rTask = rbroker.discreteTask({ 
        externalsource: regressionURL
        // more discrete-task-options...
    });

    //
    // 3. Submit instance of RTask to RBroker for execution.
    //
    var rTaskToken = dsBroker.submit(rTask);

**C#:**

    //
    // 1. Create an instance of DiscreteTaskBroker.
    //
    RBroker rBroker = RBrokerFactory.discreteTaskBroker(brokerCfg);

    //
    // 2.1 Create an instance of DiscreteTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    RTask rTask = RTaskFactory.discreteTask("regression",
                                            "demo",
                                            "george",
                                            "",
                                            discreteTaskOptions);

    //
    // 2.2 Create an instance of DiscreteTask.
    //
    // RTasks describing analytics Web services based on arbitrary
    // blocks of R code are not supported by DiscreteTaskBroker.
    //

    //
    // 2.3. Create an instance of DiscreteTask.
    //
    // This RTask describes an analytics Web service based on a
    // URL-addressable R script: [regressionURL]
    //
    // This type of RTask is available only when executing on
    // behalf of an authenticated user with POWER_USER permissions.
    //
    RTask rTask = RTaskFactory.discreteTask(regressionURL, discreteTaskOptions);

    //
    // 3. Submit instance of RTask to RBroker for execution.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

### Discrete Task Runtime Authentication

RBroker authentication is optional for this type of runtime, therefore RTask can be executed on behalf of an authenticated user or anonymously. Whether to use authentication or not depends on your specific deployment goals.

### Discrete Task Runtime Persistence

When RTask are executed on behalf of an authenticated user, optional persistence to the DeployR-repository post-execution is supported. See storageOptions on DiscreteTaskOptions as documented on the [RBroker Framework API](deployr-tools-and-samples.md). When RTasks are executed anonymously, persistence to the DeployR-repository post-execution is unsupported.

### Pooled Task Runtime

The Pooled Task Runtime acquires a dedicated pool of DeployR grid resources at startup, which has a number of important consequences at runtime. These consequences are:

1.  The time taken to initialize the dedicated grid resource pool for this runtime depends on the nature of the PooledCreationOptions indicated on the PooledBrokerConfig and the size of the pool itself.

2.  Every RTask submitted to the RBroker is guaranteed to execute eventually. There is no possibility of failure due to grid resource exhaustion.

3.  This runtime supports a `maxConcurrency` configuration property that limits the maximum number of RTasks that the RBroker will attempt to execute in parallel. By definition, the `maxConcurrency` limit determines the size of the pool.

4.  All RTasks submitted in excess of this limit are automatically queued, which ensures that no more than `maxConcurrency` RTasks are executing on the runtime at any one time.

5.  Tuning the `maxConcurrency` property to reflect the real-world limits determined by grid resources provisioned by the DeployR system administrator is an important step when deploying solutions on the Pooled Task Runtime.

    >See the [Grid Resource Management](#grid-resource-management) section of this tutorial for guidance when setting `maxConcurrency` for your RBroker instance.

### Pooled Task Runtime Suitability

This type of runtime is well suited to production deployments where consistent runtime semantics are required. If you anticipate a high-volume RTask workload, then this runtime is for you. Remember to size the pool in line with expected workload.

>If that does not sound like a suitable RBroker runtime for your client application, then consider the [Discrete Task Runtime](#discrete-task-runtime) or the [Background Task Runtime](#background-task-runtime).

<a name="pooledtaskbroker"></a>
### Pooled Task Runtime Programming Model

The Pooled Task Runtime is supported by the PooledTaskBroker. This broker executes instances of PooledTask. The R session environment assigned to handle the execution of each PooledTask can be pre-initialized using PooledTaskOptions.


**Java:**

    //
    // 1. Create an instance of PooledTaskBroker.
    //
    RBroker rBroker =
        RBrokerFactory.pooledTaskBroker(brokerCfg);

    //
    // 2.1 Create an instance of PooledTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    RTask rTask = RTaskFactory.pooledTask("regression",
                                          "demo",
                                          "george",
                                          version,
                                          pooledTaskOptions);

    //
    // 2.2 Create another instance of PooledTask.
    //
    // This RTask describes an analytics Web service based on an
    // arbitrary block of R code: [codeBlock]
    //
    RTask rTask = RTaskFactory.pooledTask(codeBlock,
                                          pooledTaskOptions);

    //
    // 2.3. Create a third instance of PooledTask.
    //
    // This RTask describes an analytics Web service based on a
    // URL-addressable R script: [regressionURL]
    //
    // This type of RTask is available only when executing on
    // behalf of an authenticated user with POWER_USER permissions.
    //
    RTask rTask = RTaskFactory.pooledTask(regressionURL,
                                          pooledTaskOptions);

    //
    // 3. Submit instance of RTask to RBroker for execution.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

**JavaScript:**

    // Browser --> window.rbroker
    <script src="rbroker.js"></script>

    // Node.js
    #!/bin/env node
    var rbroker = require('rbroker');

    //
    // 1. Create an instance of PooledTaskBroker.
    //
    var pooledBroker = rbroker.pooledTaskBroker(brokerCfg);

    //
    // 2.1 Create an instance of PooledTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    var rTask = rbroker.pooledTask({ 
        filename: 'regression',
        directory: 'demo',
        author: 'george',
        version: version
        // more pooled-task-options...
    });

    //
    // 2.2 Create another instance of PooledTask.
    //
    // This RTask describes an analytics Web service based on an
    // arbitrary block of R code: [codeBlock]
    //
    var rTask = rbroker.pooledTask({ 
      code: codeBlock 
      // more pooled-task-options...
    });

    //
    // 2.3. Create a third instance of PooledTask.
    //
    // This RTask describes an analytics Web service based on a
    // URL-addressable R script: [regressionURL]
    //
    // This type of RTask is available only when executing on
    // behalf of an authenticated user with POWER_USER permissions.
    //
    var rTask = rbroker.pooledTask({ 
      externalsource: regressionURL
      // more pooled-task-options...
    });

    //
    // 3. Submit instance of RTask to RBroker for execution.
    //
    var rTaskToken = pooledBroker.submit(rTask);

**C#:**

    //
    // 1. Create an instance of PooledTaskBroker.
    //
    RBroker rBroker = RBrokerFactory.pooledTaskBroker(brokerCfg);

    //
    // 2.1 Create an instance of PooledTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    RTask rTask = RTaskFactory.pooledTask("regression",
                                          "demo",
                                          "george",
                                          "",
                                          pooledTaskOptions);

    //
    // 2.2 Create another instance of PooledTask.
    //
    // This RTask describes an analytics Web service based on an
    // arbitrary block of R code: [codeBlock]
    //
    RTask rTask = RTaskFactory.pooledTask(codeBlock, pooledTaskOptions);

    //
    // 2.3. Create a third instance of PooledTask.
    //
    // This RTask describes an analytics Web service based on a
    // URL-addressable R script: [regressionURL]
    //
    // This type of RTask is available only when executing on
    // behalf of an authenticated user with POWER_USER permissions.
    //
    RTask rTask = RTaskFactory.pooledTask(regressionURL, pooledTaskOptions);

    //
    // 3. Submit instance of RTask to RBroker for execution.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

### Pooled Task Runtime Authentication

RBroker authentication is required for this type of runtime, therefore RTask will always execute on behalf of an authenticated user. In many instances, this user may represent your client application, and not individual end-users.

### Pooled Task Runtime Persistence

Since RTasks executed on this type of runtime are executing on behalf of an authenticated user, optional persistence to the DeployR-repository following an execution is supported. See storageOptions on PooledTaskOptions as documented on the [RBroker Framework API](deployr-tools-and-samples.md).

### Pooled Task Runtime Resource Management

While this runtime is ideal for high-volume RTask environments, it is important to proactively manage server-side resources dedicated to the pool. One such resource that requires special attention is the DeployR database.

Each RTask results in meta-data, and sometimes data, being persisted to the DeployR database. This data is preserved in the database until the PooledTaskBroker is ultimately released. Given this runtime could theoretically service hundreds of thousands or even millions of RTask we recommend that you periodically release and rebuild your pool which allows the DeployR server to flush old RTask data from the system.

### Background Task Runtime

The Background Task Runtime acquires DeployR grid resources per RTask based on the server-side management of asynchronous grid resources. This has a number of important consequences at runtime:

1.  Unlike the other runtimes, this runtime only schedules RTasks for execution. It delegates actual execution to the server-side job scheduling manager.

2.  Every RTask submitted to the RBroker is guaranteed to execute eventually. There is no possibility of failure due to grid resource exhaustion.

3.  This runtime does not support a `maxConcurrency` configuration property. The server-side job scheduling manager maintains its own queuing systems that function independently of the RBroker configuration.

### Background Task Runtime Suitability

This type of runtime is well-suited to deployments that require periodic, scheduled or batch processing.

>If that does not sound like a suitable RBroker runtime for your client application consider the [*Discrete Task Runtime*](#discrete-task-runtime) or the [*Pooled Task Runtime*](#pooled-task-runtime).

### 2. Background Task Runtime Programming Model

The background task runtime is supported by the BackgroundTaskBroker. This broker schedules instances of BackgroundTask for execution on the server. The R session environment that is assigned to handle the execution of each BackgroundTask can be pre-initialized using BackgroundTaskOptions.


**Java:**

    //
    // 1. Create an instance of BackgroundTaskBroker.
    //
    RBroker rBroker =
        RBrokerFactory.backgroundTaskBroker(brokerCfg);

    //
    // 2.1 Create an instance of BackgroundTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    RTask rTask = RTaskFactory.backgroundTask("Sample Task",
                                              "Sample description",
                                              "regression",
                                              "demo",
                                              "george",
                                              version,
                                              backgroundTaskOptions);

    //
    // 2.2 Create another instance of BackgroundTask.
    //
    // This RTask describes an analytics Web services based on an
    // arbitrary block of R code: [codeBlock]
    //
    RTask rTask = RTaskFactory.backgroundTask("Sample Task",
                                              "Sample description",
                                              codeBlock,
                                              backgroundTaskOptions);

    //
    // 2.3. Create a third instance of BackgroundTask.
    //
    // This RTask describes an analytics Web service based on a
    // URL-addressable R script: [regressionURL]
    //
    // This type of RTask is available only when executing on
    // behalf of an authenticated user with POWER_USER permissions.
    //
    RTask rTask = RTaskFactory.backgroundTask("Sample Task",
                                              "Sample description",
                                              regressionURL,
                                              backgroundTaskOptions);

    //
    // 3. Submit instance of RTask to RBroker for execution.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

**JavaScript:**

    // Browser --> window.rbroker
    <script src="rbroker.js"></script>

    // Node.js
    #!/bin/env node
    var rbroker = require('rbroker');

    //
    // 1. Create an instance of BackgroundTaskBroker.
    //
    var bgBroker = rbroker.backgroundTaskBroker(brokerCfg);

    //
    // 2.1 Create an instance of BackgroundTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    var rTask = rbroker.backgroundTask({
      name: 'Sample Task',
      descr: 'Sample description',
      rscriptname: 'regression',
      rscriptdirectory: 'demo',
      rscriptauthor: 'george',
      rscriptversion: version
      // more background-task-options...
    });

    //
    // 2.2 Create another instance of BackgroundTask.
    //
    // This RTask describes an analytics Web services based on an
    // arbitrary block of R code: [codeBlock]
    //
    var rTask = rbroker.backgroundTask({
      name: 'Sample Task',
      descr: 'Sample description',
      code: codeBlock
      // more background-task-options...
    });

    //
    // 2.3. Create a third instance of BackgroundTask.
    //
    // This RTask describes an analytics Web service based on a
    // URL-addressable R script: [regressionURL]
    //
    // This type of RTask is available only when executing on
    // behalf of an authenticated user with POWER_USER permissions.
    //
    var rTask = rbroker.backgroundTask({
      name: 'Sample Task',
      descr: 'Sample description',
      externalsource: regressionURL
      // more background-task-options...
    });

    //
    // 3. Submit instance of RTask to RBroker for execution.
    //
    var rTaskToken = bgBroker.submit(rTask);

**C#:**

    //
    // 1. Create an instance of BackgroundTaskBroker.
    //
    RBroker rBroker = RBrokerFactory.backgroundTaskBroker(brokerCfg);

    //
    // 2.1 Create an instance of BackgroundTask.
    //
    // This RTask describes an analytics Web service based on a
    // repository-managed R script: /george/demo/regression.R.
    //
    RTask rTask = RTaskFactory.backgroundTask("Sample Task",
                                              "Sample description",
                                              "regression",
                                              "demo",
                                              "george",
                                              "",
                                              backgroundTaskOptions);

    //
    // 2.2 Create another instance of BackgroundTask.
    //
    // This RTask describes an analytics Web services based on an
    // arbitrary block of R code: [codeBlock]
    //
    RTask rTask = RTaskFactory.backgroundTask("Sample Task",
                                              "Sample description",
                                              codeBlock,
                                              backgroundTaskOptions);

    //
    // 2.3. Create a third instance of BackgroundTask.
    //
    // This RTask describes an analytics Web service based on a
    // URL-addressable R script: [regressionURL]
    //
    // This type of RTask is available only when executing on
    // behalf of an authenticated user with POWER_USER permissions.
    //
    RTask rTask = RTaskFactory.backgroundTask("Sample Task",
                                              "Sample description",
                                              regressionURL,
                                              backgroundTaskOptions);

    //
    // 3. Submit instance of RTask to RBroker for execution.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

### Background Task Runtime Authentication

RBroker authentication is required for this type of runtime, therefore RTask will always execute on behalf of an authenticated user. In many instances, that user may represent your client application, and not individual end-users.

### Background Task Runtime Persistence

Since RTasks executed on this type of runtime are executing on behalf of an authenticated user, optional persistence to the DeployR-repository following an execution is supported. See storageOptions on BackgroundTaskOptions as documented on the [RBroker Framework API](deployr-tools-and-samples.md).

>Result data are not directly available on RTaskResult. Instead a DeployR Job identifier is returned. An application developer must use an appropriate [DeployR Client Library](deployr-tools-and-samples.md) that supports APIs for the retrieval of results persisted by the BackgroundTask on DeployR Job.

## RBroker Resource Management

While each RBroker runtime automatically manages all DeployR-related client-side and server-side resources on behalf of client applications, it is the responsibility of the application developer to make an explicit call on RBroker to release any residual resources associated with the broker instance whenever a client application terminates.

The following code snippets demonstrate the mechanism:


**Java:**

    //
    // Release resources held by RBroker.
    //
    // Upon application termination, release any client-side and
    // server-side resources held by the instance of RBroker.
    //
    rBroker.shutdown();

**JavaScript:**

    //
    // Release resources held by RBroker.
    //
    // Upon application termination, release any client-side and
    // server-side resources held by the instance of RBroker.
    //
    rBroker.shutdown()
      .then(function() {
         // successful shutdown...
      });

**C#:**

    //
    // Release resources held by RBroker.
    //
    // Upon application termination, release any client-side and
    // server-side resources held by the instance of RBroker.
    //
    rBroker.shutdown();

>This step is especially important for applications that make use of the [Pooled Task Broker Runtime](#pooled-task-runtime) since significant DeployR grid resources associated with that runtime will remain unavailable until explicitly released.

## RTask Priority Execution

Each RBroker runtime maintains a basic FIFO queue for RTask. Each RTask on that FIFO queue is eventually submitted to the DeployR server for execution. By default, RTask are executed in this FIFO order.

However, there are times when FIFO semantics get in the way of desired client application semantics. For example, when a high priority RTask generated by the client application needs to take precedence over any existing RTasks that may already be in the FIFO queue.

Each RBroker runtime has built-in support for high priority RTasks. When RTasks are submitted as high priority, they jump the default FIFO queue and form their own priority FIFO queue. The following code snippets demonstrate how this mechanism works in practice:


**Java:**

    //
    // 1. RTask Standard Execution
    //
    // Uses the default RBroker FIFO-queue.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

    //
    // 2. RTask Priority Execution
    //
    // Uses the priority RBroker FIFO-queue.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask, true);

**JavaScript:**

    //
    // 1. RTask Standard Execution
    //
    // Uses the default RBroker FIFO-queue.
    //
    var rTaskToken = rBroker.submit(rTask);

    //
    // 2. RTask Priority Execution
    //
    // Uses the priority RBroker FIFO-queue.
    //
    var rTaskToken = rBroker.submit(rTask, true);

**C#:**

    //
    // 1. RTask Standard Execution
    //
    // Uses the default RBroker FIFO-queue.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask);

    //
    // 2. RTask Priority Execution
    //
    // Uses the priority RBroker FIFO-queue.
    //
    RTaskToken rTaskToken = rBroker.submit(rTask, true);

All RTasks on the priority FIFO queue are guaranteed to be executed by the RBroker before any attempt is made to execute RTasks on the default FIFO queue.

## Client Application Simulations

When evaluating any new software technology, one of the first things you are likely to come across is the ubiquituous "Hello World" sample application. Not to be outdone, we provide our own ["Hello World" sample application](#hello-world-example) for the RBroker framework.

While such sample applications provide a great starting point for any new technology, they can only take you so far. This section discusses client application simulations, which is one of the key tools that is built into the RBroker framework that will help you go a whole lot further.

As a client application developer interested in integrating analytics Web services into your custom client application you really have two technical challenges that need solving:

1.  How best to build or extend your custom client application to facilitate the integration of analytics Web services?

2.  How best to integrate analytics Web services to ensure your application meets runtime performance goals?

On the first challenge, we offer no particular guidance, since you already know best in such matters. However, on the second challenge, we believe there are key questions that can and should be asked and answered before full integration commences, such as:

1.  What RBroker runtime should my application be using?

2.  What is the best configuration for my RBroker runtime based on anticipated workload and throughput requirements?

3.  How can assumptions be verified regarding throughput and overall performance of the integration?

If client application developers can answer these kinds of questions without first having to build a complete client application to test drive each scenario, it can make life a lot simpler.

It is for this reason that the RBroker framework supports client application simulations. These simulations are headless client applications that are designed to drive RTask requests through an RBroker instance in order to help developers test, measure and optimize their RBroker integration.

To run a client application simulation simply create a simulation that implements the appropriate interface and then ask an instance of RBroker to run the simulation. The following code snippets demonstrate the mechanism.


**Java:**

    //
    // 1. Create an instance of RBroker.
    //
    RBroker rBroker = RBrokerFactory.discreteTaskBroker(brokerCfg);


    //
    // 2. Create an instance of RTaskAppSimulator.
    //
    // This is your client application simulation. The logic of your
    // simulation can be as simple or as complex as you wish, giving you the
    // flexibility to simulate anything from the most simple to the most
    // complex use cases.
    // 

    SampleAppSimulation simulation = new SampleAppSimulation(rBroker);

    //
    // 3. Launch RTaskAppSimulator simulation.
    //
    // The RBroker will automatically launch and execute your simulation
    // when the simulateApp(RTaskAppSimulator) method is called.
    //
    rBroker.simulateApp(simulation);

**JavaScript:**

    //
    // 1. Create an instance of RBroker.
    //
    var broker = rbroker.discreteTaskBroker(brokerCfg);

    //
    // 2. Create an `app simulator` - SampleAppSimulation
    //
    // This is your client application simulation. The logic of your
    // simulation can be as simple or as complex as you wish, giving you the
    // flexibility to simulate anything from the most simple to the most
    // complex use cases.
    // 

    var simulator = {
       simulateApp: function(dBroker) {
          // implementation...
       }  
    };

    //
    // 3. Launch RTaskAppSimulator simulation.
    //
    // The RBroker will automatically launch and execute your simulation
    // when the simulateApp(simulator) method is called.
    //
    broker.simulateApp(simulator);

**C#:**

	// // 1. Create an instance of RBroker. // RBroker rBroker = RBrokerFactory.discreteTaskBroker(brokerCfg);
	
	// // 2. Create an instance of RTaskAppSimulator. // // This is your client application simulation. The logic of your // simulation can be as simple or as complex as you wish, giving you the // flexibility to simulate anything from the most simple to the most // complex use cases. //
	
	SampleAppSimulation simulation = new SampleAppSimulation(rBroker);
	
	// // 3. Launch RTaskAppSimulator simulation. // // The RBroker will automatically launch and execute your simulation // when the simulateApp(RTaskAppSimulator) method is called. // rBroker.simulateApp(simulation);

Now that we have seen how the basic mechanism works for launching client application simulations, let's take a look at the simulation itself. The following code snippets demonstrate the most simple simulation imaginable, the execution of a single RTask.


**Java:**

    //
    // 1. SampleAppSimulation
    //
    // Demonstrates the simulation of a single RTask execution.
    //
    public class SampleAppSimulation implements RTaskAppSimulator,
                                                RTaskListener {

        //
        // RTaskAppSimulator interface.
        //
        // This represents a complete client application simulation.
        //
        public void simulateApp(RBroker rBroker) {


            //
            // 1. Prepare RTask(s) for simulation.
            //
            // In the example, we will simply simulate the execution
            // of a single RTask.
            //

            RTask rTask = RTaskFactory.discreteTask("regression",
                                                    "demo",
                                                    "testuser",
                                                    null, null);
            try {

                RTaskToken taskToken = rBroker.submit(rTask);

            } catch(Exception ex) {
                // Exception on simulation, handle as appropriate.
            }
        }

        //
        // RTaskListener interface.
        //

        public void onTaskCompleted(RTask rTask, RTaskResult rTaskResult) {
            // Completed RTask. Handle as appropriate.
        }

        public void onTaskError(RTask rTask, Throwable throwable) {
            // Failed RTask. Handle as appropriate.
        }

    }

**JavaScript:**

    //
    // 1. SampleAppSimulation
    //
    // Demonstrates the simulation of a single RTask execution.
    //
    var SampleAppSimulation = {

        //
        // Simulator interface `simulateApp(broker)`
        //
        // This represents a complete client application simulation.
        //
        simulateApp: function (broker) {

            broker.complete(function (rTask, rTaskResult) {
                // Completed RTask. Handle as appropriate.            
            })
            .error(function (err) {
                // Failed RTask. Handle as appropriate.            
            });        

            //
            // 1. Prepare RTask(s) for simulation.
            //
            // In the example, we will simply simulate the execution
            // of a single RTask.
            //
            var rTask = rbroker.discreteTask({ 
                filename: 'rtScore',            
                directory: 'demo',
                author: 'testuser'            
            });

            var taskToken = dBroker.submit(rTask);        
        } 
    };

**C#:**

    //
    // 1. SampleAppSimulation
    //
    // Demonstrates the simulation of a single RTask execution.
    //
    public class SampleAppSimulation : RTaskAppSimulator, RTaskListener 
    {

        //
        // RTaskAppSimulator interface.
        //
        // This represents a complete client application simulation.
        //
        public void simulateApp(RBroker rBroker) 
        {


            //
            // 1. Prepare RTask(s) for simulation.
            //
            // In the example, we will simply simulate the execution
            // of a single RTask.
            //

            RTask rTask = RTaskFactory.discreteTask("regression",
                                                    "demo",
                                                    "testuser",
                                                    "", null);
            try 
            {
                RTaskToken taskToken = rBroker.submit(rTask);
            } 
            catch(Exception ex)
            {
                // Exception on simulation, handle as appropriate.
            }
        }

        //
        // RTaskListener interface.
        //

        public void onTaskCompleted(RTask rTask, RTaskResult rTaskResult)
        {
            // Completed RTask. Handle as appropriate.
        }

        public void onTaskError(RTask rTask, String error) 
        {
            // Failed RTask. Handle as appropriate.
        }

    }

As you can see, asynchronous callbacks from RBroker allow you to track the progress of the simulation.

Now, let's consider the building of a simulation of a real-time scoring engine, which is something a little more sophisticated. The following code snippets demonstrate what's involved.


**Java:**

    public class SampleAppSimultation implements RTaskAppSimulator,
                                                 RTaskListener {

        private long SIMULATE_TOTAL_TASK_COUNT = 1000;
        private long SIMULATE_TASK_RATE_PER_MINUTE = 200L;

        /*
         * RTaskAppSimulator method.
         */

        public void simulateApp(RBroker rBroker) {

            /*
             * Submit task(s) to RBroker for execution.
             */

            //
            // In the example, we simulate the execution of
            // SIMULATE_TOTAL_TASK_COUNT RTask.
            //

            for(int tasksPushedToBroker = 0;
                    tasksPushedToBroker<SIMULATE_TOTAL_TASK_COUNT;
                    tasksPushedToBroker++) {

                try {

                    //
                    // Prepare RTask for real-time scoring.
                    //
                    // In this example, we pass along a unique customer ID
                    // with each RTask. In a real-world application, the input
                    // parameters on each RTask will vary depending on need,
                    // such as customer database record keys and supplementary
                    // parameter data to facilitate the scoring.
                    //

                    PooledTaskOptions taskOptions =
                                        new PooledTaskOptions();
                    taskOptions.rinputs = Arrays.asList((RData)
                       RDataFactory.createNumeric("customerid", tasksPushedToBroker));
                    taskOptions.routputs = Arrays.asList("score");

                    RTask rTask = RTaskFactory.pooledTask("rtScore",
                                                          "demo",
                                                          "testuser",
                                                          null,
                                                          taskOptions);

                    RTaskToken taskToken = rBroker.submit(rTask);

                    //
                    // The following logic controls the simulated rate of
                    // RTasks being submitted to the RBroker, essentially
                    // controlling the simulated workload.
                    //

                    if(tasksPushedToBroker <
                            (SIMULATE_TOTAL_TASK_COUNT - 1)) {


                        try {

                            if(SIMULATE_TASK_RATE_PER_MINUTE != 0L) {

                                long staggerLoadInterval =
                                    60L / SIMULATE_TASK_RATE_PER_MINUTE;
                                Thread.currentThread().sleep(
                                                staggerLoadInterval * 1000);
                            }

                        } catch(InterruptedException iex) {}
                    }

                } catch(Exception ex) {
                    // Exception on simulation. Handle as appropriate.
                }
            }

        }

        /*
         * RTaskListener interface.
         */

        public void onTaskCompleted(RTask rTask, RTaskResult rTaskResult) {
            // Completed RTask. Handle as appropriate.
        }

        public void onTaskError(RTask rTask, Throwable throwable) {
            // Failed RTask. Handle as appropriate.
        }
    }

**JavaScript:**

    var SIMULATE_TOTAL_TASK_COUNT     = 1000,
        SIMULATE_TASK_RATE_PER_MINUTE = 200,
        simulationStartTime           = 0,
        simulationStartTime           = new Date().getTime(),
        sleep                         = 0;

    var SampleAppSimultation = {

        simulateApp: function (broker) {

            function staggeredLoad(tasksPushedToBroker, sleep) {          
               //
               // Prepare RTask for real-time scoring.
               //
               // In this example, we pass along a unique customer ID
               // with each RTask. In a real-world application, the input
               // parameters on each RTask will vary depending on need,
               // such as customer database record keys and supplementary
               // parameter data to facilitate the scoring.
               //
               setTimeout(function() {
                 var rTask = rbroker.pooledTask({
                    filename: 'rtScore',
                    directory: 'demo',
                    author: 'testuser',
                    rinputs: [ { 
                      type: RType.RNUMERIC, 
                      name: 'customerid', 
                      value: tasksPushedToBroker
                    } ],
                    routputs: ['score']              
                  });

                 broker.submit(rTask, false);
               }, sleep);
            }

            broker.complete(function (rTask, rTaskResult) {
                // Completed RTask. Handle as appropriate.
            })
            .error(function (err) {
                // Failed RTask. Handle as appropriate.
            });           

            //
            // In the example, we simulate the execution of
            // SIMULATE_TOTAL_TASK_COUNT RTask.
            //
            for(var tasksPushedToBroker = 0;
                    tasksPushedToBroker < SIMULATE_TOTAL_TASK_COUNT;
                    tasksPushedToBroker++) {

               //
               // The following logic controls the simulated rate of
               // RTasks being submitted to the RBroker, essentially
               // controlling the simulated workload.
               //
               if(tasksPushedToBroker < (SIMULATE_TOTAL_TASK_COUNT - 1)) {              
                  if(SIMULATE_TASK_RATE_PER_MINUTE !== 0) {
                     var staggerLoadInterval = 60 / SIMULATE_TASK_RATE_PER_MINUTE;
                     sleep += (staggerLoadInterval * 1000);                 
                  }
                  staggeredLoad(tasksPushedToBroker, sleep);
                }
            }
        } 
    };

**C#:**

    public class SampleAppSimultation : RTaskAppSimulator, RTaskListener
     {

        RTask rTask;
        RTaskToken rToken;
        int SIMULATE_TOTAL_TASK_COUNT = 100;
        int SIMULATE_TASK_RATE_PER_MINUTE = 200;

        /*
         * RTaskAppSimulator method.
         */

        public void simulateApp(RBroker rBroker)
        {

            /*
             * Submit task(s) to RBroker for execution.
             */

            //
            // In the example, we simulate the execution of
            // SIMULATE_TOTAL_TASK_COUNT RTask.
            //

            for( int tasksPushedToBroker = 0; tasksPushedToBroker < SIMULATE_TOTAL_TASK_COUNT; tasksPushedToBroker++)
            {

                try 
                {

                    //
                    // Prepare RTask for real-time scoring.
                    //
                    // In this example, we pass along a unique customer ID
                    // with each RTask. In a real-world application, the input
                    // parameters on each RTask will vary depending on need,
                    // such as customer database record keys and supplementary
                    // parameter data to facilitate the scoring.
                    //
                    PooledTaskOptions taskOptions = new PooledTaskOptions();

                    List<String> outputs = new List<String>();
                    outputs.Add("score");
                    taskOptions.routputs = outputs;

                    List<RData> inputs = new List<RData>();
                    inputs.Add(RDataFactory.createNumeric("customerid", tasksPushedToBroker));
                    taskOptions.rinputs = inputs;


                    RTask rTask = RTaskFactory.pooledTask("rtScore",
                                                          "demo",
                                                          "testuser",
                                                          "",
                                                          taskOptions);

                    RTaskToken taskToken = rBroker.submit(rTask);

                    //
                    // The following logic controls the simulated rate of
                    // RTasks being submitted to the RBroker, essentially
                    // controlling the simulated workload.
                    //

                    if(tasksPushedToBroker <(SIMULATE_TOTAL_TASK_COUNT - 1))
                    {
                        if(SIMULATE_TASK_RATE_PER_MINUTE != 0) 
                        {
                            int staggerLoadInterval = 60 / SIMULATE_TASK_RATE_PER_MINUTE;
                            Thread.Sleep(staggerLoadInterval * 1000);
                        }
                    }

                } 
                catch(Exception ex) 
                {
                    // Exception on simulation. Handle as appropriate.
                }
            }

        }

        /*
         * RTaskListener interface.
         */

        public void onTaskCompleted(RTask rTask, RTaskResult rTaskResult) 
        {
            // Completed RTask. Handle as appropriate.
        }

        public void onTaskError(RTask rTask, String error) 
        {
            // Failed RTask. Handle as appropriate.
        }
    }

By running client application simulations, experiencing live execution result data and execution failures, and observing overall throughput, a client application developer can learn how to tune the RBroker runtime and RTask options for predictable, even optimal runtime performance.

And to aid further in the measurement of analytics Web service runtime performance, developers can take advantage of yet another feature of the RBroker framework, [client application profiling](#client-application-profiling).

## Client Application Profiling

As we just learned, the [client application simulation](#client-application-simulations) feature of the RBroker framework helps you quickly answer key integration questions by supporting rapid, iterative experimentation and testing. The client application profiling feature extends these capabilities by helping you to accurately measure the runtime impact of each simulated test, which can greatly improve the quality of any integration.

The client application profiling features are also available beyond simulations, so production environments can also make use of this feature, for example, to maintain audit logs that detail the runtime performance details of each analytics Web service invocation.

As with most things in the RBroker framework, it is very simple to activate this feature. First, note that each RTaskResult has built-in profiling data. Second, each RBroker runtime generates runtime profiling events. By registering the appropriate asynchronous listener, a client application can receive these profiling events.

The following code snippets extend the sample demonstrated in the [client application simulation](#client-application-simulations) section with support for handling runtime profiling events.


**Java:**

    //
    // 1. SampleAppSimulation
    //
    // Demonstrates the simulation of a single RTask execution.
    //
    public class SampleAppSimulation implements RTaskAppSimulator,
                                                RTaskListener,
                                                RBrokerListener {

        private final RBroker rBroker;

        public SampleAppSimulation(RBroker rBroker) {
            this.rBroker = rBroker;
            rBroker.addTaskListener(this);
        }

        //
        // RTaskAppSimulator interface.
        //
        // This represents a complete client application simulation.
        //
        public void simulateApp(RBroker rBroker) {


            //
            // 1. Prepare RTask(s) for simulation.
            //
            // In the example, we will simulate the execution
            // of a single RTask.
            //

            RTask rTask = RTaskFactory.discreteTask("regression",
                                                    "demo",
                                                    "testuser",
                                                    null, null);
            try {

                RTaskToken taskToken = rBroker.submit(rTask);

            } catch(Exception ex) {
                // Exception on simulation. Handle as appropriate.
            }
        }

        //
        // RTaskListener interface.
        //

        public void onTaskCompleted(RTask rTask, RTaskResult rTaskResult) {
            //
            // For profiling, see the RTaskResult.getTimeOn*() properties.
            //
        }

        public void onTaskError(RTask rTask, Throwable throwable) {
            //
            // Failed RTask. Handle as appropriate.
            //
        }


        //
        // RBrokerListener interface.
        //

        public void onRuntimeError(Throwable throwable) {
            // Runtime exception. Handle as appropriate.
        }

        public void onRuntimeStats(RBrokerRuntimeStats stats, int maxConcurrency) {
            //
            // For profiling, see the RBrokerRuntimeStats.getTimeOn*() properties.
            //
        }

    }

**JavaScript:**

    var rbroker = require('rbroker');

    //
    // 1. SampleAppSimulation
    //
    // Demonstrates the simulation of a single RTask execution.
    //
    var SampleAppSimulation = {

        //
        // Simulator interface `simulateApp(broker)`
        //
        // This represents a complete client application simulation.
        //
        simulateApp: function (broker) {

            broker.complete(function (rTask, rTaskResult) {
               //
               // For profiling, see the `rTaskResult` properties.
               //            
            })
            .error(function (err) {
               //
               // Failed RTask. Handle as appropriate.
               //
            })
            .progress(function(stats) {
               //
               // For profiling, see the `stats` properties.
               //
            }) 
            .idle(function () { 
               //
               // Nothing pending 
               //
            });

            //
            // 1. Prepare RTask(s) for simulation.
            //
            // In the example, we will simply simulate the execution
            // of a single RTask.
            //
            var rTask = rbroker.discreteTask({ 
                filename: 'rtScore',            
                directory: 'demo',
                author: 'testuser'            
            });

            var taskToken = dBroker.submit(rTask);        
        } 
    };

**C#:**

    //
    // 1. SampleAppSimulation
    //
    // Demonstrates the simulation of a single RTask execution.
    //
    public class SampleAppSimulation : RTaskAppSimulator, RTaskListener, RBrokerListener 
    {

        private RBroker rBroker;

        public SampleAppSimulation(RBroker rBroker) 
        {
            this.rBroker = rBroker;
            rBroker.addTaskListener(this);
        }

        //
        // RTaskAppSimulator interface.
        //
        // This represents a complete client application simulation.
        //
        public void simulateApp(RBroker rBroker) 
        {

            //
            // 1. Prepare RTask(s) for simulation.
            //
            // In the example, we will simulate the execution
            // of a single RTask.
            //

            RTask rTask = RTaskFactory.discreteTask("regression",
                                                    "demo",
                                                    "testuser",
                                                    "", null);
            try 
            {
                RTaskToken taskToken = rBroker.submit(rTask);

            } catch(Exception ex) 
            {
                // Exception on simulation. Handle as appropriate.
            }
        }

        //
        // RTaskListener interface.
        //

        public void onTaskCompleted(RTask rTask, RTaskResult rTaskResult) 
        {
            //
            // For profiling, see the RTaskResult.getTimeOn*() properties.
            //
        }

        public void onTaskError(RTask rTask, String error) 
        {
            //
            // Failed RTask. Handle as appropriate.
            //
        }


        //
        // RBrokerListener interface.
        //

        public void onRuntimeError(String error)
        {
            // Runtime exception. Handle as appropriate.
        }

        public void onRuntimeStats(RBrokerRuntimeStats stats, int maxConcurrency) 
        {
            //
            // For profiling, see the RBrokerRuntimeStats.getTimeOn*() properties.
            //
        }
    }

<a name=gridprimer></a>

## Grid Resource Management

This short primer is provided as a note for client application developers. The guidance presented here should prove useful when considering suitable values for `maxConcurrency` on an RBrokerConfig or when discussing DeployR grid resources with a DeployR system administrator.

### What is the DeployR Grid?

The DeployR grid is a flexible, network of collaborating nodes that contribute resources (memory, CPU, disk) to the DeployR server in order to facilitate the execution of a myriad of different types of operations, many of which are exposed by the RBroker Framework.

### How is the DeployR Grid Configured?

To simplify grid management for the system administrator, the DeployR grid identifies three distinct types of operation, known as **anonymous**, **authenticated** and **asynchronous** operations respectively. These names simply provide a high-level description for related sets of runtime operations that share common characteristics.

Typically, each node on the grid can be configured by the server administrator to support just one type of operation. For example, the administrator could designate a node to asynchronous operations only. If the administrator does not want to limit the availability of the resources on a particular grid node to a single type of operation they can assign a special **mixed mode** operating type to the node. This designation permits any operation to take advantage of the grid node resources at runtime.

### How Does the DeployR Grid Configuration Impact on RBroker?

Each RBroker runtime automatically acquires grid resources at runtime on behalf of the client application. The exact nature of the resources acquired by each runtime are discussed here:

The [Discrete Task Runtime](#discrete-task-runtime) submits all tasks, whether executing on behalf of an authenticated or anonymous RBroker, to run on grid nodes configured for anonymous operations. If the server can not find an available slot on that subset of grid nodes, then the task may execute on a grid node configured for mixed mode operations.

>If your DiscreteTaskBroker instance is returning RTaskResults that indicate RGridException, then consider speaking to your DeployR system administrator about provisioning additional resources for existing grid nodes or even additional grid nodes for  <b>anonymous</b> operations. These additional resources will support greater levels of concurrent workload. Once provisioned, make sure to increase the `maxConcurrency` configuration option on your instance of RBroker to take full advantage of the new resources on the grid.

The [*Pooled Task Runtime*](#pooled-task-runtime) submits all tasks to run on grid nodes configured for authenticated operations. If the server can not find an available slot on that subset of grid nodes, then the task may execute on a grid node configured for mixed mode operations.

>If the size of the pool allocated to your PooledTaskBroker is less than requested by your `maxConcurrency` configuration option on your instance of RBroker, then consider speaking to your DeployR system administrator about provisioning additional resources for existing grid nodes or even additional grid nodes for <b>authenticated</b> operations.
>
>Also you may want to discuss increasing the per-authenticated user concurrent operation limit, which is a setting found under **Server Policies** in the Administration Console. Ultimately, this setting determines the maximum pool size that a single instance of an authenticated PooledTaskBroker can allocate.

The [*Background Task Runtime*](#background-task-runtime) submits all tasks to run on grid nodes configured for asynchronous operations. If the server can not find an available slot on that subset of grid nodes then the task may execute on a grid node configured for mixed mode operations.

>If you feel that too many of your BackgroundTask are waiting on a queue pending available resources, then consider speaking with your DeployR system administrator about provisioning additional resources for existing grid nodes or even additional grid nodes for <b>asynchronous</b> operations.