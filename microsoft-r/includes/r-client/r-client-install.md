**System Requirements:**

&nbsp;&nbsp;&nbsp;&nbsp;Operating Systems: &nbsp;&nbsp;&nbsp;  64-bit versions of **Microsoft Windows 7, 8.1, and 10**<br>
&nbsp;&nbsp;&nbsp;&nbsp;Free disk space: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 600+ MB recommended, after installation of all prerequisites <br>
&nbsp;&nbsp;&nbsp;&nbsp;Available RAM: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 4+ GB recommended <br>
&nbsp;&nbsp;&nbsp;&nbsp;Internet access: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Needed to download R Client and any dependencies   


   >You must install Microsoft R Client to a local drive on your computer.
   >
   >You may need to disable your antivirus software. If you do, please turn it back on as soon as you are finished.

<br> 

**How to Install (with Internet Access):**

1. Log in to the machine with administrator privileges.

1. [Download Microsoft R Client](http://aka.ms/rclient/download).

1. Close any other programs running on the system. 

1. Run the Microsoft R Client setup and follow the prompts:

    1. Accept the Microsoft R Client license terms.
    
    1. To install Microsoft R Client, you'll need [Microsoft R Open](../../r-open.md), Microsoft's enhanced distribution of R. The setup will install it for you automatically. You must accept the license terms for Microsoft R Open.

    1. If desired, select the option of installing [R Tools for Visual Studio](https://msdn.microsoft.com/en-us/library/mt721271.aspx#Anchor_1), an integrated development environment available as a free add-in for any edition of Visual Studio. This option is only available if the supported version of Visual Studio is already installed.  If you've selected to install it as well, accept the terms for R Tools for Visual Studio.

    1. Accept the default installation path for Microsoft R Client or choose another location.

    1. When the installation finishes, click **Finish**.  A welcome screen opens to introduce you to the product and documentation.

<br> 

**How to Install (without Internet Access):**

1. On the machine with _**unrestricted**_ internet access:

   1. [Download Microsoft R Client](http://aka.ms/rclient/download)
   
   1. Download the Microsoft R Open ( *.cab) needed to install R Client from the following link: https://go.microsoft.com/fwlink/?LinkId=761266&clcid=1033

   1. Copy the .cab file and R Client installer to a network share or portable drive.

1. On the machine with _**restricted**_ internet access:

   1. Log in with administrator privileges. 
   
   1. Close any other programs running on the system. 

   1. Copy the .cab file and R Client installer from the network share/portable drive on the first machine to a folder on the machine that has restricted internet access.

   1. Run `RClientSetup.exe`, which will also find the cab file in the same folder, and follow the onscreen prompts.

<br>

>**What's Installed with R Client**<br>
>
>The Microsoft R Client setup installs the R base packages and a set of enhanced and proprietary R packages that support parallel processing, improved performance, and connectivity to data sources including SQL Server and Hadoop. The R libraries are installed under the R Client installation directory, `C:\Program Files\Microsoft\R Client\R_SERVER`. Additionally, in this directory you will find documentation for the R base packages, sample data, and the R library.
>
>All of tools for the standard base R (RTerm, Rgui.exe, and RScript) are also included with Microsoft R Client under `<install-directory>\bin`. Documentation for these tools can be found in the setup folder: `<install-directory>\doc` and in `<install-directory>\doc\manual`. One easy way to open these files is to open `RGui`, click **Help**, and select one of the options. 
