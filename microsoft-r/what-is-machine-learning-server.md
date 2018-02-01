---
# required metadata
title: "Welcome to Machine Learning Server "
description: "Develop machine learning models and scripts in Python and R for on-prem deployment behind the firewall. Microsoft R Server and Python packages and interpreters are included."
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "get-started-article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
ms.suite: "machine-learning"
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Welcome to Machine Learning Server

Microsoft Machine Learning Server is your flexible enterprise platform for analyzing data at scale, building intelligent apps, and discovering valuable insights across your business with full support for Python and R. Machine Learning Server meets the needs of all constituents of the process – from data engineers and data scientists to line-of-business programmers and IT professionals. It offers a choice of languages and features algorithmic innovation that brings the best of open-source and proprietary worlds together. 

R support is built on a legacy of Microsoft R Server 9.x and Revolution R Enterprise products. Significant machine learning and AI capabilities enhancements have been made in every release. Python support was added in the previous release. Machine Learning Server supports the full data science lifecycle of your Python-based analytics. 

Additionally, Machine Learning Server enables [operationalization support](what-is-operationalization.md) so you can deploy your models to a scalable grid for both batch and real-time scoring.


## Why choose Machine Learning Server

Microsoft Machine Learning Server includes:

+ **High-performance machine learning and AI wherever your data lives**

  The volume of the data used by enterprises to make informed business decisions is growing exponentially. With Machine Learning Server, Microsoft takes the modern approach of bringing the computation to the data in order to unlock intelligence. Over time this data often spreads across multiple data platforms creating the challenge of how to get AI to where your data is. Machine Learning Server runs [on-premises and in the cloud on a variety of platforms](install/r-server-install-supported-platforms.md), in a variety of deployments, and can support the need to have machine learning and analytics across multiple platforms.

+ **The best AI innovation from Microsoft and open-source**

  Microsoft strives to make AI accessible to every individual and organization. Microsoft Machine Learning Server includes a rich set of highly-scalable, distributed set of algorithms such as [RevoscaleR](r-reference/revoscaler/revoscaler.md), [revoscalepy](python-reference/revoscalepy/revoscalepy-package.md), and [microsoftML](python-reference/microsoftml/microsoftml-package.md) that can work on data sizes larger than the size of physical memory, and run on a wide variety of platforms in a distributed manner. Learn more about the collection of Microsoft's custom [R packages](r-reference/introducing-r-server-r-package-reference.md) and [Python packages](python-reference/introducing-python-package-reference.md) included with the product.
  
  Machine Learning Server bridges these Microsoft innovations and those coming from the open-source community (R, Python and AI toolkits) all on top of a single enterprise-grade platform. Any R or Python open-source machine learning package can work side by side with any proprietary innovation from Microsoft. 

+ **Simple, secure, and high-scale operationalization and administration**

  Enterprises relying on traditional operationalization paradigms and environments end up investing much time and effort towards this area. Pain points often resulting in inflated costs and delays include: the translation time for models, iterations to keep them valid and current, regulatory approval, managing permissions through operationalization.

  Machine Learning Server offers best-in-class [operationalization](what-is-operationalization.md) -- from the time a machine learning model is completed, it takes just a few clicks to generate web services APIs. These [web services](operationalize/concept-what-are-web-services.md) are hosted on a server grid on-premises or in the cloud and can be integrated with line-of-business applications. Additionally, Machine Learning Server integrates seamlessly with [Active Directory and Azure Active Directory](operationalize/configure-authentication.md) and includes [role-based access control](operationalize/configure-roles.md) to satisfy security and compliance needs of your enterprise. The ability to deploy to an elastic grid lets you scale seamlessly with the needs of your business, both for batch and real-time scoring.

+ **Deep ecosystem engagements to deliver customer success with optimal total cost of ownership**

  Individuals embarking on the journey of making their applications intelligent or simply wanting to learn the new world of AI and machine learning, need the right resources to help them get started. In addition to this documentation, Microsoft provides several learning resources and has engaged several training partners to help you ramp up and become productive quickly.


## Key features of Machine Learning Server

The following features are included in Machine Learning Server. For feature descriptions in this release, see [What's New in Machine Learning Server](whats-new-in-machine-learning-server.md).

| Feature category | Description |
|------------------|-------------|
| R-enabled | [R packages](r-reference/introducing-r-server-r-package-reference.md) for solutions written in R, with an open-source distribution of R and run-time infrastructure for script execution. Everything you had in R Server and more. |
| Python-enabled | [Python modules](python-reference/introducing-python-package-reference.md) for solutions written in Python,  with an open-source distribution of Python and run-time infrastructure for script execution.  
| [Pre-trained models](install/microsoftml-install-pretrained-models.md) | For visual analysis and text sentiment analysis, ready to score data you provide. |
| [Deploy and consume](what-is-operationalization.md) | Operationalize your server and deploy solutions as a web service. |
| [Remote execution](r/how-to-execute-code-remotely.md) | Start remote sessions on a Machine Learning Server on your network from your client workstation. |
| scale out on premises | Clustered topologies for [Spark on Hadoop](install/machine-learning-server-hadoop-install.md), and Windows or Linux using the [operationalization capability](operationalize/configure-start-for-administrators.md) built into Machine Learning Server. |



## Next steps

[Install Machine Learning Server](install/machine-learning-server-install.md) on a supported platform. 

[Choose a quickstart](index.yml) to test-drive capabilities in 10 minutes or less. Move on to tutorials for in-depth exploration.

> [!Note]
> You can use any R or Python IDE to write code using libraries from Machine Learning Server. Microsoft offers Python for Visual Studio and R for Visual Studio. To use Jupyter Notebook, see [How to configure Jupyter Notebooks](python/how-to-revoscalepy-jupyter-nb-config.md).

## See also

+ [What's new in Machine Learning Server](whats-new-in-machine-learning-server.md)
+ [Learning Resources](resources-more.md)
