---

# required metadata
title: " Security in DeployR"
description: "Security in DeployR: Authentication, HTTPS, SSL, and access controls for server, Project file and Repository File, and more."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/06/2016"
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
ms.technology: "deployr"
ms.custom: ""

---

# Password Policies

>[!Important]
>This topic applies to DeployR 8.0.0 and earlier. It does not apply to DeployR for Microsoft R Server 2016 

To customize password constraints on user account passwords adjust the password policy configuration properties.

The `deployr.security.password.min.characters` property enforces a minimum length for any basic authentication password. The `deployr.security.password.upper.case` property, when enabled, enforces the requirement for user passwords to contain at least a single uppercase character. The `deployr.security.password.alphanumeric`property, when enabled, enforces the requirement for user passwords to contain both alphabetic and numeric characters.

```
/*
* DeployR Password Policy Configuration
*
* Note: enable password.upper.case if basic-auth users
* are required to have at least one upper case character
* in their password.
*/
deployr.security.password.min.characters = 8
deployr.security.password.upper.case = true
deployr.security.password.alphanumeric = true
```
    
These policies affect basic authentication only and have no impact on CA Single Sign On, LDAP/AD or PAM authentication where passwords are maintained and managed outside of the DeployR database.