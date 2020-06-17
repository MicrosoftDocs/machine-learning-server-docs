---
# required metadata
title: "Windows offline installation for Machine Learning Server"
description: "How to install Machine Learning Server for Windows with no internet connection."
keywords: ""
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 07/15/2019
ms.topic: "conceptual"
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

# Offline installation for Machine Learning Server for Windows

**Applies to:  Machine Learning Server 9.2.1 | 9.3 | 9.4**

By default, installers connect to Microsoft download sites to get required and updated components for Machine Learning Server for Windows. If firewall constraints prevent the installer from reaching these sites, you can use an internet-connected device to download files, transfer files to an offline server, and then run setup.

Before you start, review the following article for requirements and general information about setup: [Install Machine Learning Server on Windows](machine-learning-server-windows-install.md).

## 9.4.7 Downloads

On an internet-connected computer, download all of the following files.

<a name="file-list-94"></a>

| Component | Download | Used for | 
|-----------|----------|----------|
|Machine Learning Server setup | Get *Machine Learning Server for Windows* (en_machine_learning_server_for_windows_x64_<build-number>.zip) from one of these sites:<br/><br/>[Visual Studio Dev Essentials](https://my.visualstudio.com/Downloads?q=machine%20learning%20server%209.4.7)<br/>[Volume Licensing Service Center (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx) | ML Server |
|MLM|[MLM_9.4.7.0_1033.cab](https://download.microsoft.com/download/a/4/1/a41cbeb3-7933-4a57-848e-60f5cb40f2e2/MLM_9.4.7.0_1033.cab)|Pre-trained models, R or Python|
|Microsoft R Open|[SRO_3.5.2.0_1033.cab](https://download.microsoft.com/download/4/a/3/4a34263a-069b-42bf-946f-69f3b708ab11/SRO_3.5.2.0_1033.cab)|R|
|Microsoft Python Open|[SPO_4.5.12.0_1033.cab](https://download.microsoft.com/download/7/d/2/7d2aecbc-3495-4496-9dee-ca38fd7f55f0/SPO_4.5.12.0_1033.cab) *(see note below)*|Python|
|Microsoft Python Server|[SPS_9.4.7.0_1033.cab](https://download.microsoft.com/download/8/a/f/8af64f33-2014-42db-bdfb-bad77298636a/SPS_9.4.7.0_1033.cab)|Python|
|Python script |[Install-PyForMLS](https://download.microsoft.com/download/b/f/0/bf030264-2414-4c01-821b-2ec88f426cde/Install-PyForMLS.ps1) | Python |

> [!NOTE]
> If you are performing offline installation using the Python script `Install-PyForMLS.ps1`, then after you download `SPO_4.5.12.0_1033.cab`, rename the file to `SPO_9.4.7.0_1033.cab`. The installation script expects this filename.

## 9.3.0 Downloads

On an internet-connected computer, download all of the following files.

<a name="file-list-93"></a>

| Component | Download | Used for | 
|-----------|----------|----------|
|Machine Learning Server setup | Get *Machine Learning Server for Windows* (en_machine_learning_server_for_windows_x64_<build-number>.zip) from one of these sites:<br/><br/>[Visual Studio Dev Essentials](https://my.visualstudio.com/Downloads?q=machine%20learning%20server&pgroup=) <br/>[Volume Licensing Service Center (VLSC)](https://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | R Server |
|Pre-trained Models |[MLM_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859053) | Pre-trained models, R or Python |
|Microsoft R Open 3.4.3.0|[SRO_3.4.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=867186) | R |
|Microsoft Python Open |[SPO_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859054) | Python |
|Microsoft Python Server |[SPS_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859056) | Python |
|Python script |[Install-PyForMLS](https://aka.ms/mls93-py) | Python |

## 9.2.1 Downloads

If you require the previous version, use these links instead.

<a name="file-list-921"></a>

| Component | Download | Used for | 
|-----------|----------|----------|
|Machine Learning Server setup | Get *Machine Learning Server for Windows* (en_machine_learning_server_for_windows_x64_<build-number>.zip) from one of these sites:<br/><br/>[Visual Studio Dev Essentials](https://my.visualstudio.com/Downloads?q=machine%20learning%20server&pgroup=) <br/>[Volume Licensing Service Center (VLSC)](https://go.microsoft.com/fwlink/?LinkId=717966&clcid=0x409) | R Server |
|Pre-trained Models |[MLM_9.2.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852727) | Pre-trained models, R or Python |
|Microsoft R Open 3.4.3.0|[SRO_3.4.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkID=852724) | R |
|Microsoft Python Open |[SPO_9.2.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852723) | Python |
|Microsoft Python Server |[SPS_9.2.1.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=852726) | Python |
|Python script |[Install-PyForMLS](https://aka.ms/mls-py) | Python |

## Transfer and place files

Use a tool or device to transfer the files to the offline server. 

1. Put the unzipped **en_machine_learning_server_for_windows_x64_<build-number>.zip** file in a convenient folder.
2. Right-click **Extract All** to unpack the file. You should see a folder named **MLS93Win**. This folder contains **ServerSetup.exe**.
3. Put the CAB files in the setup user's temp folder: **C:\Users\<user-name>\AppData\Local\Temp**. 

> [!Tip]
> On the offline server, run `ServerSetup.exe /offline` from the [command line](machine-learning-server-windows-commandline.md) to get links for the .cab files used during installation. The list of .cab files appears in the installation wizard, after you select which components to install.

## Run setup

After files are placed, use the wizard or run setup from the command line:

+ [Install Machine Learning Server > How to install > Run setup](machine-learning-server-windows-install.md#howtoinstall)
+ [Commandline installation](machine-learning-server-windows-commandline.md)

## Check log files

If there are errors during Setup, check the log files located in the system temp directory. An easy way to get there is typing `%temp%` as a Run command or search operation in Windows. If you installed all components, your log file list looks similar to this screenshot:

  ![Machine Learning Server setup log files](./media/mlserver-setup-log-files.png)

<a name="connect-validate"></a>

### Set environment variables

Create an **MKL_CBWR** environment variable to [ensure consistent output](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) from Intel Math Kernel Library (MKL) calculations.

1. In Control Panel, click **System and Security** > **System** > **Advanced System Settings** > **Environment Variables**.

2. Create a new User or System variable. 

  + Set variable name to `MKL_CBWR`
  + Set the variable value to `AUTO`

This step requires a server restart. 

## Connect and validate

Machine Learning Server executes on demand as R Server or as a Python application. As a verification step, connect to each application and run a script or function.

**For R**

R Server runs as a background process, as **Microsoft ML Server Engine** in Task Manager. Server startup occurs when a client application like Rgui.exe connects to the server.

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
    File name: /opt/microsoft/mlserver/9.4.0/libraries/PythonServer/revoscalepy/data/sample_data/AirlineDemoSmall.xdf
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

**Verify CLI**

> [!Note]
> Before you continue, reboot the machine.

1. Open an Administrator command prompt.

2. Enter the following command to check availability of the CLI: `az ml admin --help`. If you receive the following error: `az: error argument _command_package: invalid choice: ml`, follow the [instructions to re-add the extension to the CLI](https://docs.microsoft.com/machine-learning-server/resources-known-issues#1-missing-azure-ml-admin-cli-extension-on-dsvm-environments
). 

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)