---

# required metadata
title: "Data source objects in RevoScaleR"
description: "Learn when and how to create data source objects in R code leveraging the RevoScaleR function libraries."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "06/04/2017"
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

A *data source* in RevoScaleR is an R object representing a data set. It is the return object of **rxImport** for read operations and **rxDataStep** for write operations. Although the data itself may be on disk, a data source is an in-memory object that allows you to treat data from disparate sources in a consistent manner within RevoScaleR. 

Behind the scenes, **rxImport** often creates data sources implicitly to facilitate data import. You can explicitly create data sources for more control over how data is imported, such as setting arguments to control how many rows are read into the object. This article explains how to create a variety of data sources and use them in an analytical context.  

## Data Source Constructors

To create data sources directly, use the constructors listed in the following table:

| **Source Data**                    | **Data Source Constructor** |
|------------------------------------|-----------------------------|
| Text (fixed-format or delimited)   | [RxTextData](r/how-to-revoscaler-data-import.md) |
| SAS                                | [RxSasData](r-reference/revoscaler/rxsasdata.md) |
| SPSS                               | [RxSpssData](r-reference/revoscaler/rxspssdata.md) |
| ODBC Database                      | [RxOdbcData](scaler-data-odbc.md) |
| Teradata Database                  | [RxTeradata](r-reference/revoscaler/rxteradata.md) |
| SQL Server Database                | [RxSqlServerData](scaler-data-sql.md) |
| Spark data: Hive, Parquet and ORC  | [RxSparkData](r-reference/revoscaler/rxsparkdata.md) or **RxHiveData**, **RxParquetData**, **RxOrcData** | 
| .xdf data files                    | [RxXdfData](r/concept-what-is-xdf.md) |

[XDF files](r/concept-what-is-xdf.md) are the out files for **rxImport** read operations, but you also use them as a data source input when loading all or part of an .xdf into a data frame. Using an XDF data source is recommended for repeated analysis of a single data set. It is almost always faster to import data into an .xdf file and run analyses on the .xdf data source than to load data from an original data source.

## When to create a data source

For simple data import, it's not necessary to explicitly create a data source. You can simply specify a file path for a file that **rxImport** can read, and RevoScaleR will read it using the default settings. However, if you need to provide additional options specific to that data source type, you should create a data source using a constructor from the previous list. 

## How to create a data source

You can create a data source the same way you create any object in R, by giving it a name and using a constructor. The first argument of any RevoScaleR data source is the source data:

		# Load sample text file on Linux
		> myTextDS <- RxTextData("/usr/lib64/microsoft-r/3.3/lib64/R/library/RevoScaleR/SampleData/claims.txt")

		# Load sample text file on Windows. Remember to replace Window's \ with R's /
		> myTextDS <- RxTextData("C:/Program Files/Microsoft/R Server/R_SERVER/library/RevoScaleR/SampleData/claims.txt")

As a coding best practice, create a file object first, and pass that to the data source:

		> mySrcObj <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
    	> myTextDS <- RxTextData(mySrcObj)

After the data source object is created, you can return object properties, precomputed metadata, and rows.

		# Return properties
		> myTextDS
			RxTextData Source
			"C:/Program Files/Microsoft/R Server/R_SERVER/library/RevoScaleR/SampleData/claims.txt"
			centuryCutoff: 20
			rowsToSniff: 10000
			rowsToSkip: 0
			defaultReadBufferSize: 10000
			isFixedFormat: FALSE
			useFastRead: TRUE
			fileSystem: 
				fileSystemType: native

		# Return variable metadata
		> rxGetVarInfo(myTextDS)
			Var 1: RowNum, Type: integer
			Var 2: age, Type: character
			Var 3: car.age, Type: character
			Var 4: type, Type: character
			Var 5: cost, Type: numeric, Storage: float32
			Var 6: number, Type: numeric, Storage: float32

		# Return first 10 rows			
		> rxGetInfo(myTextDS, numRows=10)
			File name: C:/Program Files/Microsoft/R Server/R_SERVER/library/RevoScaleR/SampleData/claims.txt 
			Data Source: Text 
			Data (10 rows starting with row 1):
			RowNum   age car.age type cost number
			1       1 17-20     0-3    A  289      8
			2       2 17-20     4-7    A  282      8
			3       3 17-20     8-9    A  133      4
			4       4 17-20     10+    A  160      1
			5       5 17-20     0-3    B  372     10
			6       6 17-20     4-7    B  249     28
			7       7 17-20     8-9    B  288      1
			8       8 17-20     10+    B   11      1
			9       9 17-20     0-3    C  189      9
			10     10 17-20     4-7    C  288     13


## Use standard R methods

A number of standard R methods can be used with RevoScaleR data sources. You might be familiar with **names** for viewing variable names, or **head** for viewing the first few rows:

		> names(myTextDS)
		[1] "RowNum"	"age"	"car.age"	"type"	"cost"	"number"

		> head(myTextDS)
		RowNum	age	car.age	type	cost	number
		1	1		17-20	0-3		A	289		 8
		2	2		17-20	4-7		A	282		 8
		3	3		17-20	8-9		A	133		 4
		4	4		17-20	10+		A	160		 1
		5	5		17-20	0-3		B	372		10
		6	6		17-20	4-7		B	249		28

By loading data into a data frame using **rxImport**, you can use additional functions, such as the **dim** function to return dimensions, and  **colnames** or **dimnames** as an alternative to **RxGetVarInfo** to obtain variable names.

	#  Load data into a data frame
	newDF <- rxImport(inData = myTextDS)
	dim(newDF)
	[1] 128   6  

You can obtain the number of variables using the **length** function:

	length(newDF)
	  [1] 6

View the **last** rows of a data source using the **tail** function:

	tail(claimsDS)
	    RowNum age car.age type cost number
	123    123 60+     8-9    C  227     20
	124    124 60+     10+    C  119      6
	125    125 60+     0-3    D  385     62
	126    126 60+     4-7    D  324     22
	127    127 60+     8-9    D  192      6
	128    128 60+     10+    D  123      6


By adding *outFile* to **rxImport**, you create an XDF data source, which you can return using **summary**:

	newDF <- rxImport(inData = myTextDS, outFile = "claims.xdf", overwrite = TRUE)
	  Rows Read: 128, Total Rows Processed: 128, Total Chunk Time: 0.009 seconds
	summary(newDF)
	  . . .
	  Data: object (RxXdfData Data Source)
	  File name: claims.xdf
	  . . .

For .xdf file data sources, **dimnames** returns only column names. Row names are not provided because .xdf files do not contain row names:

	colnames(newDF)
	  [1] "RowNum"  "age"     "car.age" "type"    "cost"    "number"

	dimnames(newDF)
	  [[1]]
	  NULL
	  [[2]]
	  [1] "RowNum"  "age"     "car.age" "type"    "cost"    "number"


## Data source by compute context

In the local compute context, all of RevoScaleR’s supported data sources are available to you. In a distributed context, the data source object aligns to the compute context. Thus, **RxInSqlServer** only supports **RxSqlServerData** objects. Likewise for **RxInTeradata**, which supports only the **RxTeradata** data sources. For more information, see [Compute context](r/concept-what-is-compute-context.md).

|                                |            |          | Compute Context |               |              |
|--------------------------------|------------|----------|-----------------|---------------|--------------|
| Data Source                    | RxLocalSeq | RxSpark  | RxHadoopMR      | RxInSqlServer | RxInTeradata |
| Delimited Text (RxTextData)    | x          | x        | x               |               |              |
| Fixed-Format Text (RxTextData) | x          |          |                 |               |              |
| .xdf data files (RxXdfData)    | x          | x        | x               |               |              |
| SAS data files (RxSasData)     | x          |          |                 |               |              |
| SPSS data files (RxSpssData)   | x          |          |                 |               |              |
| ODBC data (RxOdbcData)         | x          |          |                 |               |              |
| SQL Server database (RxSqlServerData) | x   |          |                 | x             |              |
| Teradata database (RxTeradata) | x          |          |                 |               |  x           |
| Spark data [RxSparkData](r-reference/revoscaler/rxsparkdata.md) | x | x   |  |               |              |


## Examples

The following examples show how to instantiate and use various data sources.

### RxSasData

Sample data includes claims.sas7bdat, which you can load without having SAS installed.
	
	inFileSAS <- file.path(rxGetOption("sampleDataDir"), "claims.sas7bdat") 
	sourceDataSAS <- RxSasData(inFileSAS, stringsAsFactors=TRUE)

Retrieve variables in the data by calling R's **names** function:

	names(sourceDataSAS)
	  [1] "RowNum"  "age"     "car_age" "type"    "cost"    "number"

Compute a regression, passing the data source as the data argument to **rxLinMod**:

	rxLinMod(cost ~ age + car_age, data = sourceDataSAS)

	  Rows Read: 128, Total Rows Processed: 128, Total Chunk Time: 0.003 seconds 
	  Computation time: 0.014 seconds.
	  Call:
	  rxLinMod(formula = cost ~ age + car_age, data = sourceDataSAS)
	
	  Linear Regression Results for: cost ~ age + car_age
	  Data: sourceDataSAS (RxSasData Data Source)
	  File name:
	      C:/Program Files/Microsoft/R Server/R_SERVER/library/RevoScaleR/SampleData/claims.sas7bdat
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

### RxSpssData

Similarly, you could use the SPSS version of the claims data as follows:

	inFileSpss <- file.path(rxGetOption("sampleDataDir"), "claims.sav") 
	sourceDataSpss <- RxSpssData(inFileSpss, stringsAsFactors=TRUE)
	rxLinMod(cost ~ age + car_age, data=sourceDataSpss)

### RxXdfData

This example shows how to create a data source from the built-in claims.xdf data set:

	claimsPath <-  file.path(rxGetOption("sampleDataDir"), "claims.xdf")
	claimsDs <- RxXdfData(claimsPath)

Use the open method **rxOpen** to open the data source:

	rxOpen(claimsDs)

Use the method **rxReadNext** to read the next block of data from the data source:

	claims <- rxReadNext(claimsDs)

Use the **rxClose** method to close the data source:

	rxClose(claimsDs)

### XDF data sources with biglm 

Since data sources for xdf files read data in chunks, it is a good match for the CRAN package **biglm**. The **biglm** package does a linear regression on an initial chunk of data, then updates the results with subsequent chunks. Below is a function that loops through an xdf file object and creates and updates the **biglm** results.

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


To use the function, we first open the data file. For example, we can again use the large airline data set AirOnTime87to12.xdf:

	bigDataDir <- "C:/MRS/Data"
	bigAirData <- file.path(bigDataDir, "AirOnTime87to12/AirOnTime87to12.xdf")
	dataSource <- RxXdfData(bigAirData, 
		varsToKeep = c("DayOfWeek", "DepDelay","ArrDelay"), blocksPerRead = 15)
	rxOpen(dataSource)

Then we will time the computation, doing the regression for all the rows— 148,619,655 if you are using the full data set. Note that it takes several minutes to load the data, even on a very fast machine.

	system.time(bigLmRes <- biglmxdf(dataSource, ArrDelay~DayOfWeek))
	rxClose(dataSource)

We can see the coefficients by looking at a summary of the object returned:

	summary(bigLmRes)
	
	} # End of use of biglm

It is, of course, much faster to compute a linear model using the **rxLinMod** function, but the **biglm** package provides alternative methods of computation.

## Next Steps

Continue on to the following data import articles to learn more about XDF and other data formats:

+ [XDF files](r/concept-what-is-xdf.md)	
+ [Import SQL Server data](scaler-data-sql.md)	
+ [Import text data](r/how-to-revoscaler-data-import.md)
+ [Import and consume data on HDFS](r/how-to-revoscaler-data-hdfs.md)

## See Also
   
 [RevoScaleR Functions](r-reference/revoscaler/revoscaler.md)   
 [Tutorial: data import and exploration](scaler-getting-started-data-import-exploration.md)
 [Tutorial: data manipulation and statistical analysis](scaler-getting-started-data-visualization-analysis.md) 