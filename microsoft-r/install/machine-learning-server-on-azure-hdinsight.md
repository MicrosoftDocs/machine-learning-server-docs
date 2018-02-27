---

# required metadata
title: "R Server on HDInsight"
description: "R Server on HDInsight introduction"
keywords: "Machine Learning Server , Microsoft R Server, HDInsight"
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "article"
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

# R Server on HDInsight

Azure HDInsight is Microsoft's distribution of Hadoop in the cloud. One way to provision the service is with R Server pre-installed on all nodes in the cluster. On HDInsight, the current version of R Server is 9.1, built on open-source R ([Microsoft R Open 3.3.3](https://mran.microsoft.com/release-history)).

To get R Server on HDInsight, do the following:

+ Sign in to the [Azure portal](https://portal.azure.com/), search for *HDInsight*.
+ Select HDInsight and click **Create**.
+ In Cluster type, choose **R Server** and continue with the rest of the [installation steps](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started).

Once the cluster is deployed, you can connect to the edge node and begin development. Your R script or code can call base R functions, third-party packages compatible with R 3.3.3, and these R Server 9.1 packages: [RevoScaleR](../r-reference/revoscaler/revoscaler.md), [MicrosoftML](../r-reference/MicrosoftML/MicrosoftML-package.md), [mrsdeploy](../r-reference/mrsdeploy/mrsdeploy-package.md). 

R Server is engineered for [distributed computing](../r/how-to-revoscaler-distributed-computing.md) across multiple nodes, aggregating calculations into a single result. Multi-threaded math libraries and transparent parallelization in Machine Learning Server can handle up to 1000x more data and up to 50x faster speeds than open-source R—helping you train more accurate models for better predictions than previously possible.

An R cluster is basically a Spark cluster with R Server libraries and interpreters. If you are not familiar with Spark, take a few minutes to review the [Spark cluster documentation for HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-overview).

R Server is installed on all of the nodes in the cluster: HN (head node), EN (edge node), WN (worker nodes).

Edge nodes are the primary nodes used for R development. Typically, R code starts on this node, and then reaches out to worker nodes to access data and operations. Worker nodes are used when you set an RxSpark compute context. Within this context, your RevoScaleR and MicrosoftML functions execute on the worker nodes.

Operationalization, which provides logging, diagnostics, and web service deployment, requires additional configuration; it is not preconfigured in the cluster automatically. For steps, see [Configure Microsoft R Server operationalization](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started#configure-microsoft-r-server-operationalization).


## Next steps

+ [R Server for HDInsight product page](https://azure.microsoft.com/services/hdinsight/r-server/)
+ [R Server for HDInsight documentation](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-overview)
+ [Get started with R on HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started)
