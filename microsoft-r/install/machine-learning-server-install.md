---
# required metadata
title: "How to install Machine Learning Server "
description: "How to install Machine Learning Server."
keywords: ""

author: "dphansen"
ms.author: "davidph"
ms.date: "11/26/2018"
ms.topic: "conceptual"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Install and configure Machine Learning Server

Machine Learning Server is available on a number of [platforms](r-server-install-supported-platforms.md). Looking for earlier versions? See [Installation guides for earlier releases](r-server-install.md) for links.

## Choose a configuration

This section covers topology configurations from single to multi-machine topologies.

### Standalone deployments

Standalone servers describe a working experience where script development and execution occur on the same Windows or Linux machine. If you install the free developer edition on either Windows or Linux, a standalone server is the likely configuration. You could also install the server on a virtual machine accessed by multiple users in turn. 

   ![standalone server on Windows or Linux](./media/machine-learning-server-install/install-topology-standalone.png)

### Client and server multi-user topology

A client-server configuration is more common in a team environment. You can install free Python client libraries or free R Client on any workstation, write script on the workstation, and then deploy script as a [web service](~/operationalize/concept-what-are-web-services.md) to a Machine Learning Server configured to [operationalize](~/what-is-operationalization.md) analytics, thus extending your options for interacting with and consuming script on a server. 

For R development only, you can also use [remote execution](~/r/how-to-execute-code-remotely.md) in this configuration: switch from local to remote server within a session to write and run script interactively. 

On the consumption side, you can write custom apps or consume an R or Python solution as a web service.

   ![client server topology](./media/machine-learning-server-install/install-topology-client-server.png)

### Large scale multi-user topologies

Scale-out topologies are available in two forms. One option is to use a distributed platform like Hadoop MapReduce or Spark (on Linux). A second option is to install Machine Learning Server on multiple computers (Windows or Linux), each one configured as either a web node or compute node that work together.

For Hadoop and Spark, you can write and run script locally and then [push the compute context](~/r/concept-what-is-compute-context.md) to the Hadoop or Spark cluster. For web and compute nodes, use the [operationalization capabilities](~/what-is-operationalization.md) in Machine Learning Server to distribute workloads on appropriate nodes.

   ![scaleout topology on clustered computers](./media/machine-learning-server-install/install-topology-scaleout.png)

## Choose a platform

Machine Learning Server runs on Windows, Linux, and Hadoop. Review the [supported platforms list](r-server-install-supported-platforms.md) for specific operating system versions.

## Begin installation

The following links provide installation and configuration instructions.

+ [Install on Windows](machine-learning-server-windows-install.md)    
+ [Install on Linux](machine-learning-server-linux-install.md)    
+ [Install on Hadoop](machine-learning-server-hadoop-install.md)    
+ [Configure server to operationalize (deploy/consume) analytics](../operationalize/configure-start-for-administrators.md)    
+ [Add pre-trained models](microsoftml-install-pretrained-models.md)    
+ [Provision on the cloud](machine-learning-server-in-the-cloud.md)    

## Local Tools

+ [Microsoft R Client](../r-client/what-is-microsoft-r-client.md)   
+ [Link Python tools and IDES to the MLServer Python interpreter ](../python/quickstart-python-tools.md) 
+ [Python interpreter & libraries on Windows](python-libraries-interpreter.md)    
