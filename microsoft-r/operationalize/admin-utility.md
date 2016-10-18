---

# required metadata
title: "Administration utility"
description: "Administration utility for DeployR"
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

# DeployR Administrator Utility

An administrator utility is installed with each front-end you configure for DeployR. You can do the following with this utility:
+ Set a password for the local administrator account, `administrator`
+ Start and stop the front-ends and back-ends
+ Run diagnostic tests of the system
+ Change the port numbers used by the system

## OPEN QUESTIONS


+ IS THE UTILITY INSTALLED ON FRONT END AND BACKEND?
+ DO WE KNOW THE DIRECTORY PATH FOR ALL VERSIONS ON WINDOWS? 

<a name="launch"></a>

## Launch the Utility

These instructions describe how to launch the DeployR Administrator Utility.

**On Windows**

Launch the utility script with administrator privileges:
```
cd C:\Program Files\Microsoft\DeployR-8.0.5\deployr\tools\ 
adminUtilities.bat
```  
The main utility menu appears.     

**On Linux**

Launch the utility script with administrator privileges as `root` or a user with `sudo` permissions:
```
cd $DEPLOYR_HOME/deployr/tools/ 
./adminUtilities.sh
```     
The main utility menu appears.     

<a name="admin-password"></a>

## Set Administrator Password

>If you enable Active Directory LDAP or Azure Active Directory authentication, you will no longer be able to log in with this password.

1. [Launch the DeployR Administrator Utility](#launch) script with administrator privileges.

1. From the main menu, choose the option to set a password for the local administrator account.

1. When prompted, enter a password for this account. 

   >Passwords must be 8-16 characters long and contain at least 1 or more uppercase character(s), 1 or more lowercase character(s), 1 or more number(s), and 1 or more special character(s).

1. Confirm the password.

<a name="startstop"></a>

## Starting and Stopping DeployR

### Start and Stop the Front-end

To start or stop all DeployR-related services on the front-end at once, use the administrator utility. 

1. [Launch the DeployR Administrator Utility](#launch) script with administrator privileges.

1. From the main menu, choose the option to **Manage Front-End**.

1. When prompted whether you want to stop (S) or restart (R) the DeployR server, enter your choice`R`. It may take some time for the Tomcat process to terminate and restart.

### Start and Stop the Back-end

To start or stop all DeployR-related services on the back-end at once, use the administrator utility. 

1. [Launch the DeployR Administrator Utility](#launch) script with administrator privileges.

1. From the main menu, choose the option to **Manage Back-End**.

1. When prompted whether you want to stop (S) or restart (R) the DeployR server, enter your choice`R`. It may take some time for the Tomcat process to terminate and restart.


<a name="test"></a>
## Diagnostic Testing

You can assess the state and health of your DeployR environment by running the diagnostic test in the Administrator Utility. 
Armed with this information, you will be able to investigate and resolve most issues. 

[Learn more about Diagnostic testing here.](admin-diagnostics-troubleshooting.md)


<a name="ports"></a>
## Updating Port Numbers

@@@@@@@@@