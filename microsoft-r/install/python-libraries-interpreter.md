---
title: "Install Python client libraries for remote access to Machine Learning Server "
description: "Installing python interpreter and packages locally to interact with a remote Machine Learning Server"
keywords: 
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 06/01/2018
ms.topic: "how-to"
ms.prod: "mlserver"
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# How to install Python client libraries for remote access to a Machine Learning Server

Machine Learning Server includes open-source and Microsoft-specific Python packages for modeling, training, and scoring data for statistical and predictive analytics. For classic client-server configurations, where multiple clients connect to and use a remote Machine Learning Server, installing the same Python client libraries on a local workstation enables you to write and run script locally and then push execution to the remote server where data resides. This is referred to as a [remote compute context](../r/concept-what-is-compute-context.md), operant when you call Python functions from libraries that exist on both client and server environments.

A remote server can be either of the following server products:

+ [SQL Server 2017 Machine Learning Services (In-Database)](https://docs.microsoft.com/sql/advanced-analytics/install/sql-machine-learning-services-windows-install)
+ [Machine Learning Server for Hadoop](machine-learning-server-hadoop-install.md)

Client workstations can be Windows or Linux. 

[Microsoft Python packages](../python-reference/introducing-python-package-reference.md) common to both client and server systems include the following:

+ [revoscalepy](../python-reference/revoscalepy/revoscalepy-package.md)
+ [microsoftml](../python-reference/microsoftml/microsoftml-package.md)
+ [azureml-model-management-sdk](../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md)
+ [pre-trained models](microsoftml-install-pretrained-models.md)
 
This article describes how to install a Python interpreter (Anaconda) and Microsoft's Python packages locally on a client machine. Once installed, you can use all of the Python modules in Anaconda, Microsoft's packages, and any third-party packages that are Python 3.5 compliant. For remote compute context, you can only call the Python functions from packages in the above list.

## Check package versions

While not required, it's a good idea to cross-check package versions so that you can match versions on the server with those on the client. On a server with restricted access, you might need an administrator to get this information for you.

+ [Package information on SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/determine-which-packages-are-installed-on-sql-server)
+ [Package information on Machine Learning Server for Hadoop](machine-learning-server-hadoop-install.md#package-list)

## Install Python libraries on Windows

1. Download the installation shell script [Install-PyForMLS.ps1](https://download.microsoft.com/download/b/f/0/bf030264-2414-4c01-821b-2ec88f426cde/Install-PyForMLS.ps1) (or use [https://aka.ms/mls93-py](https://aka.ms/mls93-py) for the 9.3 release or [https://aka.ms/mls-py](https://aka.ms/mls-py) for the 9.2. release). The script installs Miniconda 4.5.12, which includes Python 3.7.2, along with all packages listed previously.

2. Open PowerShell window with elevated administrator permissions (right-click **Run as administrator**).

3. Go to the folder in which you downloaded the installer and run the script. Add the `-InstallFolder` command-line argument to specify a folder location for the libraries. For example: 

   ```
   cd {{download-directory}}
   .\Install-PyForMLS.ps1 -InstallFolder "C:\path-to-python-for-mls")
   ```
   Installation takes some time to complete. You can monitor progress in the PowerShell window. When setup is finished, you have a complete set of packages. For example, if you specified `C:\mspythonlibs` as the folder name, you would find the packages at `C:\mspythonlibs\Lib\site-packages`.

The installation script does not modify the PATH environment variable on your computer so the new python interpreter and modules you just installed are not automatically available to your tools. For help on linking the Python interpreter and libraries to tools, see [Link Python tools and IDEs](../python/quickstart-python-tools.md), replacing the MLS server paths with the path you defined on your workstation For example, for a Python project in Visual Studio, your custom environment would specify `C:\mypythonlibs`, `C:\mypythonlibs\python.exe` and `C:\mypythonlibs\pythonw.exe` for **Prefix path**, **Interpreter path**, and **Windowed interpreter**, respectively.

## Offline install

Download .cab files used for [offline installation](machine-learning-server-windows-offline.md#file-list-94) and place them in your %TEMP% directory. You can type %TEMP% in a Run command to get the exact location, but it is usually a user directory such as `C:\Users\<your-user-name>\AppData\Local\Temp`. 

After copying the files, run the PowerShell script using the same syntax as an online install. The script knows to look in the temp directory for the files it needs.

## Install Python libraries on Linux

On each supported OS, the package manager downloads packages from the repository, determines dependencies, retrieves additional packages, and installs the software. After installation completes, mlserver-python executable is at '/usr/bin'.
 
**On Ubuntu 14.04 - 16.04**

With root or sudo permissions, run the following commands:
```
# Download packages-microsoft-prod.deb to set location of the package repository. For example for 16.04.
wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
dpkg -i packages-microsoft-prod.deb

# Add the Microsoft public signing key for Secure APT
apt-key adv --keyserver packages.microsoft.com --recv-keys 52E16F86FEE04B979B07E28DB02C46DF417A0893

# Verification step: look for the mlserver.list configuration file
ls -la /etc/apt/sources.list.d/

# Update packages on your system
apt-get update

# If your system does not have the https apt transport option
apt-get install apt-transport-https

# Install the packages
apt-get install microsoft-mlserver-packages-py-9.4.7
```     

**Red Hat and CentOS 6/7**

With root or sudo permissions, run the following commands:
```
# Set location of the package repository. For example for 7.
rpm -Uvh https://packages.microsoft.com/rhel/7/prod/microsoft-mlserver-packages-py-9.4.7.rpm

# Verification step: look for the mlserver.list configuration file
ls -la /etc/apt/sources.list.d/

# Install the packages
yum install microsoft-mlserver-packages-py-9.4.7
``` 

**SUSE Linux Enterprise Server 11**

With root or sudo permissions, run the following commands:
```
# Set location of the package repository. For example for SLES 11.
rpm -Uvh https://packages.microsoft.com/sles/11/prod/microsoft-mlserver-packages-py-9.4.7.rpm

# Verification step: look for the mlserver.list configuration file
ls -la /etc/apt/sources.list.d/

# Install the packages
zypper install microsoft-mlserver-packages-py-9.4.7
``` 


## Test local package installation

As a verification step, call functions from the revoscalepy package and from [scikit](http://scikit-learn.org/stable/), included in Anaconda.

If you get a "module not found" error for any of the instructions below, verify you are loading the python interpreter from the right location. If using Visual Studio, confirm that you selected the custom environment pointing the prefix and interpreter paths to the correct location.

> [!NOTE]
> On Windows, depending on how you run the script, you might see this message: "Express Edition will continue to be enforced". Express edition is one of the free SQL Server editions. This message is telling you that client libraries are licensed under the Express edition. Limits on this edition are the same as Standard: in-memory data sets and 2-core processing. Remote servers typically run higher editions not subjected to the same memory and processing limits. When you push the compute context to a remote server, you work under the full capabilities of that system.

1. Create some data to work with. This example loads the iris data set using scikit. 

    ```Python
    from sklearn import datasets
    import pandas as pd
    iris = datasets.load_iris()
    df = pd.DataFrame(iris.data, columns=iris.feature_names)
    ```
2. Print out the dataset. You should see a 4-column table with measurements for sepal length, sepal width, petal length, and petal width.

    ```Python
    print(df)
    ```
3. Load revoscalepy and calculate a statistical summary for data in one of the columns. Print the output to view mean, standard deviation, and other measures.

    ```Python
    from revoscalepy import rx_summary
    summary = rx_summary("petal length (cm)", df)
    print(summary)
    ```

**Results**

```text
    Call:
    rx_summary(formula = 'petal length (cm)', data = <class 'pandas.core.frame.DataFrame'>, by_group_out_file = None,
        summary_stats = ['Mean', 'StdDev', 'Min', 'Max', 'ValidObs', 'MissingObs'], by_term = True, pweights = None,
        fweights = None, row_selection = None, transforms = None, transform_objects = None, transform_function = None,
        transform_variables = None, transform_packages = None, overwrite = False, use_sparse_cube = False,
        remove_zero_counts = False, blocks_per_read = 1, rows_per_block = 100000, report_progress = None, verbose = 0,
        compute_context = <revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object at 0x000002B7EBEBCDA0>)
    
    Summary Statistics Results for: petal length (cm)
    
    Number of valid observations: 150.0
    Number of missing observations: 0.0
    
                    Name      Mean   StdDev  Min  Max  ValidObs  MissingObs
    0  petal length (cm)  3.758667  1.76442  1.0  6.9     150.0         0.0
 ```

## Next steps

Now that you have installed local client libraries and verified function calls, try the following walkthroughs to learn how to use the libraries locally and remotely when connected to resident data stores.

+ [Quickstart: Create a linear regression model in a local compute context](../python/quickstart-revoscalepy-linear-regression-model.md)
+ [How to use revoscalepy in a Spark compute context](../python/how-to-revoscalepy.md)
+ [How to use revoscalepy in a SQL Server compute context](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model)

Remote access to a SQL Server is enabled by an administrator who has configured ports and protocols, enabled remote connections, and assigned user logins. Check with your administrator to get a valid connection string when using a remote compute context to SQL Server.
