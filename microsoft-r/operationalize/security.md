---

# required metadata
title: "Enterprise-Grade Security for Operationalization | Microsoft R Server Docs"
description: "Enterprise-Grade Security for Operationalization with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/08/2016"
ms.topic: "article"
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

# Enterprise-Grade Security for Operationalization

**Applies to:  Microsoft R Server 9.0.1**

R Server has many features that support the creation of secure applications. Common security considerations, such as data theft or vandalism, apply regardless of the version of R Server you are using. Data integrity should also be considered as a security issue. If data is not protected, it is possible that it could become worthless if ad hoc data manipulation is permitted and the data is inadvertently or maliciously modified. In addition, there are often legal requirements that must be adhered to, such as the correct storage of confidential information. 

You can add greater enterprise security around Microsoft R Server's operationalization feature such as:
+ Seamless integrations with Active Directory LDAP/LDAP-Security or Azure Active Directory [authentication](security-authentication.md).

+ Secured connections using [SSL/TLS 1.2](security-https.md). For security reasons, we strongly recommend that SSL/TLS 1.2 be enabled in **all production environments.** 

+ [Cross-Origin Resource Sharing](security-cors.md) to allow restricted resources on a web page to be requested from another domain outside the originating domain.

Additionally, we recommend that you review the following [Security Considerations](security-rserve.md).


![Security](../media/o16n/security.png)