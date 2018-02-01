---

# required metadata
title: "Set or update the local administrator password - Machine Learning Server "
description: "When no other form of authentication is used, you must define a password for the local administrator account."
keywords: ""
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
ms.suite: "machine-learning"
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# Set or update the local administrator password

**Applies to:  Machine Learning Server, Microsoft R Server**

When no other form of [authentication](configure-authentication.md) is used for Machine Learning Server, you must define a password for the local administrator account called 'admin'. If you enable another form of authentication, the local administrator account is automatically disabled.

The admin password must be 8-16 characters long and contain at least one uppercase character(s), 1+ lowercase character(s), 1+ number(s), and 1+ special character(s).


## Machine Learning Server 9.3

To set or update the password:

@Heidi

## In earlier versions

To set or update the password:

1. [Launch the administration utility.](configure-admin-cli-launch.md)

1. From the main menu, choose the option to set a password for the local 'admin' account.

1. If a password was already defined, provide the current password.

1. When prompted, enter the new password for this administrator account.
   If your configuration has multiple web nodes, we recommend the same password be used.

1. Confirm the password.

>[!Note]
>You can bypass script interface using the argument '-setpassword '. Learn about all command-line switches for this script, here. For example:<br/>`dotnet Microsoft.MLServer.Utils.AdminUtil\Microsoft.MLServer.Utils.AdminUtil.dll -setpassword my-password`

