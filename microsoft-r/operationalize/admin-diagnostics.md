---

# required metadata
title: "Troubleshooting and Diagnostics - Operationalization  | Microsoft R Server Docs"
description: "Troubleshooting and Diagnostic for Operationalization of R Analytics with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/15/2016"
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

# Diagnostics & Troubleshooting of R Server's Operationalization

**Applies to:  Microsoft R Server 9.0.1**

You can assess the state and health of your environment with the set of diagnostic tests found in this Administration Utility. 
Armed with this information, you can identify unresponsive components, execution problems, and access the log files. 

The set of diagnostic tests include:
+ A general health check of the configuration
+ A raw report of the system status
+ A trace of an R code execution
+ A trace of an web service execution

<a name="test"></a>

## Running Diagnostic Tests

**To run diagnostic tests:**

1. [Launch the administration utility](admin-utility.md#launch) with administrator privileges (Windows) or `root`/ `sudo` privileges (Linux).

1. From the main menu, choose **Run Diagnostic Tests**.

1. If you haven't authenticated yet, you'll need to provide your username and password. 

1. For a 'health report' of the configuration including a code execution test, choose **Test configuration**.

   1. Review the test results. If any issues arise, investigate the [log files](#logs) and attempt to resolve the issues.

   1. After making your corrections, [restart the component](admin-utility.md#startstop) in question. It may take a few minutes for a component to restart.

   1. Rerun the diagnostic test to make sure all is running smoothly now.

   >[!NOTE]
   >You can also get a health report directly using [the `status` API call](https://microsoft.github.io/deployr-api-docs/#get-status).

1. For the 'raw details' on the health of the system and review the output, choose **Get raw server status**.

1. To trace the execution of specific R code and retrieve request IDs for debugging purposes, choose **Trace code execution**:
      1. Enter the R code you want to run and trace. 
      1. Press the Enter key (carriage return) to start the trace.
      1. Review the trace output.

1. To trace the execution of specific service and retrieve request IDs for debugging purposes, choose **Trace service execution**:  
      1. Enter the service name and version following the syntax `<service-name>/<version>` such as `my-service/1.1`. 
      1. Press the Enter key (carriage return) to start the trace.
      1. Review the trace output to better understand how the execution is running or failing.

<a name="logs"></a>

## Log Files

Review the log and configuration files for any component that was identified as experiencing issues. The [logging level](#loglevel) can be changed to capture more or less information.

**Table: Path to log files by node and operating system**

|Operating System|Path on Web Node|Path on Compute Node|
|----------------|--------|------------|
|Windows|&lt;MRS_Home>\deployr\Microsoft.DeployR.Server.WebAPI\logs |&lt;MRS_Home>\deployr\Microsoft.DeployR.Server.BackEnd\logs|
|Linux|/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/logs |/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.BackEnd/logs| 

*<small> where `<MRS_home>` is the path to the Microsoft R Server installation directory on the compute node. To find this path, enter `normalizePath(R.home())` in your R console.</small>


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

   1. On each compute node AND each web node, open the `appsettings.json` external JSON configuration file.

      |`appsettings.json`|Path on Web Node|Path on Compute Node|
      |----------------|--------|------------|
      |Windows|<small>&lt;MRS_Home>\deployr\Microsoft.DeployR.Server.WebAPI\ </small>|<small>&lt;MRS_Home>\deployr\Microsoft.DeployR.Server.BackEnd\</small>|
      |Linux|<small>/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/ </small>|<small>/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.BackEnd/</small>|  

      *<small> where `<MRS_home>` is the path to the Microsoft R Server installation directory on the compute node. To find this path, enter `normalizePath(R.home())` in your R console.</small>

   1. Search for the section starting with `"Logging": {`

   1. Set the logging level for `"Default"`, which captures R Server default events. For debugging support, use the `Debug` level.

   1. Set the logging level for `"System"`, which captures R Server .NET core events. For debugging support, use the `Debug` level. Use the same value as for `"Default"`.

   1. Save the file.

   1. [Restart](admin-utility.md#startstop) the node services. 

   1. Repeat these changes on every compute node and every web node.
      >Each node should have the same `appsettings.json` properties.

   1. Repeat the same operation(s) that where running when the error(s) occurred. 
   
   1. Collect the [log files](#logs) from each node for debugging.



## Troubleshooting

This section contains pointers to help you troubleshoot some problems that can occur.

### "BackEndConfiguration is missing URI" Error

If you get an `BackEndConfiguration is missing URIs` error when trying to install a web node, then verify that your compute nodes are installed and [declared](configuration-initial.md#webnode) prior to installing the web node. 

```
Unhandled Exception: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> Microsoft.DeployR.Server.App.Common.Exceptions.ConfigurationException: BackEndConfiguration is missing URIs
   at Microsoft.DeployR.Server.WebAPI.Extensions.ServicesExtensions.AddDomainServices(IServiceCollection serviceCollection, IHostingEnvironment env, IConfigurationRoot configurationRoot, String LogPath)
   --- End of inner exception stack trace ---
```


### Compute Node Failed / HTTP status 503 on APIs (Linux Only)

If you get an `HTTP status 503 (Service Unavailable)` response when using operationalization Rest APIs -or- get a `FAIL` result for the compute node during diagnostic testing, then it may be that one or more symlinks needed to start [`deployr-rserve`](https://github.com/Microsoft/deployr-rserve), the R execution component for the compute node, are missing.

   1. Launch a command window with administrator privileges with `root`/ `sudo` privileges.

   1. Run a [diagnostic test](#test) of the system on the machine hosting the compute node.

   1. If the test reveals that the compute node test has failed, type `pgrep -u rserve2` at a command prompt. 

   1. If no process ID is returned, then the R execution component isn't running and we need to check which symlinks are missing.

   1. At the command prompt, type `tail -f /opt/deployr/9.0.1/rserve/R/log`. The symlinks are revealed. 

   1. Compare these to those listed in the [configuration](configuration-initial.md) article.

   1. Add a few symlinks using the commands in the [configuration](configuration-initial.md) article.

   1. [Restart](admin-utility.md#startstop) the compute node services.

   1. Run the [diagnostic test](#test) or try the APIs again.