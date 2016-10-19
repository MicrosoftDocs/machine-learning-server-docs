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
+ [Set a password for the local administrator account](#admin-password)
+ [Start and stop the front-ends and back-ends](#startstop)
+ [Change the port numbers used by the system](#ports)
+ [Run diagnostic tests of the system](#test)
+ [Evaluate the capacity of the configuration](#capacity)

## OPEN QUESTIONS
+ IS THE UTILITY INSTALLED ON FRONT END AND ALSO THE BACKEND?
+ DO WE KNOW THE DIRECTORY PATH FOR ALL VERSIONS ON WINDOWS? 
+ DO YOU HAVE TO LOG IN WHEN THE UTILITY IS LAUNCHED?
+ ARE ALL USERS ALLOWED TO LAUNCH THE UTILITY?
+ ARE WE IMPLEMENTING AN OPTION FOR ENCRYPTING SECRETS FOR A REMOTE DB OR LDAP

<br>
<a name="launch"></a>

## Launch the Utility

These instructions describe how to launch the DeployR Administrator Utility.

**On Windows**

Launch the utility script with administrator privileges:
```
@@@@@@@@@@@@@
cd C:\Program Files\Microsoft\DeployR-8.0.5\deployr\tools\ 
adminUtilities.bat
```  
The main utility menu appears.     

**On Linux**

Launch the utility script with administrator privileges as `root` or a user with `sudo` permissions:
```
@@@@@@@@@@@@@
cd $DEPLOYR_HOME/deployr/tools/ 
./adminUtilities.sh
```     
The main utility menu appears.     

<br><a name="admin-password"></a>

## Set/Update Administrator Password

When no other form of [authentication](security-authentication.md) is used, you must define a password for the local DeployR administrator account called `administrator`.  If you do enable another form of authentication, the local administrator account is automatically disabled.

>The password for the local administrator account must be 8-16 characters long and contain at least 1 or more uppercase character(s), 1 or more lowercase character(s), 1 or more number(s), and 1 or more special character(s).

**To set or update the local administrator account password:**

1. [Launch the DeployR Administrator Utility](#launch) script with administrator privileges.

1. From the main menu, choose the option to set a password for the local administrator account.

1. If a password was already defined, provide the current password.

1. When prompted, enter the new password for this administrator account. 
   >If your configuration has multiple front-ends, we recommend the same password be used.

1. Confirm the password.

<br><a name="startstop"></a>

## Starting and Stopping DeployR

To start or stop all DeployR-related services on the machine at once, use the administrator utility. 

**To stop or start a front-end or back-end:**

1. [Launch the DeployR Administrator Utility](#launch) script with administrator privileges.

1. From the main menu, choose the option to stop and start the services.

1. When prompted, identify which processes you want to stop, start, or restart.  @@@  

<br><a name="ports"></a>

## Update Port Numbers

@@@@@@@@@

<br><a name="test"></a>

## Diagnostic Testing

You can assess the state and health of your DeployR environment by running the diagnostic test in the Administrator Utility. 
Armed with this information, you will be able to investigate and resolve most issues. 

[Learn more about Diagnostic testing here.](admin-diagnostics-troubleshooting.md)

<br><a name="capacity"></a>

## Evaluate Capacity

@@@@@@@@@