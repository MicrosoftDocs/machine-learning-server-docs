---

# required metadata
title: "Set or update the local administrator password - Machine Learning Server "
description: "When no other form of authentication is used, you must define a password for the local administrator account."
keywords: 
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

# Set or update the local administrator password

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

**Applies to:  Machine Learning Server, Microsoft R Server**

When no other form of [authentication](configure-authentication.md) is used for Machine Learning Server, you must define a password for the local administrator account called 'admin'. If you enable another form of authentication, the local administrator account is automatically disabled.

The admin password must be 8-16 characters long and contain:
+ At least one uppercase character
+ At least one lowercase character
+ At least one number
+ At least one of the following special characters:<br/> `~ ! @ # $ % ^ & ( ) - _ + = | < > \ / ; : , .`

## Machine Learning Server 9.3 and later

In Machine Learning Server 9.3 and later, you can use `admin` extension of the Azure Command Line Interface ([Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)) to set up and manage your configuration, including updating the local 'admin' account password.

>[!Important]
>- This password is set while you are first configuring your nodes.
>- You do not need an Azure subscription to use this CLI. It is installed as part of Machine Learning Server and runs locally.  

To update the password:

1. On the machine hosting the node, launch a command-line window or terminal  with administrator (Windows) or root/sudo (Linux) privileges.

1. Run the command to update the 'admin' password.
   ```azurecli
   # With elevated privileges, run the following commands.
   # Update the admin account password
   az ml admin password set --old-password <OLD-PW> --admin-password <NEW-PW> --confirm-password <NEW-PW>
   ```

## Earlier versions: 9.0 - 9.2

To set or update the password:

1. [Launch the administration utility.](configure-admin-cli-launch.md)

1. From the main menu, choose the option to set a password for the local 'admin' account.

1. If a password was already defined, provide the current password.

1. When prompted, enter the new password for this administrator account.
   If your configuration has multiple web nodes, we recommend the same password be used.

1. Confirm the password.

>[!Note]
>You can bypass script interface using the argument '-setpassword '. Learn about all command-line switches for this script, here. For example:<br/>`dotnet Microsoft.MLServer.Utils.AdminUtil\Microsoft.MLServer.Utils.AdminUtil.dll -setpassword my-password`

