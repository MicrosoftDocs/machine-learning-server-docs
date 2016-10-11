---

# required metadata
title: "Diagnostics & Troubleshooting"
description: "Diagnostic Testing and Troubleshooting FAQS for DeployR"
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

# Diagnostics & Troubleshooting

This article describes how to run and interpret the DeployR diagnostic test. Additionally, this document offers solutions to issues that you might be [troubleshooting](#troubleshooting) during your use of DeployR.

## Diagnostic Testing

You can assess the state and health of your DeployR environment by running the diagnostic test described in this document. The diagnostic script:

-   Outputs basic details about your environment

-   Identifies unresponsive components, such as the front-end, any back-ends, or RServe.

-   Gathers all relevant [log and configuration files](#inspecting-diagnostic-log-files)

Armed with this information, you will be able to investigate and resolve most issues. And, in the event that you need additional support, you can send the diagnostics tar/zip file to the Microsoft Corporation technical support team.

Behind the scenes, the script evaluates the system and creates the `logs` subdirectory to store:

-   The resulting log file (`diagnostics.log`), which provides details, including the state of all components, plus pertinent configuration and environment information.

-   Copies of each [log and configuration file](#inspecting-diagnostic-log-files)

The results are also printed to the screen.

### Running the Diagnostic Check


+ **On Windows**: 
    1. Launch the DeployR administrator utility script with administrator privileges:
       ```
       cd C:\Program Files\Microsoft\DeployR-8.0.5\deployr\tools\ 
       adminUtilities.bat
       ```       
    
    1. From the main menu, run the DeployR diagnostic tests.  If there are any issues, you must solve them before continuing. Consult the Troubleshooting section of this document for additional help or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

   >All output from the diagnostic test are stored in `C:\Program Files\Microsoft\DeployR-<VERSION>\deployr\tmp\logs\diagnostics.zip`.

+ **On Linux**:
    1. Launch the DeployR administrator utility script as `root` or a user with `sudo` permissions:
       ```
       cd $DEPLOYR_HOME/deployr/tools/ 
       ./adminUtilities.sh
       ```       
    
    1. From the main menu, run the DeployR diagnostic tests.  If there are any issues, you must solve them before continuing. Consult the Troubleshooting section of this document for additional help or post questions to our <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr" target="_blank">forum</a>.

   >All output from the diagnostic test are stored into `$DEPLOYR_HOME/deployr/tmp/logs/diagnostics.zip`.

## Troubleshooting

This section contains pointers to help you troubleshoot some problems that can occur with DeployR.

### Inspecting Diagnostic Log Files

A copy of the following log and configuration files are bundled together during the diagnostic check. Review the log and configuration files for any component that was identified as experiencing issues.

**For Windows**

The following log files can be found in the resulting `diagnostics.zip` file as well as under `C:\Program Files\Microsoft\DeployR-<VERSION>\deployr\tmp\logs` directly on the DeployR host.

| Component| Log&nbsp;&&nbsp;Configuration&nbsp;Files                | Description|
|----------|---------------------------------------------------|------------------------------------------------------------------------|
| Diagnostic Results |- `diagnostics.log`                 | The DeployR diagnostic log provides details, including the state of all components, plus pertinent configuration and environment information. |
| DeployR            | - `deployr.groovy`<br />- `Stacktrace.log`<br />- `catalina.out`                     | `deployr.groovy` is the DeployR external configuration file. Tomcat's `catalina.out` serves as the main [DeployR log](deployr-common-administration-tasks.md#inspecting-server-logs). [Learn more](deployr-common-administration-tasks.md#inspecting-server-logs) about this file. |
| DeployR RServe             | - `Rserv.cfg`                       | The configuration file for the DeployR RServe component. The IP address is added to the filename for your convenience.<br /><br />**DeployR Enterprise Only**: The RServe files for remote grid nodes are not bundled. If you suspect an issue on a node, please log onto that machine to retrieve its RServe log file.                                                                                                             |

**For Linux / OS X**

The following log files can be found under `$DEPLOYR_HOME/deployr/tmp/logs` directly on the DeployR host.

| Component          | Log&nbsp;&&nbsp;Configuration&nbsp;Files                | Description |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Diagnostic Results | - `diagnostics.log`                 | The DeployR diagnostic log provides details, including the state of all components, plus pertinent configuration and environment information. |
| DeployR            | - `deployr.groovy`<br />- `catalina.out`                     | `deployr.groovy` is the DeployR external configuration file. Tomcat's `catalina.out` serves as the main [DeployR log](deployr-common-administration-tasks.md#inspecting-server-logs). [Learn more](deployr-common-administration-tasks.md#inspecting-server-logs) about this file. |
| DeployR RServe             | - `Rserv.conf`<br />- `Rserv-localhost.log`              | The log and configuration files for RServe.<br /><br />**DeployR Enterprise Only**: The RServe files for remote grid nodes are not bundled. If you suspect an issue on a node, please log onto that machine to retrieve its RServe log file.                                                                                                             |

### Resolving Issues

Use the following instructions if you have [run the diagnostic test](#running-the-diagnostic-check) and a problem was indicated.

**To resolve component issues:**

1.  Look through the [log and configuration files](#inspecting-diagnostic-log-files) for each component that was identified in the diagnostic report.

1.  If the log file indicates an error, then fix it.

1.  If Server Web Context points to the wrong IP, [update it now](#set-context).

1.  After making your corrections, [restart the component](deployr-common-administration-tasks.md#startstop) in question. It may take a few minutes for a component to restart.

1.  [Re-run the diagnostic test](#running-the-diagnostic-check) again to make sure all is running smoothly now.
     If problem persists:

    + After trying the first time, repeat steps 1-4.

    + Review the other [Troubleshooting](#troubleshooting) topics.

    + Post your questions to our [DeployR Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr).

    + After the second time, send the diagnostics tar/zip file to the Microsoft Corporation technical support team.
