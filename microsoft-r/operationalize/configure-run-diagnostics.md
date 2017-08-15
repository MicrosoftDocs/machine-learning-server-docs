---

# required metadata
title: "Running diagnostics & troubleshooting the configuration for operationalization - Machine Learning Server || Microsoft Docs"
description: "Troubleshooting and Diagnostics when configuring Microsoft R Server to operationalize"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "6/21/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""
---

# Troubleshooting & diagnostics for R Server

**Applies to:  Microsoft R Server 9.x**

You can assess the health of your web and compute node environment using the diagnostic tests in the Administration Utility. 
Armed with this information, you can identify unresponsive components, execution problems, and access the log files. 

The set of diagnostic tests include:
+ A general health check of the configuration
+ A raw report of the system status
+ A trace of an R code execution
+ A trace of a web service execution

<a name="test"></a>

## Running Diagnostic Tests

**To run diagnostic tests:**

1. [Launch the administration utility](configure-use-admin-utility.md#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

1. From the main menu, choose **Run Diagnostic Tests**.

1. If you have not authenticated yet, you must provide your username and password. 

1. Choose **Test configuration** for a 'health report' of the configuration including a code execution test:

   1. Review the test results. If any issues arise, investigate the [log files](#logs) and attempt to resolve the issues.

   1. After making your corrections, [restart the component](configure-use-admin-utility.md#startstop) in question. It may take a few minutes for a component to restart.

   1. Rerun the diagnostic test to make sure all is running smoothly now.

   >[!NOTE]
   >You can also get a health report directly using [the `status` API call](https://microsoft.github.io/deployr-api-docs/#get-status).

1. For the 'raw details' on the health of the system and review the output, choose **Get raw server status**.

1. To trace the execution of specific R code and retrieve request IDs for debugging purposes, choose **Trace code execution**:
      1. Enter the R code you want to run and trace. 
      1. To start the trace, press the Enter key (carriage return).
      1. Review the trace output.

1. To trace the execution of specific service and retrieve request IDs for debugging purposes, choose **Trace service execution**:  
      1. Enter the service name and version after the syntax `<service-name>/<version>` such as `my-service/1.1`. 
      1. To start the trace, press the Enter key (carriage return).
      1. Review the trace output to better understand how the execution is running or failing.

<a name="logs"></a>

## Log files

Review the log and configuration files for any component that was identified as experiencing issues. The [logging level](#loglevel) can be changed to capture more or less information.

### Windows logs path

The logs can be found as follows where `<MRS_home>` is the path to the Microsoft R Server installation directory. Type `normalizePath(R.home())` in your R console to find the path to `<MRS_home>`.

|Node|Path on version 9.1|
|----|------------|
|Web|<MRS_home>\o16n\Microsoft.RServer.WebNode\logs|
|Compute|<MRS_home>\o16n\Microsoft.RServer.ComputeNode\logs|

<br>

|Node|Path on version 9.0|
|----|------------|
|Web|<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\logs|
|Compute|<MRS_home>\deployr\Microsoft.DeployR.Server.BackEnd\logs|


### Linux logs path

The logs can be found here: 


|Node|Path on version 9.1|
|----|------------|
|Web|/usr/lib64/microsoft-r/rserver/o16n/9.1.0/Microsoft.RServer.WebNode/logs|
|Compute|/usr/lib64/microsoft-r/rserver/o16n/9.1.0/Microsoft.RServer.ComputeNode/logs|

<br>

|Node|Path on version 9.0|
|----|------------|
|Web|/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/logs|
|Compute|/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.BackEnd/logs|


> If there are any issues, you must solve them before continuing. For extra help, consult or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a> or contact technical support.

<a name="loglevel"></a>

## Logging Levels

By default, the logging level is set to `Warning` so as not to slow performance. However, whenever you encounter an issue that you want to share with technical support or a forum, you can change the logging level to capture more information. 

The following logging levels are available:
+ `Verbose`: The most detailed comprehensive logging level of all activity, which is rarely (if ever) enabled in production environments

+ `Debug`: Logs robust details including internal system events, which are not necessarily observable

+ `Information`: Logs system events that correspond to its responsibilities and functions

+ `Warning`: Logs only when service is degraded, endangered, or may be behaving outside of its expected parameters.  (Default level)

+ `Error`: Logs only errors (functionality is unavailable or expectations broken)

+ `Fatal`: Logs only fatal events that crash the application

**To update the logging level:**

   1. On each compute node AND each web node, [open the appsettings.json configuration file](configure-find-admin-configuration-file.md).

   1. Search for the section starting with `"Logging": {`

   1. Set the logging level for `"Default"`, which captures R Server default events. For debugging support, use the `Debug` level.

   1. Set the logging level for `"System"`, which captures R Server .NET core events. For debugging support, use the `Debug` level. Use the same value as for `"Default"`.

   1. Save the file.

   1. [Restart](configure-use-admin-utility.md#startstop) the node services. 

   1. Repeat these changes on every compute node and every web node.
      >Each node should have the same appsettings.json properties.

   1. Repeat the same operation(s) that were running when the error(s) occurred. 
   
   1. Collect the [log files](#logs) from each node for debugging.



## Troubleshooting

This section contains pointers to help you troubleshoot some problems that can occur.

>[!IMPORTANT]
>If this section do not help solve your issue, file a ticket with technical support or post in our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

### "BackEndConfiguration is missing URI" Error

If you get an `BackEndConfiguration is missing URIs` error when trying to install a web node, then ensure your compute nodes are installed and [declared](../install/operationalize-r-server-enterprise-config.md#webnode) prior to installing web nodes. 

```
Unhandled Exception: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> Microsoft.DeployR.Server.App.Common.Exceptions.ConfigurationException: BackEndConfiguration is missing URIs
   at Microsoft.DeployR.Server.WebAPI.Extensions.ServicesExtensions.AddDomainServices(IServiceCollection serviceCollection, IHostingEnvironment env, IConfigurationRoot configurationRoot, String LogPath)
   --- End of inner exception stack trace ---
```

### “Cannot establish connection with the web node” Error

If you get the `Cannot establish connection with the web node` error, then the client is unable to establish a connection with the web node in order to log in. Perform the following steps:
+ Verify that the web address and port number displayed on the main menu of the admin utility are correct. Learn how to launch the utility, in this article: [R Server Administration](configure-use-admin-utility.md#launch)
+ Look for web node startup errors or notifications in the stdout/stderr/[logs files](#logs). 
+ Restart the web node if you have recently changed the port the server is bound to or the certificate used for HTTPS. Learn how to restart, in this article: [R Server Operationalization Administration](configure-use-admin-utility.md#startstop)

If the issue persists, verify you can post to the `login` API using curl, fiddler, or something similar. Then, share this information with technical support or post it in our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

### Long delays when consuming web service on Spark

If you encounter long delays when trying to consume a web service created with mrsdeploy functions in a Spark compute context, you may need to add some missing folders. The Spark application belongs to a user called 'rserve2' whenever it is invoked from a web service using mrsdeploy functions. 

To work around this issue, create these required folders for user 'rserve2' in local and hdfs:

```
hadoop fs -mkdir /user/RevoShare/rserve2
hadoop fs -chmod 777 /user/RevoShare/rserve2
 
mkdir /var/RevoShare/rserve2
chmod 777 /var/RevoShare/rserve2
``` 

Next, create a new Spark compute context:

```R 
rxSparkConnect(reset = TRUE)
```

When 'reset = TRUE', all cached Spark Data Frames are freed and all existing Spark applications belonging to the current user are shut down.
  

### Compute Node Failed / HTTP status 503 on APIs (Linux Only)

If you get an `HTTP status 503 (Service Unavailable)` response when using the Rest APIs or encounter a failure for the compute node during diagnostic testing, then one or more of the symlinks needed by [deployr-rserve](https://github.com/Microsoft/deployr-rserve) are missing. deployr-rserve is the R execution component for the compute node,

   1. Launch a command window with administrator privileges with `root`/ `sudo` privileges.

   1. Run a [diagnostic test](#test) of the system on the machine hosting the compute node.

   1. If the test reveals that the compute node test has failed, type `pgrep -u rserve2` at a command prompt. 

   1. If no process ID is returned, then the R execution component is not running and we need to check which symlinks are missing.

   1. At the command prompt, type `tail -f /opt/deployr/9.0.1/rserve/R/log`. The symlinks are revealed. 

   1. Compare these symlinks to the symlinks listed in the [configuration](../install/operationalize-r-server-one-box-config.md) article.

   1. Add a few symlinks using the commands in the [configuration](../install/operationalize-r-server-one-box-config.md) article.

   1. [Restart](configure-use-admin-utility.md#startstop) the compute node services.

   1. Run the [diagnostic test](#test) or try the APIs again.

### Unauthorized / HTTP status 401

If you have [configured R Server to authenticate](configure-authentication.md) against LDAP/AD, but have encountered connection issues or a `401` error, then try the ldp.exe tool to search the LDAP settings and compare them to what you have declared in appsettings.json. You can also consult with any Active Directory experts in your organization to identify the correct parameters.

### Configuration did not restore after upgrade

If you followed the upgrade instructions but your configuration did not persist, then put the backup of the appsettings.json file under the following directories and reinstall R Server 9.1 again:
   + On Windows: `C:\Users\Default\AppData\Local\DeployR\current`

   + On Linux: `/etc/deployr/current`


### Alphanumeric error message when consuming service

If you get an alphanumeric error message similar to `Message: b55088c4-e563-459a-8c41-dd2c625e891d` when consuming a web service, use that string to find the full error message text in the [compute node's log file](#logs). 


### Failed code execution with “ServiceBusyException” in the log
 
If you encounter a code execution failure and find a `ServiceBusyException` error in the Web node log file, then a proxy issue may be blocking the execution.

The workaround is to:
1. Open the R initialization file <install folder>\R_SERVER\etc\Rprofile.site.
1. Add the following code as a new line in Rprofile.site:

   ```R
   utils::setInternet2(TRUE)
   ```
1. Save the file and restart Microsoft R Server.
1. Repeat on every machine on which Microsoft R Server is installed.
1. Run the diagnostic test or code execution again.
