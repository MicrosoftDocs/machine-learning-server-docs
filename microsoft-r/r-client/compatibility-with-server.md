---

# required metadata
title: "Microsoft R Client compatibility with Machine Learning Server and R Server "
description: "Table of Microsoft R Client compatibility with various offerings of Machine Learning Server and Microsoft R Server."
keywords: R Client compatibility, Microsoft R Client
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 2/16/2018
ms.topic: "how-to"
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

# R Client compatibility with Machine Learning Server or R Server

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

Microsoft R Client is backwards compatible with the following versions of Machine Learning Server and Microsoft R Server:

|Machine Learning Server or R Server Offerings|Compatible with <br/>R Client 3.4.3|
|-----------|:--------------------------:|
|SQL Server 2017 Machine Learning Services<br>&nbsp;&nbsp;&nbsp;& SQL Server 2016 R Services (in-database & standalone)|
|Machine Learning Server <br/>&nbsp;&nbsp;&nbsp;& R Server for **Linux**|8.0.5 - 9.3|
|Machine Learning Server <br/>&nbsp;&nbsp;&nbsp;& R Server for **Hadoop**|8.0.5 - 9.3|
|Machine Learning Server <br/>&nbsp;&nbsp;&nbsp;& R Server for **Windows**|9.1 - 9.3|
|HDInsight: cluster type = Machine Learning or R Server|9.2 - 9.3|

Although R Client is backwards compatible, R Client and Machine Learning Server follow the same release cycle and it is a best practice to use packages at the same functional level. The following versions of R Client correspond to Machine Learning Server versions:

|R Client releases | Server releases| Release date | Downloads|
|------------------|----------------|--------------|--------------------|
| 3.5.2 | 9.4 | July 2019 | [Linux](https://aka.ms/rclientlinux)<br/> [Windows](https://aka.ms/rclient) |
| 3.4.3 | 9.3 | February 2018 | [Linux](https://aka.ms/rclientlinux)<br/> [Windows](https://download.microsoft.com/download/4/9/0/4901FEC3-70F8-4BAE-868B-FD1A9845937C/RClientSetup3.4.3.exe) |
| 3.4.1 | 9.2.1 | September 2017 | [Linux](https://aka.ms/rclientlinux)<br/>[Windows](https://download.microsoft.com/download/A/E/E/AEE0FDC0-11CB-49C7-BCAF-A942A2830F3B/RClientSetup3.4.1.exe)|
| 3.3.3 (Windows only) | 9.1 | March 2017 | [Windows](https://download.microsoft.com/download/F/B/0/FB04AB56-F035-4FF8-A2DB-3602DE423301/RClientSetup3.3.3.exe) | 
| 3.3.2 (Windows only) | 9.0 | December 2016 | [Windows](https://download.microsoft.com/download/D/1/0/D10D147D-AAF4-4927-A59D-FD85B3B65310/RClientSetup.exe)|
| 3.2.2 (Windows only) | 8.0.5 | June 2016 | [Windows](https://download.microsoft.com/download/0/6/4/064DA0F7-4BFA-4271-928F-7859A5990DB7/RClientSetup.exe)|

Learn about what's new in the latest [Microsoft R Client release](what-is-microsoft-r-client.md#r-client-whats-new).