---

# required metadata
title: "Security Authentication"
description: "Operationalization of R Analytics with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/06/2016"
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
  - deployr
  - r-server
ms.custom: ""
---

# Enterprise-Grade Security for DeployR

R Server has many features that support creating secure applications. Common security considerations, such as data theft or vandalism, apply regardless of the version of R Server you are using. Data integrity should also be considered as a security issue. If data is not protected, it is possible that it could become worthless if ad hoc data manipulation is permitted and the data is inadvertently or maliciously modified. In addition, there are often legal requirements that must be adhered to, such as the correct storage of confidential information. 

When you enable DeployR, you can configure it to take advantage of enterprise-ready security features such as:
+ [Authenticating](security-authentication.md) with Active Directory LDAP/LDAP-Security or with Azure Active Directory
+ [Using HTTPS](security-https.md) to secure communication
+ [Cross-Origin Resource Sharing](security-cors.md) to allow restricted resources on a web page to be requested from another domain outside the originating domain.

>[!IMPORTANT] 
>For security reasons, we strongly recommend that HTTPS be enabled in **all production environments.**  Since we cannot ship certificates for you, HTTPS protocols are disabled by default.

