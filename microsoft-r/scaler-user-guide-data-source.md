---

# required metadata
title: "Data source objects in RevoScaleR"
description: "Learn when and how to create data source objects in R code leveraging the RevoScaleR function libraries."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/12/2017"
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

# Data Sources

A *data source* in RevoScaleR is an R object representing a data set and constitute the return objects of rxImport and rxDataStep read and write operations. Although the data itself may be on disk, a data source is an in-memory object that allows you to treat data from disparate sources in a consistent manner within RevoScaleR. 

Behind the scenes, rxImport often creates data sources implicitly to facilitate data import. You can create explicit data sources to get more control over how data is imported. This article explains how to create a variety of data sources and use them in an analytical context.  

## Data Source Constructors

To create data sources directly, use the constructors listed in the following table:

| **Source Data**                    | **Data Source Constructor** | **More info** |
|------------------------------------|-----------------------------|---------------|
| Text (fixed-format or delimited)   | RxTextData                  |               |
| SAS                                | RxSasData                   |               |
| SPSS                               | RxSpssData                  |               |
| ODBC Database                      | RxOdbcData                  | [View](scaler-odbc.md) |
| Teradata Database                  | RxTeradata                  |               |
| SQL Server Database                | RxSqlServerData             |               |
| .xdf data files                    | RxXdfData                   | [View](rserver-create-xdf.md) |

## When to create a data source

For simple data import, you do not need to create a data source. You can simply specify a file path of a file that rxImport can read and RevoScaleR will read it using the default settings. However, if you need to provide additional options specific to that data source type, you should create a data source using a constructor from the previous list. 

XDF data sources are the native data file format for R Client and R Server, created through rxImport, populated with data from an external data source, and then saved to disk on an R Client or R Server machine. Using an XDF data source is recommended when you need to perform repeated analysis of a single data set. It will almost always be faster for you to import the data into the .xdf format and run your analyses on the .xdf data source.

## Compute Contexts and Data Sources

In the local compute context, all of RevoScaleR’s supported data sources are available to you. In a distributed context, however your choice of data sources may be severely limited. The most extreme case is the RxInTeradata compute context, which supports only the RxTeradata data source – this makes sense, as the computations are being performed on data inside the Teradata database. Please refer to the table below to see which data sources are available for each compute context (x indicates available).

|                                |            | Compute Context |              |
|--------------------------------|------------|-----------------|--------------|
| Data Source                    | RxLocalSeq | RxHadoopMR      | RxInTeradata |
| Delimited Text (RxTextData)    | x          | x               |              |
| Fixed-Format Text (RxTextData) | x          |                 |              |
| .xdf data files (RxXdfData)    | x          | x               |              |
| SAS data files (RxSasData)     | x          |                 |              |
| SPSS data files (RxSpssData)   | x          |                 |              |
| ODBC data (RxOdbcData)         | x          |                 |              |
| Teradata database (RxTeradata) | x          |                 | x            |

Beyond the compute context, there may also be differences in availability within a single data source type depending on the file system. For example, the composite .xdf files that we discussed in section 2.13 created on the Hadoop File System are somewhat different from .xdf created in a non-distributed file system. Similarly, as discussed in Section 2.12, you may need to split and distribute your data across the available nodes of your cluster.

## Methods for Looking at Data Sources

A number of standard R methods for looking at data have been provided for RevoScaleR data sources. We’ve already seen the use of *names* to view the variable names and *head* to view the first few rows. Returning to the *claimsDS* data source created in the previous section Importing Delimited Text Data, we can obtain the dimensions of our claims data using the *dim* function:

	#  Data Sources
	
	readPath <- rxGetOption("sampleDataDir")
	infile <- file.path(readPath, "claims.txt")
	claimsDS <- rxImport(inData = infile, outFile = "claims.xdf", 
		overwrite = TRUE)
	dim(claimsDS)
	[1] 128   6  


Similarly, an alternative method of obtaining the variable names is to use the *colnames* or *dimnames* functions, but notice that for .xdf file data sources, *dimnames* returns only column names; row names are not provided, because .xdf files do not contain row names:

	colnames(claimsDS)

	  [1] "RowNum"  "age"     "car.age" "type"    "cost"    "number"

	dimnames(claimsDS)

	  [[1]]
	  NULL
	
	  [[2]]
	  [1] "RowNum"  "age"     "car.age" "type"    "cost"    "number"

You can obtain the number of variables using the *length* function:

	length(claimsDS)

	  [1] 6


If you want to see just the top few rows of data, you can use the *head* function:

	head(claimsDS)
	  RowNum   age car.age type cost number  
	1      1 17-20     0-3    A  289      8 
	2      2 17-20     4-7    A  282      8 
	3      3 17-20     8-9    A  133      4 
	4      4 17-20     10+    A  160      1 
	5      5 17-20     0-3    B  372     10 
	6      6 17-20     4-7    B  249     28 

You can view the *last* rows of a data source using the *tail* function:

	tail(claimsDS)
	    RowNum age car.age type cost number
	123    123 60+     8-9    C  227     20
	124    124 60+     10+    C  119      6
	125    125 60+     0-3    D  385     62
	126    126 60+     4-7    D  324     22
	127    127 60+     8-9    D  192      6
	128    128 60+     10+    D  123      6

## Text file imports and specifying Delimiters

As a simple example, RevoScaleR includes a sample text data file hyphens.txt that is not separated by commas or tabs, but by hyphens, with the following contents:

	Name-Rank-SerialNumber
	Smith-Sgt-02912
	Johnson-Cpl-90210
	Michaels-Pvt-02931
	Brown-Pvt-11311

The *rxImport* function does not include a parameter for specifying a delimiter; in this case, you must create an *RxTextData* data source and specify the delimiter using the *delimiter* argument:

	#  Chapter 3: Data Sources
	Ch3Start <- Sys.time()
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

## Using Data Sources

Suppose, for example, that you would like to perform a linear regression on the data in the SAS file claims.sas7bdat. You would first create an RxSasData data source as follows:
	
	inFileSAS <- file.path(rxGetOption("sampleDataDir"), "claims.sas7bdat") 
	sourceDataSAS <- RxSasData(inFileSAS, stringsAsFactors=TRUE)


Once we have the data source, we can remind ourselves of the variables in the data by calling the names function:

	names(sourceDataSAS)
	  [1] "RowNum"  "age"     "car_age" "type"    "cost"    "number"

We can now compute our desired regression using our data source as the data argument to *rxLinMod* (more on this method in Chapter 8):

	rxLinMod(cost ~ age + car_age, data = sourceDataSAS)

	  Rows Read: 128, Total Rows Processed: 128, Total Chunk Time: 0.003 seconds 
	  Computation time: 0.014 seconds.
	  Call:
	  rxLinMod(formula = cost ~ age + car_age, data = sourceDataSAS)
	
	  Linear Regression Results for: cost ~ age + car_age
	  Data: sourceDataSAS (RxSasData Data Source)
	  File name:
	      C:/Program Files/Microsoft/MRO-for-RRE/8.0/R-3.2.2/library/RevoScaleR/SampleData/  claims.sas7bdat
	  Dependent variable(s): cost
	  Total independent variables: 13 (Including number dropped: 2)
	  Number of valid observations: 123
	  Number of missing observations: 5 
	 
	  Coefficients:
	                 cost
	  (Intercept) 117.38544
	  age=17-20    88.15174
	  age=21-24    34.15903
	  age=25-29    54.68750
	  age=30-34     2.93750
	  age=35-39   -20.77430
	  age=40-49     1.68750
	  age=50-59    63.12500
	  age=60+       Dropped
	  car_age=0-3 159.30531


Similarly, you could use the SPSS version of the claims data as follows:

	inFileSpss <- file.path(rxGetOption("sampleDataDir"), "claims.sav") 
	sourceDataSpss <- RxSpssData(inFileSpss, stringsAsFactors=TRUE)
	rxLinMod(cost ~ age + car_age, data=sourceDataSpss)

## Working with an Xdf Data Source

To create an .xdf data source for reading, you can use the *RxXdfData* function. For example, the following creates a data source from the built-in claims.xdf data set:

	#  Working with an Xdf Data Source
	  
	claimsPath <-  file.path(rxGetOption("sampleDataDir"), "claims.xdf")
	claimsDs <- RxXdfData(claimsPath)

Use the open method *rxOpen* to open the data source:

	rxOpen(claimsDs)

Use the method *rxReadNext* to read the next block of data from the data source:

	claims <- rxReadNext(claimsDs)

Use the *rxClose* method to close the data source:

	rxClose(claimsDs)

## Using an Xdf Data Source with biglm 

Since data sources for xdf files read data in chunks, it is a good match for the CRAN package *biglm*. The *biglm* package does a linear regression on an initial chunk of data, then updates the results with subsequent chunks. Below is a function that loops through an xdf file object and creates and updates the *biglm* results.

	#  Using an Xdf Data Source with biglm
	  
	if ("biglm" %in% .packages()){
	require(biglm)
	biglmxdf <- function(dataSource, formula)
	{	
		moreData <- TRUE
		df <- rxReadNext(dataSource)
		biglmRes <- biglm(formula, df)	
		while (moreData)
		{
			df <- rxReadNext(dataSource)	
			if (length(df) != 0)
			{
				biglmRes <- update(biglmRes, df)						
			}
			else
	        {
				moreData <- FALSE
	        }
		}							
		return(biglmRes)			
	}


To use the function, we first open the data file. For example, we can again use the large airline data set AirOnTime87to12.xdf (if you have downloaded the data set, modify the first line below to reflect your local path):

	bigDataDir <- "C:/MRS/Data"
	bigAirData <- file.path(bigDataDir, "AirOnTime87to12/AirOnTime87to12.xdf")
	dataSource <- RxXdfData(bigAirData, 
		varsToKeep = c("DayOfWeek", "DepDelay","ArrDelay"), blocksPerRead = 15)
	rxOpen(dataSource)

>The `blocksPerRead` argument is ignored if run locally using R Client. [Learn more...](scaler-getting-started.md#chunking)

Then we will time the computation, doing the regression for all the rows— 148,619,655 if you are using the full data set. (Note that in this case it will take at least 5 minutes, even on a very fast machine.)

	system.time(bigLmRes <- biglmxdf(dataSource, ArrDelay~DayOfWeek))
	rxClose(dataSource)

We can see the coefficients by looking at a summary of the object returned:

	summary(bigLmRes)
	
	} # End of use of biglm

It is, of course, much faster to compute a linear model using the *rxLinMod* function, but the *biglm* package provides alternative methods of computation.

