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

To view the files available within the package, execute the following open source R command at an R console command line:

	list.files(system.file("SampleData", package = "RevoScaleR"))

You should see the following list:

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

The location of the sample data directory is stored as an option in RevoScaleR, and you can access it with the following command:

	rxGetOption("sampleDataDir")

To get the list of data files shown above, use the open source R command, list.files:

	list.files(rxGetOption("sampleDataDir"))

Most of the built-in data sets are small enough to fit in-memory. Larger data sets containing the full airline, census, and mortgage default data sets are available for download [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). 


## See Also

 [Introduction to Microsoft R](microsoft-r-getting-started.md)	
 [Diving into data analysis in Microsoft R](data-analysis-in-microsoft-r.md)	
 [RevoScaleR Functions](/scaler/scaler.md)	
