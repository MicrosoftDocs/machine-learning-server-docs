---

# required metadata
title: "Running diagnostics & troubleshooting the configuration for operationalization - Machine Learning Server "
description: "Troubleshooting and Diagnostics when configuring Machine Learning Server and Microsoft R Server to operationalize"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "11/10/2017"
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

# Troubleshooting & diagnostics for Machine Learning Server

**Applies to:  Machine Learning Server, Microsoft R Server 9.1**

You can assess the health of your web and compute node environment using the diagnostic tests in the Administration Utility. This utility is installed by default with Machine Learning Server (and R Server).

Armed with this information, you can identify unresponsive components, execution problems, and access [the log files](#logs). 

The set of diagnostic tests include:
+ A general health check of the configuration
+ A trace of an execution (R code, Python code, or a web service)

Additional [troubleshooting topics](#trouble) are also covered.

<a name="test"></a>

## Test your configuration
1. [Launch the administration utility](configure-use-admin-utility.md#launch) with administrator privileges (Windows) or root/sudo privileges (Linux).

1. From the main menu, choose **Run Diagnostic Tests**.

   If you have not authenticated yet, you must provide your username and password. 

1. From the diagnostic menu, choose **Test configuration** for a 'health report' of the configuration including a code execution test.

1. Review the test results. If any issues arise, a raw report appears. You can also investigate the [log files](#logs) and attempt to resolve the issues.

1. After making your corrections, [restart the component](configure-use-admin-utility.md#startstop) in question. It may take a few minutes for a component to restart.

1. Rerun the diagnostic test to make sure all is running smoothly now.

You can also get a health report directly using the [status](https://microsoft.github.io/deployr-api-docs/#get-status) API call.

## Trace a code execution 

To go through the execution of a specific line of code and retrieve request IDs for debugging purposes, run a trace. 

1. [Launch the administration utility](configure-use-admin-utility.md#launch) with administrator privileges (Windows) or root/sudo privileges (Linux).

1. From the main menu, choose **Run Diagnostic Tests**.

   If you have not authenticated yet, you must provide your username and password. 

1. From the diagnostic submenu, choose either **Trace R code execution** or **Trace Python code execution** depending on the language you are using. 

1. When prompted, enter the code you want to trace. 

1. To start the trace, press the Enter key (carriage return).

1. Review the trace output.


## Trace a web service execution

To go through the execution of a specific web service and retrieve request IDs for debugging purposes, run a trace. 

1. [Launch the administration utility](configure-use-admin-utility.md#launch) with administrator privileges (Windows) or root/sudo privileges (Linux).

1. From the main menu, choose **Run Diagnostic Tests**.

   If you have not authenticated yet, you must provide your username and password. 

1. From the diagnostic submenu, choose  **Trace service execution**.

1. Enter the service name and version after the syntax '\<service-name>/\<version>' such as `my-service/1.1`. 

1. To start the trace, press the Enter key (carriage return).

1. Review the trace output to better understand how the execution is running or failing.

<a name="logs"></a>

## Log files and levels

Review the log and configuration files for any component that was identified as experiencing issues. 
You can find the logs in the \<node-install-path>\logs folder under your web and compute node installation paths.  (Locate the [install path](../operationalize/configure-find-admin-configuration-file.md) for your version.) 

If there are any issues, you must solve them before continuing. For extra help, consult or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a> or contact technical support.

By default, the logging level is set to Warning so as not to slow performance. However, whenever you encounter an issue that you want to share with technical support or a forum, you can change the logging level to capture more information.  The following logging levels are available:

|Level|Description|
|----|---------|
|Verbose|The most detailed comprehensive logging level of all activity, which is rarely (if ever) enabled in production environments|
|Debug|Logs robust details including internal system events, which are not necessarily observable|
|Information|Logs system events that correspond to its responsibilities and functions|
|Warning|Logs only when service is degraded, endangered, or may be behaving outside of its expected parameters.  (Default level)|
|Error|Logs only errors (functionality is unavailable or expectations broken)|
|Critical|Logs only fatal events that crash the application|

**To update the logging level:**

1. On each compute node AND each web node, open the configuration file, \<node-install-path>/appsettings.json. (Find the [install path](../operationalize/configure-find-admin-configuration-file.md) for your version.) 

1. Search for the section starting with `"Logging": {`

1. Set the logging level for `"Default"`, which captures Machine Learning Server default events. For debugging support, use the 'Debug' level.

1. Set the logging level for `"System"`, which captures Machine Learning Server .NET core events. For debugging support, use the 'Debug' level. Use the same value as for `"Default"`.

1. Save the file.

1. [Restart](configure-use-admin-utility.md#startstop) the node services. 

1. Repeat these changes on every compute node and every web node.  
   Each node should have the same appsettings.json properties.

1. Repeat the same operation(s) that were running when the error(s) occurred. 
   
1. Collect the [log files](#logs) from each node for debugging.

<a name="trouble"></a>

## Troubleshooting

This section contains pointers to help you troubleshoot some problems that can occur.

>[!IMPORTANT]
>1. In addition to the info below, review the issues listed in the **[Known Issues article](../resources-known-issues.md)** as well.
>2. If this section does not help solve your issue, file a ticket with technical support or post in our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

### "BackEndConfiguration is missing URI" Error

If you get an `BackEndConfiguration is missing URIs` error when trying to install a web node, then ensure your compute nodes are installed and [declared](../install/operationalize-r-server-enterprise-config.md#webnode) prior to installing web nodes. 

```
Unhandled Exception: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> Microsoft.DeployR.Server.App.Common.Exceptions.ConfigurationException: BackEndConfiguration is missing URIs
   at Microsoft.DeployR.Server.WebAPI.Extensions.ServicesExtensions.AddDomainServices(IServiceCollection serviceCollection, IHostingEnvironment env, IConfigurationRoot configurationRoot, String LogPath)
   --- End of inner exception stack trace ---
```

### “Cannot establish connection with the web node” Error

If you get the `Cannot establish connection with the web node` error, then the client is unable to establish a connection with the web node in order to log in. Perform the following steps:
+ Verify that the web address and port number displayed on the main menu of the admin utility are correct. Learn how to launch the utility, in this article: [Machine Learning Server Administration](configure-use-admin-utility.md#launch)
+ Look for web node startup errors or notifications in the stdout/stderr/[logs files](#logs). 
+ Restart the web node if you have recently changed the port the server is bound to or the certificate used for HTTPS. Learn how to restart, in this article: [Machine Learning Server Operationalization Administration](configure-use-admin-utility.md#startstop)

If the issue persists, verify you can post to the login API using curl, fiddler, or something similar. Then, share this information with technical support or post it in our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

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
  

### Compute Node Failed / HTTP status 503 on APIs (9.0.1 - Linux Only)

If you get an `HTTP status 503 (Service Unavailable)` response when using the Rest APIs or encounter a failure for the compute node during diagnostic testing, then one or more of the symlinks needed by [deployr-rserve](https://github.com/Microsoft/deployr-rserve) are missing. deployr-rserve is the R execution component for the compute node,

   1. Launch a command window with administrator privileges with root/sudo privileges.

   1. Run a [diagnostic test](#test) of the system on the machine hosting the compute node.

   1. If the test reveals that the compute node test has failed, type `pgrep -u rserve2` at a command prompt. 

   1. If no process ID is returned, then the R execution component is not running and we need to check which symlinks are missing.

   1. At the command prompt, type `tail -f /opt/deployr/9.0.1/rserve/R/log`. The symlinks are revealed. 

   1. Compare these symlinks to the symlinks listed in the [configuration](../install/operationalize-r-server-one-box-config.md) article.

   1. Add a few symlinks using the commands in the [configuration](../install/operationalize-r-server-one-box-config.md) article.

   1. [Restart](configure-use-admin-utility.md#startstop) the compute node services.

   1. Run the [diagnostic test](#test) or try the APIs again.

### Unauthorized / HTTP status 401

If you [configured Machine Learning Server to authenticate](configure-authentication.md) against LDAP/AD, and you experience connection issues or a `401` error, verify the LDAP settings you declared in appsettings.json. Use the ldp.exe tool to search the correct LDAP settings and compare them to what you have declared. You can also consult with any Active Directory experts in your organization to identify the correct parameters.

### Configuration did not restore after upgrade

If you followed the upgrade instructions but your configuration did not persist, then put the backup of the appsettings.json file under the following directories and reinstall Machine Learning Server again:
   + On Windows: C:\Users\Default\AppData\Local\DeployR\current

   + On Linux: /etc/deployr/current


### Alphanumeric error message when consuming service

If you get an error similar to `Message: b55088c4-e563-459a-8c41-dd2c625e891d` when consuming a web service, search [compute node's log file](#logs) for the alphanumeric error code to read the full error message. 


### Failed code execution with “ServiceBusyException” in the log
 
If a code execution fails and returns a `ServiceBusyException` error in the Web node log file, then a proxy issue may be blocking the execution.

The workaround is to:
1. Open the R initialization file \<install folder>\R_SERVER\etc\Rprofile.site.
1. Add the following code as a new line in Rprofile.site:

   ```R
   utils::setInternet2(TRUE)
   ```
1. Save the file and restart Machine Learning Server.
1. Repeat on every machine on which Machine Learning Server is installed.
1. Run the diagnostic test or code execution again.

### Error: "Microsoft.AspNetCore.Server.Kestrel.Internal.Networking.UvException: Error -97 EAFNOSUPPORT address family not supported"

Applies to: Machine Learning Server

If the following error occurs when attempting to start a web node _"Microsoft.AspNetCore.Server.Kestrel.Internal.Networking.UvException: Error -97 EAFNOSUPPORT address family not supported"_, then disable IPv6 on your operating system.