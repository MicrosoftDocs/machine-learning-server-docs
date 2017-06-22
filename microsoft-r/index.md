---

# required metadata
title: "Introduction to Microsoft R Server and R Client"
description: "Learn about Microsoft R features and components in R Server, R Client, R Open."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/10/2017"
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
ms.technology:
  - r-client
  - r-server
ms.custom: ""

---

# Microsoft R Server and R Client

R is the world’s most powerful, and preferred, programming language for statistical computing, machine learning, and visualizations. As an open source technology, R is supported by a thriving global community of users, developers, and contributors. Developers frequently provide tools incorporating their expertise in the form of R packages. Although R is widely used, deploying R in an enterprise setting has presented certain challenges, especially as the volume of data rises, or requirements for scale and support in production environments emerge.

Microsoft R fills the functional void in enterprise deployments by providing a collection of servers and tools that extend the capabilities of R. The Microsoft R product family builds on top of open source R, offering free and commercial products in the form of [Microsoft R Server](rserver.md), [Microsoft R Client](r-client.md), and [Microsoft R Open](https://mran.microsoft.com/open/). In addition to the over 10,000 standard R packages available to all R users, Microsoft R Server and R Client include additional R packages and connectivity tools that enable remote compute context and remote execution, web service deployment, machine learning integration, and scalable solutions through clusters or parallelized workloads on platforms that support it.

## Quickstarts and Step-by-Step Tutorials

In minutes, you can step through a classic what-if problem using sample data and R script in the first quickstart. Additional tutorials offer a hands-on experience to practial data science tasks.

* [Quickstart: Run R code in Microsoft R](quickstart-r-code.md) 
* [Tutorial: Explore R-to-RevoScaleR](microsoft-r-tutorial-R2RevoScaleR.md) 
* [Tutorial: Import and transform data](scaler-getting-started-data-import-exploration.md)  
* [Tutorial: Visualize and analyze data](scaler-getting-started-data-visualization-analysis.md) 

## Next steps

If you are new to Microsoft R, we recommend starting with either the free R Server developer edition, or the free R Client, and an integrated development environment like [R Tools for Visual Studio (RTVS)](https://docs.microsoft.com/visualstudio/rtvs/installation). This configuration gives you Microsoft R Open with full support of all base R functions so that you can write R-only solutions, but also includes Microsoft R proprietary packages that run locally on your development computer.

Since the R engine lies underneath, you can use the R Core Team manuals that are part of every R distribution to learn how to code in R. Built-in manuals include *An Introduction to R*, *The R Language Definition*, *Writing R Extensions* and more. Beyond the standard R manuals, there are many other resources. [Learn about them here](microsoft-r-more-resources.md).

However, because you have R Client, your script can also include functions shipped only with Microsoft R products, including the MicrosoftML, olapR, mrsdeploy, and RevoScaleR packages. All of these packages are available in both R Client and R Server, but at different levels of capacity.

Tutorials in Microsoft R product documentation will help you learn how to use the functions in the proprietary packages:  

+ [Practice data import and exploration](scaler-getting-started-data-import-exploration.md)

+ [Explore R and ScaleR in 25 functions](microsoft-r-tutorial-R2RevoScaleR.md)

+ [Overview of MicrosoftML algorithms](microsoftml/microsoftml.md)

Learn more about Microsoft R in these articles as well:

+ [What's new in R Server](rserver-whats-new.md)

+ [What's new in R Client](rserver-whats-new.md)

+ [Microsoft R Product Web Page](https://www.microsoft.com/en-us/cloud-platform/r-server)

+ [Supported platforms in Microsoft R Server](rserver-install-supported-platforms.md)

+ [Microsoft R Client Get Started](r-client-get-started.md)

+ [Microsoft R Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)

+ [SQL Server Machine Learning Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx)

+ [Additional Resources](microsoft-r-more-resources.md)

