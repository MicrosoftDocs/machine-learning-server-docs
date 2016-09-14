---

# required metadata
title: "Install Microsoft R Client on Windows"
description: "Windows installation of Microsoft R Client."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/17/2016"
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
ms.technology: "r-client"
ms.custom: ""

---

#Install Microsoft R Client

Microsoft R Client is a free, data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the powerful ScaleR technology and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. [Learn more about its compatibility.](r-client-compatibility.md)

###Installing R Client

[Download Microsoft R Client](http://aka.ms/rclient/download), available through Visual Studio Dev Essentials, and install it today. 

[!include[Install](./includes/r-client/r-client-install.md)]
<br>

###What's Next

Once you've installed R Client, the next step is to [configure your favorite R integrated development environment (IDE)](r-client-get-started.md#configure-ide) to point to the R Client R executable. 

<br>

###Learn More

You can learn more with these guides:

+ [Get Started with Microsoft R Client](r-client-get-started.md) 

+ [Microsoft R Getting Started](microsoft-r-getting-started.md) 

+ [RevoScaleR Getting Started](scaler-getting-started.md)