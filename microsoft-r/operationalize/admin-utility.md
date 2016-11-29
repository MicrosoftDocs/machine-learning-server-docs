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

# Administrator Utility for Operationalization

[!include[Admin Utility Introduction](../includes/o16n/admin-utility-intro.md)]

<br>
<a name="launch"></a>

## Launch the Utility

These instructions describe how to launch the Administrator Utility.

**To launch the administration utility:**

Launch the administration utility script with administrator, `root`, or `sudo` privileges:
```
cd <MRS_home>\deployr
dotnet Microsoft.DeployR.Utils.AdminUtil\Microsoft.DeployR.Utils.AdminUtil.dll
```

where `<MRS_home>` is the path to the Microsoft R Server installation directory. To find this path, enter `normalizePath(R.home())` in your R console.

<br><a name="admin-password"></a>

## Set/Update Local Administrator Password

When no other form of [authentication](security-authentication.md) is used, you must define a password for the local administrator account called `admin`.  If you do enable another form of authentication, the local administrator account is automatically disabled.

>The password for the local `admin` account must be 8-16 characters long and contain at least 1 or more uppercase character(s), 1 or more lowercase character(s), 1 or more number(s), and 1 or more special character(s).

**To set or update the local admin account password:**

1. [Launch the administration utility](#launch) with administrator, `root`, or `sudo` privileges.

1. From the main menu, choose the option to set a password for the local `admin` account.

1. If a password was already defined, provide the current password.

1. When prompted, enter the new password for this administrator account. 
   >If your configuration has multiple web nodes, we recommend the same password be used.

1. Confirm the password.

<br><a name="startstop"></a>

## Starting and Stopping Services

To start or stop all operationalization-related services on the machine at once, use the administrator utility. 

**To stop or start a web node or compute node:**

1. [Launch the administration utility](#launch) with administrator, `root`, or `sudo` privileges.

1. From the main menu, choose the option **Stop and start services**.

1. From the sub-menu, choose which services to start or stop.

<br><a name="ports"></a>

## Update Port Numbers

You can update the ports numbers for the web node, compute node, or Rserve.

**To update the port values:**

1. Log into the machine on which your web node or compute node is installed.

1. [Launch the administration utility](#launch) with administrator, `root`, or `sudo` privileges.

1. From the main menu, choose the option **Change service ports**.

1. From the sub-menu, choose the port you want to update.

1. Enter the port number. 

>The port number will be updated the next time the [service is restarted](#startstop).

<br><a name="encrypt"></a>

## Encrypt Credentials 

For security purposes, we strongly recommend that you encrypt the connection string for the remote database and/or the password for the LDAP/LDAP-S credentials rather than store them in plain text in the `appsettings.json` configuration file. Here's how: 
       
1. On the web node, install a credential encryption certificate with a private key into the default certificate store on the local machine. 

1. [Launch the administration utility](#launch) with administrator, `root`, or `sudo` privileges.

1. From the main menu, choose the option **Encrypt Credentials**.

1. Specify where is the encryption certificate installed: 
   + Local machine (Computer account)
   + Current user (My user account)

   The list of available certificates appears.

1. Specify the certificate to use for encryption.

1. Enter information you want to encrypt.  The tool will return an encrypted string.

1. Insert that string in the appropriate section of the configuration file, `appsettings.json` for a [remote database](configure-remote-database.md) or the [authentication](security-authentication.md) strings. You can find that file under `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server installation directory. To find this path, enter `normalizePath(R.home())` in your R console.

<br><a name="test"></a>

## Diagnostic Testing

You can assess the state and health of your environment with the set of diagnostic tests found in this Administration Utility. 
Armed with this information, you can identify unresponsive components, execution problems, and access the log files. 

The set of diagnostic tests include:
+ A general health check of the configuration
+ A raw report of the system status
+ A trace of an R code execution
+ A trace of an web service execution


**To run diagnostic tests:**

1. [Launch the administration utility](#launch) with administrator, `root`, or `sudo` privileges.

1. From the main menu, choose **Run Diagnostic Tests**.

1. If you haven't authenticated yet, you'll need to provide your username and password. 

1. To retrieve a 'health check' of the configuration including a code execution test, choose **Test configuration**.

      1. Review the test results.

      1. If any issues arise, attempt to resolve them. If needed, look through the log files to find any errors reported there.
         + On the web node: `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\logs`
         + On the compute node: `<MRS_home>\deployrMicrosoft.DeployR.Server.BackEnd\logs`
         
         where `<MRS_home>` is the path to the Microsoft R Server install directory. To find this path, enter `normalizePath(R.home())` in your R console.

      1. After making your corrections, [restart the component](admin-utility.md#startstop) in question. It may take a few minutes for a component to restart.

      1. Rerun the diagnostic test to make sure all is running smoothly now.

1. To retrieve the 'raw details' on the health of the system and review the output, choose **Get raw server status** .

1. To trace the execution of specific R code and retrieve request IDs for debugging purposes, choose **Trace code execution**:
      1. Enter the R code you want to run and trace. 
      1. Press the Enter key (carriage return) to start the trace.
      1. Review the trace output.

1. To trace the execution of specific service and retrieve request IDs for debugging purposes, choose **Trace service execution** :  
      1. Enter the service name and version following the syntax `<service-name>/<version>` such as `my-service/1.1`. 
      1. Press the Enter key (carriage return) to start the trace.
      1. Review the trace output to better understand how the execution is running or failing.

> If there are any issues, you must solve them before continuing. For extra help, consult or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

<br><a name="capacity"></a>

## Evaluate Capacity

To evaluate the load balancing capacity, you can simulate the traffic for the configuration or for a given web service. There are two ways in which you can test:
+ **Maximum Latency:** Define a maximal threshold for the duration of a web node request as well as define the initial thread count and a thread increments for the test. Then, the test will increase the number of threads by the defined increment until the defined time limit is reached.

+ **Maximum Thread Count:** Define how many threads you want to run it against, such as 1, 10, 20, or 40. Then, see how much time is spent on each processing stage.

During the simulation, you'll be able to see the maximum latency or number of parallel requests that can be supported. The results are divided into request processing stages to enable you to see if any configuration changes are warranted, such as adding more web or compute nodes, increase the pool size, and so on. The stages are:
  1. **Web Node Request**: the time it took the request from the web node's controller to go all the way to RServe and back.

  1. **Create Shell**: the time it took to create a shell or take it from the pool

  1. **Initialize Shell**: the time it took to load the data (model or snapshot) into the shell prior to execution
 
  1. **Web Node to Compute Node**: the time it took for a request from the web node to reach the compute node

  1. **Compute Node Request**: the time it took for a request from the compute node to reach RServe and return to the node
 
After the tool is run, the results are printed to the console. You can also explore the results visually using the URL that is returned to the console. 

<br>

**To run or design a capacity simulation test:**


> What does this mean?@@        Enter a JSON object representing the input parameters.
>
> Where do you run this? @@On the front end??

1. [Launch the administration utility](#launch) with administrator, `root`, or `sudo` privileges.

1. From the main menu, choose the option to **Evaluate Capacity**. The current test parameters appears.

1. To start a capacity simulation, choose the option to **Run capacity simulation** from the sub-menu. Review the results and paste the results URL into your browser for a visual representation of the test results.

1. To change services, choose **Change the service for simulation** from the sub-menu and specify a new service:
      + To specify an existing service:
        1. Enter `Yes`.
        1. Provide the service's name and version as `<name>/<version>`. For example, `my-service/1.1`.
        1. Enter a JSON object representing the input parameters.@@

      + To use the generated [default service], enter `No` and enter a JSON object representing the input parameters.

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
     