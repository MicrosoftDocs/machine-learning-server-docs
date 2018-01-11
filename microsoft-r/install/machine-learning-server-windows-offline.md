---
# required metadata
title: "Windows offline installation for Machine Learning Server "
description: "How to install Machine Learning Server for Windows with no internet connection."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "12/05/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# Offline installation for Machine Learning Server for Windows

By default, installers connect to Microsoft download sites to get required and updated components for Machine Learning Server 9.2.1 for Windows. If firewall constraints prevent the installer from reaching these sites, you can use an internet-connected device to download files, transfer files to an offline server, and then run setup.

Before you start, review the following article for requirements and general information about setup: [Install Machine Learning Server 9.2.1 on Windows](machine-learning-server-windows-install.md).

## Downloads

On an internet-connected computer, download all of the following files.

<a name="file-list"></a>

| Component | Download | Used for | 
|-----------|----------|----------|
|Machine Learning Server setup | Get *Machine Learning Server for Windows* (en_machine_learning_server_for_windows_x64_11452137.zip) from one of these sites:<br/><br/>[Visual Studio Dev Essentials](https://my.visualstudio.com/Downloads?q=machine%20learning%20server&pgroup=) <br/>[Volume Licensing Service Center (VLSC)](http://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | R Server |
|Pre-trained Models |[MLM_9.2.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852727) | Pre-trained models, R or Python |
|Microsoft R Open 3.4.1.0|[SRO_3.4.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkID=852724) | R |
|Microsoft Python Open |[SPO_9.2.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852723) | Python |
|Microsoft Python Server |[SPS_9.2.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852726) | Python |

## Transfer and place files

Use a tool or device to transfer the files to the offline server. 

1. Put the unzipped **en_machine_learning_server_for_windows_x64_11452137.zip** file in a convenient folder.
2. Right-click **Extract All** to unpack the file. You should see a folder named **MLS921Win**. This folder contains **ServerSetup.exe**.
3. Put the CAB files in the setup user's temp folder: **C:\Users\<user-name>\AppData\Local\Temp**. 

> [!Tip]
> Run `ServerSetup.exe /offline` from the [command line](machine-learning-server-windows-commandline.md) to get links for the .cab files used during intallation.

## Run setup

After files are placed, use the wizard or run setup from the command line:

+ [Install Machine Learning Server > How to install > Run setup](machine-learning-server-windows-install.md#howtoinstall)
+ [Commandline installation](machine-learning-server-windows-commandline.md)

## Check log files

If there are errors during Setup, check the log files located in the system temp directory. An easy way to get there is typing `%temp%` as a Run command or search operation in Windows. If you installed all components, your log file list looks similar to this screenshot:

  ![Machine Learning Server setup log files](./media/mlserver-setup-log-files.png)

<a name="connect-validate"></a>

## Connect and validate

Machine Learning Server executes on demand as R Server or as a Python application. As a verification step, connect to each application and run a script or function.

**For R**

R Server runs as a background process, as **Microsoft ML Server Engine** in Task Manager. Server startup occurs when a client application like [R Tools for Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation) or Rgui.exe connects to the server.

1. Go to C:\Program Files\Microsoft\ML Server\R_SERVER\bin\x64.
2. Double-click **Rgui.exe** to start the R Console application.
3. At the command line, type `search()` to show preloaded objects, including the `RevoScaleR` package. 
4. Type `print(Revo.version)` to show the software version.
5. Type `rxSummary(~., iris)` to return summary statistics on the built-in iris sample dataset. The `rxSummary` function is from `RevoScaleR`. 

**For Python**

Python runs when you execute a .py script or run commands in a Python console window.

1. Go to C:\Program Files\Microsoft\ML Server\PYTHON_SERVER.
2. Double-click **Python.exe**.
3. At the command line, type `help()` to open interactive help.
4. Type ` revoscalepy` at the help prompt, followed by `microsoftml` to print the function list for each module.
5. Paste in the following revoscalepy script to return summary statistics from the built-in AirlineDemo demo data:

    ~~~~
    import os
    import revoscalepy 
    sample_data_path = revoscalepy.RxOptions.get_option("sampleDataDir")
    ds = revoscalepy.RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
    summary = revoscalepy.rx_summary("ArrDelay+DayOfWeek", ds)  
    print(summary)
    ~~~~

  Output from the sample dataset should look similar to the following:

    ~~~~ 
    Summary Statistics Results for: ArrDelay+DayOfWeek
    File name: /opt/microsoft/mlserver/9.2.1/libraries/PythonServer/revoscalepy/data/sample_data/AirlineDemoSmall.xdf
    Number of valid observations: 600000.0
    
            Name       Mean     StdDev   Min     Max  ValidObs  MissingObs
    0  ArrDelay  11.317935  40.688536 -86.0  1490.0  582628.0     17372.0
    
    Category Counts for DayOfWeek
    Number of categories: 7
    
                Counts
    DayOfWeek         
    1          97975.0
    2          77725.0
    3          78875.0
    4          81304.0
    5          82987.0
    6          86159.0
    7          94975.0
    ~~~~

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)