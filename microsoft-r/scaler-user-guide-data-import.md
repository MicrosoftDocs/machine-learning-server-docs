---

# required metadata
title: "Import data into Microsoft R Server using rxImport"
description: "Load data in Microsoft R Server using the RevoScaleR rxImport function."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/22/2017"
ms.topic: "article"
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

# Importing data in Microsoft R Server

To load data for analysis and visualization in Microsoft R, you can use a *data frame* or an .xdf file to provide the data set. A RevoScaleR function called **rxImport** is used to load the data.

A data frame is the fundmantal data structure in R and is fully supported in R Server. A data frame is a tabular data structure composed of rows and columns. Columns contain variables and the first row, called the *header*, stores column names. All subsequent rows provide data values for each variable associated with a single observation. In Microsoft R, a data frame is a temporary data structure created when you connect to an external data source and load some data. It exists only for the duration of the session.

An .xdf file is a binary file format native to Microsoft R, used for persisting data on disk. XDF files can store data in multiple physical files to accomodate very large data sets. Similar to data frames, storage is column-based, one column per variable, which is highly efficient for targeted read-write operations on individual variables. Additionally, .xdf files include precomputed metadata, which is immediately available with no additional processing.

> [!Note]
> To take full advantage of XDF, you need R Server (as opposed to R Client). Reading and writing chunked data on disk is exclusive to R Server.

## About rxImport

RevoScaleR can use data from a wide range of external data sources, including text (.csv) files, database files on disk (SPSS and SAS), and relational data sources. 

To convert external data into a format understood by RevoScaleR, use the **rxImport** function. Although the function takes several arguments, it's essentially just loading data. In the simplest case, you can provide a path to an input file. If your text format data set is delimited by commas or tabs, this is all that is required for simple data conversion. The following example creates a data object loaded with data from a text delimited file:

		> mydataobject <-rxImport("C:/user/temp/mydatafile.csv")

Depending on arguments, **rxImport** either loads data as a data frame, or outputs the data to an .xdf file saved to disk. This article covers a range of data access scenarios for multiple data source types. For more information about .xdf administration, see [XDF files](rserver-xdf.md).

## How to import a text file

1. Set the location of the source file. One approach is to use R's `file.path` command. 

		> mySourceFile <- file.path("C:/Users/Temp/my-data-file.txt")

  To try this out using [built-in samples](scaler-user-guide-sample-data.md), run the first command to verify the files are available, and the second command to set the location and source file.

		# Verify the sample files exist and then set mySourceFile to the sample directory
		> list.files(rxGetOption("sampleDataDir"))
		> mySourceFile <- file.path(rxGetOption("sampleDataDir"), "claims.txt")

2. Run **rxImport** to load the data into a data frame.

		> claimsDF <- rxImport(mySourceFile)

3. Follow up with the **rxGetInfo** function to get summary information. If we include the argument *getVarInfo=TRUE*, the summary includes the names of the variables and their types:
	
		> rxGetInfo(claimsDF, getVarInfo = TRUE)
	
		Data frame: claimsDF  
		Number of observations: 128 
		Number of variables: 6 
		Number of blocks: 1 
		Variable information: 
		Var 1: RowNum, Type: integer, Low/High: (1, 128)
		Var 2: age, Type: character
		Var 3: car.age, Type: character
		Var 4: type, Type: character
		Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
		Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)

  For just variables in the data file, use the *names* function:

		> names(claimsDF)
	      [1] "RowNum"  "age"     "car.age" "type"    "cost"    "number"

5. To save the data into an .xdf file rather than importing data into a data frame in memory, add the *outFile* parameter. Specify a path to a writeable directory:  

		> claimsDF <- rxImport(inData=mySourceFile, outFile = "c:/users/temp/claims.xdf")

  An .xdf file is created, and instead of returning a data frame, the **rxImport** function returns a data source object. This is a small R object that contains information about a data source, in this case the name and path of the .xdf file it represents. This data source object can be used as the input data in most RevoScaleR functions. For example:

		> rxGetInfo(claimsDF, getVarInfo = TRUE)
		> names(claimsDF)

6. Optionally, you can simplify the path designation by using the working directory. Use R's working directory commands to get and set the folder. Run the first command to determine the default working directory, and the second command to switch to a writable folder.

		> getwd()
		> setwd("c:/users/temp")

## Modifications to variables during import

During import, you can fix problems in the underlying data by specifying arguments for replacement values, changing metadata on the variable, changing data types, and creating new variables based on calculations.

### Set a replacement string for missing values

If your text data file uses a string other than NA to identify missing values, you must use the *missingValueString* argument. Only one missing value string is allowed per file. We used this briefly in Chapter 1 when we imported the AirlineDemoSmall data:

	inFile <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.csv")
	airData <- rxImport(inData = inFile, outFile="airExample.xdf", 
		stringsAsFactors = TRUE, missingValueString = "M", 
		rowsPerRead = 200000, overwrite = TRUE)
	
### Change variable metadata during import

If you need to modify the name of a variable or the names of the factor levels, or add a description of a variable, you can do this using the *colInfo* argument. For example, the claims data includes a variable *type* specifying the type of car, but the levels A, B, C, and D give us no particular information. If we knew what the types signified, perhaps “Subcompact”, “Compact”, “Mid-size”, and “Full-size”, we could relabel the levels as follows:

	#  Specifying Additional Variable Information
	
	inFileAddVars <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
	outfileTypeRelabeled <- "claimsTypeRelabeled.xdf"
	colInfoList <- list("type" = list(type = "factor", levels = c("A", 
	    "B", "C", "D"), newLevels=c("Subcompact", "Compact", "Mid-size", 
	    "Full-size"), description="Body Type"))
	claimsNew <- rxImport(inFileAddVars, outFile = outfileTypeRelabeled, 
		colInfo = colInfoList)
	rxGetInfo(claimsNew, getVarInfo = TRUE) 

This produces the following output:

	File name: C:\YourOutputPath\claimsTypeRelabeled.xdf 
	Number of observations: 128 
	Number of variables: 6 
	Number of blocks: 1 
	Compression type: zlib
	Variable information: 
	Var 1: RowNum, Type: integer, Low/High: (1, 128)
	Var 2: age, Type: character
	Var 3: car.age, Type: character
	Var 4: type, Body Type 
	       4 factor levels: Subcompact Compact Mid-size Full-size
	Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
	Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)


To specify *newLevels*, you must also specify *levels*, and it is important to note that the *newLevels* argument can only be used to rename levels—it cannot be used to fully recode the factor. That is, the number of *levels* and number of *newLevels* must be the same.

## Convert date strings into Date objects

The .xdf format can store dates using the standard R **Date** class. When importing data from other data formats that support dates such as SAS or SPSS, the **rxImport** function will convert dates data automatically. However, some data sets even in those formats include dates as character string data. 

You can store such data more efficiently by converting it to *Date* data using the *transforms* argument. For example, suppose you have a character variable *TransactionDate* with a representative date of the form "14 Sep 2017". You could convert this to a *Date* variable using the following *transforms* argument:

	transforms=list(TransactionDate=as.Date(TransactionDate, format="%d %b %Y")))

The *format* argument is a character string that may contain conversion specifications, as in the example shown. These conversion specifications are described in the *strptime* help file.

## Create or modify variables during import

You can use the *transforms* argument to **rxImport** to create new variables or modify existing variables when you initially read the data into .xdf format. For example, we could create a new variable, *logcost*, by taking the log of the existing cost variable as follows:
	
	inFile <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
	outfile <- "claimsXform.xdf"
	claimsDS <-rxImport(inFile, outFile = outfile, transforms=list(logcost=log(cost)))
	rxGetInfo(claimsDS, getVarInfo=TRUE)
	
This gives the following output, showing the new variable:

	File name: C:\YourOutputPath\claimsXform.xdf 
	Number of observations: 128 
	Number of variables: 7 
	Number of blocks: 1 
	Compression type: zlib
	Variable information: 
	Var 1: RowNum, Type: integer, Low/High: (1, 128)
	Var 2: age, Type: character
	Var 3: car.age, Type: character
	Var 4: type, Type: character
	Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
	Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)
	Var 7: logcost, Type: numeric, Low/High: (2.3979, 6.7452)

### Change data types during import

The **rxImport** function supports three arguments for specifying variable data types: *stringsAsFactors*, *colClasses*, and *colInfo*. For example, consider storing character data. Often data stored in text files as string data actually represents categorical or *factor* data, which can be more compactly represented as a set of integers denoting the distinct *levels* of the factor. This is common enough that users frequently want to transform *all* string data to factors. This can be done using the *stringsAsFactors* argument:

	#  Specifying Variable Data Types

	inFile <- file.path(rxGetOption("sampleDataDir"), "claims.sts")
	rxImport(inFile, outFile = "claimsSAF.xdf", stringsAsFactors = TRUE)
	rxGetInfo("claimsSAF.xdf", getVarInfo = TRUE)

		File name: C:\YourOutputPath\claimsSAF.xdf 
		Number of observations: 128 
		Number of variables: 6 
		Number of blocks: 1 
		Variable information: 
		Var 1: RowNum, Type: integer, Low/High: (1, 128)
		Var 2: age
		       8 factor levels: 17-20 21-24 25-29 30-34 35-39 40-49 50-59 60+
		Var 3: car.age
		       4 factor levels: 0-3 4-7 8-9 10+
		Var 4: type
		       4 factor levels: A B C D
		Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
		Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)

You can also specify data types for individual variables using the *colClasses* argument, and even more specific instructions for converting each variable using the *colInfo* argument. Here we use the *colClasses* argument to specify that the variable *number* in the claims data be stored as an integer:

	outfileColClass <- "claimsCCNum.xdf"
	rxImport(infile, outFile = outfileColClass, colClasses=c(number = "integer"))
	rxGetInfo("claimsCCNum.xdf", getVarInfo = TRUE)

		File name: C:\YourOutputPath\claimsCCNum.xdf 
		Number of observations: 128 
		Number of variables: 6 
		Number of blocks: 1 
		Compression type: zlib
		Variable information: 
		Var 1: RowNum, Type: integer, Low/High: (1, 128)
		Var 2: age, Type: character
		Var 3: car.age, Type: character
		Var 4: type, Type: character
		Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
		Var 6: number, Type: integer, Low/High: (0, 434)


We can use the *colInfo* argument to specify the levels of the *car.age* column. This is particularly useful when reading data in chunks, to assure that factors levels are in the desired order.

	outfileCAOrdered <- "claimsCAOrdered.xdf"
	colInfoList <- list("car.age"= list(type = "factor", levels = c("0-3", 
	    "4-7", "8-9", "10+")))
	rxImport(infile, outFile = outfileCAOrdered, colInfo = colInfoList)
	rxGetInfo("claimsCAOrdered.xdf", getVarInfo = TRUE)


This gives the following output:

	File name: C:\YourOutputPath\claimsCAOrdered.xdf 
	Number of observations: 128 
	Number of variables: 6 
	Number of blocks: 1 
	Compression type: zlib
	Variable information: 
	Var 1: RowNum, Type: integer, Low/High: (1, 128)
	Var 2: age, Type: character
	Var 3: car.age
	       4 factor levels: 0-3 4-7 8-9 10+
	Var 4: type, Type: character
	Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
	Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)


These various methods of providing column information can be combined as follows:

	outfileCAOrdered2 <- "claimsCAOrdered2.xdf"
	colInfoList <- list("car.age" = list(type = "factor", levels = c("0-3", 
	    "4-7", "8-9", "10+")))
	claimsOrdered <- rxImport(infile, outfileCAOrdered2, 
		colClasses = c(number = "integer"), 
	    	colInfo = colInfoList, stringsAsFactors = TRUE)
	rxGetInfo(claimsOrdered, getVarInfo = TRUE) 

This produces the following output:

	File name: C:\YourOutputPath\claimsCAOrdered2.xdf 
	Number of observations: 128 
	Number of variables: 6 
	Number of blocks: 1 
	Compression type: zlib
	Variable information: 
	Var 1: RowNum, Type: integer, Low/High: (1, 128)
	Var 2: age
	       8 factor levels: 17-20 21-24 25-29 30-34 35-39 40-49 50-59 60+
	Var 3: car.age
	       4 factor levels: 0-3 4-7 8-9 10+
	Var 4: type
	       4 factor levels: A B C D
	Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
	Var 6: number, Type: integer, Low/High: (0, 434)

In general, variable specifications provided by the *colInfo* argument are used in preference to *colClasses*, and those in *colClasses* are used in preference to the *stringsAsFactors* argument.

Also note that the .xdf data format supports a wider variety of data types than R, allowing for efficient storage. For example, by default floating point variables are stored as 32-bit floats in .xdf files. When they are read into R for processing, they are converted to doubles (64-bit floats).

## Example: Fixed-Format Data Files

*Fixed-format* data is text data in which each variable occupies a fixed-width column in the input data file. Column width, rather than a delimiter, gives the data its structure. You can import fixed-format data using the *rxImport* function.

Optionally, fixed-format data might be associated with a *schema file* having an .sts extension. The schema describes the width and type of each column. For complete details on creating a schema file, see page 93 of the Stat/Transfer PDF Manual (<http://www.stattransfer.com/stman10.pdf>). If you have a schema file, you can create the input data source very simply by specifying the schema file name as the input data file. 

The [built-in samples](scaler-user-guide-sample-data.md) include a fixed-format version of the claims data as the file claims.dat and a schema file named claims.sts. To import the data using this schema file, we use *RxImport* as follows:

	# Verify the sample files exist
	> list.files(rxGetOption("sampleDataDir"))

	# (Windows only) Set a working directory for which you have write access
	> setwd("c:/users/temp")

	# Specify the source schema file, load data, and save output as a new XDF
	> inFile <- file.path(rxGetOption("sampleDataDir"), "claims.sts")
	> claimsFF <- rxImport(inData=inFile, outFile="claimsFF.xdf")

Return summary infomration using *rxGetInfo*:

	rxGetInfo(claimsFF, getVarInfo=TRUE)

	File name: C:\\YourOutputPath\\claims.xdf

		File name: C:\YourOutputPath\claims.xdf 
		Number of observations: 128 
		Number of variables: 6 
		Number of blocks: 1 
		Compression type: zlib
		Variable information: 
		Var 1: RowNum, Type: numeric, Storage: float32, Low/High: (1.0000, 128.0000)
		Var 2: age, Type: character
		Var 3: car.age, Type: character
		Var 4: type, Type: character
		Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
		Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)

To read all string data as factors, set the *stringsAsFactors* argument to *TRUE* in your call to *rxImport*

	claimsFF2 <- rxImport(inFile, outFile = "claimsFF2.xdf", stringsAsFactors=TRUE)
	rxGetInfo(claimsFF2, getVarInfo=TRUE)

If you have fixed-format data without a schema file, you can specify the start, width, and type information for each variable using the *colInfo* argument to the *RxTextData* data source constructor, and then read in the data using *rxImport*:

	inFileNS <- file.path(readPath, "claims.dat")
	outFileNS <- "claimsNS.xdf"	
	colInfo=list("rownum" = list(start = 1, width = 3, type = "integer"),
	             "age" = list(start = 4, width = 5, type = "factor"),
	             "car.age" = list(start = 9, width = 3, type = "factor"),
	             "type" = list(start = 12, width = 1, type = "factor"),
	             "cost" = list(start = 13, width = 6, type = "numeric"),
	             "number" = list(start = 19, width = 3, type = "numeric"))
	claimsNS <- rxImport(inFileNS, outFile = outFileNS, colInfo = colInfo)
	rxGetInfo(claimsNS, getVarInfo=TRUE)

If you have a schema file, you can still use the *colInfo* argument to specify type information or factor level information. However, in this case, all the variables will be read according to the schema file, and then the *colInfo* data specifications will be applied. This means that you cannot use your *colInfo* list to omit variables, as you can when there is no schema file.

Fixed-width character data is treated as a special type by RevoScaleR for efficiency purposes. You can use this same type for character data in delimited data by specifying a colInfo argument with a width argument for the character column. (Typically, you will need to find the longest string in the column and specify a width sufficient to include it.)

## Example: SAS Data 

The **rxImport** function can also be used to read data from SAS files having a .sas7bdat or .sd7 extension. You do not need to have SAS installed on your computer; simple file access is used to read in the data.

The sample directory contains a SAS version of the claims data as claims.sas7bdat. We can read it into .xdf format most simply as follows:
	
	inFileSAS <- file.path(rxGetOption("sampleDataDir"), "claims.sas7bdat")
	xdfFileSAS <- "claimsSAS.xdf"
	claimsSAS <- rxImport(inData = inFileSAS, outFile = xdfFileSAS)
	rxGetInfo(claimsSAS, getVarInfo=TRUE)

This gives the following output:

	File name: C:\YourOutputPath\claimsSAS.xdf 
	Number of observations: 128 
	Number of variables: 6 
	Number of blocks: 1 
	Compression type: zlib
	Variable information: 
	Var 1: RowNum, Type: character
	Var 2: age, Type: character
	Var 3: car_age, Type: character
	Var 4: type, Type: character
	Var 5: cost, Type: numeric, Low/High: (11.0000, 850.0000)
	Var 6: number, Type: numeric, Low/High: (0.0000, 434.0000)

Sometimes, SAS data files on Windows come in two pieces, a *.sas7bdat* file containing the data and a *.sas7bcat* file containing value label information. You can read both the data and the value label information by specifying the *.sas7bdat* file with the *inData* argument and the *.sas7bcat* file with the *formatFile* argument. To do so, in the following code replace *myfile* with your SAS file name:

	myData <- rxImport(inData = "myfile.sas7bdat",
	     outFile ="myfile.xdf",
	     formatFile = "myfile.sas7bcat")


## Example: SPSS Data 

The **rxImport** function can also be used to read data from SPSS files. The sample directory contains an SPSS version of the claims data as claims.sav. We can read it into .xdf format as follows:
	
	inFileSpss <- file.path(rxGetOption("sampleDataDir"), "claims.sav")
	xdfFileSpss <- "claimsSpss.xdf"
	claimsSpss <- rxImport(inData = inFileSpss, outFile = xdfFileSpss)
	rxGetInfo(claimsSpss, getVarInfo=TRUE)


This gives the following output:

	File name: C:\YourOutputPath\claimsSpss.xdf 
	Number of observations: 128 
	Number of variables: 6 
	Number of blocks: 1
	Compression type: zlib 
	Variable information: 
	Var 1: RowNum, Type: character
	Var 2: age, Type: character
	Var 3: car_age, Type: character
	Var 4: type, Type: character
	Var 5: cost, Type: numeric, Low/High: (11.0000, 850.0000)
	Var 6: number, Type: numeric, Low/High: (0.0000, 434.0000)

Variables in SPSS data sets often contain value labels with important information about the data. SPSS variables with value labels are typically most usefully imported into R as categorical “factor” data; that is, there is a one-to-one mapping between the values in the data set (such as 1, 2, 3) and the labels that apply to them (Gold, Silver, Bronze). 

Interestingly, SPSS allows for value labels on a subset of existing values. For example, a variable with values from 1 to 99 might have value labels of “NOT HOME” for 97, “DIDN’T KNOW” for 98, and “NOT APPLICABLE” for 99. If this variable were converted to a factor in R using the value labels as the factor labels, all of the values from 1 to 96 would be set to missing because there would be no corresponding factor level. Essentially all of the actual data would be thrown away. 

To avoid data loss when converting to factors, use the flag *labelsAsLevels=FALSE*. By default, the information from the value labels is retained even if the variables aren’t converted to factors. This information can be returned using **rxGetVarInfo**. If you don’t wish to retain the information from the value labels you can specify *labelsAsInfo=FALSE*.

### Specify a delimiter for RxTextData data 

You cannot create or modify delimiters through **rxImport**, but for text data, you can create an **RxTextData** data source and specify the delimiter using the *delimiter* argument. For more information about **RxTextData**, see [Data Sources in Microsoft R](scaler-user-guide-data-source.md).

As a simple example, RevoScaleR includes a sample text data file hyphens.txt that is not separated by commas or tabs, but by hyphens, with the following contents:

	Name-Rank-SerialNumber
	Smith-Sgt-02912
	Johnson-Cpl-90210
	Michaels-Pvt-02931
	Brown-Pvt-11311

By creating an **RxTextData** data source for this file, you can specify the delimiter using the *delimiter* argument:

	readPath <- rxGetOption("sampleDataDir")
	infile <- file.path(readPath, "hyphens.txt")
	hyphensTxt <- RxTextData(infile, delimiter="-")
	hyphensDF <- rxImport(hyphensTxt)
	hyphensDF
	
	      Name Rank SerialNumber
	1    Smith  Sgt         2912
	2  Johnson  Cpl        90210
	3 Michaels  Pvt         2931
	4    Brown  Pvt        11311

In normal usage, the *delimiter* argument is a single character, such as *delimiter="\\t"* for tab-delimited data or *delimiter=","* for comma-delimited data. However, each column may be delimited by a different character; all the delimiters must be concatenated together into a single character string. For example, if you have one column delimited by a comma, a second by a plus sign, and a third by a tab, you would use the argument *delimiter=",+\\t". *
	

## Importing wide data

Big data mainly comes in two forms, long or wide, each presenting unique challenges. Most of the topics in the guide deal with long data, or data with many observations compared to the number of variables or columns. With wide data, or data sets with a large number of variables, there are specific considerations to take into account during import.

We highly recommend that you import your wide data into the .xdf format using the *rxImport* function whenever you plan to do repeated analyses on your dataset. For wide data with many variables it is especially convenient to store the data in an .xdf file and then read subsets of columns into a data frame in memory for specific analyses. Please see earlier sections of this chapter for specifics on importing different data formats into .xdf.

Section 6 of this chapter mentions multiple ways in which the user can specify variable data types upon import. For wide data that contain many categorical variables, the time to import will be substantially improved by using the *colInfo* argument to define the variable levels. If an import is done using just the *stringsAsFactors* argument or is done without specifying variable levels using *colInfo*, RSR will go through the data and create new levels on a “first-come, first-served” basis for each factor variable. Each time a new level is encountered the metadata has to be updated. For a wide dataset with many factor variables this takes a considerable amount of time. Using the *colInfo* argument to define the levels first speeds up the import.

For wide data, it is useful to create the *colInfo* as an object prior to using rxImport. The following is an example of defining the colInfo with factor levels for the claims data from an earlier chapter:

	colInfoList <- list("age" = list(type = "factor",
	      levels = c("17-20","21-24","25-29","30-34","35-39","40-49","50-59","60+")),
	    "car.age" = list(type = "factor",
	      levels = c("0-3","4-7","8-9","10+")),
	    "type" = list(type = "factor",
	      levels = c("A","B","C","D")))


The colInfo list can then be used as the *colInfo* argument in the *rxImport* function to get your data into .xdf format:

	inFileClaims <- file.path(rxGetOption("sampleDataDir"),"claims.txt")
	outFileClaims <- "claimsWithColInfo.xdf"
	rxImport(inFile, outFile = outFileClaims, colInfo = colInfoList)
	

## Importing data into multiple XDF files for Hadoop

The .xdf file format has been modified for analyses on Hadoop to store data on HDFS in a composite set of files rather than a single file. The composite set consists of a named directory with two subdirectories, ‘data’ and ‘metadata’, containing split ‘.xdfd’ files and a metadata ‘.xdfm’ files respectively. Data is split into individual ‘.xdfd’ files such that each file remains within a single HDFS block. (The HDFS block size varies from installation to installation but is typically either 64MB or 128MB). The ‘.xdfm’ file contains the metadata for all of the .xdfd files. For more in depth information about the composite XDF format and its use within a Hadoop compute context see the [*RevoScaleR MapReduce Getting Started Guide*](scaler-hadoop-getting-started.md).

In most cases, a single .xdf file will be the most efficient way to store and analyze your data in a local compute context, unless you are using data from HDFS as input .(discussed in the next section). However, you can still read, write and analyze a set of composite XDF files in a local compute context.

*rxImport* is used to read in a .csv file or directory of .csv files into the composite XDF format described above. When the compute context is *RxHadoopMR*, a composite set of XDF is always created. However, while working in a local compute context you must specify the option *createCompositeSet=TRUE* within the *RxXdfData* data source object being used as the *outFile* argument for *rxImport*.

In the following example we demonstrate creating a composite set of .xdf files within the native file system in a local compute context. Using *rxImport*, you specify an *RxTextData* data source, in this case a directory containing the mortDefaultSmall .csv files, as the *inData* and an *RxXdfData* source object as the *outFile* argument. We define our data source objects as follows (assuming you’ve copied the 10 mortDefaultSmall .csv files into a separate directory from the SampleData folder of the RevoScaleR package):

	mortDefaultCsvDir <- “C:/MRS/Data/mortDefaultCsv”
	mortDefaultCsv <- RxTextData(mortDefaultCsvDir)
	mortDefaultCompXdfDir <- “C:/MRS/Data/mortDefaultXdf”
	mortDefaultCompXdf <- RxXdfData(mortDefaultCompXdfDir, createCompositeSet=TRUE)

We then import the data using *rxImport*:

	rxImport(inData=mortDefaultCsv, outFile=mortDefaultCompXdf)

This creates a directory named ‘mortDefaultXdf’ and subdirectories ‘data’ and ‘metadata’ containing the split ‘.xdfd’ files and a metadata ‘.xdfm’ file respectively as shown below.

	list.files(mortDefaultCompXdfDir, recursive=TRUE, full.names=TRUE)

		[1] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_1.xdfd"  
		[2] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_10.xdfd" 
		[3] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_2.xdfd"  
		[4] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_3.xdfd"  
		[5] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_4.xdfd"  
		[6] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_5.xdfd"  
		[7] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_6.xdfd"  
		[8] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_7.xdfd"  
		[9] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_8.xdfd"  
		[10] "C:/MRS/Data/mortDefaultXdf/data/mortDefaultXdf_9.xdfd"  
		[11] "C:/MRS/Data/mortDefaultXdf/metadata/mortDefaultXdf.xdfm"

Note that the composite XDF can then be referenced in future analyses using the data source object that was used as the *outFile* for *rxImport*. See the help file for *RxXdfData* for more information regarding how the numbers of blocks in each ‘.xdfd’ file is determined depending on the compute context.

## Consuming data from the Hadoop Distributed File System (HDFS)

If you are using RevoScaleR on a Linux system that is part of a Hadoop cluster, you can store and access text or xdf data files on your system’s native file system or the Hadoop Distributed File System (HDFS). By default, data is expected to be found on the native file system. If all your data is on HDFS, you can use *rxSetFileSystem* to specify this as a global option.. You can then specify your global option as follows:

	rxSetFileSystem(RxHdfsFileSystem())

If only some files are on HDFS, you can use *RxTextData* or *RxXdfData* data sources to specify these as files on HDFS. For example,

	hdfsFS <- RxHdfsFileSystem() 
	txtSource <- RxTextData("/test/HdfsData/AirlineCSV/CSVs/1987.csv", 
	    fileSystem=hdfsFS)
	xdfSource <- RxXdfData("/test/HdfsData/AirlineData1987", 
	    fileSystem=hdfsFS)

The *RxHdfsFileSystem* function is used to create a file system object for the HDFS file system; the *RxNativeFileSystem* function does the same thing for the native file system.

If you are using a local compute context, you can currently use data files on HDFS as input files only; output must be written to the native file system.

As a more complete example, consider again the mortgage default example from *section 2.13*. If the mortgage default composite XDF resides in the HDFS file system, we can run the example as follows:

	rxSetFileSystem(RxHdfsFileSystem())
	mortDS <- RxXdfData("/share/SampleData/mortDefaultXdf")
	rxGetInfo(mortDS, numRows = 5)
	rxSummary(~., data = mortDS, blocksPerRead = 2)
	logitObj <- rxLogit(default~F(year) + creditScore + yearsEmploy + ccDebt,
	               data = mortDS, blocksPerRead = 2,  reportProgress = 1)
	summary(logitObj)

Note that in this example we set the file system to HDFS globally so we did not need to specify the file system within the data source constructors.

>The `blocksPerRead` argument is ignored if run locally using R Client. [Learn more...](scaler-getting-started-data-import-exploration.md#chunking)

## Using RevoScaleR with rhdfs

If you are using both RevoScaleR and the RHadoop connector package rhdfs, you need to ensure that the two do not interfere with each other. The rhdfs package depends upon the rJava package, which will prevent access to HDFS by RevoScaleR if it is called before RevoScaleR makes its connection to HDFS. To prevent this interaction, use the function rxHdfsConnect to establish a connection between RevoScaleR and HDFS. An install-time option on Linux can be used to trigger such a call from the Rprofile.site startup file. If the install-time option is not chosen, you can add it later by setting the REVOHADOOPHOST and REVOHADOOPPORT environment variables with the host name of your Hadoop name node and the name node’s port number, respectively. You can also call rxHdfsConnect interactively within a session, provided you have not yet attempted any other rJava or rhdfs commands. For example, the following call will fix a connection between the Hadoop host sandbox-01 and RevoScaleR; if you make a subsequent call to rhdfs, RevoScaleR can continue to use the previously established connection. Note, however, that once rhdfs (or any other rJava call) has been invoked, you cannot change the host or port you use to connect to RevoScaleR:

	rxHdfsConnect(hostName = "sandbox-01", port = 8020)
