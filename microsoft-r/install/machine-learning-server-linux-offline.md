---

# required metadata
title: "Linux offline installation for Machine Learning Server 9.4 "
description: "How to install Machine Learning Server for Linux with no internet connection."
keywords: 
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 12/17/2018
ms.topic: "how-to"
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

# Offline installation Machine Learning Server for Linux 

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

**Applies to:  Machine Learning Server 9.2.1 | 9.3 | 9.4**

By default, installers connect to Microsoft download sites to get required and updated components for Machine Learning Server 9.x for Linux. If firewall constraints prevent the installer from reaching these sites, you can use an internet-connected device to download files, transfer files to an offline server, and then run setup.

Before you start, review the following article for general information about setup: [Install Machine Learning Server on Linux](machine-learning-server-linux-install.md).

<a name="package-manager"></a>

## Package managers

In contrast with previous releases, there is no install.sh script. Package managers are used for installation.

| Package manager | Platform |
|-----------------|----------|
|[yum](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-yum.html) | RHEL, CentOS|
|[apt](https://help.ubuntu.com/lts/serverguide/apt.html) | Ubuntu online |
|[dpkg](https://help.ubuntu.com/lts/serverguide/dpkg.html) | Ubuntu offline |
|[zypper](https://www.suse.com/documentation/opensuse111/opensuse111_reference/data/sec_zypper.html) | SUSE |
|[rpm](https://en.wikipedia.org/wiki/Rpm_(software)) | RHEL, CentOS, SUSE |

<a name="file-list"></a>

## Package location

Packages for all supported versions of Linux can be found at [packages.microsoft.com](https://packages.microsoft.com).

+ https://packages.microsoft.com/rhel/6/prod/ (CentOS 6)
+ https://packages.microsoft.com/rhel/7/prod/ (CentOS 7)
+ https://packages.microsoft.com/sles/11/prod/ (SLES 11)
+ https://packages.microsoft.com/ubuntu/14.04/prod/pool/main/m/ (Ubuntu 14.04)
+ https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/ (Ubuntu 16.04)


<a name="package-list"></a>

## Package list

The following packages comprise a full Machine Learning Server installation:

```
 microsoft-mlserver-packages-r-9.4.7        ** core
 microsoft-mlserver-python-9.4.7            ** core
 microsoft-mlserver-packages-py-9.4.7       ** core
 microsoft-mlserver-mlm-r-9.4.7             ** pre-trained models
 microsoft-mlserver-mlm-py-9.4.7            ** pre-trained models
 microsoft-mlserver-hadoop-9.4.7            ** hadoop (required for hadoop)
 microsoft-mlserver-adminutil-9.4.7         ** operationalization (optional)
 microsoft-mlserver-computenode-9.4.7       ** operationalization (optional)
 microsoft-mlserver-config-rserve-9.4.7     ** operationalization (optional) 
 microsoft-mlserver-webnode-9.4.7           ** operationalization (optional)
 azure-cli_2.0.25-1.el7.x86_64              ** operationalization (optional)
```
The microsoft-mlserver-python-9.4.7 package provides Python 3.7.1, executing as mlserver-python, found in `/opt/microsoft/mlserver/9.4.7/bin/python/python`

Microsoft R Open is required for R execution:

```
 microsoft-r-open-mkl-3.5.2
 microsoft-r-open-mro-3.5.2 
```

Microsoft .NET Core 2.0, used for operationalization, must be added to Ubuntu:

```
 dotnet-host-2.0.0
 dotnet-hostfxr-2.0.0
 dotnet-runtime-2.0.0  
```

Additional open-source packages must be installed if a package is required but not found on the system. This list varies for each installation. Here is one example of the additional packages that were added to a clean RHEL image during a connected (online) setup:

```
 cairo
 fontconfig 
 fontpackages-filesystem
 graphite2 
 harfbuzz 
 libICE  
 libSM   
 libXdamage  
 libXext  
 libXfixes  
 libXft  
 libXrender  
 libXt    
 libXxf86vm  
 libicu      
 libpng12   
 libthai
 libunwind     
 libxshmfence   
 mesa-libEGL
 mesa-libGL     
 mesa-libgbm    
 mesa-libglapi 
 pango   
 paratype-pt-sans-caption-fonts  
 pixman  
```

## Download packages

If your system provides a graphical user interface, you can click a file to download it. Otherwise, use `wget`. We recommend downloading all packages to a single directory so that you can install all of them in a single command. By default, `wget` uses the working directory, but you can specify an alternative path using the `-outfile` parameter.

The following example is for the first package. Each command references the version number of the platform. Remember to change the number if your version is different. For more information, see [Linux Software Repository for Microsoft Products](/windows-server/administration/linux-package-repository-for-microsoft-software).

+ Download to CentOS or RHEL: `wget https://packages.microsoft.com/rhel/7/prod/microsoft-mlserver-packages-r-9.4.7.rpm` 
+ Download to Ubuntu 16.04: `wget https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/microsoft-mlserver-packages-r-9.4.7/microsoft-mlserver-packages-r-9.4.0.deb`
+ Download to SUSE: `wget https://packages.microsoft.com/sles/11/prod/microsoft-mlserver-packages-r-9.4.7.rpm`

Repeat for each package in the [package list](#package-list).

## Install packages

The package manager determines the installation order. Assuming all packages are in the same folder:

+ Install on CentOS or RHEL: `yum install *.rpm` 
+ Install on Ubuntu: `dpkg -i *.deb`
+ Install on SUSE: `zypper install *.rpm`

This step completes installation.

## Activate the server

Run the activation script from either the R or Python directory:

+ `/opt/microsoft/mlserver/9.4.7/bin/R/activate.sh`
+ or `/opt/microsoft/mlserver/9.4.7/bin/python/activate.sh`

## Connect and validate

1. List installed packages:

   + On CentOS and RHEL: `rpm -qa | grep microsoft` 
   + On Ubuntu: `apt list --installed | grep microsoft`  
   + On SLES11: `zypper se microsoft`

2. Once you have a package name, you can obtain verbose version information. For example:

   + On CentOS and RHEL: `$ rpm -qi microsoft-mlserver-packages-r-9.4.7`
   + On Ubuntu: `$ dpkg --status microsoft-mlserver-packages-r-9.4.7`  
   + On SLES: `zypper info microsoft-mlserver-packages-r-9.4.7`

   Output on Ubuntu is as follows:

   ```
    Package: microsoft-mlserver-packages-r-9.4.7
    Status: install ok installed
    Priority: optional
    Section: devel
    Installed-Size: 195249
    Maintainer: revobuil@microsoft.com
    Architecture: amd64
    Version: 9.4.7
    Depends: microsoft-r-open-mro-3.5.2, microsoft-r-open-mkl-3.5.2, microsoft-r-open-foreachiterators-3.5.2
    Description: Microsoft Machine Learning Server
	  . . .
   ```

## Set a MKL_CBWR variable

Set an MKL_CBWR environment variable to ensure consistent output from Intel Math Kernel Library (MKL) calculations.

+ Edit or create a file named **.bash_profile** in your user home directory, adding the line `export MKL_CBWR="AUTO"` to the file.

+ Execute this file by typing **source .bash_profile** at a bash command prompt.

### Start Revo64

As another verification step, run the Revo64 program. By default, Revo64 is linked to the /usr/bin directory, available to any user who can log in to the machine:

1. From /Home or any other working directory:

   `[<path>] $ Revo64`

2. Run a RevoScaleR function, such as **rxSummary** on a dataset. Many sample datasets, such as the iris dataset, are ready to use because they are installed with the software:

   `> rxSummary(~., iris)`

   Output from the iris dataset should look similar to the following:

~~~~
        Rows Read: 150, Total Rows Processed: 150, Total Chunk Time: 0.001 seconds
        Computation time: 0.005 seconds.
        Call:
        rxSummary(formula = ~., data = iris)

        Summary Statistics Results for: ~.
        Data: iris
        Number of valid observations: 150

         Name         Mean     StdDev    Min Max ValidObs MissingObs
         Sepal.Length 5.843333 0.8280661 4.3 7.9 150      0
         Sepal.Width  3.057333 0.4358663 2.0 4.4 150      0
         Petal.Length 3.758000 1.7652982 1.0 6.9 150      0
         Petal.Width  1.199333 0.7622377 0.1 2.5 150      0

        Category Counts for Species
        Number of categories: 3
        Number of valid observations: 150
        Number of missing observations: 0

         Species    Counts
         setosa     50
         versicolor 50
         virginica  50
~~~~

To quit the program, type `q()` at the command line with no arguments.

### Start Python

1. From Home or any other user directory:

   `[<path>] $ mlserver-python`

2. Run a revoscalepy function, such as **rx_Summary** on a dataset. Many sample datasets are built in. At the Python command prompt, paste the following script:

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
    File name: /opt/microsoft/mlserver/9.4.7/libraries/PythonServer/revoscalepy/data/sample_data/AirlineDemoSmall.xdf
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

To quit the program, type `quit()` at the command line with no arguments.

## Verify CLI

1. Open an Administrator command prompt.

2. Enter the following command to check availability of the CLI: `az ml admin --help`. If you receive the following error: `az: error argument _command_package: invalid choice: ml`, follow the [instructions to re-add the extension to the CLI](../resources-known-issues.md#1-missing-azure-ml-admin-cli-extension-on-dsvm-environments
).

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)
