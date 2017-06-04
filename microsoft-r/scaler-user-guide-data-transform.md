---

# required metadata
title: "Transform and subset data in Microsft R"
description: "Data manipulation with RevoScaleR."
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

# Transform and subset data in Microsft R

RevoScaleR provides a full set of functions for modifying, transforming, and subsetting data stored in memory in a data frame or in an .xdf file on disk:

| Function | Use case|
|----------|-----------|
| **rxDataStep** | Create a subset rows or variables, or create new variables by transforming existing variables. Also used for easy conversion between data in data frames and .xdf files. |
| **rxSetVarInfo** | Change variable information, such as the name or description, in an .xdf file or data frame. |
| **rxSetInfo** | Add or change a data set description. |
| **rxFactors** | Create or modify factors (categorical variables) based on existing variables. |
| **rxSort** | Sort a data set by one or more key variables. |
| **rxMerge** | Merge two data sets by one or more key variables. |


## Create a subset

A common use of **rxDataStep** is to create a new data set with a subset of rows and variables. The following simple example uses a data frame as the input data set. 

The call to **rxDataStep** uses the *rowSelection* argument to select only the rows where the variable *y* is greater than .5, and the *varsToKeep* argument to keeps only the variables *y* and *z*. The *rowSelection* argument is an R expression that evaluates to *TRUE* if the observation should be kept. The *varsToKeep* argument contains a list of variable names to read in from the original data set. Because no *outFile* is specified, a data frame is returned.
	
	# Create a data frame
	set.seed(59)
	myData <- data.frame(
	x = rnorm(100), 
	y = runif(100), 
	z = rep(1:20, times = 5))
	
	# Subset observations and variables
	myNewData <- rxDataStep( inData = myData,
		rowSelection = y > .5,
		varsToKeep = c("y", "z"))
	
	# Get information about the new data frame
	rxGetInfo(data = myNewData, getVarInfo = TRUE)
	
	Data frame: myNewData 
	Number of observations: 52 
	Number of variables: 2 
	Variable information: 
	Var 1: y, Type: numeric, Low/High: (0.5516, 0.9941)
	Var 2: z, Type: integer, Low/High: (1, 20)


Subsetting is particularly useful if your original data set contains millions of rows or hundreds of thousands of variables. As a smaller example, the CensusWorkers.xdf sample data file has six variables and 351,121 rows. 

To create a subset containing only workers who have worked less than 30 weeks in the year and five variables, we can again use **rxDataStep** with the *rowSelection* and *varsToKeep* arguments. Since the resulting data set will clearly fit in memory, we omit the *outFile* argument and assign the result of the data step, then use **rxGetInfo** to see our results as usual:

	readPath <- rxGetOption("sampleDataDir")
	censusWorkers <- file.path(readPath, "CensusWorkers.xdf")
	partWorkers <- rxDataStep(inData = censusWorkers, rowSelection = wkswork1 < 30,
	              varsToKeep = c("age", "sex", "wkswork1", "incwage", "perwt"))
	rxGetInfo(partWorkers, getVarInfo = TRUE)

The result is a data set with 14,317 rows:

	Data frame: partWorkers 
	Number of observations: 14317 
	Number of variables: 5 
	Variable information: 
	Var 1: age, Age 
	       Type: integer, Low/High: (20, 65)
	Var 2: sex, Sex 
	       2 factor levels: Male Female
	Var 3: wkswork1, Weeks worked last year 
	       Type: integer, Low/High: (21, 29)
	Var 4: incwage, Wage and salary income 
	       Type: integer, Low/High: (0, 354000)
	Var 5: perwt, Type: integer, Low/High: (2, 163)

Alternatively, and in this case more easily, you can use *varsToDrop* to prevent variables from being read in from the original data set. This time we’ll specify an *outFile*; use the *overwrite* argument to allow the output file to be replaced.

	partWorkersDS <- rxDataStep(inData = censusWorkers, 
			outFile = "partWorkers.xdf", 
	        rowSelection = wkswork1 < 30,
	        varsToDrop = c("state"), overwrite = TRUE)

As noted above, if you omit the *outFile* argument to **rxDataStep**, then the results will be returned in a data frame in memory. This is true whether or not the input data is a data frame or an .xdf file (assuming the resulting data is small enough to reside in memory). If an *outFile* is specified, a data source object representing the new .xdf file is returned, which can be used in subsequent RevoScaleR function calls.

	rxGetVarInfo(partWorkersDS)

## Transform Data with rxDataStep

A crucial step in many analyses is transforming the data into the form best suited for the chosen analysis. For example, variations in scale between variables can sometimes make it convenient to take a log or a power of the original variable before fitting a linear model. In RevoScaleR, transforming data is an important issue because we are typically reading and processing data from disk, so we want to be as efficient as possible, minimizing the passes through the data. 

To provide maximum flexibility, RevoScaleR allows you to perform data transformations in virtually all of its functions, from **rxImport** to **rxDataStep**, as well as the analysis functions **rxSummary**, **rxLinMod**, **rxLogit**, **rxGlm**,**rxCrossTabs**, **rxCube**, **rxCovCor**, and **rxKmeans**. In all cases, the basic approach for data transforms are the same.

The heart of the RevoScaleR data step is a list of *transforms*, each of which specifies an R expression to be evaluated and typically is an assignment either creating a new variable or modifying an existing variable from the original data set.

### Create or modify a variable

To create or modify variables, we use the *transforms* argument to **rxDataStep**. The *transforms* argument is specified as a list of expressions. Any R expression that operates row-by-row (that is, the computed value of the new variable for an observation is only dependent on values of other variables for that observation) can be used. 

Start with a simple data frame containing a small sample of transaction data:
	
	#  Transforming Data with rxDataStep
	
	expData <- data.frame(
	   BuyDate = c("2011/10/1", "2011/10/1", "2011/10/1", 
	    "2011/10/2", "2011/10/2", "2011/10/2", "2011/10/2", 
	    "2011/10/3", "2011/10/4", "2011/10/4"),
	   Food =      c( 32, 102,  34,  5,   0, 175,  15, 76, 23, 14),
	   Wine =      c( 0,  212,   0,  0, 425,  22,   0, 12,  0, 56),
	   Garden =    c( 0,   46,   0,  0,   0,  45, 223,  0,  0, 0),
	   House = c( 22,  72,  56,  3,   0,  0,    0, 37, 48, 23),
	   Sex =   factor(c("F", "F", "M", "M", "M", "F", "F", "F", "M", "F")),
	   Age =          c( 20,  51,  32,  16,  61, 42,  35, 99, 29, 55),
	   stringsAsFactors = FALSE)
	
Apply a series of data transformations:

-   Compute the total expenditures for each store visit
-   Compute the average category expenditure for each store visit
-   Set the variable *Age* to missing if the value is 99
-   Create a new logical variable *UnderAge* if the purchaser is under 21
-   Find the day of week the purchase was made using R’s *as.POSIXlt* function, then convert it to a factor. [Note that when creating factors in a transform, you must specify the levels or you may get unpredictable results. Alternatively use *rxFactors*.]
-   Create a factor variable for categories of expenditure levels
-   Create a logical variable for expenditures of $50 or more on either Food or Wine
-   Remove the variable BuyDate from the data set

The following call to **rxDataStep** will accomplish all of the above, returning a new data frame with the transformed variables:

	newExpData = rxDataStep( inData = expData, 
	    transforms = list(
	        Total      = Food + Wine + Garden + House,
	        AveCat     = Total/4,
	        Age        = ifelse(Age == 99, NA, Age),
	        UnderAge   = Age < 21,
	        Day        = (as.POSIXlt(BuyDate))$wday, 
	        Day        = factor(Day, levels = 0:6, 
	                        labels = c("Su","M","Tu", "W", "Th","F", "Sa")),
	        SpendCat   = cut(Total, breaks=c(0, 75, 250, 10000), 
					   labels=c("low", "medium", "high"), right=FALSE),
	        FoodWine = ifelse( Food > 50, TRUE, FALSE),
	        FoodWine = ifelse( Wine > 50, TRUE, FoodWine),
	        BuyDate  = NULL))
	newExpData

The new data frame shows all of the transformed data:

	   Food Wine Garden House Sex Age Total AveCat UnderAge Day SpendCat FoodWine
	1    32    0      0    22   F  20    54  13.50     TRUE  Sa      low    FALSE
	2   102  212     46    72   F  51   432 108.00    FALSE  Sa     high     TRUE
	3    34    0      0    56   M  32    90  22.50    FALSE  Sa   medium    FALSE
	4     5    0      0     3   M  16     8   2.00     TRUE  Su      low    FALSE
	5     0  425      0     0   M  61   425 106.25    FALSE  Su     high     TRUE
	6   175   22     45     0   F  42   242  60.50    FALSE  Su   medium     TRUE
	7    15    0    223     0   F  35   238  59.50    FALSE  Su   medium    FALSE
	8    76   12      0    37   F  NA   125  31.25       NA   M   medium     TRUE
	9    23    0      0    48   M  29    71  17.75    FALSE  Tu      low    FALSE
	10   14   56      0    23   F  55    93  23.25    FALSE  Tu   medium     TRUE


If we had a large data set containing expenditure data in an .xdf file, we could use exactly the same transformation code; the only changes in the call to **rxDataStep** would be the *inData* and the addition of an *outFile* for the newly created data set.

Sometimes it is useful to use computed values as part of a transformation. For example, you might want to impute missing values, replacing any omissions with the variable mean. Let’s look at a simple data frame example; again the same basic code could be used for a huge data set. 

First create a data set with missing values:

	# Create a data frame with missing values
	set.seed(59)
	myData1 <- data.frame(x = rnorm(100), y = runif(100))
	
	xmiss <- seq.int(from = 5, to = 100, by = 5)
	ymiss <- seq.int(from = 2, to = 100, by = 5)
	myData1$x[xmiss] <- NA
	myData1$y[ymiss] <- NA
	rxGetInfo(myData1, numRows = 5)

The call to **rxGetInfo** shows the following:

	Data frame: myData1 
	Number of observations: 100 
	Number of variables: 2 
	Data (5 rows starting with row 1):
	           x          y
	1 -1.8621337 0.06206201
	2  1.1398069         NA
	3  0.3176267 0.84132161
	4  1.3998593 0.26298559
	5         NA 0.97069679

Now use **rxSummary** to compute summary statistics, putting the computed means into a named vector:

	# Compute the summary statistics and extract
	# the means in a named vector
	sumStats <- rxResultsDF(rxSummary(~., myData1))
	sumStats
	meanVals <- sumStats$Mean
	names(meanVals) <- row.names(sumStats)

The computed statistics are:

	        Mean    StdDev         Min       Max ValidObs MissingObs
	x 0.07431126 0.9350711 -1.94160646 1.9933814       80         20
	y 0.54622241 0.3003457  0.04997869 0.9930338       80         20


Finally, we pass the computed means into a **rxDataStep** using the *transformObjects* argument:

	# Use rxDataStep to replace missings with imputed mean values
	myData2 <- rxDataStep(inData = myData1, transforms = list(
	    x = ifelse(is.na(x), meanVals["x"], x),
	    y = ifelse(is.na(y), meanVals["y"], y)),
	    transformObjects = list(meanVals = meanVals))
	rxGetInfo(myData2, numRows = 5)

The resulting data set information is:

	Data frame: myData2 
	Number of observations: 100 
	Number of variables: 2 
	Data (5 rows starting with row 1):
	            x          y
	1 -1.86213372 0.06206201
	2  1.13980693 0.54622241
	3  0.31762673 0.84132161
	4  1.39985928 0.26298559
	5  0.07431126 0.97069679


### Subsetting and Transforming Variables

Returning to our earlier example with CensusWorkers, we will now combine subsetting and transformations in one data step operation. Suppose we want to extract the same five variables as before from the CensusWorkers data set, but also add a factor variable based on the integer variable *age*. For example, to create our factor variable, we can use the following *transforms* argument:

	transforms = list(ageFactor = cut(age, breaks=seq(from = 20, to = 70, 
	                                  by = 5), right = FALSE))

In doing a data step operation, RevoScaleR reads in a chunk of data read from the original data set, including only the variables indicated in *varsToKeep*, or omitting variables specified in *varsToDrop*. It then passes the variables needed for data transformations back to R for manipulation:

	rxDataStep (inData = censusWorkers, outFile = "newCensusWorkers",
	varsToDrop = c("state"), transforms = list(
		ageFactor = cut(age, breaks=seq(from = 20, to = 70, by = 5), 
	    right = FALSE)))

The **rxGetInfo** function reveals the added variable:

	rxGetInfo("newCensusWorkers", getVarInfo = TRUE)
	  File name: C:\YourOutputPath\newCensusWorkers.xdf 
	  Number of observations: 351121 
	  Number of variables: 6 
	  Number of blocks: 6
	  Compression type: zlib 
	  Variable information: 
	  Var 1: age, Age 
	         Type: integer, Low/High: (20, 65)
	  Var 2: incwage, Wage and salary income 
	         Type: integer, Low/High: (0, 354000)
	  Var 3: perwt, Type: integer, Low/High: (2, 168)
	  Var 4: sex, Sex 
	         2 factor levels: Male Female
	  Var 5: wkswork1, Weeks worked last year 
	         Type: integer, Low/High: (21, 52)
	  Var 6: ageFactor
	         10 factor levels: [20,25) [25,30) [30,35) [35,40) [40,45) [45,50) [50,55) [55,60) [60,65) [65,70)
	  
We can combine the *transforms* argument with the *transformObjects* argument to create new variables from objects in your global environment (or other environments in your current search path).

For example, suppose you would like to estimate a linear model using wage income as the dependent variable, and want to include state-level of per capita expenditure on education as one of the independent variables. We can define a named vector to contain this state-level data as follows:

	educExp <- c(Connecticut=1795.57, Washington=1170.46, Indiana = 1289.66)

We can then use **rxDataStep** to add the per capita education expenditure as a new variable using the *transforms* argument, passing *educExp* to the *transformObjects* argument as a named list:

	censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
	rxDataStep(inData = censusWorkers, outFile = "censusWorkersWithEduc",
		transforms = list(
	         stateEducExpPC = educExp[match(state, names(educExp))] ), 
		transformObjects= list(educExp=educExp))

The **rxGetInfo** function reveals the added variable:

	rxGetInfo("censusWorkersWithEduc.xdf",getVarInfo=TRUE)
	  File name: C:\YourOutputPath\censusWorkersWithEduc.xdf 
	  Number of observations: 351121 
	  Number of variables: 7 
	  Number of blocks: 6 
	  Compression type: zlib
	  Variable information: 
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
	  Var 7: stateEducExpPC, Type: numeric, Low/High: (1170.4600, 1795.5700)
	  
## Convert a data frame to XDF

You can use all of the functionality provided by the **rxDataStep** function to create an .xdf file from a data frame for further use. For example, create a simple data frame:

	#  Using the Data Step to Create an .xdf File from a Data Frame
	
	set.seed(39)
	myData <- data.frame(x1 = rnorm(10000), x2 = runif(10000))

Now create an .xdf file, using a row selection and creating a new variable. The *rowsPerRead* argument will specify how many rows of the original data frame to process at a time before writing out a block of the new .xdf file.

	rxDataStep(inData = myData, outFile = "testFile.xdf",
	    rowSelection = x2 > .1,
	    transforms = list( x3 = x1 + x2 ),
	    rowsPerRead = 5000 )
	rxGetInfo("testFile.xdf")

	  File name: C:\Users\...\testFile.xdf 
	  Number of observations: 8970 
	  Number of variables: 3 
	  Number of blocks: 2


## Convert XDF to Text

If you need to share data with others not using .xdf data files, you can export your .xdf files to text format using the **rxDataStep** function. For example, we can write the claims.xdf file we created earlier to text format as follows:

	#  Converting .xdf Files to Text
	claimsCsv <- RxTextData(file="claims.csv")
	claimsXdf <- RxXdfData(file="claims.xdf")
	
	rxDataStep(inData=claimsXdf, outFile=claimsCsv)

By default, the text file created is comma-delimited, but you can change this by specifying a different delimiter with the *delimiter* argument to the RxTextData function:

	claimsTxt <- RxTextData(file="claims.txt", delimiter="\t")
	rxDataStep(inData=claimsXdf, outFile=claimsTxt)

If you have a large number of variables, you can choose to write out only a subsample by using the *VarsToKeep* or *VarsToDrop* argument. Here we write out the claims data, omitting the *number* variable:

	rxDataStep(inData=claimsXdf, outFile=claimsTxt, varsToDrop="number", 
	            overwrite=TRUE)

## Re-Block an .xdf File 

After a series of data import or row selection steps, you may find that you have an .xdf file with very uneven block sizes. This may make it difficult to efficiently perform computations by “chunk.” To find the sizes of the blocks in your .xdf file, use **rxGetInf** with the *getBlockSizes* argument set to TRUE. For example, let’s look at the block sizes for the sample CensusWorkers.xdf file:

	#  Re-Blocking an .xdf File
	
	fileName <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
	rxGetInfo(fileName, getBlockSizes = TRUE)

The following information is provided:
	
	File name: C:\Program Files\Microsoft\MRO-for-RRE\8.0\R-3.2.2\ library\RevoScaleR\SampleData\CensusWorkers.xdf 
	Number of observations: 351121 
	Number of variables: 6 
	Number of blocks: 6 
	Compression type: zlib
	Rows per block: 95420 42503 1799 131234 34726 45439

We see that, in fact, the number of rows per block varies from a low of 1799 to a high of 131,234. To create a new file with more even-sized blocks, use the *rowsPerRead* argument in **rxDataStep**:

	newFile <- "censusWorkersEvenBlocks.xdf"
	rxDataStep(inData = fileName, outFile = newFile, rowsPerRead = 60000)
	rxGetInfo(newFile, getBlockSizes = TRUE)	
	
The new file has blocks sizes of 60,000 for all but the last slightly smaller block:

	File name: C:\Users\...\censusWorkersEvenBlocks.xdf 
	Number of observations: 351121 
	Number of variables: 6 
	Number of blocks: 6 
	Compression type: zlib
	Rows per block: 60000 60000 60000 60000 60000 51121

## Modify variable metadata

To change variable information (rather than the data values themselves), use the function **rxSetVarInfo**. For example, using the data file created above, we can change the names of two variables and add descriptions:
	
	#  Modifying Variable Information
	
	newVarInfo <- list(
	    incwage = list(newName = "WageIncome"),
	    state   = list(newName = "State", description = "State of Residence"),
	    stateEducExpPC = list(description = "State Per Capita Educ Exp"))     
	fileName <- "censusWorkersWithEduc.xdf"
	rxSetVarInfo(varInfo = newVarInfo, data = fileName)
	rxGetVarInfo( fileName )
	
	Var 1: age, Age 
	       Type: integer, Low/High: (20, 65)
	Var 2: WageIncome, Wage and salary income 
	       Type: integer, Low/High: (0, 354000)
	Var 3: perwt, Type: integer, Low/High: (2, 168)
	Var 4: sex, Sex 
	       2 factor levels: Male Female
	Var 5: wkswork1, Weeks worked last year 
	       Type: integer, Low/High: (21, 52)
	Var 6: State, State of Residence 
	       3 factor levels: Connecticut Indiana Washington
	Var 7: stateEducExpPC, State Per Capita Educ Exp 
	       Type: numeric, Low/High: (1170.4600, 1795.5700)

## Sort data

Many analysis and plotting algorithms require as a first step that the data be sorted. Sorting a massive data set is both memory-intensive and time-consuming, but the **rxSort** function provides an efficient solution. The **rxSort** function allows you to sort by one or many keys. A *stable* sorting routine is used, so that, in the case of ties, remaining columns are left in the same order as they were in the original data set.

As a simple example, we can sort the census worker data by *age* and *incwage*. We will sort first by *age*, using the default increasing sort, and then by *incwage*, which we will sort in decreasing order:

	#  Sorting Data
	
	censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
	outXDF <- "censusWorkersSorted.xdf"
	rxSort(inData = censusWorkers, outFile = outXDF, sortByVars=c("age",
		"incwage"), decreasing=c(FALSE, TRUE))

The first few lines of the sorted file can be viewed as follows:

	rxGetInfo(outXDF, numRows=10)

	  File name: C:\YourOutputPath\censusWorkersSorted.xdf 
	  Number of observations: 351121 
	  Number of variables: 6 
	  Number of blocks: 6 
	  Compression type: zlib
	  Data (10 rows starting with row 1):
	     age incwage perwt    sex wkswork1      state
	  1   20  336000     3   Male       40 Washington
	  2   20  336000    23   Male       46 Washington
	  3   20  336000    11   Male       52 Washington
	  4   20  314000    33   Male       52    Indiana
	  5   20  168000    13   Male       24    Indiana
	  6   20  163000    16   Male       26 Washington
	  7   20  144000    27 Female       24    Indiana
	  8   20   96000    21   Male       48 Washington
	  9   20   93000    24   Male       24    Indiana
	  10  20   90000     6   Male       52 Washington

If the sort keys contain missing values, you can use the *missingsLow* flag to specify whether they are sorted as low values (*missingsLow=TRUE*, the default) or high values (*missingsLow=FALSE*).

### Remove duplicates during sort

In many situations, you are sorting a large data set by a particular key, for example, userID, but are looking for a sorted list of unique userIDs. The *removeDupKeys* argument to **rxSort** allows you to remove the duplicate entries from a sorted list. This argument is supported only for *type="auto"* and *type="mergeSort"*; it is ignored for *type="varByVar"*. 

When you use *removeDupKeys=TRUE*, the first record containing a unique combination of the *sortByVars* is retained; subsequent matching records are omitted from the sorted results, but, if desired, a count of the matching records is maintained in a new *dupFreqVar* output column. For example, the following artificial data set simulates a small amount of transaction data, with a user name, a state, and a transaction amount. When we sort by the variables *users* and *state* and specify *removeDupKeys=TRUE*, the *transAmt* shown for duplicate entries is the transaction amount for the *first* transaction encountered:

	set.seed(17)
	users <- sample(c("Aiden", "Ella", "Jayden", "Ava", "Max", "Grace", "Riley", 
		              "Lolita", "Liam", "Emma", "Ethan", "Elizabeth", "Jack", 
		              "Genevieve", "Avery", "Aurora", "Dylan", "Isabella",
		              "Caleb", "Bella"), 100, replace=TRUE)
	state <- sample(c("Washington", "California", "Texas", "North Carolina", 
		              "New York", "Massachusetts"), 100, replace=TRUE)
	transAmt <- round(runif(100)*100, digits=3)
	df <- data.frame(users=users, state=state, transAmt=transAmt)
	
	rxSort(df, sortByVars=c("users", "state"), removeDupKeys=TRUE, 
	    dupFreqVar = "DUP_COUNT")	
	  Number of rows written to file: 66, Variable(s): users, state, transAmt, DUP_COUNT, Total number of rows in file: 66
	  Time to sort data file: 0.100 seconds
	         users          state transAmt DUP_COUNT
	  1      Aiden       New York   11.010         1
	  2      Aiden North Carolina   73.307         1
	  3      Aiden          Texas    8.037         2
	  4     Aurora     California    8.787         1
	  5     Aurora  Massachusetts   55.187         1
	  6     Aurora       New York   91.648         1
	  7     Aurora          Texas   30.566         1
	  8     Aurora     Washington   27.374         1
	  9        Ava     California   70.638         2
	  10       Ava  Massachusetts   82.916         2
	  11       Ava       New York   45.683         2
	  12       Ava North Carolina   51.748         1
	  13       Ava          Texas   52.674         1
	  14     Avery     California    9.756         4
	  15     Avery  Massachusetts   63.715         1
	  16     Avery       New York   93.430         1
	  17     Avery North Carolina    1.889         2
	  18     Bella     California   60.258         2
	  19     Bella          Texas   32.684         1
	  20     Bella     Washington   60.230         2
	  21     Caleb  Massachusetts   94.527         1
	  22     Caleb North Carolina   89.259         2
	  23     Dylan     California   73.665         1
	  24     Dylan North Carolina   98.384         1
	  25     Dylan          Texas   27.067         1
	  26     Dylan     Washington   82.141         3
	  27 Elizabeth     California   95.497         2
	  28 Elizabeth       New York   35.546         3
	  29 Elizabeth North Carolina   18.892         2
	  30      Ella     California   11.644         1
	  31      Ella North Carolina   72.289         1
	  32      Ella     Washington   66.453         2
	  33      Emma  Massachusetts   28.502         1
	  34      Emma North Carolina   63.067         1
	  35     Ethan  Massachusetts   31.480         1
	  36     Ethan       New York   95.639         1
	  37     Ethan North Carolina    6.561         1
	  38     Ethan          Texas   29.963         1
	  39     Ethan     Washington   44.187         2
	  40 Genevieve  Massachusetts   90.783         1
	  41     Grace       New York   18.232         1
	  42     Grace North Carolina    5.355         2
	  43     Grace     Washington   91.084         1
	  44  Isabella       New York    4.115         1
	  45  Isabella North Carolina   12.942         2
	  46  Isabella          Texas    2.227         2
	  47      Jack     California   40.905         1
	  48      Jack  Massachusetts   98.080         2
	  49      Jack       New York    8.071         2
	  50      Jack North Carolina   11.304         3
	  51      Jack          Texas   18.795         2
	  52    Jayden  Massachusetts   83.949         1
	  53    Jayden North Carolina   67.769         1
	  54    Jayden     Washington    4.360         1
	  55      Liam     California    3.300         1
	  56      Liam  Massachusetts   87.585         1
	  57      Liam       New York   96.599         1
	  58      Liam North Carolina   32.997         1
	  59    Lolita     California   18.102         1
	  60    Lolita     Washington   30.649         2
	  61       Max  Massachusetts   21.683         2
	  62       Max       New York   14.852         2
	  63       Max North Carolina   79.982         2
	  64       Max          Texas   66.749         2
	  65       Max     Washington   67.326         2
	  66     Riley       New York   20.527         2
	  

Removing duplicates can be a useful way to reduce the size of a data set without losing information of interest. For example, consider an analysis of using data from the sample *AirlineDemoSmall* xdf file. It has 600,000 observations and contains the variables *DayOfWeek*, *CRSDepTime*, and *ArrDelay*. We can create a smaller data set reducing the number of observations, and adding a variable that contains the frequency of the duplicated observation.

	sampleDataDir <- rxGetOption("sampleDataDir")
	airDemo <- file.path(sampleDataDir, "AirlineDemoSmall.xdf")
	airDedup <- file.path(tempdir(), "rxAirDedup.xdf")
	rxSort(inData = airDemo, outFile = airDedup, 
		sortByVars =  c("DayOfWeek", "CRSDepTime", "ArrDelay"),
	  	removeDupKeys = TRUE, dupFreqVar = "FreqWt")
	rxGetInfo(airDedup)

The new data file contains about 1/3 of the observations and one additional variable:

	File name: C:\YourTempDir\rxAirDedup.xdf 
	Number of observations: 232451 
	Number of variables: 4 
	Number of blocks: 2
	Compression type: zlib

By using the frequency weights argument, we can use many of the RevoScaleR analysis functions on this smaller data set and get same results as we would using the full data set. For example, a linear model for Arrival Delay can be specified as follows, using the *fweights* argument:

	linModObj <- rxLinMod(ArrDelay~CRSDepTime + DayOfWeek, data = airDedup, 
		fweights = "FreqWt")
	summary(linModObj)
	 
	 Call:
	 rxLinMod(formula = ArrDelay ~ CRSDepTime + DayOfWeek, data = airDedup, 
	     fweights = "FreqWt")
	 
	 Linear Regression Results for: ArrDelay ~ CRSDepTime + DayOfWeek
	 File name: C:\YourTempDir\rxAirDedup.xdf
	 Frequency weights: FreqWt
	 Dependent variable(s): ArrDelay
	 Total independent variables: 9 (Including number dropped: 1)
	 Sum of weights of valid observations: 582628
	 Number of missing observations: 3503 
	  
	 Coefficients: (1 not defined because of singularities)
	                     Estimate Std. Error t value Pr(>|t|)    
	 (Intercept)         -3.19458    0.20413 -15.650 2.22e-16 ***
	 CRSDepTime           0.97862    0.01126  86.948 2.22e-16 ***
	 DayOfWeek=Monday     2.08100    0.18602  11.187 2.22e-16 ***
	 DayOfWeek=Tuesday    1.34015    0.19881   6.741 1.58e-11 ***
	 DayOfWeek=Wednesday  0.15155    0.19679   0.770    0.441    
	 DayOfWeek=Thursday  -1.32301    0.19518  -6.778 1.22e-11 ***
	 DayOfWeek=Friday     4.80042    0.19452  24.679 2.22e-16 ***
	 DayOfWeek=Saturday   2.18965    0.19229  11.387 2.22e-16 ***
	 DayOfWeek=Sunday     Dropped    Dropped Dropped  Dropped    
	 ---
	 Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 
	 
	 Residual standard error: 40.39 on 582620 degrees of freedom
	 Multiple R-squared: 0.01465 
	 Adjusted R-squared: 0.01464 
	 F-statistic:  1238 on 7 and 582620 DF,  p-value: < 2.2e-16 
	 Condition number: 10.6542
	 
Using the full data set, we get the following results:

	linModObjBig <- rxLinMod(ArrDelay~CRSDepTime + DayOfWeek, data = airDemo)
	summary(linModObjBig)
	  
	  Call:
	  rxLinMod(formula = ArrDelay ~ CRSDepTime + DayOfWeek, data = airDemo)
	  
	  Linear Regression Results for: ArrDelay ~ CRSDepTime + DayOfWeek
	  File name:    C:\YourSampleDir\AirlineDemoSmall.xdf
	  Dependent variable(s): ArrDelay
	  Total independent variables: 9 (Including number dropped: 1)
	  Number of valid observations: 582628
	  Number of missing observations: 17372 
	   
	  Coefficients: (1 not defined because of singularities)
	                      Estimate Std. Error t value Pr(>|t|)    
	  (Intercept)         -3.19458    0.20413 -15.650 2.22e-16 ***
	  CRSDepTime           0.97862    0.01126  86.948 2.22e-16 ***
	  DayOfWeek=Monday     2.08100    0.18602  11.187 2.22e-16 ***
	  DayOfWeek=Tuesday    1.34015    0.19881   6.741 1.58e-11 ***
	  DayOfWeek=Wednesday  0.15155    0.19679   0.770    0.441    
	  DayOfWeek=Thursday  -1.32301    0.19518  -6.778 1.22e-11 ***
	  DayOfWeek=Friday     4.80042    0.19452  24.679 2.22e-16 ***
	  DayOfWeek=Saturday   2.18965    0.19229  11.387 2.22e-16 ***
	  DayOfWeek=Sunday     Dropped    Dropped Dropped  Dropped    
	  ---
	  Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 
	  
	  Residual standard error: 40.39 on 582620 degrees of freedom
	  Multiple R-squared: 0.01465 
	  Adjusted R-squared: 0.01464 
	  F-statistic:  1238 on 7 and 582620 DF,  p-value: < 2.2e-16 
	  Condition number: 10.6542
	  
### The rxQuantile Function and the Five-Number Summary

Sorting data is, in the general case, a prerequisite to finding exact quantiles, including medians. However, it is possible to compute approximate quantiles by counting binned data then computing a linear interpolation of the empirical cdf. If the data are integers, or can be converted to integers by exact multiplication, and integral bins are used, the computation is exact. The RevoScaleR function rxQuantile does this computation, and by default returns an approximate five-number summary:

	#  The rxQuantile Function and the Five-Number Summary
	
	readPath <- rxGetOption("sampleDataDir")
	AirlinePath <- file.path(readPath, "AirlineDemoSmall.xdf")
	rxQuantile("ArrDelay", AirlinePath)
	 Rows Processed: 600000 
	  0%  25%  50%  75% 100% 
	 -86   -9    0   16 1490


## Merge data

Merging allows you to combine the information from two data sets into a third data set that can be used for subsequent analysis. One example is merging account information such as account number and billing address with transaction data such as account number and purchase details to create invoices. In this case, the two files are merged on the common information, that is, the account number.

In RevoScaleR, you merge .xdf files and/or data frames with the rxMerge function. This function supports a number of types of merge that are best illustrated by example. The available types are as follows:

-   Inner
-   Outer: left, right, and full
-   One-to-One
-   Union

We describe each of these types in the following sections.

### Inner merge

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

### Outer merge

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


### One-to-one merge

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

### Union merge

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


### Using rxMerge with .xdf files

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
	  
## Create and recode factors

Factors are variables that represent categories. An example is “sex”, which has the categories “Male” and “Female”. There are two parts to a factor variable:

1.  A vector of integer indexes with values in the range of 1:K, where K is the number of categories. For example, “sex” has two categories with indexes 1 and 2. The length of the vector corresponds to the number of observations.

2.  A vector of K character strings that are used when the factor is displayed. In R, these are normally printed without quote marks, to indicate that the variable is a factor instead of a character string.

If you have character data in an .xdf file or data frame, you can use the **rxFactors** function to convert it to a factor. Let’s create a simple data frame with character data. Note that by default, *data.frame* converts character data to factor data. That is, the *stringsAsFactors* argument defaults to *TRUE*. In RevoScaleR’s **rxImport** function, *stringsAsFactors* has a default of *FALSE*.

	# Creating factors from character data
	  
	myData <- data.frame(
	  id = 1:10,
	  sex = c("M","F", "M", "F", "F", "M", "F", "F", "M", "M"),
	  state = c(rep(c("WA", "CA"), each = 5)),
	  stringsAsFactors = FALSE)
	rxGetVarInfo(myData)
	Var 1: id, Type: integer, Low/High: (1, 10)
	Var 2: sex, Type: character
	Var 3: state, Type: character

Now we can use **rxFactors** to convert the character data to factors. We can just specify a vector of variable names to convert as the *factorInfo*:

	myNewData <- rxFactors(inData = myData, factorInfo = c("sex", "state"))
	rxGetVarInfo(myNewData)
	Var 1: id, Type: integer, Low/High: (1, 10)
	Var 2: sex
	       2 factor levels: M F
	Var 3: state
	       2 factor levels: WA CA

Note that by default, the factor levels are in the order in which they are encountered. If you would like to have them sorted alphabetically, specify *sortLevels* to be *TRUE*.

	myNewData <- rxFactors(inData = myData, factorInfo = c("sex", "state"), 
	    sortLevels = TRUE)
	rxGetVarInfo(myNewData)
	Var 1: id, Type: integer, Low/High: (1, 10)
	Var 2: sex
	       2 factor levels: F M
	Var 3: state
	       2 factor levels: CA WA

If you have variables that are already factors, you may want change the order of the levels, the names of the levels, or how they are grouped. Typically recoding a factor means changing from one set of indexes to another. For example, if the levels of “sex” are currently arranged in the order “M”, “F” and you want to change that to “F”, “M”, you need to change the index for every observation.

You can use the **rxFactors** function to recode factors in RevoScaleR. For example, suppose we have some test scores for a set of male and female subjects. We can generate such data randomly as follows:

	#  Recoding Factors
	
	set.seed(100)
	sex <- factor(sample(c("M","F"), size = 10, replace = TRUE), 
	              levels = c("M", "F"))
	DF <- data.frame(sex = sex, score = rnorm(10))

If we look at just the sex variable, we see the levels M or F for each observation:

	DF[["sex"]]

	  [1] M M F M M M F M F M
	  Levels: M F

To recode this factor so that “Female” is the first level and “Male” the second, we can use **rxFactors** as follows:

	newDF <- rxFactors(inData = DF, overwrite = TRUE,
	          factorInfo = list(Gender = list(newLevels = c(Female = "F", 
	                                              Male = "M"), 
	                                          varName = "sex")))

Looking at the new Gender variable, we see how the levels have changed:

	newDF$Gender 	

	  [1] Male   Male   Female Male   Male   Male   Female Male   Female Male  
	  Levels: Female Male

As mentioned earlier, by default, RevoScaleR codes factor levels in the order in which they are encountered in the input file(s). This could lead you to have a State variable ordered as “Maine”, “Vermont”, “New Hampshire”, “Massachusetts”, “Connecticut”, and so forth. 

Usually, you would prefer to have the levels of such a variable sorted in alphabetical order. You can do this with **rxFactors** using the sortLevels flag. It is most useful to specify this flag as part of the *factorInfo* list for each variable, although if you have a large number of factors and want most of them to be sorted, you can also set the flag to TRUE globally and then specify *sortLevels=FALSE* for those variables you want to order in a different way.

When using the *sortLevels* flag, it is useful to keep in mind that it is the *levels* that are being sorted, not the data itself, and that the levels are always character data. If you are using the individual values of a continuous variable as factor levels, you may be surprised by the sorted order of the levels: for example, the levels 1, 3, 20 are sorted as “1”, “20”, “3”.

Another common use of factor recoding is in analyzing survey data gathered using Likert items with five or seven level responses. For example, suppose a customer satisfaction survey offered the following seven-level responses to each of four questions:

1.  Completely satisfied
2.  Mostly satisfied
3.  Somewhat satisfied
4.  Neither satisfied nor dissatisfied
5.  Somewhat dissatisfied
6.  Mostly dissatisfied
7.  Completely dissatisfied

In analyzing this data, the survey analyst may recode the factors to focus on those who were largely satisfied (combining levels 1 and 2), largely dissatisfied (combining levels 6 and 7) and somewhere in-between (combining levels 3, 4, and 5). The CustomerSurvey.xdf file in the SampleData directory contains 25 responses to each of the four survey questions. We can read it into a data frame as follows:

	# Combining factor levels
	  
	surveyDF <- rxDataStep(inData = 
		file.path(rxGetOption("sampleDataDir"),"CustomerSurvey.xdf"))

To recode each question as desired, we can use **rxFactors** as follows:

	sl <- levels(surveyDF[[1]])
	quarterList <-  list(newLevels = list(
	                         "Largely Satisfied" = sl[1:2],
	                         "Neither Satisfied Nor Dissatisfied" = sl[3:5],
	                         "Largely Dissatisfied" = sl[6:7]))
	surveyDF <- rxFactors(inData = surveyDF,
	            factorInfo <- list(
	                   Q1 = quarterList,
	                   Q2 = quarterList,
	                   Q3 = quarterList,
	                   Q4 = quarterList))

Looking at just Q1, we see the recoded factor:

	surveyDF[["Q1"]]	

	  [1] Neither Satisfied Nor Dissatisfied Largely Satisfied                 
	  [3] Neither Satisfied Nor Dissatisfied Largely Satisfied                 
	  [5] Neither Satisfied Nor Dissatisfied Neither Satisfied Nor Dissatisfied
	  [7] Largely Dissatisfied               Neither Satisfied Nor Dissatisfied
	  [9] Neither Satisfied Nor Dissatisfied Largely Satisfied                 
	 [11] Neither Satisfied Nor Dissatisfied Largely Dissatisfied              
	 [13] Largely Satisfied                  Neither Satisfied Nor Dissatisfied
	 [15] Largely Dissatisfied               Neither Satisfied Nor Dissatisfied
	 [17] Largely Satisfied                  Neither Satisfied Nor Dissatisfied
	 [19] Neither Satisfied Nor Dissatisfied Neither Satisfied Nor Dissatisfied
	 [21] Neither Satisfied Nor Dissatisfied Neither Satisfied Nor Dissatisfied
	 [23] Neither Satisfied Nor Dissatisfied Largely Dissatisfied              
	 [25] Neither Satisfied Nor Dissatisfied
	 3 Levels: Largely Satisfied ... Largely Dissatisfied

#### Recoding Factors to Ensure Variable Compatibility

One important use of factor recoding in RevoScaleR is to ensure that the factor variables in two files are compatible, that is, have the same levels with the same coding. This use comes up in a variety of contexts, including prediction, merging, and distributed computing. For example, suppose you are creating a logistic regression model of whether a given airline flight will be late, and are using the first fifteen years of the airline data as a training set. You then want to test the model on the remaining years of the airline data. You need to ensure that the two files, the training set and the test set, have compatible factor variables. You can generally do this easily using **rxGetInfo** (with getVarInfo=TRUE) together with **rxFactors**. Use **rxGetInfo** to find all the levels in all the files, then use **rxFactors** to recode each file so that every factor variable contains all the levels found in any of the files.

The **rxMerge** function automatically checks for factor variable compatibility and recodes on the fly if necessary.

## Next Steps

Continue on to the following data-related articles to learn more about XDF, data source objects, and other data formats:

+ [Transformation functions](scaler-user-guide-transform-functions.md)	
+ [XDF files](scaler-data-xdf.md)	
+ [Data Sources](scaler-user-guide-data-source.md)	
+ [Import text data](scaler-user-guide-data-import.md)
+ [Import ODBC data](scaler-data-odbc.md)
+ [Import and consume data on HDFS](scaler-data-hdfs.md)

## See Also
   
 [RevoScaleR Functions](scaler/scaler.md)   
 [Tutorial: data import and exploration](scaler-getting-started-data-import-exploration.md)
 [Tutorial: data visualization and analysis](scaler-getting-started-data-manipulation.md) 