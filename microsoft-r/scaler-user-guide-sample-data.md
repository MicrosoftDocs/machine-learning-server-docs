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
	 [3] "AirlineDemoSmall.xdf"       "CensusWorkers.xdf"         
	 [5] "claims.dat"                 "claims.sas7bdat"           
	 [7] "claims.sav"                 "claims.sd7"                
	 [9] "claims.sqlite"              "claims.sts"                
	[11] "claims.txt"                 "claims.xdf"                
	[13] "claimsExtra.txt"            "claimsQuote.txt"           
	[15] "claimsTab.txt"              "CustomerSurvey.xdf"        
	[17] "DJIAdaily.xdf"              "fourthgraders.xdf"         
	[19] "Kyphosis.xdf"               "mortDefaultSmall.xdf"      
	[21] "mortDefaultSmall2000.csv"   "mortDefaultSmall2001.csv"  
	[23] "mortDefaultSmall2002.csv"   "mortDefaultSmall2003.csv"  
	[25] "mortDefaultSmall2004.csv"   "mortDefaultSmall2005.csv"  
	[27] "mortDefaultSmall2006.csv"   "mortDefaultSmall2007.csv"  
	[29] "mortDefaultSmall2008.csv"   "mortDefaultSmall2009.csv" "


Sample data is provided in multiple formats so that you can step through data import scenarios using different data formats and techniques. 

+ XDF is the native file format engineered for fast retrieval of columnar data. In this format, data is stored in blocks, which is an advantage on distributed file systems and for import operations that include append or overwrites at the block level.	
+ CSV files are used to showcase multi-file import operations.
+ Several other data formats are used for platform-specific lessons.

The location of the sample data directory is stored as an option in RevoScaleR, and you can access it with the following command:

	rxGetOption("sampleDataDir")

Most of the built-in data sets are small enough to fit in-memory. Larger data sets containing the full airline, census, and mortgage default data sets are available for download [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). 


## See Also

 [Introduction to Microsoft R](microsoft-r-getting-started.md)	
 [Diving into data analysis in Microsoft R](data-analysis-in-microsoft-r.md)	
 [RevoScaleR Functions](/scaler/scaler.md)	
