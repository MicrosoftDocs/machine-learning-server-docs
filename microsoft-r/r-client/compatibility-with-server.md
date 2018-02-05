---

# required metadata
title: "Microsoft R Client compatibility with Machine Learning Server and R Server "
description: "Table of Microsoft R Client compatibility with various offerings of Machine Learning Server and Microsoft R Server."
keywords: "R Client compatibility, Microsoft R Client"
author: "j-martens"
ms.author: "jmartens"
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
# R Client compatibility with Machine Learning Server or R Server

Microsoft R Client is backwards compatible with the following versions of Machine Learning Server and Microsoft R Server:

|Machine Learning Server or R Server Offerings|Compatible with <br/>R Client 3.4.3|
|-----------|:--------------------------:|
|SQL Server 2017 Machine Learning Services<br>&nbsp;&nbsp;&nbsp;& SQL Server 2016 R Services (in-database & standalone)|
|Machine Learning Server <br/>&nbsp;&nbsp;&nbsp;& R Server for **Linux**|8.0.5 - 9.3|
|Machine Learning Server <br/>&nbsp;&nbsp;&nbsp;& R Server for **Hadoop**|8.0.5 - 9.3|
|Machine Learning Server <br/>&nbsp;&nbsp;&nbsp;& R Server for **Windows**|9.1 - 9.3|
|HDInsight: cluster type = Machine Learning or R Server|9.2 - 9.3|

Although R Client is backwards compaible, R Client and Machine Learning Server follow the same release cycle and it is a best practice to use packages at the same functional level. The following versions of R Client correspond to Machine Learning Server versions:

|R Client releases | Server releases| Release date |
|------------------|----------------|--------------|
| 3.4.3 | 9.3 | February 2018 |
| 3.4.1 | 9.2.1 | September 2017 |
| 3.4.0 | 9.1 | March 2017 |
| 3.3.2 | 9.0 | December 2016
| 3.3.3 | 8.0.5 | June 2016 |

Learn about what's new in the latest [Microsoft R Client release](what-is-microsoft-r-client.md#r-client-whats-new).