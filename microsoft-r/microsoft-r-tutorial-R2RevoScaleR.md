---

# required metadata
title: "Tutorial: Explore R-to-RevoScaleR in 25 functions"
description: "Explore and execute R and RevoScaleR commands using R Microsoft R Client or R Server."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/12/2017"
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
ms.technology:
  - r-client
  - r-server
ms.custom: ""

---

# Tutorial: Explore R and RevoScaleR in 25 functions

**Applies to: Microsoft R Server or R Client**

If you are new to *both* R and Microsoft R, this tutorial introduces you to 25 (or so) of the more commonly used R functions. In this tutorial, you'll learn how to load small data sets into R and perform simple computations. A key point to take away from this tutorial is that you can combine R and RevoScaleR functions in the same R script.

This tutorial starts with base R commands before transitioning to RevoScaleR functions in Microsoft R. If you already know R, you might want to skip down to [Explore RevoScaleR Functions](#ExploreScaleRFunctions).

## Prerequisites

This is our simplest and most light-weight tutorial. To complete it, you can use the command line tool RGui.exe on Windows or start the **Revo64** program on Linux. The tutorial uses ready-to-use, built-in sample data provided in base R or RevoScaleR function library so once you have the software, there is nothing more to download or install.

+ On Windows, go to \Program Files\Microsoft\R Client\R_SERVER\bin\x64 and double-click **Rgui.exe**.	
+ On Linux, at the command prompt, type **Revo64**.

The command prompt for a R is `>`. You can hand-type commands line by line, or copy-paste multiple commands at once. Press Enter to execute the statement.

## Start with R

Because Microsoft R is built on R, this tutorial begins with an exploration of base R commands.

### Create Vectors

R is an environment for analyzing data, so the natural starting point is to load some data. For small data sets, such as the following 20 measurements of the speed of light taken from the famous Michelson-Morley experiment, the simplest approach uses R’s `c` function to combine the data into a vector. Type or copy the following script and paste it at the &gt; prompt at the beginning of the command line:

	c(850, 740, 900, 1070, 930, 850, 950, 980, 980, 880,
	1000, 980, 930, 650, 760, 810, 1000, 1000, 960, 960)

 When you type the closing parenthesis and press *Enter*, R responds as follows:

	[1]  850  740  900 1070  930  850  950  	980  980  880  
	[11]  1000 980  930  650  760  810 1000 1000  960  960

This indicates that R has interpreted what you typed, created a vector with 20 elements, and returned that vector. But we have a problem. R hasn’t saved what we typed. If we want to use this vector again (and that’s the usual reason for creating a vector in the first place), we need to *assign* it. The R assignment operator has the suggestive form *&lt;-* to indicate a value is being assigned to a name. You can use most combinations of letters, numbers, and periods to form names (but note that names can’t begin with a number). Here we’ll use `michelson`:

	michelson <- c(850, 740, 900, 1070, 930, 850, 950, 980, 980, 880,
	1000, 980, 930, 650, 760, 810, 1000, 1000, 960, 960)

R responds with a &gt; prompt. Notice that the named vector is not automatically printed when it is assigned. However, you can view the vector by typing its name at the prompt:

	> michelson

	[1]  850  740  900 1070  930  850  950  980   980  880
	[11] 1000  980  930  650  760  810 1000 1000  960  960

The `c` function is useful for hand typing in small vectors such as you might find in textbook examples, and it is also useful for combining existing vectors. For example, if we discovered another five observations that extended the Michelson-Morley data, we could extend the vector using `c` as follows:

	michelsonNew <- c(michelson, 850, 930, 940, 970, 870)
	
	> michelsonNew

	[1]  850  740  900 1070  930  850  950  	980  980  880
	[11] 1000  980  930  650  760  810 1000 1000  960  960
	[21]  850  930  940  970  870

Often for testing purposes you want to use randomly generated data. R has a number of built-in distributions from which you can generate random numbers; two of the most commonly used are the normal and the uniform distributions. To obtain a set of numbers from a normal distribution, you use the `rnorm` function:

	normalDat <- rnorm(25)
	normalDat

	[1] -0.66184983  1.71895416  2.12166699
	[4]  1.49715368 -0.03614058  1.23194518
	[7] -0.06488077  1.06899373 -0.37696531
	[10]  1.04318309 -0.38282188  0.29942160
	[13]  0.67423976 -0.29281632  0.48805336
	[16]  0.88280182  1.86274898  1.61172529
	[19]  0.13547954  1.08808601 -1.26681476
	[22] -0.19858329  0.13886578 -0.27933600
	[25]  0.70891942

 By default, the data are generated from a standard normal with mean 0 and standard deviation 1. You can use the `mean` and `sd` arguments to `rnorm` to specify a different normal distribution:

	normalSat <- rnorm(25, mean=450, sd=100)
	normalSat

	[1] 373.3390 594.3363 534.4879 410.0630 307.2232
	[6] 307.8008 417.1772 478.4570 521.9336 493.2416
	[11] 414.8075 479.7721 423.8568 580.8690 451.5870
	[16] 406.8826 488.2447 454.1125 444.0776 320.3576
	[21] 236.3024 360.6385 511.2733 508.2971 449.4118

Similarly, you can use the `runif` function to generate random data from a uniform distribution:

	uniformDat <- runif(25)
	uniformDat

	[1] 0.03105927 0.18295065 0.96637386 0.71535963
	[5] 0.16081450 0.15216891 0.07346868 0.15047337
	[9] 0.49408599 0.35582231 0.70424152 0.63671421
	[13] 0.20865305 0.20167994 0.37511929 0.54082887
	[17] 0.86681824 0.23792988 0.44364083 0.88482396
	[21] 0.41863803 0.42392873 0.24800036 0.22084038
	[25] 0.48285406

The default uniform distribution is that over the interval 0 to 1; you can specify alternatives by setting the `min` and `max` arguments:

	uniformPerc <- runif(25, min=0, max=100)
	uniformPerc

	[1] 66.221400 12.270863 33.417174 21.985229
	[5] 92.767213 17.911602  1.935963 53.551991
	[9] 75.110760 22.436347 63.172258 95.977501
	[13] 79.317351 56.767608 89.416080 79.546495
	[17]  8.961152 49.315612 43.432128 68.871867
	[21] 73.598221 63.888835 35.261694 54.481692
	[25] 37.575176

Another commonly used vector is the `sequence`, a uniformly-spaced run of numbers. For the common case of a run of integers, you can use the infix operator, :, as follows:

	1:10

	 [1]  1  2  3  4  5  6  7  8  9 10

For more general sequences, use the `seq` function:

	seq(length = 11, from = 10, to = 30)

	[1] 10 12 14 16 18 20 22 24 26 28 30

	seq(from = 10,length = 20, by = 4)

	[1] 10 14 18 22 26 30 34 38 42 46 50 54 58 62 66 70 74
	[18] 78 82 86

> [!Tip]
> If you are working with big data, you’ll still use vectors to manipulate parameters and information about your data, but you'll probably store the data in the RevoScaleR high-performance .xdf file format.

### Exploratory Data Analysis

After you have some data, you will want to explore it graphically. For most small data sets, the place to begin is with the `plot` function, which provides a default graphical view of the data:

	plot(michelson)
	plot(normalDat)
	plot(uniformPerc)
	plot(1:10)

For numeric vectors such as ours, the default view is a scatter plot of the observations against their index, resulting in the following plots:

![](media/rserver-getting-started/image5.jpeg)

For an exploration of the shape of the data, the usual tools are `stem` (to create a stemplot) and `hist` (to create a histogram):

	stem(michelson)

	 The decimal point is 2 digit(s) to the right of the |
	  6 | 5
	  7 | 46
	  8 | 1558
	  9 | 033566888
	 10 | 0007

	hist(michelson)

The resulting histogram is shown as the left plot below. We can make the histogram look more like the stemplot by specifying the `nclass` argument to `hist`:

	hist(michelson, nclass=5)

The resulting histogram is shown as the right plot in the figure below.

![](media/rserver-getting-started/image6.jpeg)

From the histogram and stemplot, it appears that the Michelson-Morley observations are not obviously normal. A normal Q-Q plot gives a graphical test of whether a data set is normal:

	qqnorm(michelson)

The decided bend in the resulting plot confirms the suspicion that the data are not normal.

![](media/rserver-getting-started/image7.jpeg)

Another useful exploratory plot, especially for comparing two distributions, is the boxplot:

	boxplot(normalDat, uniformDat)

![](media/rserver-getting-started/image8.jpeg)

> [!Tip]
> These plots are great if you have a small data set in memory. However, when working with big data, some plot types may not be very informative when working directly with the data (e.g., scatter plots can produce a big blob of ink) and others may be computational intensive (e.g., require sorting data). A good starting place is the *rxHistogram* function in RevoScaleR that efficiently computes and renders histograms for large data sets.  And remember that RevoScaleR functions such as *rxCube* can provide summary information that is easily amenable to the impressive plotting capabilities provided by R packages.

### Summary Statistics

While an informative graphic often gives the fullest description of a data set, numerical summaries provide a useful shorthand for describing certain features of the data. For example, estimators such as the mean and median help to locate the data set, and the standard deviation and variance measure the scale or spread of the data. R has a full set of summary statistics available:

	> mean(michelson)
	[1] 909
	> median(michelson)
	[1] 940
	> sd(michelson)
	[1] 104.9260
	> var(michelson)
	[1] 11009.47

 The generic summary function provides a meaningful summary of a data set; for a numeric vector it provides the five-number summary plus the mean:

	summary(michelson)

	 Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
	 650     850     940     909     980    1070

> [!Tip]
> The *rxSummary* function in RevoScaleR will efficiently compute summary statistics for a data frame in memory or a large data file stored on disk.

### Multivariate Data Sets

In most disciplines, meaningful data sets have multiple variables, typically observations of various quantities and qualities of individual subjects. Such data sets are typically represented as tables in which the columns correspond to variables and the rows correspond to subjects, or cases. In R, such tables can be created as *data frame* objects. For example, at the 2008 All-Star Break, the Seattle Mariners had five players who met the minimum qualifications to be considered for a batting title (that is, at least 3.1 at bats per game played by the team). Their statistics are shown in the following table:

	Player          Games  AB   R    H   2B  3B   HR   TB  RBI   BA  OBP  SLG  OPS
	"I. Suzuki"        95 391  63  119   11   3    3  145   21 .304 .366 .371 .737
	"J. Lopez"         92 379  46  113   26   1    5  156   48 .298 .318 .412 .729
	"R. Ibanez"        95 370  41  101   26   1   11  162   55 .273 .338 .438 .776
	"Y. Betancourt"    90 326  34   87   22   2    3  122   29 .267 .278 .374 .656 
	"A. Beltre"        92 352  46   91   16   0   16  155   46 .259 .329 .440 .769

Copy and paste the table into a text editor (such as Notepad on Windows, or emacs or vi on Unix-type machines) and save the file as **msStats.txt** in the working directory returned by the *getwd* function, for example:

	getwd()

	 [1] "/Users/joe"

You can then read the data into R using the `read.table` function. The argument *header=TRUE* specifies that the first line is a header of variable names:

	msStats <- read.table("msStats.txt", header=TRUE)
	msStats

	> msStats
	Player Games  AB  R   H X2B X3B HR  TB RBI
	1	I. Suzuki     95 391 63 119  11   3  3 145  21
	2	J. Lopez     92 379 46 113  26   1  5 156  48
	3	R. Ibanez     95 370 41 101  26   1 11 162  55
	4 	Y. Betancourt     90 326 34  87  22   2  3 122  29
	5	A. Beltre     92 352 46  91  16   0 16 155  46
    
		BA   OBP   SLG   OPS
	1 0.304 0.366 0.371 0.737
	2 0.298 0.318 0.412 0.729
	3 0.273 0.338 0.438 0.776
	4 0.267 0.278 0.374 0.656
	5 0.259 0.329 0.440 0.769

Notice how `read.table` changed the names of our original “2B" and “3B" columns to be valid R names; R names cannot begin with a numeral.

Most small R data sets in daily use are data frames. The built-in package, **datasets**, is a rich source of data frames for further experimentation. In the next section, we turn our attention to the built-in data set *attitude*, part of the **datasets** package.

> [!Tip]
> Check out the *rxImport* function for an efficient and flexible way to bring data stored in a variety of data formats (e.g., text, SQL Server, ODBC, SAS, SPSS, Teradata) into a data frame in memory or an .xdf file.

### Linear Models

The *attitude* data set is a data frame with 30 observations on 7 variables, measuring the percent proportion of favorable responses to seven survey questions in each of 30 departments. The survey was conducted in a large financial organization; there were approximately 35 respondents in each department.

We mentioned that the `plot` function could be used with virtually any data set to get an initial visualization; let’s see what it gives for the *attitude* data:

	plot(attitude)

The resulting plot is a pairwise scatter plot of the numeric variables in the data set.

![](media/rserver-getting-started/image9.jpeg)

The first two variables (*rating* and *complaints*) show a strong linear relationship. To model that relationship, we use the `lm` function:

	attitudeLM1 <- lm(rating ~ complaints, data=attitude)

To view a summary of the model, we can use the `summary` function:

	summary(attitudeLM1)

	Call:
	lm(formula = rating ~ complaints, data = attitude)
	Residuals:
	   Min       1Q   Median       3Q      Max 
	-12.8799  -5.9905   0.1783   6.2978   9.6294 

	Coefficients:
		Estimate Std. Error t value Pr(>|t|)    
	(Intercept) 14.37632    6.61999   2.172   0.0385 *  
	complaints   0.75461    0.09753   7.737 1.99e-08 ***
	---
	Signif. codes:  0 *** 0.001 ** 0.01 * 0.05 . 0.1  1 

	Residual standard error: 6.993 on 28 degrees of freedom
	Multiple R-squared: 0.6813, Adjusted R-squared: 0.6699 
	F-statistic: 59.86 on 1 and 28 DF,  p-value: 1.988e-08 

We can also try to visualize the model using `plot`:

	plot(attitudeLM1)

The default plot for a fitted linear model is a set of four plots; by default they are shown one at a time, and you are prompted before each new plot is displayed. To view them all at once, use the `par` function with the *mfrow* parameter to specify a 2 x 2 layout:

	par(mfrow=c(2,2))
	plot(attitudeLM1)

![](media/rserver-getting-started/image10.jpeg)

> [!Tip]
> The rxLinMod function is a full-featured alternative to lm that can efficiently handle large data sets.  Also look at rxLogit and rxGlm as alternatives to `glm`, rxKmeans as an alternative to `kmeans`, and rxDTree as an alternative to `rpart`.

### Matrices and *apply* function

A matrix is a two-dimensional data array. Unlike data frames, which can have different data types in their columns, matrices may contain data of only one type. Most commonly, matrices are used to hold numeric data. You create matrices with the `matrix` function:

	A <- matrix(c(3, 5, 7, 9, 13, 15, 8, 4, 2), ncol=3)
	A

	[,1] [,2] [,3]
	[1,]    3    9    8
	[2,]    5   13    4
	[3,]    7   15    2

	B <- matrix(c(4, 7, 9, 5, 8, 6), ncol=3)
	B

	[,1] [,2] [,3]
	[1,]    4    9    8
	[2,]    7    5    6

Ordinary arithmetic acts *element-by-element* on matrices:

	A + A

	[,1] [,2] [,3]
	[1,]    6   18   16
	[2,]   10   26    8
	[3,]   14   30    4

	A * A

	[,1] [,2] [,3]
	[1,]    9   81   64
	[2,]   25  169   16
	[3,]   49  225    4

Matrix multiplication in the linear algebra sense requires a special operator, `%*%`:

	A %*% A

	[,1] [,2] [,3]
	[1,]  110  264   76
	[2,]  108  274  100
	[3,]  110  288  120

Matrix multiplication requires two matrices to be *conformable*, which means that the number of *columns* of the first matrix is equal to the number of *rows* of the second:

	B %*% A

	[,1] [,2] [,3]
	[1,]  113  273   84
	[2,]   88  218   88

	A %*% B

	Error in A %*% B : non-conformable arguments

When you need to manipulate the rows or columns of a matrix, an incredibly useful tool is the `apply` function. With `apply`, you can apply a function to all the rows or columns of matrix at once. For example, to find the column products of *A*, you could use `apply` as follows:

	apply(A, 2, prod)

	[1]  105 1755   64

The row products are just as simple:

	apply(A, 1, prod)

	[1] 216 260 210

To sort the columns of *A*, just replace *prod* with *sort*:

	apply(A, 2, sort)

		 [,1] [,2] [,3]
	[1,]    3    9    2
	[2,]    5   13    4
	[3,]    7   15    8

### Lists and *lapply* function

A `list` in R is a very flexible data object that can be used to combine data of different types and different lengths for almost any purpose. Arbitrary lists can be created with either the list function or the c function; many other functions, especially the statistical modeling functions, return their output as list objects.

For example, we can combine a character vector, a numeric vector, and a numeric matrix in a single list as follows:

	list1 <- list(x = 1:10, y = c("Tami", "Victor", "Emily"), 
	z = matrix(c(3, 5, 4, 7), nrow=2))
	list1

	$x
	[1]  1  2  3  4  5  6  7  8  9 10
	$y
	[1] "Tami"   "Victor" "Emily" 
	$z
	[,1] [,2]
	[1,]    3    4
	[2,]    5    7

The function `lapply` can be used to apply the same function to each component of a list in turn:

	lapply(list1, length)

	$x
	[1] 10

	$y
	[1] 3

	$z
	[1] 4

> [!Tip]
> You will regularly use lists and functions that manipulate them when handling input and output for your big data analyses.  

<a name="ExploreScaleRFunctions"></a>
## Expore RevoScaleR Functions

The **RevoScaleR** package, included in Microsoft R Server and R Client, provides a framework for quickly writing start-to-finish, scalable R code for data analysis. 

RevoScaleR functions are denoted with an **rx** or **Rx** prefix to make them readily identifiable. You can also work with base functions in the R language. RevoScaleR is built on the R language, which means you can write scripts or code that use both types of functions in the same solution.

### Step 1: Access Data with *rxImport*

The *rxImport* function allows you to import data from fixed or delimited text files, SAS files, SPSS files, or a SQL Server, Teradata or ODBC connection. There’s no need to have SAS or SPSS installed on your system to import those file types, but you will need a locally installed ODBC driver for your database to access data on a local or remote computer. 

Let’s start simply by using a delimited text file available in the [built-in sample data directory](scaler-user-guide-sample-data.md) of the **RevoScaleR** package. We’ll store the location of the file in a character string (*inDataFile*), then import the data into an in-memory data set (data frame) called *mortData*:

	inDataFile <- file.path(rxGetOption("sampleDataDir"), "mortDefaultSmall2000.csv")

	mortData <- rxImport(inData = inDataFile)

If we anticipate repeating the same analysis on a larger data set later, we could prepare for that by putting placeholders in our code for output files. An output file is an XDF file, native to R Client and R Server, persisted on disk and structured to hold modular data. If we included an output file with rxImport, the output object returned from *rxImport* would be a small object representing the .xdf file on disk (an RxXdfData object), rather than an in-memory data frame containing all of the data. 

But for the time being, let's continue to work with the data in memory. We can do this by leaving outFile out of the command, or by setting the *outFile* parameter to NULL. The following code is equivalent to the importing task of that above:

	mortOutput <- NULL
	mortData <- rxImport(inData = inDataFile, outFile = mortOutput)

### Step 2: Retrieve metadata

There are a number of basic methods we can use to learn about the data set and its variables that will work on the return object of *rxImport*, regardless of whether it is a data frame or *RxXdfData* object. 

Input: Get the number of rows, cols, and names of the imported data:

	nrow(mortData)
	ncol(mortData)
	names(mortData)

Output:

	> nrow(mortData)
	[1] 10000
	> ncol(mortData)
	[1] 6
	> names(mortData)
	[1] "creditScore" "houseAge" "yearsEmploy" "ccDebt" "year"
	[6] "default"

To print out the first few rows of the data set, you can use *head()*:

	head(mortData, n = 3)

Output:

	creditScore houseAge yearsEmploy ccDebt year default
	1 691 16 9 6725 2000 0
	2 691 4 4 5077 2000 0
	3 743 18 3 3080 2000 0

The rxGetInfo function allows you to quickly get information about your data set and its variables all at one time, including more information about variable types and ranges. Let’s try it on *mortData*, having the first 3 rows of the data set printed out:

	rxGetInfo(mortData, getVarInfo = TRUE, numRows=3)

Output:

	Data frame: mortData
	Data frame: mortData
	Number of observations: 10000
	Number of variables: 6
	Variable information:
	Var 1: creditScore, Type: integer, Low/High: (486, 895)
	Var 2: houseAge, Type: integer, Low/High: (0, 40)
	Var 3: yearsEmploy, Type: integer, Low/High: (0, 14)
	Var 4: ccDebt, Type: integer, Low/High: (0, 12275)
	Var 5: year, Type: integer, Low/High: (2000, 2000)
	Var 6: default, Type: integer, Low/High: (0, 1)
	Data (3 rows starting with row 1):
		creditScore houseAge yearsEmploy ccDebt year default
	1 691 16 9 6725 2000 0
	2 691 4 4 5077 2000 0
	3 743 18 3 3080 2000 0

### Step 3: Select and transform data with *rxDataStep*

The rxDataStep function provides a framework for the majority of your data manipulation tasks. It allows for row selection (the *rowSelection* argument), variable selection (the *varsToKeep* or *varsToDrop* arguments), and the creation of new variables from existing ones (the *transforms* argument). Here’s an example that does all three with one function call:

	mortDataNew <- rxDataStep(
		# Specify the input data set
		inData = mortData,
		# Put in a placeholder for an output file
		outFile = outFile2,
		# Specify any variables to keep or drop
		varsToDrop = c("year"),
		# Specify rows to select
		rowSelection = creditScore < 850,
		# Specify a list of new variables to create
		transforms = list(
			catDebt = cut(ccDebt, breaks = c(0, 6500, 13000),
				labels = c("Low Debt", "High Debt")),
			lowScore = creditScore < 625))

Our new data set, *mortDataNew*, will not have the variable year, but adds two new variables: a categorical variable named *catDebt* that uses R’s `cut` function to break the *ccDebt* variable into two categories, and a logical variable, *lowScore*, that will be TRUE for individuals with low credit scores. These *transforms* expressions follow the rule that they must be able to operate on a chunk of data at a time; that is, the computation for a single row of data cannot depend on values in other rows of data. 

With the *rowSelection* argument, we have also removed any observations with high credit scores, above or equal to 850. We can use the rxGetVarInfo function to confirm:

	rxGetVarInfo(mortDataNew)

	Var 1: creditScore, Type: integer, Low/High: (486, 847)
	Var 2: houseAge, Type: integer, Low/High: (0, 40)
	Var 3: yearsEmploy, Type: integer, Low/High: (0, 14)
	Var 4: ccDebt, Type: integer, Low/High: (0, 12275)
	Var 5: default, Type: integer, Low/High: (0, 1)
	Var 6: catDebt
		2 factor levels: Low Debt High Debt
	Var 7: lowScore, Type: logical, Low/High: (0, 1)

### Step 4: Visualize Data with *rxHistogram*, *rxCube*, and *rxLinePlot*

The rxHistogram function will show us the distribution of any of the variables in our data set. For example, let’s look at credit score:

	rxHistogram(~creditScore, data = mortDataNew )

![](media/rserver-getting-started/image14.png)

The rxCube function will compute category counts, and can operate on the interaction of categorical variables. Using the *F()* notation to convert a variable into an on-the-fly categorical factor variable (with a level for each integer value), we can compute the counts for each credit score for the two groups who have low and high credit card debt:

	mortCube <- rxCube(~F(creditScore):catDebt, data = mortDataNew)

The *rxLinePlot* function is a convenient way to plot output from *rxCube*. We use the *rxResultsDF* helper function to convert cube output into a data frame convenient for plotting:

	rxLinePlot(Counts~creditScore|catDebt, data=rxResultsDF(mortCube))

![](media/rserver-getting-started/image15.png)

### Step 5: Analyze Data with *rxLogit*

**RevoScaleR** provides the foundation for a variety of high performance, scalable data analyses. Here we’ll do a logistic regression, but you’ll probably also want to take look at computing summary statistics (*rxSummary*), computing cross-tabs (*rxCrossTabs*), estimating linear models (*rxLinMod*) or generalized linear models (*rxGlm*), and estimating variance-covariance or correlation matrices (*rxCovCor*) that can be used as inputs to other R functions such as principal components analysis and factor analysis. Now, let’s estimate a logistic regression on whether or not an individual defaulted on their loan, using credit card debt and years of employment as independent variables:

	myLogit <- rxLogit(default~ccDebt+yearsEmploy , data=mortDataNew)
	summary(myLogit)

We get the following output:

	Call:
	rxLogit(formula = default ~ ccDebt + yearsEmploy, data = mortDataNew)

	Logistic Regression Results for: default ~ ccDebt + yearsEmploy
	Data: mortDataNew
	Dependent variable(s): default
	Total independent variables: 3
	Number of valid observations: 9982
	Number of missing observations: 0
	-2*LogLikelihood: 100.6036 (Residual deviance on 9979 degrees of freedom)

	Coefficients:
		Estimate Std. Error z value Pr(>|z|)
	(Intercept) -1.614e+01 2.074e+00 -7.781 2.22e-16 ***
	ccDebt 1.414e-03 2.139e-04 6.610 3.83e-11 ***
	yearsEmploy -3.317e-01 1.608e-01 -2.063 0.0391 *
	---
	Signif. codes: 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

	Condition number of final variance-covariance matrix: 1.4455
	Number of iterations: 9

### Step 6: Scale Your Analysis

So, we’ve finished experimenting with our small data set in memory. Let’s scale up to a data set with a million rows rather than just 10000. These larger text data files are available [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). Windows users should download the zip version, mortDefault.zip, and Linux users mortDefault.tar.gz. 

After downloading and unpacking the data, set your path to the correct location in the code below. It will be more efficient to store the imported data on disk, so we’ll also specify the locations for our imported and transformed data sets:

	# bigDataDir <- "C:/MicrosoftR/Data" # Specify the location
	inDataFile <- file.path(bigDataDir, "mortDefault",
		"mortDefault2000.csv")
	outFile <- "myMortData.xdf"
	outFile2 <- "myMortData2.xdf"

That’s it! Now you can re-use all of the importing, data step, plotting and analysis code above on the larger data set.

	# Import data
	mortData <- rxImport(inData = inDataFile, outFile = outFile)

	Rows Read: 500000, Total Rows Processed: 500000, Total Chunk Time: 1.043 seconds
	Rows Read: 500000, Total Rows Processed: 1000000, Total Chunk Time: 1.001 seconds

Note that because we have specified an output file when importing the data, the returned *mortData* object is a small object in memory representing the .xdf data file, rather than a full data frame containing all of the data in memory. It can be used in **RevoScaleR** analysis functions in the same way as data frames.

	# Some quick information about my data
	rxGetInfo(mortData, getVarInfo = TRUE, numRows=5)

		File name: C:\\MicrosoftR\\Data\\myMortData.xdf
		Number of observations: 1e+06
		Number of variables: 6
		Number of blocks: 2
		Compression type: zlib
		Variable information:
		Var 1: creditScore, Type: integer, Low/High: (459, 942)
		Var 2: houseAge, Type: integer, Low/High: (0, 40)
		Var 3: yearsEmploy, Type: integer, Low/High: (0, 15)
		Var 4: ccDebt, Type: integer, Low/High: (0, 14639)
		Var 5: year, Type: integer, Low/High: (2000, 2000)
		Var 6: default, Type: integer, Low/High: (0, 1)
		Data (5 rows starting with row 1):
		creditScore houseAge yearsEmploy ccDebt year default
		1 615 10 5 2818 2000 0
		2 780 34 5 3575 2000 0
		3 735 12 1 3184 2000 0
		4 713 15 5 6236 2000 0
		5 689 10 5 6817 2000 0

	# The data step
	mortDataNew <- rxDataStep(
		# Specify the input data set
		inData = mortData,
		# Put in a placeholder for an output file
		outFile = outFile2,
		# Specify any variables to keep or drop
		varsToDrop = c("year"),
		# Specify rows to select
		rowSelection = creditScore < 850,
		# Specify a list of new variables to create
		transforms = list(
			catDebt = cut(ccDebt, breaks = c(0, 6500, 13000),
				labels = c("Low Debt", "High Debt")),
			lowScore = creditScore < 625))

	Rows Read: 500000, Total Rows Processed: 500000, Total Chunk Time: 0.673 seconds
	Rows Read: 500000, Total Rows Processed: 1000000, Total Chunk Time: 0.448 seconds
	>

	# Looking at the data
	rxHistogram(~creditScore, data = mortDataNew )

		Rows Read: 499294, Total Rows Processed: 499294, Total Chunk Time: 0.329 seconds
		Rows Read: 499293, Total Rows Processed: 998587, Total Chunk Time: 0.335 seconds
		Computation time: 0.678 seconds.

![](media/rserver-getting-started/image16.png)

	myCube = rxCube(~F(creditScore):catDebt, data = mortDataNew)
	rxLinePlot(Counts~creditScore|catDebt, data=rxResultsDF(myCube))

![](media/rserver-getting-started/image17.png)

	# Compute a logistic regression
	myLogit <- rxLogit(default~ccDebt+yearsEmploy , data=mortDataNew)
	summary(myLogit)

	Call:
	rxLogit(formula = default ~ ccDebt + yearsEmploy, data = mortDataNew)

	Logistic Regression Results for: default ~ ccDebt + yearsEmploy
	File name:
		C:\Users\RUser\myMortData2.xdf
	Dependent variable(s): default
	Total independent variables: 3
	Number of valid observations: 998587
	Number of missing observations: 0
	-2*LogLikelihood: 8837.7644 (Residual deviance on 998584 degrees of freedom)

	Coefficients:

	Estimate Std. Error z value Pr(>|z|)
	(Intercept) -1.725e+01 2.330e-01 -74.04 2.22e-16 ***
	ccDebt 1.509e-03 2.327e-05 64.85 2.22e-16 ***
	yearsEmploy -3.054e-01 1.713e-02 -17.83 2.22e-16 ***
	---
	Signif. codes: 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

	Condition number of final variance-covariance matrix: 1.3005
	Number of iterations: 9


## Next Steps: Deeper dive into R

This section is a continuation of the introduction to R.

### How to load more R Packages

An R *package* is a collection of R objects and documentation. The R objects may be functions, data sets, or a combination, and they are usually related in some way, although this is not an absolute requirement. The standard R distribution consists of the following packages:

| stats      | graphics | grDevices | utils    |
|------------|----------|-----------|----------|
| datasets   | methods  | base      | -         |
| KernSmooth | MASS     | Matrix    | boot     |
| class      | cluster  | codetools | compiler |
| foreign    | grid     | lattice   | mgcv     |
| nlme       | nnet     | parallel  | rpart    |
| spatial    | splines  | stats4    | survival |
| tcltk      | tools    | boot      |  -        |

The packages in the top two lines are automatically loaded when you start R. You can load other packages using the *library* function. For example, to load the *MASS* library, which contains functions and data sets used in the book *Modern Applied Statistics with S* by Venables and Ripley, you call *library* as follows:

	library(MASS)

Other packages are available through the Comprehensive R Archive Network (CRAN); to obtain a package you use the `install.packages` function:

	install.packages("SuppDists")

	trying URL 'http://cran.fhcrc.org/src/contrib/SuppDists\_1.1-8.tar.gz'
	Content type 'application/x-gzip' length 139864 bytes (136 Kb)
	opened URL
	==================================================
	downloaded 136 Kb
	* installing *source* package 'SuppDists' ...
	** libs
	** arch - x86\_64
	g++ -arch x86\_64  -I/opt/REvolution/Revo-3.2/Revo64/R.framework/Resources/includ
	e -I/opt/REvolution/Revo-3.2/Revo64/R.framework/Resources/include/x86\_64  -I/usr
	/local/include    -fPIC  -g -O2 -c dists.cc -o dists.o
	g++ -arch x86\_64 -dynamiclib -Wl,-headerpad\_max\_install\_names -undefined dynamic
	\_lookup -single\_module -multiply\_defined suppress -L/usr/local/lib -o SuppDists.
	so dists.o -F/opt/REvolution/Revo-3.2/Revo64/R.framework/.. -framework R -Wl,-fr
	amework -Wl,CoreFoundation
	** R
	** preparing package for lazy loading
	** help
	*** installing help indices
	** building package indices ...
	* DONE (SuppDists)

	The downloaded packages are in
		'/private/var/folders/cy/cy2tNRpEHrmxPZPrN1EINU+++TI/-Tmp-/RtmpZnKn6Z/do
	wnloaded\_packages'
	Updating HTML index of packages in '.Library'

On Linux systems, you should not use CRAN as a source for third-party packages, because they may require a current version of R that may be different than that distributed with Microsoft R Server and R Client. Microsoft R Server and R Client sets the default repository to a fixed CRAN snapshot maintained at [mran.microsoft.com](http://mran.microsoft.com).

> [!Tip]
> The RevoScaleR package is included with every distribution of Microsoft R Server and R Client, and is automatically loaded into memory when you start the program. All of the “rx” RevoScaleR functions mentioned in this tutorial are at your fingertips.  You can get information on them by using the ? at the command line, for example: *?rxLinMod*

### Calling Microsoft R functions in Rscript and R CMD BATCH

Microsoft R Server and R Client are intended for high-performance computing and analytics, and some users are accustomed to running their analyses via batch mode and command-line scripting. *R CMD BATCH* generally works with Microsoft R Server and R Client with no modifications needed, but to get full advantage of the Microsoft R Server and R Client extensions with other command line invocations, you need to know a little bit about how Microsoft R Server and R Client work. 

Microsoft R is 100% R, with the standard R BLAS and LAPACK libraries substituted out for the Intel Math Kernel Libraries, and with a number of additional packages. Some of these packages are added to the default package list by the *Rprofile.site* file distributed with Microsoft R Server and R Client. If you use *Rscript* (or, on some systems, the equivalent Revoscript) with a Microsoft R Server or R Client script, be sure to add the flag `–default-packages=` to your call; this ensures that the Microsoft R default packages are loaded (including the *methods* package from base R).

Similarly, you should avoid the `–vanilla` construction for invoking Microsoft R Server or R Client; this method of invocation avoids evaluating the *Rprofile.site* file, so that this is equivalent to calling R without the Microsoft R Server or R Client extensions (except the MKL BLAS and LAPACK libraries).


### Learn about the optimized math libraries

One feature of Microsoft R Server and R Client is its inclusion of optimized libraries for linear algebra. These libraries are used throughout R’s modeling applications, including linear models, principal components analysis, and others.

Matrix multiplication, eigenvalue calculations, and singular value decompositions are significantly faster using these optimized libraries. For example, we ran the following computations, first with R built from source using the standard R BLAS, then with Microsoft R Server built with the optimized math libraries:

	set.seed(14)
	x <- matrix(rnorm(1000000),nrow=1000)
	xout <- numeric(20)
	for (i in 1:20) xout[i] <- system.time(eigen(x))[3]
	xout2 <- numeric(20)
	for (i in 1:20) xout[i] <- system.time(svd(x))[3]
	xout3 <- numeric(20)
	for (i in 1:20) xout[i] <- system.time(qr(x))[3]
	xout4 <- numeric(20)
	for (i in 1:20) xout[i] <- system.time(lm(x[,i]~x[,-i]))[3]
	xout5 <- numeric(20)
	for (i in 1:20) xout[i] <- system.time(t(x)%*%x)[3]

The mean times for each calculation are shown below. Matrix multiplication, eigenvalue, and singular value decomposition calculations show the greatest speedups.

| Calculation           | Reference Libraries  | Optimized Libraries  |
|-----------------------|-----------|-----------|
| eigen                 | 11.45350  | 3.63485   |
| svd                   | 7.34740   | 1.19455   |
| gr                    | 1.16765   | 0.93600   |
| lm                    | 1.47795   | 1.23430   |
| Matrix multiplication | 1.75610   | 0.09604   |


#### Learn about R Numerics

Most mathematical computations in R are performed using binary double-precision floating-point numbers. Arithmetic performed using these numbers is called *floating-point arithmetic*. In floating-point arithmetic, numbers are stored with finite precision according to internationally-recognized standards established by the IEEE. There are only finitely many floating-point numbers. In particular, there is a largest (and smallest) floating-point number. There is also a smallest nonnegative floating-point number. Consider, for example, the following R statements:

	10^308 * 10

		[1] Inf

	2^(-1074) / 2

		[1] 0

illustrating that R (and floating-point arithmetic) has a somewhat limited sense of the infinite and the minutely small. When one or more terms in a computation involve the special floating-point value *inf*, the computation is said to *overflow*. Similarly, *underflow* occurs when a number too small in magnitude to be represented is encountered.

The distance between the floating-point number \(1\) and the next-largest floating-point number is typically called *machine epsilon*, or just *eps* for short. We can compute *eps* with a simple R program:

	eps <- 1
	while ((1 + eps/2) != 1) { eps <- eps/2 }
	eps

	[1] 2.220446e-16

In a relative sense *eps* is as large as the gaps between floating-point numbers get.

In between their largest and smallest values, floating-point numbers generally only approximate real numbers. For example, although 1 and 10 *are* floating-point numbers, the rational number 1/10 has no exact binary floating-point representation. It is *very* closely approximated by a nearby floating-point number, but not close enough that arithmetic operations are always unaffected. Consider the following somewhat counter-intuitive R example:

	(11/10 - 1)*10—1

	[1] 8.881784e-16

Yet,

	(11/10)*10 - 1*10—1

	[1] 0

This example shows that floating-point arithmetic does not always obey the usual algebraic laws; here the distributive law is violated. The following example shows a situation in which the associative law is violated:

	x <- rnorm(100000)
	sum(x) - sum(sort(x))

	[1] -8.526513e-14

The violation of associativity results from the accumulation many small approximations over the summation. Re-ordering the computation can affect the result.

For a good introduction to the pitfalls involving numeric computation, read _"What every computer scientist should know about floating-point arithmetic"_ by Goldberg.

#### Performance Optimization and Numerics

Many of the most widely-used and compute-intensive linear algebra and arithmetic operations performed by R are computed by low-level numerics libraries, including the basic linear algebra subroutine (BLAS) library. Microsoft R includes highly-tuned low-level numerics libraries that are optimized for speed on a wide variety of x86, x86-64 and IA-64 processor architectures.

Many of the performance optimizations try to:

- Make use of available SIMD-style vector registers and operations

- Make efficient re-use of speedy processor memory caches.

Both approaches often require blocking or otherwise re-ordering computations, for example to fit in a small cache.

Because floating-point arithmetic always involves approximation, and the exact nature of the approximation depends upon the particular algorithms used, it is possible that the results from the highly-tuned numerics available in Microsoft R Server and R Client differ from results computed by other implementations of R, just as results can differ across system architectures. These computational differences typically manifest themselves on the order of machine epsilon.

Some high-performance numerics routines, including those that use x86 SIMD vector instructions (SSE, etc.), require that their input data be loaded into memory addresses divisible by 16 bytes. Unfortunately, R does not presently guarantee alignment of data on 16-byte boundaries. Therefore, it is possible that, depending on data alignment, parts of some computations may be grouped off differently (for less efficient computation). This alignment-dependent blocking of some computations can also result in differences from run to run on the order of machine epsilon.
