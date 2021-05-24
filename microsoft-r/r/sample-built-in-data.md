---

# required metadata
title: "Sample data for RevoScaleR (Microsoft R)"
description: "ScaleR provides built-in sample data sets that make it easier to get started with tutorials and examples."
keywords: ""
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: "09/21/2017"
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

# Sample data for RevoScaleR and revoscalepy

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

Sample data is available both within the RevoScaleR and revoscalepy packages, and online.

## Downloads

+ On the web, additional data sets, including large ones that we don't ship in the product, can be found at [https://packages.revolutionanalytics.com/datasets/](https://packages.revolutionanalytics.com/datasets/)

## Location of built-in RevoScaleR sample data

+ On Windows, the default location is C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR\SampleData
+ On Linux, the default location is /opt/microsoft/mlserver/9.2.1/libraries/RServer/RevoScaleR/SampleData

## Location of built-in revoscalepy sample data

+ On Windows, the default location is C:\Program Files\Microsoft\ML Server\PYTHON_SERVER\Lib\site-packages\revoscalepy\data\sample_data
+ On Linux, the default location is /opt/microsoft/mlserver/9.2.1/libraries/PythonServer/revoscalepy/data/sample_data

## How to access built-in sample data using RevoScaleR functions

RevoScaleR provides functions for retrieving information about sample data. Use one of the R console applications to execute the open R `list.files` command with the RevoScaleR rxGetOption function and the SampleDataDir argument. 

1. Open the R console or start a Revo64 session.

   On Windows, you can run **Rgui.exe**, located at \Program Files\Microsoft\ML Server\R_SERVER\bin\x64. On Linux, you can type **Revo64** at the command line.

2. Retrieve the file list by entering the following command at the command prompt:

    ```
    list.files(rxGetOption("sampleDataDir"))
    ```

    The open-source R `list.files` command returns a file list provided by the RevoScaleR **rxGetOption** function and the **sampleDataDir** argument. 

> [!NOTE]
> If you are a Windows user and new to R, be aware that R script is case-sensitive. If you mistype the **rxGetOption** function or its parameter, you will get an error instead of the file list.

## File list

You should see the following files 

```
    [1] "AirlineDemo1kNoMissing.csv" "AirlineDemoSmall.csv"      
    [3] "AirlineDemoSmall.xdf"       "AirlineDemoSmallComposite" 
    [5] "AirlineDemoSmallOrc"        "AirlineDemoSmallParquet"   
    [7] "AirlineDemoSmallSplit"      "AirlineDemoSmallUC.xdf"    
    [9] "ccFraudScoreSmall.csv"      "ccFraudSmall.csv"          
    [11] "CensusWorkers.xdf"          "claims.dat"                
    [13] "claims.sas7bdat"            "claims.sav"                
    [15] "claims.sd7"                 "claims.sqlite"             
    [17] "claims.sts"                 "claims.txt"                
    [19] "claims.xdf"                 "claims_.txt"               
    [21] "claims4blocks.xdf"          "claimsExtra.txt"           
    [23] "claimsParquet"              "claimsQuote.txt"           
    [25] "claimsTab.txt"              "claimsTxt"                 
    [27] "claimsXdf"                  "CustomerSurvey.xdf"        
    [29] "DJIAdaily.xdf"              "fourthgraders.xdf"         
    [31] "hyphens.txt"                "Kyphosis.xdf"              
    [33] "mortDefaultSmall.xdf"       "mortDefaultSmall2000.csv"  
    [35] "mortDefaultSmall2001.csv"   "mortDefaultSmall2002.csv"  
    [37] "mortDefaultSmall2003.csv"   "mortDefaultSmall2004.csv"  
    [39] "mortDefaultSmall2005.csv"   "mortDefaultSmall2006.csv"  
    [41] "mortDefaultSmall2007.csv"   "mortDefaultSmall2008.csv"  
    [43] "mortDefaultSmall2009.csv"   "mrsDebugParquet"           
    [45] "README"                     "testAvro4.bin"             
    [47] "Utf16leDb.sqlite"     
```

Sample data is provided in multiple formats so that you can step through various data import scenarios using different data formats and techniques. 

+ XDF is the native file format engineered for fast retrieval of columnar data. In this format, data is stored in blocks, which is an advantage on distributed file systems and for import operations that include append or overwrites at the block level. 
+ CSV files are used to showcase multi-file import operations.
+ Several other data formats are used for platform-specific lessons.

Most of the built-in data sets are small enough to fit in-memory. Larger data sets containing the full airline, census, and mortgage default data sets are available for download [online](https://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). 

<a name="demo-sql-data"></a>

## How to load sample data into SQL Server

You can easily upload any CSV file into SQL Server if you want to step through demos or example script in the documentation. Use SQL Server Management Studio to import the data. You must have write access on the server. The following exercise creates two databases: RevoClaimsDB based on claims.txt, and RevoAirlineDB based on AirlineDemoSmall.csv.

1. In Object Explorer, create a new database named *RevoClaimsDB*, using default values for everything else.
2. Right-click **RevoClaimsDB > Tasks > Import Data**.
3. In Choose a Data Source:
   + For Data source, select **Flat File Source**. 
   + For File name, navigate to the sample data directory (by default, it is C:\Program Files\Microsoft\ML Server\R_SERVER\Library\RevoScaleR\SampleData), and select *claims.txt* and click **Open**.
   + For Format > Text qualifier, replace *<none>* with a double quote character ". Stripping out the double quotation marks from column headings will simplify the query syntax later when importing data into a RevoScaleR session.
   + Accept the rest of the defaults by clicking **Next**.

4. In Choose a Destination:
    + For Destination, scroll down and select **SQL Server Native Client 11.0**. 
    + Server name should be local, authentication should be Windows, and the database should be *RevoClaimsDB*
    + Accept the defaults by clicking **Next**.
5. In Select Source Tables and Views:
    + Click **Preview** to confirm that column headings do not contain double quotes. 
    + If you see quotes enclosing the column header names, the easiest way to remove them is start over, remembering to set the *Text qualifier* field in step 3.
6. In Save and Run Package, click **Finish** to create the database.
7. Repeat for the airline demo data and any other data sets you want to upload.

## How to generate random numbers

Random numbers are useful in testing and learning scenarios. R has extensive facilities for managing random number generation, and these facilities are all fully supported in RevoScaleR. In addition, RevoScaleR provides an interface to random number generators supplied by the Vector Statistical Library (VSL) that is part of Intel’s Math Kernel Library. The VSL random number generators are most useful in a distributed setting, where they allow parallel processes to generate uncorrelated random number streams. 

To use one of these generators, call the RevoScaleR function [rxRngNewStream](~/r-reference/revoscaler/rxrng.md), specifying the generator (the default is a version of the Mersenne-Twister, MT-2203), the desired substream (if applicable), and a seed (optional). 

## See Also

 [Machine Learning Server](../what-is-machine-learning-server.md)   
 [How-to guides in Machine Learning Server](how-to-introduction.md) 
 [RevoScaleR Functions](~/r-reference/revoscaler/revoscaler.md) 
