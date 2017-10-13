---
# required metadata
title: "Linux offline installation for Machine Learning Server 9.2.1 "
description: "How to install Machine Learning Server for Linux with no internet connection."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/15/2017"
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

# Offline installation Machine Learning Server for Linux 

By default, installers connect to Microsoft download sites to get required and updated components for Machine Learning Server 9.2.1 for Linux. If firewall constraints prevent the installer from reaching these sites, you can use an internet-connected device to download files, transfer files to an offline server, and then run setup.

Before you start, review the following article for requirements and restrictions:

+ [Install Machine Learning Server 9.2.1 on Linux](machine-learning-server-linux-install.md) for an internet-connected installation.

<a name="package-manager"></a>

## Package managers

In contrast with previous releases, there is no install.sh script. Package managers are used for installation.

| Package manager | Platform |
|-----------------|----------|
|[yum](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-yum.html) | RHEL, CentOS|
|[apt](https://help.ubuntu.com/lts/serverguide/apt.html) | Ubuntu online |
|[dpkg](https://help.ubuntu.com/lts/serverguide/dpkg.html) | Ubuntu offline |
|[zypper](https://www.suse.com/documentation/opensuse111/opensuse111_reference/data/sec_zypper.html) | SUSE |
|[rpm](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/System_Administration_Guide/s1-rpm-using.html) | RHEL, CentOS, SUSE |

<a name="file-list"></a>

## Package location

Packages for all supported versions of Linux can be found at [packages.microsoft.com](https://packages.microsoft.com). 

+ https://packages.microsoft.com/rhel/7/prod/ (centos 7)
+ https://packages.microsoft.com/sles/11/prod/ (sles 11)
+ https://packages.microsoft.com/rhel/6/prod/ (centos 6)
+ https://packages.microsoft.com/ubuntu/14.04/prod/ (Ubuntu 14.04)
+ https://packages.microsoft.com/ubuntu/16.04/prod/ (Ubuntu 16.04)


<a name="package-list"></a>

## Package list

The following packages comprise a full Machine Learning Server installation:

```
 microsoft-mlserver-packages-r-9.2.1        ** core
 microsoft-mlserver-python-9.2.1            ** core
 microsoft-mlserver-packages-py-9.2.1       ** core
 microsoft-mlserver-mml-r-9.2.1             ** microsoftml for R (optional)
 microsoft-mlserver-mml-py-9.2.1            ** microsoftml for Python (optional)
 microsoft-mlserver-mlm-r-9.2.1             ** pre-trained models (requires mml)
 microsoft-mlserver-mlm-py-9.2.1            ** pre-trained models (requires mml)
 microsoft-mlserver-hadoop-9.2.1            ** hadoop (required for hadoop)
 microsoft-mlserver-adminutil-9.2           ** operationalization (optional)
 microsoft-mlserver-computenode-9.2         ** operationalization (optional)
 microsoft-mlserver-config-rserve-9.2       ** operationalization (optional) 
 microsoft-mlserver-dotnet-9.2              ** operationalization (optional)
 microsoft-mlserver-webnode-9.2             ** operationalization (optional)
```
The microsoft-mlserver-python-9.2.1 package provides Anaconda 4.2 with Python 3.5, executing as mlserver-python, found in `/opt/microsoft/mlserver/9.2.1/bin/python/python`

Microsoft R Open is required for R execution:

```
 microsoft-r-open-foreachiterators-3.4.1 
 microsoft-r-open-mkl-3.4.1
 microsoft-r-open-mro-3.4.1 
```

Microsoft .NET Core 1.1, used for operationalization, must be added to Ubuntu:

```
 dotnet-host
 dotnet-hostfxr-1.1.0
 dotnet-sharedframework-microsoft.netcore.app-1.1.2 
```

Additional open source packages must be installed if a package is required but not found on the system. This list varies for each installation. Here is one example of the additional packages that were added to a clean RHEL image during a connected (online) setup:

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

The following example is for the first package. Each command references the version number of the platform. Remember to change the number if your version is different. For more information, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

+ Download to CentOS or RHEL: `wget https://packages.microsoft.com/rhel/7/prod/microsoft-mlserver-packages-r-9.2.1.rpm` 
+ Download to Ubuntu 16.04: `wget https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/microsoft-mlserver-packages-r-9.2.1/microsoft-mlserver-packages-r-9.2.1.deb`
+ Download to SUSE: `wget https://packages.microsoft.com/sles/11/prod/microsoft-mlserver-packages-r-9.2.1.rpm`

Repeat for each package in the [package list](#Package list).

## Install packages

The package manager determines the installation order. Assuming all packages are in the same folder:

+ Install on CentOS or RHEL: `yum install *.rpm` 
+ Install on Ubuntu: `dpkg -i *.deb`
+ Install on SUSE: `zypper install *.rpm`

This step completes installation.

## Activate the server

Run the activation script from either the R or Python directory:

+ `/opt/microsoft/mlserver/9.2.1/bin/R/activate.sh`
+ or `/opt/microsoft/mlserver/9.2.1/bin/python/activate.sh`

## Connect and validate

1. List installed packages:

  + On CentOS and RHEL: `rpm -qa | grep microsoft` 
  + On Ubuntu: `apt list --installed | grep microsoft`  
  + On SLES11: `zypper se microsoft`

2. Once you have a package name, you can obtain verbose version information. For example:

   + On CentOS and RHEL: `$ rpm -qi microsoft-mlserver-packages-r-9.2.1`
   + On Ubuntu: `$ dpkg --status microsoft-mlserver-packages-r-9.2.1`  
   + On SLES: `zypper info microsoft-mlserver-packages-r-9.2.1`

  Output on Ubuntu is as follows:

   ```
    Package: microsoft-mlserver-packages-r-9.2.1
    Status: install ok installed
    Priority: optional
    Section: devel
    Installed-Size: 195249
    Maintainer: revobuil@microsoft.com
    Architecture: amd64
    Version: 9.2.1.1287
    Depends: microsoft-r-open-mro-3.4.1, microsoft-r-open-mkl-3.4.1, microsoft-r-open-foreachiterators-3.4.1
    Description: Microsoft Machine Learning Server
	  . . .
   ```

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

To quit the program, type `quit()` at the command line with no arguments.

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)