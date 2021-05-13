---

# required metadata
title: "Install Machine Learning Server for Linux"
description: "How to install, connect to, and use Machine Learning Server on computers running a Linux operating system."
keywords: ""
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: "12/17/2018"
ms.topic: "how-to"
ms.prod: "mlserver"

# optional metadata

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

# Install Machine Learning Server for Linux

**Applies to:  Machine Learning Server 9.2.1 | 9.3 | 9.4**

Machine Learning Server for Linux runs machine learning and data mining solutions written in R or Python in standalone and clustered topologies. 

This article explains how to install Machine Learning Server on a standalone Linux server that has an internet connection. If your server has restrictions on internet access, see [offline installation](machine-learning-server-linux-offline.md). 

This article covers the following items:
> [!div class="checklist"]
> - Prerequisites
> - Package manager overview
> - In-place upgrades of existing installations
> - Installation steps
> - An inventory of what's installed


## System and setup requirements

+ Operating system must be a [supported version of 64-bit Linux](r-server-install-supported-platforms.md).

+ Minimum RAM is 2 GB. Minimum disk space is 500 MB (8 GB or more is recommended).

+ An internet connection. If you do not have an internet connection, use the [offline installation instructions](machine-learning-server-linux-offline.md).

+ Root or super user permissions

## Licensing

Machine Learning Server is licensed as a SQL Server supplemental feature, even though SQL Server itself is not installed or required on a standalone Machine Learning Server installation. 

On development workstations, you can install the developer edition at no charge. For example, if you are learning how to use the RevoScaleR libraries, or developing a solution that is not in production, you would use this edition. 

On production servers where code supports ongoing business operations or is part of a solution you are selling commercially, you will need the enterprise edition. The enterprise edition of Machine Learning Server for Windows is licensed by the core. Enterprise licenses are sold in 2-core packs, and you must have a license for every core on the machine. For example, on an 8-core server, you would need four 2-core packs.

If you have questions, [review the pricing page or contact Microsoft](https://www.microsoft.com/sql-server/sql-server-2017-pricing) for more information.

> [!Note]
> When you purchase an enterprise license of Machine Learning Server for Linux, you can install [Machine Learning Server for Hadoop](machine-learning-server-hadoop-install.md) for free (5 nodes for each core licensed under enterprise licensing).

<a name="package-manager"></a>

## Package managers

Setup is through package managers that retrieve distributions from Microsoft web sites and install the software. Unlike previous releases, there is no install.sh script. You must have one of these package managers to install Machine Learning Server for Linux.

| Package manager | Platform |
|-----------------|----------|
|[yum](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-yum.html) | RHEL, CentOS|
|[apt](https://help.ubuntu.com/lts/serverguide/apt.html) | Ubuntu online |
| [dpkg](https://help.ubuntu.com/lts/serverguide/dpkg.html) | Ubuntu offline |
|[zypper](https://www.suse.com/documentation/opensuse111/opensuse111_reference/data/sec_zypper.html) | SUSE |
|[rpm](https://en.wikipedia.org/wiki/Rpm_(software)) | RHEL, CentOS, SUSE |

The package manager downloads packages from the [packages.microsoft.com](https://packages.microsoft.com) repo, determines dependencies, retrieves additional packages, sets the installation order, and installs the software. For example syntax on linking to the repo, see [Linux Software Repository for Microsoft Products](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

Machine Learning Server activation is a separate step *not* performed by the package manager. If you forget to activate, the server works, but the following error appears when you call an API: "Express Edition will continue to be enforced."

## Upgrade existing installations

If your existing server was configured for [operationalization](../what-is-operationalization.md), follow these alternative steps for upgrade: [Configure Machine Learning Server to operationalize analytics (One-box) > How to upgrade](../operationalize/configure-machine-learning-server-one-box.md#how-to-upgrade) or [Configure Machine Learning Server to operationalize analytics (Enterprise) > How to upgrade](../operationalize/configure-machine-learning-server-enterprise.md#how-to-upgrade).

For all other configurations, Setup performs an in-place upgrade on an existing installation. Although the installation path is new (`/opt/microsoft/mlserver/9.4.7`), when R Server 9.x is present, Machine Learning Server 9.4 finds R Server at the old path (`/usr/lib64/microsoft-r/9.1.0`) and upgrades it to the new version. 

There is no support for side-by-side installations of older and newer versions, nor is there support for hybrid versions (such as R Server 9.1 and Machine Learning Server 9.4). An installation is either entirely 9.4 or an earlier version.

## Installation paths

After installation completes, software can be found at the following paths:

+ Install root: `/opt/microsoft/mlserver/9.4.7`
+ Microsoft R Open root: `/opt/microsoft/ropen/3.5.2`
+ Executables like Revo64 and mlserver-python are at `/usr/bin`

<a name="how-to-install"></a>

## <a name="redhat">Install on Red Hat or CentOS</a>

Run the following commands to install Machine Learning Server for Linux on Red Hat Enterprise (RHEL) and CentOS (6.x - 7.x). If you run into problems, try [manual configuration](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software#manual-configuration) instead.

  ```bash
  # Install as root
  sudo su

  # Import the Microsoft repository key
  sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
  
  # Create local `azure-cli` repository
  sudo sh -c 'echo -e "[azure-cli]\nname=Azure CLI\nbaseurl=https://packages.microsoft.com/yumrepos/azure-cli\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'
  
  # Set the location of the package repo at the "prod" directory
  # The following command is for version 7.x
  # For 6.x, replace 7 with 6 to get that version
  rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

  # Verify that the "microsoft-prod.repo" configuration file exists
  ls -la /etc/yum.repos.d/
  
  # Update packages on your system:
  yum update
  
  # Install the server
  # The following command is for version 7.x
  # For 6.x: yum install microsoft-mlserver-el6-9.4.7
  yum install microsoft-mlserver-all-9.4.7
  
  # Activate the server
  /opt/microsoft/mlserver/9.4.7/bin/R/activate.sh

  # List installed packages as a verification step
  rpm -qa | grep microsoft
  
  # Choose a package name and obtain verbose version information
  rpm -qi microsoft-mlserver-packages-r-9.4.7
  ```


## <a name="ubuntu">Install on Ubuntu </a>

Follow these instructions for Machine Learning Server for Linux on Ubuntu (14.04 - 16.04 only).

  ```bash
  # Install as root
  sudo su
  
  # Optionally, if your system does not have the https apt transport option
  apt-get install apt-transport-https
  
    # Add the **azure-cli** repo to your apt sources list
  AZ_REPO=$(lsb_release -cs)
  
  echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
  
  # Set the location of the package repo the "prod" directory containing the distribution.
  # This example specifies 16.04. Replace with 14.04 if you want that version
  wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
  
  # Register the repo
  dpkg -i packages-microsoft-prod.deb
  
  # Verify whether the "microsoft-prod.list" configuration file exists
  ls -la /etc/apt/sources.list.d/
  
  # Add the Microsoft public signing key for Secure APT
  apt-key adv --keyserver packages.microsoft.com --recv-keys 52E16F86FEE04B979B07E28DB02C46DF417A0893
  
  # Update packages on your system
  apt-get update
  
  # Install the server
  apt-get install microsoft-mlserver-all-9.4.7

  # Activate the server
  /opt/microsoft/mlserver/9.4.7/bin/R/activate.sh     
  
  # List installed packages as a verification step
  apt list --installed | grep microsoft  
  
  # Choose a package name and obtain verbose version information
  dpkg --status microsoft-mlserver-packages-r-9.4.7
  ```

## <a name="suse">Install on SUSE </a>

Follow these instructions for Machine Learning Server for Linux on SUSE (SLES11 only).

  ```bash
  # Install as root
  sudo su
  
  # Set the location of the package repo at the "prod" directory containing the distribution
  # This example is for SLES11, the only supported version of SUSE in Machine Learning Server
  zypper ar -f https://packages.microsoft.com/sles/11/prod packages-microsoft-com
  
  # Update packages on your system:
  zypper update
  
  # Install the server
  zypper install microsoft-mlserver-sles11-9.4.7
  
  # You might get a message stating that PackageKit is blocking zypper
  # Enter `y` to quit PackageKit and allow zypper to continue
  y
  
  # You are prompted whether to trust the repository signing key
  # Enter `t` to temporarily trust the key for download and install
  t 
  
  # You might get a "Digest verification failed" message
  # Enter `y` to continue
  y
  
  # You are asked to confirm the list of packages to install
  # Enter `y` to continue
  y
  
  # Review and accept the license agreements for MRO, Anaconda, and Machine Learning Server.
  y
  
  #Activate the server
  /opt/microsoft/mlserver/9.4.7/bin/R/activate.sh
  
  # List installed packages as a verification step
  zypper se microsoft
  
  # Choose a package name and obtain verbose version information
  zypper info microsoft-mlserver-packages-r-9.4.7
  ```

## Set a MKL_CBWR variable

Set an MKL_CBWR environment variable to ensure consistent output from Intel Math Kernel Library (MKL) calculations.

+ Edit or create a file named **.bash_profile** in your user home directory, adding the line `export MKL_CBWR="AUTO"` to the file.

+ Execute this file by typing **source .bash_profile** at a bash command prompt.

## Start Revo64

As another verification step, run the **Revo64** program. By default, **Revo64** is installed in the /usr/bin directory, available to any user who can log in to the machine:

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

## Start Python

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

2. Enter the following command to check availability of the CLI: `az mlserver admin --help` (use `az ml admin --help` for 9.3 or older). If you receive the following error: `az: error argument _command_package: invalid choice: ml`, follow the [instructions to re-add the extension to the CLI](https://docs.microsoft.com/machine-learning-server/resources-known-issues#1-missing-azure-ml-admin-cli-extension-on-dsvm-environments
).

## Enable web service deployment and remote connections

After you confirm the basic install, continue with the next step: [configuring the server for operationalization](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) to enable additional functionality, including logging, diagnostics, and web service hosting.

You can use the `bootstrap` command for this step. This command enables operationalization features on a standalone server. It creates and starts a web node and compute node, and runs a series of diagnostic tests against the configuration to confirm the internal data storage is functional and that web services can be successfully deployed.

Alternatively, if you have multiple servers, you can designate each one as either a web node or compute node, and then link them up. For instructions, see [Configure Machine Learning Server (Enterprise)](../operationalize/configure-machine-learning-server-enterprise.md).

1. Open an Administrator command prompt.

2. Enter the following command to invoke the Administrator Command Line Interface (CLI) and configure the server: `az mlserver admin bootstrap` (use `az ml admin bootstrap` for 9.3 or older)

3. Set a password used to protect your configuration settings. Later, after configuration is finished, anyone who wants to use the CLI to modify a configuration must provide this password to gain access to settings and operations.  

   The password must meet these requirements: 8-16 characters long, with at least one upper-case letter, one lower-case letter, one number, and one special character.

After you provide the password, the tool does the rest. Your server is fully operationalized once the process is complete. For more information about the benefits of operationalization:

+ [Deploy Python and R script as a web service](../operationalize/concept-what-are-web-services.md) 
+ [Connect to a remote R server for code execution](../r/how-to-execute-code-remotely.md). Remote execution makes the server accessible to client workstations running [R Client](../r-client/install-on-linux.md) or other Machine Learning Server nodes on your network. 

> [!Note]
> Python support is new and there are a few limitations in remote computing scenarios. Remote execution is not supported on Windows or Linux in Python code. Additionally, you cannot set a [remote compute context](../r/concept-what-is-compute-context.md) to HadoopMR in Python. 

## What's installed

An installation of Machine Learning Server includes some or all of the following components.

| Component | Description |
|-----------|-------------|
| Microsoft R Open (MRO) | An open-source distribution of the base R language, plus the Intel Math Kernel library (int-mkl). |
| R proprietary libraries and script engine | Properietary R libraries are co-located with R base libraries. Libraries include RevoScaleR, MicrosoftML, mrsdeploy, olapR, RevoPemaR, and others listed in [R Package Reference](../r-reference/introducing-r-server-r-package-reference.md). <br/><br/>On Linux, the default R installation directory is `/opt/microsoft/mlserver/9.4.7`. <br/><br/>RevoScaleR is engineered for distributed and parallel processing for all multi-threaded functions, utilizing available cores and disk storage of the local machine. RevoScaleR also supports the ability to transfer computations to other RevoScaleRr instances on other computers and platforms through compute context instructions. |
| Python proprietary libraries | Proprietary packages provide modules of class objects and static functions. Libraries include revoscalepy, microsoftml, and azureml-model-management-sdk. |
| Miniconda 4.5.12 with Python 3.7.1 | An open-source distribution of Python.|
| [Admin CLI](../operationalize/configure-admin-cli-launch.md) | Used for enabling remote execution and web service deployment, operationalizing analytics, and configuring web and compute nodes.| 
| [Pre-trained models](microsoftml-install-pretrained-models.md) | Used for sentiment analysis and image detection. |

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
 microsoft-mlserver-dotnet-9.4.7            ** operationalization (optional)
 microsoft-mlserver-webnode-9.4.7           ** operationalization (optional)
 azure-cli-2.0.25-1.el7.x86_64              ** operationalization (optional)
```
The microsoft-mlserver-python-9.4.7 package provides Miniconda 4.5.12 with Python 3.7.1, executing as mlserver-python, found in `/opt/microsoft/mlserver/9.4.7/bin/python/python`

Microsoft R Open is required for R execution:

```
 microsoft-r-open-foreachiterators-3.5.2
 microsoft-r-open-mkl-3.5.2
 microsoft-r-open-mro-3.5.2 
```

Microsoft .NET Core 2.0, used for operationalization, must be added to Ubuntu:

```
 dotnet-host-2.0.0
 dotnet-hostfxr-2.0.0
 dotnet-runtime-2.0.0 
```

Additional open-source packages are installed if a package is required but not found on the system. This list varies for each installation. Refer to [offline installation](machine-learning-server-linux-offline.md) for an example list.

## Configure RStudio for RevoScaleR

If you are using the RStudio IDE, perform the following steps to load RevoScaleR and other R Client libraries.

1. Close RStudio if it is already open.

2. Start a terminal session and sign on as root (`sudo su`).

3. Open the **Renviron** file for editing:

  ```bash
  gedit /opt/microsoft/rclient/3.5.2/runtime/R/etc/Renviron
  ```

4. Scroll down to **R_LIBS_USER** and add a new configuration setting just below it:

  ```bash
  R_LIBS_SITE=/opt/microsoft/rclient/3.5.2/libraries/RServer
  ```

5. Save the file.

6. Start RStudio. In the Console window, you should see messages indicating both the Microsoft R Open and Microsoft R Client packages are loaded.

7. To confirm RevoScaleR is operational, run the RevoScaleR **rxSummary** function to return statistical summary information on the built-in Iris dataset: 

  ```r
  rxSummary(~., iris)
  ```

## Next steps

We recommend starting with any Quickstart tutorial listed in the contents pane. 

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)
