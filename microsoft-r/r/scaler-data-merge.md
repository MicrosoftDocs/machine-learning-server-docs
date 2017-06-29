---

# required metadata
title: "Merge data in RevoScaleR using rxSort (Microsoft R)"
description: "How to merge data in a data frame or XDF file with the RevoScaleR rxMerge function."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "06/06/2017"
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

# How to merge data using rxMerge (Microsoft R)

Merging allows you to combine the information from two data sets into a third data set that can be used for subsequent analysis. One example is merging account information such as account number and billing address with transaction data such as account number and purchase details to create invoices. In this case, the two files are merged on the common information, that is, the account number.

In RevoScaleR, you merge .xdf files and/or data frames with the rxMerge function. This function supports a number of types of merge that are best illustrated by example. The available types are as follows:

-   Inner
-   Outer: left, right, and full
-   One-to-One
-   Union

We describe each of these types in the following sections.

## Inner merge

In the default inner merge type, one or more merge key columns is specified, and only those observations for which the specified key columns match exactly are combined to create new observations in the merged data set.

Suppose we have the following data from a dentist’s office:

	AccountNo 	Billee 	Patient
	     0538	Rich C	    1
	     0538	Rich C 	  	2
	     0538	Rich C 		3
	     0763	 Tom D 	    1
	     1534	Kath P 	   	1

We can create a data frame with this information:

	#  Merging Data
	
	acct <- c(0538, 0538, 0538, 0763, 1534)
	billee <- c("Rich C", "Rich C", "Rich C", "Tom D", "Kath P")
	patient <- c(1, 2, 3, 1, 1)
	acctDF<- data.frame( acct=acct, billee= billee, patient=patient)

Suppose further we have the following information about procedures performed:

	AccountNo	Patient 	Procedure
		 0538		  3 	OffVisit
     	 0538	  	  2 	AdultPro
     	 0538	 	  2 	OffVisit
      	 0538	 	  3 	2SurfCom
     	 0763	  	  1 	OffVisit
     	 0763	   	  1 	AdultPro
		 0763		  2 	OffVisit


This data is put into another data frame:

	acct <- c(0538, 0538, 0538, 0538, 0763, 0763, 0763)
	patient <- c(3, 2, 2, 3, 1, 1, 2)
	type <- c("OffVisit", "AdultPro", "OffVisit", "2SurfCom", "OffVisit", "AdultPro", "OffVisit")
	procedureDF <- data.frame(acct=acct, patient=patient, type=type)

Then we use rxMerge to create an inner merge matching on the columns *acct* and *patient:*

	rxMerge(inData1 = acctDF, inData2 = procedureDF, type = "inner", 
		matchVars=c("acct", "patient"))
	
	   acct billee patient     type
	 1  538 Rich C       2 AdultPro
	 2  538 Rich C       2 OffVisit
	 3  538 Rich C       3 OffVisit
	 4  538 Rich C       3 2SurfCom
	 5  763 Tom D        1 OffVisit
	 6  763 Tom D        1 AdultPro
	 
Because the patient 1 in account 538 and patient 1 in account 1534 had no visits, they are omitted from the merged file. Similarly, patient 2 in account 763 had a visit, but does not have any information in the accounts file, so it too is omitted from the merged data set. Also, note that the two input data files are automatically sorted on the merge keys before merging.

## Outer merge

There are three types of outer merge: left, right, and full. In a left outer merge, all the lines from the first file are present in the merged file, either matched with lines from the second file that match on the key columns, or if no match, filled out with missing values. A right outer merge is similar, except all the lines from the second file are present, either matched with matching lines from the first file or filled out with missings. A full outer merge includes all lines in both files, either matched or filled out with missings. We can use the same dentist data to illustrate the various types of outer merge:
	
	rxMerge(inData1 = acctDF, inData2 = procedureDF, type = "left", 
		matchVars=c("acct", "patient"))
	
	    acct billee patient     type
	  1  538 Rich C       1     <NA>
	  2  538 Rich C       2 AdultPro
	  3  538 Rich C       2 OffVisit
	  4  538 Rich C       3 OffVisit
	  5  538 Rich C       3 2SurfCom
	  6  763  Tom D       1 OffVisit
	  7  763  Tom D       1 AdultPro
	  8 1534 Kath P       1     <NA>
	
	rxMerge(inData1 = acctDF, inData2 = procedureDF, type = "right", 
		matchVars=c("acct", "patient"))
	    acct billee patient     type
	  1  538 Rich C       2 AdultPro
	  2  538 Rich C       2 OffVisit
	  3  538 Rich C       3 OffVisit
	  4  538 Rich C       3 2SurfCom
	  5  763  Tom D       1 OffVisit
	  6  763  Tom D       1 AdultPro
	  7  763   <NA>       2 OffVisit
	
	
	rxMerge(inData1 = acctDF, inData2 = procedureDF, type = "full", 
		matchVars=c("acct", "patient"))
	
	    acct billee patient     type
	  1  538 Rich C       1     <NA>
	  2  538 Rich C       2 AdultPro
	  3  538 Rich C       2 OffVisit
	  4  538 Rich C       3 OffVisit
	  5  538 Rich C       3 2SurfCom
	  6  763  Tom D       1 OffVisit
	  7  763  Tom D       1 AdultPro
	  8  763   <NA>       2 OffVisit
	  9 1534 Kath P       1     <NA>


## One-to-one merge

In the one-to-one merge type, the first observation in the first data set is paired with the first observation in the second data set to create the first observation in the merged data set, the second observation is paired with the second observation to create the second observation in the merged data set, and so on. The data sets must have the same number of rows. It is equivalent to using *append=*"*cols*" in a data step.

For example, suppose our first data set contains three observations as follows:

	1 a x
	2 b y
	3 c z

Create a data frame with this data:

	myData1 <- data.frame( x1 = 1:3, y1 = c("a", "b", "c"), z1 = c("x", "y", "z"))

Suppose our second data set contains three different variables:

	101 d u
	102 e v
	103 f w

Create a data frame with this data:

	myData2 <- data.frame( x2 = 101:103, y2 = c("d", "e", "f"), 
	          z2 = c("u", "v", "w"))

A one-to-one merge of these two data sets combines the columns from the two data sets into one data set:
	
	rxMerge(inData1 = myData1, inData2 = myData2, type = "oneToOne")
	
	  x1 y1 z1  x2 y2 z2
	1  1  a  x 101  d  u
	2  2  b  y 102  e  v
	3  3  c  z 103  f  w

## Union merge

A union merge is simply the concatenation of two files with the same set of variables. It is equivalent to using *append="rows"* in a data step.

Using the example from one-to-one merge, we rename the variables in the second data frame to be the same as in the first:

	names(myData2) \<- c("x1", "x2", "x3")

Then use a union merge:

	rxMerge(inData1 = myData1, inData2 = myData2, type = "union")
	
	   x1 y1 z1
	1   1  a  x
	2   2  b  y
	3   3  c  z
	4 101  d  u
	5 102  e  v
	6 103  f  w


## Using rxMerge with .xdf files

You can use **rxMerge** with a combination of .xdf files or data frames. For example, you specify the two the paths for two input .xdf files as the *inData1* and *inData2* arguments, and the path to an output file as the *outFile* argument. As a simple example, we can stack two copies of the claims data using the union merge type as follows:

	claimsXdf <- file.path(rxGetOption("sampleDataDir"), "claims.xdf")
	
	rxMerge(inData1 = claimsXdf, inData2 = claimsXdf, outFile = "claimsTwice.xdf", 
		type = "union")

A new .xdf file is created containing twice the number of rows of the original claims file.

You can also merge an .xdf file and data frame into a new .xdf file. For example, suppose that you would like to add a variable on state expenditure on education into each observation in the censusWorkers sample .xdf file. First, take a quick look at the state variable in the .xdf file:

	censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
	rxGetVarInfo(censusWorkers, varsToKeep = "state")
	
	
	Var 1: state
	       3 factor levels: Connecticut Indiana Washington


We can create a data frame with per capita educational expenditures for the same three states. (Note that because R alphabetizes factor levels by default, the factor levels in the data frame will be in the same order as those in the .xdf file).

	educExp <- data.frame(state=c("Connecticut", "Washington", "Indiana"),
		EducExp = c(1795.57,1170.46,1289.66 ))

Now use rxMerge, matching by the variable *state:*

	rxMerge(inData1 = censusWorkers, inData2 = educExp, 
		outFile="censusWorkersEd.xdf", matchVars = "state", overwrite=TRUE)

The new .xdf file has an additional variable, EducExp:
	
	rxGetVarInfo("censusWorkersEd.xdf")
	
	  Var 1: age, Age 
	         Type: integer, Low/High: (20, 65)
	  Var 2: incwage, Wage and salary income 
	         Type: integer, Low/High: (0, 354000)
	  Var 3: perwt, Type: integer, Low/High: (2, 168)
	  Var 4: sex, Sex 
	         2 factor levels: Male Female
	  Var 5: wkswork1, Weeks worked last year 
	         Type: integer, Low/High: (21, 52)
	  Var 6: state
	         3 factor levels: Connecticut Indiana Washington
	  Var 7: EducExp, Type: numeric, Low/High: (1170.4600, 1795.5700)


## See Also

 [Introduction to R Server](what-is-microsoft-r-server.md) 
 [Install R Server on Windows](install/r-server-install-windows.md)  
 [Install R Server on Linux](install/r-server-install-linux-server.md)  
 [Install R Server on Hadoop](install/r-server-install-hadoop.md)