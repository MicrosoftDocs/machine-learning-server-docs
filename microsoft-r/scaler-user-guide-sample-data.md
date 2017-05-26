---

# required metadata
title: "Sample data for RevoScaleR (Microsoft R)"
description: "ScaleR provides built-in sample data sets that make it easier to get started with tutorials and examples."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/01/2017"
ms.topic: "get-started-article"
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
ms.technology: "r-server"
ms.custom: ""

---

# Sample data for RevoScaleR

Sample data is available both within the RevoScaleR package and [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). 

## Locate built-in sample data in RevoScaleR

RevoScaleR provides functions for retrieving information about sample data. Use one of the R console applications to execute the open R `list.files` command with the RevoScaleR rxGetOption function and the SampleDataDir argument. 

1. Open the R console or start a Revo64 session.

  On Windows, you can run **Rgui.exe**, located at \Program Files\Microsoft\R Server\R_SERVER\bin\x64. On Linux, you can type **Revo64** at the command line.

2. Retrieve the file list by entering the following command at the command prompt:

        list.files(rxGetOption("sampleDataDir"))

  The open source R `list.files` command returns a file list provided by the RevoScaleR **rxGetOption** function and the **sampleDataDir** argument. 

> [!NOTE]
> If you are a Windows user and new to R, be aware that R script is case-sensitive. If you mistype the **rxGetOption** function or its parameter, you will get an error instead of the file list.

## File list

You should see the following files 

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

Sample data is provided in multiple formats so that you can step through various data import scenarios using different data formats and techniques. 

+ XDF is the native file format engineered for fast retrieval of columnar data. In this format, data is stored in blocks, which is an advantage on distributed file systems and for import operations that include append or overwrites at the block level.	
+ CSV files are used to showcase multi-file import operations.
+ Several other data formats are used for platform-specific lessons.

Most of the built-in data sets are small enough to fit in-memory. Larger data sets containing the full airline, census, and mortgage default data sets are available for download [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). 

<a name="demo-sql-data"></a>
## How to load sample data into SQL Server

You can easily upload any CSV file into SQL Server if you want to step through demos or example script in the documentation. Use SQL Server Management Studio to import the data. You must have write access on the server. The following exercise creates two databases: RevoClaimsDB based on claims.txt, and RevoAirlineDB based on AirlineDemoSmall.csv.

1. In Object Explorer, create a new database using default values. Name the databases RevoClaimsDB and RevoAirlineDB.
2. Right-click **RevoClaimsDB > Tasks > Import Data**.
3. In Choose a data source, select **Flat File Source**. 
4. Navigate to the sample data directory. By default, it is C:\Program Files\Microsoft\R Server\R_SERVER\Library\RevoScaleR\SampleData. Select the file.
5. Accept the defaults and click **Next**.
4. In Choose a destination, scroll down and select **SQL Server Native Client 11.0**. Accept the defaults.
5. Click **Next** and then click **Finish** to create the database.
6. Repeat for the airline demo data and any other data sets you want to upload.

## See Also

 [Introduction to Microsoft R](microsoft-r-getting-started.md)	
 [Diving into data analysis in Microsoft R](data-analysis-in-microsoft-r.md)	
 [RevoScaleR Functions](/scaler/scaler.md)	
