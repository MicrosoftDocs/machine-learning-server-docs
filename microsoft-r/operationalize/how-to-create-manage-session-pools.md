---

# required metadata
title: "Create and manage session pools for fast web service connections in R (Machine Learning Server)"
description: "Allocate resources for pre-loading web service connections and dependencies in R solutions (Machine Learning Server ). "
keywords: ""
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
#ms.custom: ""
---

# How to create and manage session pools for fast web service connections**

**Applies to: Machine Learning Server 9.3 (R)**

Fast connections to a web service are possible when you create sessions and load dependencies in advance. Sessions are available in a dedicated pool, where each session is an instance of the R interpreter. Dependences are pre-loaded once per web service and shared by all connections. For example, creating ten sessions in advance for a web service that uses numpy, pandas, scikit, revoscalepy, microsoftml, and azureml-model-management-sdk would result in ten instances of the Python interpreter and one copy of each   

## See also

 + [What are web services in Machine Learning Server?](concept-what-are-web-services.md)
 + [Evaluate web service capacity: generic session pools](configure-evaluate-capacity.md#shell-pools)