---

# required metadata
title: "Import text, SAS, and SPSS data into Machine Learning Server using rxImport "
description: "Load data in Machine Learning Server using the RevoScaleR rxImport function."
keywords: 
author: "chuckheinzelman"
ms.author: "charlhe"
manager: "cgronlun"
ms.date: 05/22/2017
ms.topic: "how-to"
ms.service: mlserver

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

# Importing text data in Machine Learning Server

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

RevoScaleR can use data from a wide range of external data sources, including text files, database files on disk (SPSS and SAS), and relational data sources. This article puts the focus on text files: delimited (.csv) and fixed-format, plus database files accessed through simple file reads.

To store text data for analysis and visualization, you can load it into memory as a *data frame* for the duration of your session, or save it to disk as a .xdf file. For either approach, the RevoScaleR **rxImport** function loads the data.

> [!Note]
> To get the best use of persisted data in XDF, you need Machine Learning Server (as opposed to R Client). Reading and writing *chunked data* on disk is exclusive to Machine Learning Server.

## About rxImport

Converting external data into a format understood by RevoScaleR is achieved using the **rxImport** function. Although the function takes several arguments, it's just loading data from a source file that you provide. In the simplest case, you can give it a file path. If the data is delimited by commas or tabs, this is all that is required loading the data. To illustrate, the following example creates a data object loaded with data from a local text-delimited file:

```
> mydataobject <-rxImport("C:/user/temp/mydatafile.csv")
```

Depending on arguments, **rxImport** either loads data as a data frame, or outputs the data to a .xdf file saved to disk. This article covers a range of data access scenarios for text files and file-based data access of SPSS and SAS data. To learn more about other data sources, see related articles in the table of contents or in the link list at the end of this article.

## How to import a text file

1. Set the location of the source file. One approach is to use R's `file.path` command. 

    ```
	> mySourceFile <- file.path("C:/Users/Temp/my-data-file.txt")
    ```

   To try this out using [built-in samples](sample-built-in-data.md), run the first command to verify the files are available, and the second command to set the location and source file.

    ```
	# Verify the sample files exist and then set mySourceFile to the sample directory
	> list.files(rxGetOption("sampleDataDir"))
	> mySourceFile <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
    ```

2. Run **rxImport** to load the data into a data frame.

    ```
	> claimsDF <- rxImport(mySourceFile)
    ```

3. Follow up with the **rxGetInfo** function to get summary information. If we include the argument *getVarInfo=TRUE*, the summary includes the names of the variables and their types:
	
    ```
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
    ```

   For just variables in the data file, use the *names* function:

    ```
	> names(claimsDF)
		[1] "RowNum"  "age"     "car.age" "type"    "cost"    "number"
    ```

4. To save the data into a .xdf file rather than importing data into a data frame in memory, add the *outFile* parameter. Specify a path to a writable directory:  

    ```
	> claimsDF <- rxImport(inData=mySourceFile, outFile = "c:/users/temp/claims.xdf")
    ```

   A .xdf file is created, and instead of returning a data frame, the **rxImport** function returns a data source object. This is a small R object that contains information about a data source, in this case the name and path of the .xdf file it represents. This data source object can be used as the input data in most RevoScaleR functions. For example:

    ```
	> rxGetInfo(claimsDF, getVarInfo = TRUE)
	> names(claimsDF)
    ```

5. Optionally, you can simplify the path designation by using the working directory. Use R's working directory commands to get and set the folder. Run the first command to determine the default working directory, and the second command to switch to a writable folder.

    ```
	> getwd()
	> setwd("c:/users/temp")
    ```

## Modifications during import

During import, you can fix problems in the underlying data by specifying arguments for replacement values, changing metadata on the variable, changing data types, and creating new variables based on calculations.

### Set a replacement string

If your text data file uses a string other than NA to identify missing values, you can use the *missingValueString* argument to define the replacement string. Only one missing value string is allowed per file. The following example is from the [Airline demo tutorial](tutorial-revoscaler-data-import-transform.md):

```
inFile <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.csv")
airData <- rxImport(inData = inFile, outFile="airExample.xdf", 
	stringsAsFactors = TRUE, missingValueString = "M", 
	rowsPerRead = 200000, overwrite = TRUE)
```
	
### Change variable metadata

If you need to modify the name of a variable or the names of the factor levels, or add a description of a variable, you can do this using the *colInfo* argument. For example, the claims data includes a variable *type* specifying the type of car, but the levels A, B, C, and D give us no particular information. If we knew what the types signified, perhaps “Subcompact”, “Compact”, “Mid-size”, and “Full-size”, we could relabel the levels as follows:

```
#  Specifying Additional Variable Information

inFileAddVars <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
outfileTypeRelabeled <- "claimsTypeRelabeled.xdf"
colInfoList <- list("type" = list(type = "factor", levels = c("A", 
	"B", "C", "D"), newLevels=c("Subcompact", "Compact", "Mid-size", 
	"Full-size"), description="Body Type"))
claimsNew <- rxImport(inFileAddVars, outFile = outfileTypeRelabeled, 
	colInfo = colInfoList)
rxGetInfo(claimsNew, getVarInfo = TRUE) 
```

This produces the following output:

```
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
```


To specify *newLevels*, you must also specify *levels*, and it is important to note that the *newLevels* argument can only be used to rename levels. It cannot be used to fully recode the factor. That is, the number of *levels* and number of *newLevels* must be the same.

### Change data types

The **rxImport** function supports three arguments for specifying variable data types: *stringsAsFactors*, *colClasses*, and *colInfo*. For example, consider storing character data. Often data stored in text files as string data actually represents categorical or *factor* data, which can be more compactly represented as a set of integers denoting the distinct *levels* of the factor. This is common enough that users frequently want to transform *all* string data to factors. This can be done using the *stringsAsFactors* argument:

```
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
```

You can also specify data types for individual variables using the *colClasses* argument, and even more specific instructions for converting each variable using the *colInfo* argument. Here we use the *colClasses* argument to specify that the variable *number* in the claims data be stored as an integer:

```
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
```


We can use the *colInfo* argument to specify the levels of the *car.age* column. This is useful when reading data in chunks, to assure that factors levels are in the desired order.

```
outfileCAOrdered <- "claimsCAOrdered.xdf"
colInfoList <- list("car.age"= list(type = "factor", levels = c("0-3", 
	"4-7", "8-9", "10+")))
rxImport(infile, outFile = outfileCAOrdered, colInfo = colInfoList)
rxGetInfo("claimsCAOrdered.xdf", getVarInfo = TRUE)
```


Gives the following output:

```
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
```


These various methods of providing column information can be combined as follows:

```
outfileCAOrdered2 <- "claimsCAOrdered2.xdf"
colInfoList <- list("car.age" = list(type = "factor", levels = c("0-3", 
	"4-7", "8-9", "10+")))
claimsOrdered <- rxImport(infile, outfileCAOrdered2, 
	colClasses = c(number = "integer"), 
		colInfo = colInfoList, stringsAsFactors = TRUE)
rxGetInfo(claimsOrdered, getVarInfo = TRUE) 
```

Produces the following output:

```
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
```

In general, variable specifications provided by the *colInfo* argument are used in preference to *colClasses*, and those in *colClasses* are used in preference to the *stringsAsFactors* argument.

Also note that the .xdf data format supports a wider variety of data types than R, allowing for efficient storage. For example, by default floating point variables are stored as 32-bit floats in .xdf files. When they are read into R for processing, they are converted to doubles (64-bit floats).

### Create or modify variables

You can use the *transforms* argument to **rxImport** to create new variables or modify existing variables when you initially read the data into .xdf format. For example, we could create a new variable, *logcost*, by taking the log of the existing cost variable as follows:
	
```
inFile <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
outfile <- "claimsXform.xdf"
claimsDS <-rxImport(inFile, outFile = outfile, transforms=list(logcost=log(cost)))
rxGetInfo(claimsDS, getVarInfo=TRUE)
```
	
Gives the following output, showing the new variable:

```
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
```

### Change date formats

The .xdf format can store dates using the standard R **Date** class. When importing data from other data formats that support dates such as SAS or SPSS, the **rxImport** function converts dates data automatically. However, some data sets even in those formats include dates as character string data. 

You can store such data more efficiently by converting it to *Date* data using the *transforms* argument. For example, suppose you have a character variable *TransactionDate* with a representative date of the form "14 Sep 2017". You could convert this to a *Date* variable using the following *transforms* argument:

```
transforms=list(TransactionDate=as.Date(TransactionDate, format="%d %b %Y")))
```

The *format* argument is a character string that may contain conversion specifications, as in the example shown. These conversion specifications are described in the *strptime* help file.

### Change a delimiter

You cannot create or modify delimiters through **rxImport**, but for text data, you can create an **RxTextData** data source and specify the delimiter using the *delimiter* argument. For more information about creating a data source object, see [Data Sources in Microsoft R](how-to-revoscaler-data-source.md).

As a simple example, RevoScaleR includes a sample text data file hyphens.txt that is not separated by commas or tabs, but by hyphens, with the following contents:

```
Name-Rank-SerialNumber
Smith-Sgt-02912
Johnson-Cpl-90210
Michaels-Pvt-02931
Brown-Pvt-11311
```

By creating an **RxTextData** data source for this file, you can specify the delimiter using the *delimiter* argument:

```
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
```

In normal usage, the *delimiter* argument is a single character, such as *delimiter="\\t"* for tab-delimited data or *delimiter=","* for comma-delimited data. However, each column may be delimited by a different character; all the delimiters must be concatenated together into a single character string. For example, if you have one column delimited by a comma, a second by a plus sign, and a third by a tab, you would use the argument *delimiter=",+\\t". *

## Examples

This section provides example script demonstrating additional import tasks. 

### Import multiple files

This example demonstrates an approach for importing multiple text files at once. You can use [sample data](sample-built-in-data.md) for this exercise. It includes mortgage default data for consecutive years, with each year's data in a separate file. In this exercise, you will import all of them to a single XDF by appending one after another, using a combination of base R commands and RevoScaleR functions.

Create a source object for a list of files, obtained using the R `list.files` function with a pattern for selecting specific file names:

```
mySourceFiles <- list.files(rxGetOption("sampleDataDir"), pattern = "mortDefaultSmall\\d*.csv", full.names=TRUE)
```

Create an object for the XDF file at a writable location:

```
myLargeXdf <- file.path("C:/users/temp/mortgagelarge.xdf")
```

To iterate over multiple files, use the R `lapply` function and create a function to call **rxImport** with the **append** argument. Importing multiple files requires the **append** argument on **rxImport** to avoid overwriting existing content from the first iteration. Each new chunk of data is imported as a block. 

```
lapply(mySourceFiles, FUN = function(csv_file) {
	rxImport(inData = csv_file, outFile=myLargeXdf, append = file.exists(myLargeXdf)) } )
```

Partial output from this operation reports out the processing details.

```
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.025 seconds 
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.022 seconds 
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.019 seconds 
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
Rows Read: 10000, Total Rows Processed: 10000, Total Chunk Time: 0.020 seconds 
```

Use **rxGetInfo** to view precomputed metadata. As you would expect, the block count reflects the presence of multiple concatenated data sets.

```
rxGetInfo(myLargeXdf)
```

Results from this command confirm that you have 10 blocks, one for each .csv file. On a distributed file system, you could place these blocks on separate nodes. You could also retrieve or overwrite individual blocks. For more information, see [Import and consume data on HDFS](how-to-revoscaler-data-hdfs.md) and [XDF files](concept-what-is-xdf.md).

```
File name: C:\Users\TEMP\mortgagelarge.xdf 
Number of observations: 1e+05 
Number of variables: 6 
Number of blocks: 10 
Compression type: zlib 
```

### Import fixed-format data

*Fixed-format* data is text data in which each variable occupies a fixed-width column in the input data file. Column width, rather than a delimiter, gives the data its structure. You can import fixed-format data using the *rxImport* function.

Optionally, fixed-format data might be associated with a *schema file* having a .sts extension. The schema describes the width and type of each column. For complete details on creating a schema file, see page 93 of the [Stat/Transfer PDF Manual](https://stattransfer.com/downloads/StatTransferManual.pdf). If you have a schema file, you can create the input data source simply by specifying the schema file name as the input data file. 

The [built-in samples](sample-built-in-data.md) include a fixed-format version of the claims data as the file claims.dat and a schema file named claims.sts. To import the data using this schema file, we use *RxImport* as follows:

```
# Verify the sample files exist
> list.files(rxGetOption("sampleDataDir"))

# (Windows only) Set a working directory for which you have write access
> setwd("c:/users/temp")

# Specify the source schema file, load data, and save output as a new XDF
> inFile <- file.path(rxGetOption("sampleDataDir"), "claims.sts")
> claimsFF <- rxImport(inData=inFile, outFile="claimsFF.xdf")
```

Return summary information using *rxGetInfo*:

```
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
```

To read all string data as factors, set the *stringsAsFactors* argument to *TRUE* in your call to *rxImport*

```
claimsFF2 <- rxImport(inFile, outFile = "claimsFF2.xdf", stringsAsFactors=TRUE)
rxGetInfo(claimsFF2, getVarInfo=TRUE)
```

If you have fixed-format data without a schema file, you can specify the start, width, and type information for each variable using the *colInfo* argument to the *RxTextData* data source constructor, and then read in the data using *rxImport*:

```
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
```

If you have a schema file, you can still use the *colInfo* argument to specify type information or factor level information. However, in this case, all the variables are read according to the schema file, and then the *colInfo* data specifications are applied. This means that you cannot use your *colInfo* list to omit variables, as you can when there is no schema file.

Fixed-width character data is treated as a special type by RevoScaleR for efficiency purposes. You can use this same type for character data in delimited data by specifying a colInfo argument with a width argument for the character column. (Typically, you need to find the longest string in the column and specify a width sufficient to include it.)

### Import SAS data 

The **rxImport** function can also be used to read data from SAS files having a .sas7bdat or .sd7 extension. You do not need to have SAS installed on your computer; simple file access is used to read in the data.

The sample directory contains a SAS version of the claims data as claims.sas7bdat. We can read it into .xdf format most simply as follows:
	
```
inFileSAS <- file.path(rxGetOption("sampleDataDir"), "claims.sas7bdat")
xdfFileSAS <- "claimsSAS.xdf"
claimsSAS <- rxImport(inData = inFileSAS, outFile = xdfFileSAS)
rxGetInfo(claimsSAS, getVarInfo=TRUE)
```

Gives the following output:

```
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
```

Sometimes, SAS data files on Windows come in two pieces, a *.sas7bdat* file containing the data and a *.sas7bcat* file containing value label information. You can read both the data and the value label information by specifying the *.sas7bdat* file with the *inData* argument and the *.sas7bcat* file with the *formatFile* argument. To do so, in the following code replace *myfile* with your SAS file name:

```
myData <- rxImport(inData = "myfile.sas7bdat",
		outFile ="myfile.xdf",
		formatFile = "myfile.sas7bcat")
```


### Import SPSS data 

The **rxImport** function can also be used to read data from SPSS files. The sample directory contains an SPSS version of the claims data as claims.sav. We can read it into .xdf format as follows:
	
```
inFileSpss <- file.path(rxGetOption("sampleDataDir"), "claims.sav")
xdfFileSpss <- "claimsSpss.xdf"
claimsSpss <- rxImport(inData = inFileSpss, outFile = xdfFileSpss)
rxGetInfo(claimsSpss, getVarInfo=TRUE)
```


Gives the following output:

```
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
```

Variables in SPSS data sets often contain value labels with important information about the data. SPSS variables with value labels are typically most usefully imported into R as categorical “factor” data; that is, there is a one-to-one mapping between the values in the data set (such as 1, 2, 3) and the labels that apply to them (Gold, Silver, Bronze). 

Interestingly, SPSS allows for value labels on a subset of existing values. For example, a variable with values from 1 to 99 might have value labels of “NOT HOME” for 97, “DIDN’T KNOW” for 98, and “NOT APPLICABLE” for 99. If this variable is converted to a factor in R using the value labels as the factor labels, all of the values from 1 to 96 would be set to missing because there would be no corresponding factor level. Essentially all of the actual data would be thrown away. 

To avoid data loss when converting to factors, use the flag *labelsAsLevels=FALSE*. By default, the information from the value labels is retained even if the variables aren’t converted to factors. This information can be returned using **rxGetVarInfo**. If you don’t wish to retain the information from the value labels, you can specify *labelsAsInfo=FALSE*.	

## Importing wide data

Big data mainly comes in two forms, long or wide, each presenting unique challenges. The common case is long data, having many observations relative to the number of variables in the data set. With wide data, or data sets with a large number of variables, there are specific considerations to take into account during import.

First, we recommend importing wide data into the .xdf format using the **rxImport** function whenever you plan to do repeated analyses on your dat aset. Doing so allows you to read subsets of columns into a data frame in memory for specific analyses. For more information, see [Transform and subset data](how-to-revoscaler-data-transform.md).

Second, review the data set for categorical variables that can be marked as factors. If possible use the *colInfo* argument to define the levels rather than *stringsAsFactors*. Explicitly setting the levels results in faster processing speeds because you avoid recomputations of variable metadata whenever a new level is encountered. For wide data sets having a very large number of variables, the extra processing due to recomputation can be significant so its worth considering the *colInfo* argument as a way to speed up the import.

Third, if you are using *colinfo*, considering creating it as an object prior to using **rxImport**. The following is an example of defining the colInfo with factor levels for the claims data from earlier examples:

```
colInfoList <- list("age" = list(type = "factor",
		levels = c("17-20","21-24","25-29","30-34","35-39","40-49","50-59","60+")),
	"car.age" = list(type = "factor",
		levels = c("0-3","4-7","8-9","10+")),
	"type" = list(type = "factor",
		levels = c("A","B","C","D")))
```

The colInfo list can then be used as the *colInfo* argument in the **rxImport** function to get your data into .xdf format:

```
inFileClaims <- file.path(rxGetOption("sampleDataDir"),"claims.txt")
outFileClaims <- "claimsWithColInfo.xdf"
rxImport(inFile, outFile = outFileClaims, colInfo = colInfoList)
```
	
## Next steps

Continue on to the following data import articles to learn more about XDF, data source objects, and other data formats:

+ [XDF files](concept-what-is-xdf.md)	
+ [Data Sources](how-to-revoscaler-data-source.md)	
+ [Import relational data using ODBC](how-to-revoscaler-data-odbc.md)
+ [Import and consume data on HDFS](how-to-revoscaler-data-hdfs.md)

## See also

 [Tutorial: data import](tutorial-revoscaler-data-import-transform.md)	
 [Tutorial: data manipulation](tutorial-revoscaler-data-model-analysis.md)

