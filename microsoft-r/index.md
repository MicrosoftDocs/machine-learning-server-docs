---

# required metadata
title: "Microsoft R Server and R Client Getting Started Guide"
description: "Microsoft R features and components overview."
keywords: ""
author: "j-martens"
manager: "paulette.mckay"
ms.date: "06/13/2016"
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

#Microsoft R

R is the world’s most powerful, and preferred, programming language for statistical computing, machine learning, and graphics, and is supported by a thriving global community of users, developers, and contributors. Developers frequently provide tools incorporating their expertise in the form of R packages. Traditionally, using R in an enterprise setting has presented certain challenges, especially as the volume of data rises, or when faced with a need to deploy solutions to production environments. 

The Microsoft R product family includes:
+ <a href="#mrs">Microsoft R Server</a>
+ <a href="#mrc">Microsoft R Client</a>
+ <a href="#mro">Microsoft R Open</a>
+ <a href="#sqlr">SQL Server R Services</a>

To learn about the difference between these Microsoft R products, see the [comparison here](#compare-prods).

<br>
<a name="mrs"></a>
##Microsoft R Server

Microsoft R Server is the most broadly deployable enterprise-class analytics platform for R available today. Supporting a variety of big data statistics, predictive modeling and machine learning capabilities, R Server supports the full range of analytics – exploration, analysis, visualization and modeling based on open source R. 

By using and extending open source R, Microsoft R Server is fully compatible with R scripts, functions and CRAN packages, to analyze data at enterprise scale. We also address the in-memory limitations of open source R by adding parallel and chunked processing of data in Microsoft R Server, enabling users to run analytics on data much bigger than what fits in main memory. And since R Server is built on top of Microsoft R Open, you can use any open source R packages to build your analytics. 

Microsoft R Server delivers enterprise class performance and scalability for your R-based applications with libraries that allow you to write once and deploy across multiple platforms with minimal effort, whether on-premises or in the cloud.

Get started by reading this [Getting Started](microsoft-r-getting-started.md) guide.

|Microsoft R Server Editions|Description                                                          |Install|ScaleR Get Started|
|---------------------------|---------------------------------------------------------------------|:-------:|:------------------:|
|R Server for Hadoop        |Scale your analysis transparently by distributing work across nodes without complex programming|[Doc](rserver-install-hadoop.md)|[Doc](scaler-hadoop-getting-started.md)|
|R Server for Teradata DB   |Run advanced analytics in-database for seamless data analysis|[Doc](rserver-install-teradata-server.md)|[Doc](scaler-teradata-getting-started.md)|
|R Server for Linux         |Bring predictive and prescriptive analytics power to your Linux environments|[Doc](rserver-install-linux-server.md)|[Doc](scaler-getting-started.md)|

<br />
For a list of supported operating systems, see [Supported platforms in Microsoft R Server](rserver-install-supported-platforms.md).

<br />
<a name="mrc"></a>
##Microsoft R Client

[!include[MicrosoftRClient](./includes/r-client/r-client-intro.md)]

<br>
<a name="mro"></a>
##Microsoft R Open

[!include[MicrosoftROpen](./includes/r-open/mro-intro.md)]


<br>
<a name="sqlr"></a>
##SQL Server R Services

[!include[SQLServerRServices](./includes/ss-r-services/r-services-intro.md)]

<br>
<a name="compare-prods"></a>
##The Microsoft R: A Product Comparison

The feature set provided by Microsoft R Server, Microsoft R Client, and Microsoft R Open can be categorized as shown in this table. For information on SQL Server R Services, see the [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx).

|Features   |Microsoft R Open|Microsoft R Client|Microsoft R Server|
|-----------|----------------|------------------|-----------|
|Big Data   |In-memory bound<br>Can only process datasets that fit into the available memory|In-memory bound<br>Can process datasets that fit into the available memory<br>Operates on large volumes when connected to R Server|Disk scalability<br>Operates on bigger volumes & factors|  
|Speed of<br>Analysis    |Multi-threaded when MKL is installed for non-ScaleR functions|Multi-threaded with MKL for non-ScaleR functions<br>Up to 2 threads for ScaleR functions with a local compute context|Full parallel threading & processing|
|Enterprise<br>Readiness   |Community support|Community support|Commercial support|
|Analytic<br>Breadth <br>& Depth     |8000+ open source packages|Leverage & optimize open source R packages plus 'Big Data'-ready ScaleR packages|Leverage & optimize open source R packages plus 'Big Data'-ready + Multithreaded ready ScaleR packages|
|Commercial<br>Viability   |Risk of deployment to open source|Free for everyone|Commercial licenses|
|[DeployR <br>Enterprise](microsoft-r-getting-started.md#deployr-intro)  |Not available|Not available|Included|


<br>
##Learn More

Start learning about Microsoft R today:
+ [Microsoft R Getting Started](microsoft-r-getting-started.md)

+ [Supported platforms in Microsoft R Server](rserver-install-supported-platforms.md)

+ [DeployR Getting Started](deployr-about.md)

+ [Microsoft R Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)

+ [Microsoft R Product Web Page](https://www.microsoft.com/en-us/server-cloud/products/r-server/) 

+ [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) 

+ [Additional Resources](microsoft-r-more-resources.md)

+ [Documentation Archives](microsoft-r-old-versions.md)
