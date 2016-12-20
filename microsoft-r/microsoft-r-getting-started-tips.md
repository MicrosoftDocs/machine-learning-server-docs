---

# required metadata
title: "Tips on Computing with Big Data in R"
description: "Tips on Computing with Big Data in R"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/20/2016"
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

# Tips on Computing with Big Data in R

Big data is ubiquitous. The good news is that it provides great opportunities for the data analyst. With more data comes more information and more insight. You can relax assumptions required with smaller data sets and let the data speak for itself. But big data also presents problems. The size of data sets is now increasing much more rapidly than the speed of single cores, of RAM, and of hard drives. Most software tools aren’t handling this well; many of us have experienced that moment when our data has grown just a little too big—and our software has stopped working.

The use of the R statistical programming language has seen phenomenal growth because of R’s flexibility and power. Cutting-edge algorithms are written in R, and analysts love it for quick prototyping. But R users are faced with two big problems when scaling to big data: capacity and speed. Data sets typically must fit into memory, and even if they can, there are limits to what types of analyses can be done. Even without a capacity limit, computation may be too slow to be useful.

If you are used to working with smaller data sets in R, you will want to think differently about how you perform your analyses when using big data. If you are used to working in a High Performance Computing (HPC) environment, you will also want to think differently when you add analysis of big data to the picture. High Performance Computing is CPU centric, typically focusing on using many cores to perform lots of processing on small amounts of data. High Performance Analytics (HPA) is data centric. The focus is on feeding data to the cores—on disk I/O, data locality, efficient threading, and data management in RAM.

The **RevoScaleR** package that is included with Microsoft R Server and Microsoft R Client provides tools and examples for addressing the speed and capacity issues involved in High Performance Analytics. It provides data management and analysis functionality that scales from small, in-memory data sets to huge data sets stored on disk. The analysis functions are threaded to use multiple cores, and computations can be distributed across multiple computers (nodes) on a cluster or in the cloud.

Here are some tips for handling big data with R:

## Upgrade Your Hardware

It is always best to start with the easiest things first, and in some cases getting a better computer, or improving the one you have, can help a great deal. Usually the most important consideration is memory. If you are analyzing data that just about fits in R on your current system, getting more memory will not only let you finish your analysis, it is also likely to speed things up by a lot. This is because your operating system starts to “thrash” when it gets low on memory, removing some things from memory to let others continue to run. This can slow your system to a crawl. Getting more cores can also help, but only up to a point. R itself can generally only use one core at a time internally. In addition, for many data analysis problems the bottlenecks are disk I/O and the speed of RAM, so efficiently using more than 4 or 8 cores on commodity hardware can be difficult.

### Upgrade Your Software

Some software is simply more optimized for use with big data. For instance, getting better math libraries can greatly speed some computations. R allows its core math libraries to be replaced, and in Microsoft R Server and R Client they are replaced with extremely fast, threaded libraries. And, of course, the ***RevoScaleR*** package provides the underlying high-performance compute engine used by its Big Data Big Analytics algorithms.

## Minimize Copies of Data

When working with small data sets, an extra copy is not a problem. With big data it can slow the analysis, or even bring it to a screeching halt. Be aware of the ‘automatic’ copying that occurs in R. For example, if a data frame is passed into a function, a copy is only made if the data frame is modified. But if a data frame is put into a list, a copy is automatically made. In many of the basic analysis algorithms, such as *lm* and *glm*, multiple copies of the data set are made as the computations progress, resulting in serious limitations in processing big data sets. The RevoScaleR analysis functions (for instance, *rxSummary*, *rxCube*, *rxLinMod*, *rxLogit,* *rxGlm*, *rxKmeans*) are all implemented with a focus on efficient use of memory; data is not copied unless absolutely necessary. The plot below shows an example of how reducing copies of data and tuning algorithms can dramatically increase speed and capacity.

![](media/rserver-getting-started/image11.jpeg)

## Process Data in Chunks

Processing your data a chunk at a time is the key to being able to scale your computations without increasing memory requirements. External memory (or “out-of-core”) algorithms don’t require that all of your data be in RAM at one time. Data is processed a chunk at time, with intermediate results updated for each chunk. When all of the data is processed, final results are computed. The core functions provided with **RevoScaleR** all process data in chunks. So, if the number of rows of your data set doubles, you can still perform the same data analyses—it will just take longer, typically scaling linearly with the number of rows. Your analysis is not bound by memory constraints. The plot below shows how data chunking allows unlimited rows in limited RAM.

![](media/rserver-getting-started/image12.jpeg)

The *biglm* package, available on CRAN, also estimates linear and generalized linear models using external memory algorithms, although they are not parallelized.

## Compute in Parallel Across Cores or Nodes

Using more cores and more computers (nodes) is the key to scaling computations to really big data. Since data analysis algorithms tend to be I/O bound when data cannot fit into memory, the use of multiple hard drives can be even more important than the use of multiple cores.

The **RevoScaleR** analysis functions are written to automatically compute in parallel on available cores, and can also be used to automatically distribute computations across the nodes of a cluster. These functions combine the advantages of external memory algorithms (see [Process Data in Chunks](#process-data-in-chunks) above) with the advantages of High Performance Computing. That is, these are Parallel External Memory Algorithm’s (PEMA’s)—external memory algorithms that have been parallelized. Such algorithms process data a chunk at a time in parallel, storing intermediate results from each chunk and combining them at the end. Iterative algorithms repeat this process until convergence is determined. Any external memory algorithm that is not “inherently sequential” can be parallelized; results for one chunk of data cannot depend upon prior results. Dependence on data from a prior chunk is OK, but must be handled specially. The plot below shows an example of how using multiple computers can dramatically increase speed, in this case taking advantage of memory caching on the nodes to achieve super-linear speedups.

![](media/rserver-getting-started/image13.jpeg)

Microsofts’ *foreach* package, which is open source and available on CRAN, provides very easy- to-use tools for executing R functions in parallel, both on a single computer and on multiple computers. This is particularly useful for “embarrassingly parallel” types of computations such as simulations, which do not involve lots of data and do not involve communication among the parallel tasks.

## Take Advantage of Integers

In R the two choices for “continuous” data are *numeric*, which is an 8 byte (double) floating point number and *integer*, which is a 4 byte integer. If your data can be stored and processed as an integer, there can be big advantages to doing so. First, it will only take half of the memory. Second, in some cases integers can be processed much faster than doubles. For example, if you have a variable whose values are integral numbers in the range from 1 to 1000 and you want to find the median, it is much faster to count all the occurrences of the integers than it is to sort the variable. A tabulation of all the integers, in fact, can be thought of as a way to compress the data with no loss of information. The resulting tabulation can be converted into an exact empirical distribution of the data by dividing the counts by the sum of the counts, and all of the empirical quantiles including the median can be obtained from this. The R function *tabulate* can be used for this, and is very fast. The following code illustrates this:

	nd = sample(as.numeric(1:1000),size = 1e+8, replace = TRUE)
	system.time(nit <- tabulate(as.integer(nd)))
	system.time(nds <- sort(nd))

A vector of 100 million doubles is created, with randomized integral values in the range from 1 to 1,000. Sorting this vector takes about 15 times longer than converting to integers and tabulating, and 25 times longer if the conversion to integers is not included in the timing (this is relevant if you convert to integers once and then operate multiple times on the resulting vector).

Sometimes decimal numbers can be converted to integers without losing information. An example is temperature measurements of the weather, such as 32.7, which can be multiplied by 10 to convert them into integers.

**RevoScaleR** provides several tools for the fast handling of integral values. For instance, in formulas for linear and generalized linear models and other analysis functions, the “F()” function can be used to virtually “convert” numeric variables into factors, with the levels represented by integers. The *rxCube* function allows rapid tabulations of factors and their interactions (for example, age by state by income) for arbitrarily large data sets.

## Store Your Data Efficiently

If your data doesn’t easily fit into memory, you’ll want to store it efficiently for fast access from disk. If you use appropriate data types, you can save on storage space and access time. Take advantage of integers, and store data in 32-bit floats not 64-bit doubles. A 32-bit float can represent 7 decimal digits of precision, which is more than enough for most data, and it takes up half the space of doubles. (Save the 64-bit doubles for computations).

Recognize that relational databases are not always optimal for storing data for analysis. Even with the best indexing they are typically not designed to provide fast sequential reads of blocks of rows for specified columns, which is the key to fast access to data on disk. **RevoScaleR** provides an efficient .xdf file format that provides storage of a wide variety of data types, and is designed for fast sequential reads of blocks of data. There are tools for rapidly accessing data in .xdf files from R and for importing data into this format from SAS, SPSS, and text files and SQL Server, Teradata and ODBC connections.

## Only Read in The Data That Is Needed

Even though a data set may have many thousands of variables, typically not all of them are being analyzed at one time. If you have your entire data set in memory, you can easily run into out-of-memory problems. By just reading from disk the actual variables and observations you will use in analysis, you can speed up the analysis considerably. **RevoScaleR** functions provide this automatically. For example, when estimating a model, only the variables used in the model are read from the .xdf file.

## Avoid Loops when Transforming Data

It is well-known that processing data in loops in R can be very slow compared with vector operations. For example, if you compare the timings of adding two vectors, one with a loop and the other with a simple vector operation, you will find the vector operation to be orders of magnitude faster:

	n <- 1000000
	x1 <- 1:n
	x2 <- 1:n
	y <- vector()
	system.time( for(i in 1:n){ y[i] <- x1[i] + x2[i] })
	system.time( y <- x1 + x2)

On a good laptop the loop over the data was timed at about 430 seconds, while the vectorized add is barely timeable. In R the core operations on vectors are typically written in C, C++ or FORTRAN, and these compiled languages can provide much greater speed for this type of code than can the R interpreter.

## Use C, C++, or FORTRAN for Critical Functions

One of the best features of R is its ability to integrate easily with other languages, including C, C++, and FORTRAN. You can pass R data objects to other languages, do some computations, and return the results in R data objects. It is typically the case that only small portions of an R program can benefit from the speedups that compiled languages like C, C++, and FORTRAN can provide. Indeed, much of the code in the base and recommended packages in R is written in this way—the bulk of the code is in R but a few core pieces of functionality are written in C, C++, or FORTRAN. The type of code that benefits the most from this involves loops over data vectors. The package *Rcpp,* which is available on CRAN, provides tools that make it very easy to convert R code into C++ and to integrate C and C++ code into R. Of course, before writing code in another language, it pays to do some research to see if the type of functionality you want is already available in R, either in the base and recommended packages or in a 3<sup>rd</sup> party package. For example, all of the core algorithms for the **RevoScaleR** package are written in optimized C++ code. It also pays to do some research to see if there is publically-available code in one of these compiled languages that does what you want.

## Process Data Transformations in Batches

When working with small data sets, it is common to perform data transformations one at a time. For instance, one line of code might create a new variable, and the next line might multiply that variable by 10. Each of these lines of code processes all rows of the data. With a big data set that cannot fit into memory, there can be substantial overhead to making a pass through the data. With **RevoScaleR**’s *rxDataStep* function you can specify multiple data transformations that can be performed in just one pass through the data, processing the data a chunk at a time. A little planning ahead can save a lot of time.

## User Row-Oriented Data Transformations where Possible

When data is processed in chunks, basic data transformations for a single row of data should in general not be dependent on values in other rows of data. The key is that your transformation expression should give the same result even if only some of the rows of data are in memory at one time. Data manipulations using lags can be done but require special handling.

## Handle Categorical Variables Efficiently and with Care

Categorical or factor variables are extremely useful in visualizing and analyzing big data, but they need to be handled efficiently with big data because they are typically expanded when used in modeling. For example, if you use a factor variable with 100 categories as an independent variable in a linear model with *lm*, behind the scenes 100 dummy variables are created when estimating the model. The analysis modeling functions in **RevoScaleR** use special handling of categorical data to minimize the use of memory when processing them; they do not generally need to explicitly create dummy variable to represent factors.

Creating factor variables also often takes more careful handling with big data sets. This is because not all of the factor levels may be represented in a single chunk of data. For example, if you want to use the *factor* function in a data transformation used on chunks of data, you must explicitly specify the levels or you might end up with incompatible factor levels from chunk to chunk. The *rxImport* and *rxFactors* functions in **RevoScaleR** provide functionality for creating factor variables in big data sets.

## Be Aware of Output with the Same Number of Rows as Your Data

Most analysis functions return a relatively small object of results that can easily be handled in memory. But occasionally, output will have the same number of rows as your data, for example, when computing predictions and residuals from a model. In order for this to scale, you will want the output written out to a file rather than kept in memory. For this reason, the **RevoScaleR** modeling functions such as *rxLinMod*, *rxLogit*, and *rxGlm* do not automatically compute predictions and residuals. The *rxPredict* function provides this functionality and can add predicted values to an existing .xdf file.

## Think Twice Before Sorting

When working with small data sets, it is common to sort data at various stages of the analysis process. Although **RevoScaleR**’s *rxSort* function is very efficient for .xdf files and can handle data sets far too large to fit into memory, sorting is by nature a time-intensive operation, especially on big data.

One of the major reasons for sorting is to compute medians and other quantiles. As noted above in the section on taking advantage of integers, when the data consists of integral values, a tabulation of those values is generally much faster than sorting and gives exact values for all empirical quantiles. Even when the data is not integral, scaling the data and converting to integers can give very fast and accurate quantiles. As an example, if the data consists of floating point values in the range from 0 to 1,000, converting to integers and tabulating will bound the median or any other quantile to within two adjacent integers. Interpolation within those values can get you closer, as can a small number of additional iterations. If the original data falls into some other range (for example, 0 to 1), scaling to a larger range (for example, 0 to 1,000) can accomplish the same thing. The *rxQuantile* function uses this approach to rapidly compute approximate quantiles for arbitrarily large data.

Another major reason for sorting is to make it easier to compute aggregate statistics by groups. If the data are sorted by groups, then contiguous observations can be aggregated. However, it is often possible, and much faster, to make a single pass through the original data and to accumulate the desired statistics by group. The *aggregate* function can do this for data that fits into memory, and **RevoScaleR**’s *rxSummary*, *rxCube*, and *rxCrossTabs* provide extremely fast ways to do this on large data.

The **RevoScaleR** functions *rxRoc*, and *rxLorenz* are other examples of ‘big data’ alternatives to functions that traditionally rely on sorting.

In summary, by using the tips and tools outlined above you can have the best of both worlds: the ability to rapidly extract information from big data sets using R and the flexibility and power of the R language to manipulate and graph this information.

# Getting Started with Big Data in R

The **RevoScaleR** package, included in Microsoft R Server and R Client, provides a framework for quickly writing start-to-finish, scalable R code for data analysis. Even if you are relatively new to R, you can get started with just a few basic functions.

## Step 1: Accessing Your Data with *rxImport*

The *rxImport* function allows you to import data from fixed or delimited text files, SAS files, SPSS files, or a SQL Server, Teradata or ODBC connection. There’s no need to have SAS or SPSS installed on your system to import those file types, but you will need an ODBC driver for your data base to access it. Let’s use an example of a delimited text file available in the sample data directory of the **RevoScaleR** package, a small data set containing simulated mortgage default data. We’ll store the location of the file in a character string, then import the data into an in-memory data set (data frame) using the default settings:

	inDataFile <- file.path(rxGetOption("sampleDataDir"),
		"mortDefaultSmall2000.csv")

	mortData <- rxImport(inData = inDataFile)

If we think that we may want to do the same analysis on a larger data set later, we can prepare for that by putting placeholders in our code for output files. If we set these values to a file path, the data will be stored on disk in the efficient .xdf file format, rather than storing it in memory. The output object returned from *rxImport* would be a small object representing the .xdf file (an *RxXdfData* object), rather than a data frame containing all of the data. But for the time being, we will continue to work with the data in memory. We can do this by setting the *outFile* parameters to NULL. The following code will accomplish the same importing task of that above:

	outFile <- NULL
	outFile2 <- NULL
	mortData <- rxImport(inData = inDataFile, outFile = outFile)

## Step 2: A Quick Look at the Data

There are a number of basic methods we can use to quickly get some information about the data set and its variables that will work on the output of *rxImport*, whether it is a data frame or *RxXdfData* object. For example, to get the number of rows, cols, and names of the imported data:

	nrow(mortData)
	ncol(mortData)
	names(mortData)

	> nrow(mortData)
	[1] 10000
	> ncol(mortData)
	[1] 6
	> names(mortData)
	[1] "creditScore" "houseAge" "yearsEmploy" "ccDebt" "year"
	[6] "default"

To print out the first few rows of the data set, you can use *head()*:

	head(mortData, n = 3)

	creditScore houseAge yearsEmploy ccDebt year default
	1 691 16 9 6725 2000 0
	2 691 4 4 5077 2000 0
	3 743 18 3 3080 2000 0

The *rxGetInfo* function allows you to quickly get information about your data set and its variables all at one time, including more information about variable types and ranges. Let’s try it on *mortData*, having the first 3 rows of the data set printed out:

	rxGetInfo(mortData, getVarInfo = TRUE, numRows=3)

The output shows us:

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

## Step 3: Data Selection and Transformations with *rxDataStep*

The *rxDataStep* function provides a framework for the majority of your data manipulation tasks. It allows for row selection (the *rowSelection* argument), variable selection (the *varsToKeep* or *varsToDrop* arguments), and the creation of new variables from existing ones (the *transforms* argument). Here’s an example that does all three with one function call:

	mortDataNew &lt;- rxDataStep(
		# Specify the input data set
		inData = mortData,
		# Put in a placeholder for an output file
		outFile = outFile2,
		# Specify any variables to keep or drop
		varsToDrop = c("year"),
		# Specify rows to select
		rowSelection = creditScore &lt; 850,
		# Specify a list of new variables to create
		transforms = list(
			catDebt = cut(ccDebt, breaks = c(0, 6500, 13000),
				labels = c("Low Debt", "High Debt")),
			lowScore = creditScore &lt; 625))

Our new data set, *mortDataNew*, will not have the variable year, but adds two new variables: a categorical variable named *catDebt* that uses R’s cut function to break the *ccDebt* variable into two categories, and a logical variable, *lowScore*, that will be TRUE for individuals with low credit scores. These *transforms* expressions follow the rule that they must be able to operate on a chunk of data at a time; that is, the computation for a single row of data cannot depend on values in other rows of data. With the *rowSelection* argument, we have also removed any observations with high credit scores, above or equal to 850. We can use the *rxGetVarInfo* function to confirm:

	rxGetVarInfo(mortDataNew)

	Var 1: creditScore, Type: integer, Low/High: (486, 847)
	Var 2: houseAge, Type: integer, Low/High: (0, 40)
	Var 3: yearsEmploy, Type: integer, Low/High: (0, 14)
	Var 4: ccDebt, Type: integer, Low/High: (0, 12275)
	Var 5: default, Type: integer, Low/High: (0, 1)
	Var 6: catDebt
		2 factor levels: Low Debt High Debt
	Var 7: lowScore, Type: logical, Low/High: (0, 1)

## Step 4: Visualizing Your Data with *rxHistogram*, *rxCube*, and *rxLinePlot*

The *rxHistogram* function will show us the distribution of any of the variables in our data set. For example, let’s look at credit score:

	rxHistogram(~creditScore, data = mortDataNew )

![](media/rserver-getting-started/image14.png)

The *rxCube* function will compute category counts, and can operate on the interaction of categorical variables. Using the *F()* notation to convert a variable into an on-the-fly categorical factor variable (with a level for each integer value), we can compute the counts for each credit score for the two groups who have low and high credit card debt:

	mortCube <- rxCube(~F(creditScore):catDebt, data = mortDataNew)

The *rxLinePlot* function is a convenient way to plot output from *rxCube*. We use the *rxResultsDF* helper function to convert cube output into a data frame convenient for plotting:

	rxLinePlot(Counts~creditScore|catDebt, data=rxResultsDF(mortCube))

![](media/rserver-getting-started/image15.png)

## Step 5: Analyzing Your Data with *rxLogit*

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

## Step 6: Scaling Your Analysis

So, we’ve finished experimenting with our small data set in memory. Let’s scale up to a data set with a million rows rather than just 10000. These larger text data files are available [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). (Windows users should download the zip version, mortDefault.zip, and Linux users mortDefault.tar.gz). After downloading and unpacking the data, set your path to the correct location in the code below. It will be more efficient to store the imported data on disk, so we’ll also specify the locations for our imported and transformed data sets:

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



# Next Steps: A Roadmap to Documentation

Having completed the tutorials, you are now ready to dive right in and start using R for your own purposes. While the tutorials have given you the basic tools to begin exploring, you may still want more guidance for your specific tasks. Luckily, there is a huge library of Microsoft R, open-source R, and S documentation that can help you perform almost any task with R. This brief roadmap points you toward some of the most useful documentation that Microsoft is aware of. (If you find other useful resources, drop us a line at revodoc@microsoft.com!)

The obvious place to start is with the rest of the **Microsoft R** document set, which includes documentation on the **RevoScaleR** package for scalable data analysis (on all platforms). You can find this documentation on this site using the table of contents on this page.

Next, you should be aware of the R Core Team manuals, which are part of every R distribution, including *An Introduction to R*, *The R Language Definition*, *Writing R Extensions* and so on.

Beyond the standard R manuals, there are many other resources. [Learn about them here](microsoft-r-more-resources.md).

# Appendix

### Optimized Math Libraries

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


#### More on R Numerics

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


## Getting Function Help

Know a function’s name, but not how to call it? Need examples of how to set up the data for a function? Help is just a few keystrokes away. R has two main functions for obtaining help: the ? operator and the help function. You can use the operator by simply typing a question mark at the prompt, followed by the name of the function you want to know about:

	?q

Depending on your operating system (and whether you have started the GUI help browser), help for the q function will then appear either in your console window or in a separate window.

The help function is much the same:

	help(q)

Most users will probably use ? because it is easy to type; help allows you to specify a number of arguments that can extend its usefulness.
