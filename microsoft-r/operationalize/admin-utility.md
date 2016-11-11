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

[!include[Admin Utility Introduction](../includes/o16n/admin-utility-intro.md)]

<!--<br>
<a name="launch"></a>

## Launch the Utility

These instructions describe how to launch the DeployR Administrator Utility.

**To launch the administration utility:**

1. If R Server (Standalone) is not yet installed, install it now.

1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges.
  
The main utility menu appears.     -->

<br><a name="admin-password"></a>

## Set/Update Local Administrator Password

When no other form of [authentication](security-authentication.md) is used, you must define a password for the local DeployR administrator account called `administrator`.  If you do enable another form of authentication, the local administrator account is automatically disabled.

>The password for the local administrator account must be 8-16 characters long and contain at least 1 or more uppercase character(s), 1 or more lowercase character(s), 1 or more number(s), and 1 or more special character(s).

**To set or update the local administrator account password:**

1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges.

1. From the main menu, choose the option to set a password for the local administrator account.

1. If a password was already defined, provide the current password.

1. When prompted, enter the new password for this administrator account. 
   >If your configuration has multiple front-ends, we recommend the same password be used.

1. Confirm the password.

<br><a name="startstop"></a>

## Starting and Stopping DeployR

To start or stop all DeployR-related services on the machine at once, use the administrator utility. 

**To stop or start a front-end or back-end:**

1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges.

1. From the main menu, choose the option **Stop and start DeployR services**.

1. From the sub-menu, choose which services to start or stop.

<br><a name="ports"></a>

## Update Port Numbers

You can update the ports numbers for the front-end, back-end, or Rserve.

**To update the port values:**

1. Log into the machine on which your front-end or back-end is installed.

1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges.

1. From the main menu, choose the option **Change service ports**.

1. From the sub-menu, choose the port you want to update.

1. Enter the port number. 

The port number has now been updated and the service restarted.

<br><a name="encrypt"></a>

## Encrypt Secret Credentials

For security purposes, we strongly recommend that you encrypt the login credentials for any remote database and/or LDAP/LDAP-S rather than store them in plain text in the `appsettings.json` configuration file. Here's how: 
       
1. Make sure a credential encryption certificate with a private key is installed on the front-end. 

1. Encrypt the credentials using the DeployR Administrator Utility as follows:
   1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges.

   1. From the main menu, choose the option **Encrypt Credentials**.

   1. When prompted, enter the username and password.  The tool will return an encrypted string.

1. Insert that string in the appropriate section of the configuration file, `appsettings.json` for a [remote database](configure-remote-database.md) or the [authentication](security-authentication.md) strings.

<br><a name="test"></a>

## Diagnostic Testing

You can assess the state and health of your DeployR environment by running the diagnostic test in the Administrator Utility. 
Armed with this information, you will be able to identifies unresponsive components and access the log files. 

**To run diagnostic tests:**

1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges.

1. From the main menu, choose **Run Diagnostic Tests**.

1. If you haven't authenticated yet, you'll need to provide your username and password. 
   > @@ ASK USER TO AUTHENTICATE HERE INSTEAD OF FROM THE SUB MENU CHOICE

1. From the sub-menu, choose which tests you want to run.
    + Choose **Test configuration** to retrieve a 'health check' of the configuration including a code execution test.

      1. Review the test results.

      1. If any issues arise, attempt to resolve them. If needed, look through the log files to find any errors reported there.
         + On the front-end: `$MRS_DEPLOYR_HOME\deployr\Microsoft.DeployR.Server.WebAPI\logs`
         + On the back-end: `$MRS_DEPLOYR_HOME\deployr\Microsoft.DeployR.Server.BackEnd\logs`

      1. After making your corrections, [restart the component](admin-utility.md#startstop) in question. It may take a few minutes for a component to restart.

      1. Rerun the diagnostic test to make sure all is running smoothly now.

    + Choose **Get raw server status** to retrieve a 'raw details' on the health of the system and review the output.

    + Choose **Trace code execution** to get the complete log tracing the execution of user-defined R code.
      1. Enter the R code you want to run and trace. 
      1. Press the Enter key (carriage return) to run the code and start the trace.
      1. Review the trace output.

    + Choose **Trace service execution** to get the complete log tracing the response to a call to a user-specified service. 
      1. Enter the service name and version following the syntax `<service-name>/v<version>` such as `my-service/v1.0.0`. 
      1. Press the Enter key (carriage return) to run the code and start the trace.
      1. Review the trace output.

> If there are any issues, you must solve them before continuing. For extra help, consult or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

<br><a name="capacity"></a>

## Evaluate Capacity

To evaluate the load balancing capacity of the DeployR configuration, you can simulate the thresholds of your configuration using this tool. 

**To run or design a capacity simulation test:**

1. Launch the utility script, `$MRS_DEPLOYR_HOME\deployr\runAdminUtils.ps1`, with administrator privileges.

1. From the main menu, choose the option to **Evaluate Capacity**. The current test parameters appears.

1. To start a capacity simulation, choose the option to **Run capacity simulation** from the sub-menu.

1. To change services, choose **Change the service for simulation** from the sub-menu and specify a new service:
      + To specify an existing service:
        1. Enter `Yes`.
        1. Provide the service's name and version as `<name>/<version>`. For example, `my-service/v1.0.0`.
      + To use the generated [default service], enter `No`.

1. To change threshold rules, choose **Change thread/latency limits** from the sub-menu and one of the following:

      + To stop after the maximum thread count is reached:

         1. Enter `Threads`.
         1. Specify the maximum thread count after which the test will stop running.
         1. Specify the minimum thread count at which the test will start.
         1. Specify the increment by which the number of threads will increase for each iteration.

      + To determine how many concurrent threads can be run before maximum latency is met:

         1. Enter `Time`.
         1. Specify the maximum latency in milliseconds after which the test will stop.
         1. Specify the minimum thread count at which the test will start.
         1. Specify the increment by which the number of threads will increase for each iteration until the maximum latency is reached.
     

>@@WHAT CAN YOU DO WITH THE OUTPUT OF THE SIMULATION?