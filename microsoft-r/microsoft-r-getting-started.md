---

# required metadata
title: "Microsoft R Server and R Client Getting Started Guide"
description: "Microsoft R features and components overview."
keywords: ""
author: "j-martens"
manager: "paulette.mckay"
ms.date: "06/09/2016"
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

# Microsoft R Getting Started Guide

##Microsoft R Products

R is the world’s most powerful, and preferred, programming language for statistical computing, machine learning, and graphics, and is supported by a thriving global community of users, developers, and contributors. Developers frequently provide tools incorporating their expertise in the form of R packages. Traditionally, using R in an enterprise setting has presented certain challenges, especially as the volume of data rises, or when faced with a need to deploy solutions to production environments. 

For a side by side comparison of R features in Microsoft R Server, R Client, and R Open, [see here](index.md#compare-prods).

###Microsoft R Server
Microsoft R Server is R for the Enterprise and solves the problem of deployment and operationalization of R code. In addition to the over 7000 standard R packages available to all R users, Microsoft R Server provides additional the R packages and connectivity tools to enable remote compute context and to support scalable, parallelizable solutions.

Performance of R solutions in Microsoft R Server is expected to generally be better than any conventional R implementation, given the same hardware, because R can be run using server resources and sometimes distributed to multiple processes. Depending on the input data and the processing of it, queries can be distributed across multiple partitions for faster processing.

Users can also expect to see considerable differences in performance and scalability for the same ScaleR functions if run in R Server versus being run locally in R Client. Reasons include support for chunking, increased threads available for R worker processing and parallel processing not only with standard R packages, but also with ScaleR functions. R Server offers optimized performance and scalability through parallelization and streaming.

However, performance even on identical hardware can be affected by many factors outside the R code, including competing demands on server resources, the type of query plan that is created, schema changes, the need to update statistics or create a new query plan, fragmentation and so on. It is possible that a stored procedure containing R code might run in seconds under one workload, but take minutes when there are other services running. We recommend that you monitor multiple aspects of server performance, including networking for remote compute contexts, when quantifying R job performance.

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization. The DeployR package, available for Microsoft R Server only, provides the tools for doing just that; it is a full-featured web services software development kit for R which allows programmers to use Java, JavaScript or .Net to integrate the R analysis output with a third party package. [Learn more about DeployR...](deployr-about.md)

|Microsoft R Server Editions|Description                                                          |Install|ScaleR Get Started|
|---------------------------|---------------------------------------------------------------------|:-------:|:------------------:|
|R Server for Hadoop        |Scale your analysis transparently by distributing work across nodes without complex programming|[Doc](rserver-install-hadoop.md)|[Doc](scaler-hadoop-getting-started.md)|
|R Server for Teradata DB   |Run advanced analytics in-database for seamless data analysis|[Doc](rserver-install-teradata-server.md)|[Doc](scaler-teradata-getting-started.md)|
|R Server for Linux         |Bring predictive and prescriptive analytics power to your Linux environments|[Doc](rserver-install-linux-server.md)|[Doc](scaler-getting-started.md)|


##Microsoft R Client

Microsoft R Client is a free, community-supported, data science tool for high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the powerful ScaleR technology and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server for Hadoop. 

Learn how to [install Microsoft R Client](install-r-client-windows.md).


##Microsoft R Open

Microsoft R Open is the enhanced distribution of R from Microsoft Corporation. It is a complete open source platform for statistical analysis and data science. Being based on the open source R engine makes Microsoft R Open fully compatibility with all R packages, scripts and applications that work with that version of R. Microsoft R Open delivers [performance boosts](https://mran.microsoft.com/documents/rro/multithread/#mt-bench), in comparison to the standard R distribution, since R Open leverages high-performance, multi-threaded math libraries. Like open source R from CRAN, Microsoft R Open is open source and free to download, use, and share.   

Microsoft R Open provides limited performance and scalability in comparison to Microsoft R Server and Microsoft R Client Editions. Specifically, none of the proprietary ScaleR functions and packages included with Microsoft R Server and Microsoft R Client are available in standalone Microsoft R Open. Also, data that can be processed is limited to the data that can fit in server memory.

Visit the [MRAN Website](https://mran.microsoft.com/) to learn more about Microsoft R Open and download it.

## What's in R Server and R Client

For a high-level, side-by-side comparison of R features in Microsoft R Server, R Client, and R Open, [see here](index.md##compare-prods).

**DistributedR: Parallel and distributed computing framework for _Big Data Big Analytics_.**

One of the limitations of R frequently encountered is scalability. R has many tools and techniques for handling small problems, but when the data set to be analyzed starts to get big, speed and memory limitations can be a problem. The Microsoft R ‘Big Data Big Analytics’ platform is built upon a high-performance, scalable computing framework that eradicates these technology barriers.

This ‘Big Data Big Analytics’ compute engine works behind-the-scenes to process computations in parallel and, if available, distribute them across nodes of a distributed compute environment such as clusters or Massively Parallel Processing (MPP) databases. While you don’t interact directly with DistributedR, it is the framework that allows Microsoft R Server to break through the technology barriers in R to deliver blindingly fast results on enterprise compute platforms.

*DistributedR* allows you to run the same R script on multiple platforms; you can create a model in one environment such as a workstation and then deploy it on a different environment such as an on-site Microsoft SQL Server, a Teradata platform, or a Hadoop cluster in the cloud. You just need to specify the information about where these computations should be performed and what data should be analyzed.

This ‘Big Data Big Analytics’ compute engine is the core of the RevoScaleR package, included in your distribution of Microsoft R Server and Microsoft R Client. For information on supported computing environments, look for the [‘compute contexts’ in the RevoScaleR package](scaler-getting-started.md#computecontext).

<br>
**ScaleR: High performance, scalable, parallelized and distributable ‘Big Data Big Analytics’ in R.**

The ‘Big Data Big Analytics’ functions built on DistributedR provide high performance, parallelized, and distributable analytics functions that scale from small data sets in memory to huge data sets stored on disk on a cluster of computers. The analytics functions provided include summary statistics, cubes and crosstabs, linear models, logistic regression, generalized linear models, kmeans clustering, decision trees, and decision forests. These algorithms are parallelized and distributed automatically, and process data in chunks so that all of your data does not need to be in memory at one time; you can use the same analysis code for your giant data set as you do for a small data set in memory.

RevoScaleR also provides traditional ‘high performance computing’ (HPC) tools if you prefer to construct your own distributed computations. In addition, in many environments, there are full-featured tools for data cleaning and manipulation.

R is a flexible and powerful statistical programming language. The RevoScaleR package provides efficient, scalable computational power. Combining the two allows for the development of ready-to-deploy suites of data processing and analytics with R.

To learn more, look for the [RevoScaleR ‘rx’ analysis and data manipulation functions](scaler-user-guide-data-transform.md) and [‘rxExec’ for HPC functionality](scaler-distributed-computing.md). If you are computing decision trees, also check out the included [RevoTreeView package](scaler-user-guide-decision-tree.md) that allows you to interactively visualize your decision trees.

<br>
**ConnectR: Move your data efficiently and work with data in a variety of formats, including SAS, SPSS, Hadoop, and text files.**

A key to data analysis is, of course, the data. The RevoScaleR package provides a way for you to connect with the data you may have stored in a variety of formats, such as SAS, SPSS, Teradata, ODBC, delimited and fixed format text, and Hadoop Distributed File System (HDFS) text files. You have a choice of:

1. Analyzing data in place directly with RevoScaleR analysis functions
1. Loading it into your local R development environment using the efficient and performant .xdf file format
1. Bringing some or all of your data into memory as an R data frame to use with any R analysis function

To learn more, look for [data sources in the RevoScaleR package](scaler-user-guide-data-import.md).

<a name="deployr-intro"></a>
<br>
**DeployR, the R Integration Server, is an optional framework for deploying R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. **


In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization. The optional DeployR package, available for Microsoft R Server only, provides the tools for doing just that. 

DeployR is an integration technology for deploying R analytics inside web, desktop, mobile, and dashboard applications as well as backend systems. DeployR turns your R scripts into [analytics web services](#analytics-web-service), so R code can be easily executed by applications running on a secure server.

Using analytics web services, DeployR also solves key integration problems faced by those adopting R-based analytics alongside existing IT infrastructure. These services make it easy for application developers to collaborate with data scientists to integrate R analytics into their applications without any R programming knowledge.

DeployR Enterprise scales for business-critical applications and offers support for production-grade workloads, as well as seamless integration with popular [enterprise security solutions](deployr-admin-security/deployr-security.md) such as single sign-on (SSO), Lightweight Directory Access Protocol (LDAP), Active Directory, or Pluggable Authentication Modules (PAM).

[Learn more about DeployR...](deployr-about.md)

## Starting & Stopping Microsoft R

### Starting Microsoft R

**To launch Microsoft R Server on Linux**:

1. Open a terminal or console window.
1. At the prompt, type: 
```
Revo64
```

If you get the message “Revo64: Command not found," this means that Microsoft R Server is not in your search path. Check with your system administrator to find the correct path to your R Server installation, then modify your search path (typically in your .bashrc file):
```
PATH=$PATH:/path/to/Microsoft R Server
export PATH
```

<br>
**To launch Microsoft R Client**:

After you have installed the software, you launch Microsoft R Client as follows.

For Windows 10:

+ Choose **All Apps > Microsoft R Client > Rgui**.


For Windows 8.1:

1. Move the pointer to the lower left corner of the Desktop until the **Start** icon appears.
  
1. Click **Start** to view the **Start** screen.

1. Locate and click the tile for **Microsoft R Client**.


For Windows 7:

+ From the **Task Bar**, choose **Start > All Programs > Microsoft R Client > Rgui**.

<br />
### Stopping Microsoft R

From any command-line version of R, the standard way to exit is by calling the q function. All R functions are called by typing the name of the function, followed by a pair of parentheses that may include one or more arguments. So, to quit R, you call q with no arguments, following the R prompt &gt;:

	q()

Whenever you quit R, you are asked if you want to save the workspace image; if you have created functions or data that you want to keep, saving the workspace image will preserve them for future use. (Most R users, however, create their functions and data in script files which can be read, or *sourced*, into R. If you follow this model, you will usually say “no” to saving the workspace image.)


## Getting Function Help

Know a function’s name, but not how to call it? Need examples of how to set up the data for a function? Help is just a few keystrokes away. R has two main functions for obtaining help: the ? operator and the help function. You can use the operator by simply typing a question mark at the prompt, followed by the name of the function you want to know about:

	?q

Depending on your operating system (and whether you have started the GUI help browser), help for the q function will then appear either in your console window or in a separate window.

The help function is much the same:

	help(q)

Most users will probably use ? because it is easy to type; help allows you to specify a number of arguments that can extend its usefulness.

## An R Tutorial in 25 Functions or So

To get you started with Microsoft R Server and R Client, this brief tutorial will present you with 25 (or so) of the most commonly used R functions. You can learn to load your own small data sets into R, and begin to do useful analysis on them. We’ll also provide some initial tips on the next steps for performing scalable data analysis in R. 

### Creating Vectors

R is an environment for analyzing data, so the natural starting point is with some data to analyze. For small data sets, such as the following 20 measurements of the speed of light taken from the famous Michelson-Morley experiment, it is simplest to use R’s *c* function to combine the data into a vector. Type the following at the &gt;  prompt at the beginning of the line:

	c(850, 740, 900, 1070, 930, 850, 950, 980, 980, 880,
	1000, 980, 930, 650, 760, 810, 1000, 1000, 960, 960)

 When you type the closing parenthesis and press *Enter*, R responds as follows:

	[1]  850  740  900 1070  930  850  950  	980  980  880  
	[11]  1000 980  930  650  760  810 1000 1000  960  960

This indicates that R has interpreted what you typed, created a vector with 20 elements, and returned that vector. But we have a problem. R hasn’t saved what we typed. If we want to use this vector again (and that’s the usual reason for creating a vector in the first place), we need to *assign* it. The R assignment operator has the suggestive form *&lt;-* to indicate a value is being assigned to a name. You can use most combinations of letters, numbers, and periods to form names (but note that names can’t begin with a number); here we’ll use michelson:

	michelson <- c(850, 740, 900, 1070, 930, 850, 950, 980, 980, 880,
	1000, 980, 930, 650, 760, 810, 1000, 1000, 960, 960)

Now R responds with a prompt; the vector is not automatically printed when it is assigned. But now we can view the vector we created by typing its name at the prompt:

	michelson

	[1]  850  740  900 1070  930  850  950  980   980  880
	[11] 1000  980  930  650  760  810 1000 1000  960  960

The `c` function is useful for typing in small vectors such as you might find in textbook examples, and it is also useful for combining existing vectors. For example, if we discovered another five observations that extended the Michelson-Morley data, we could extend the vector using c as follows:

	michelsonNew <- c(michelson, 850, 930, 940, 970, 870)
	michelsonNew

	[1]  850  740  900 1070  930  850  950  	980  980  880
	[11] 1000  980  930  650  760  810 1000 1000  960  960
	[21]  850  930  940  970  870

Often for testing purposes you want to use randomly generated data. R has a number of built-in distributions from which you can generate random numbers; two of the most commonly used are the normal and the uniform distributions. To obtain a set of numbers from a normal distribution, you use the *rnorm* function:

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

 By default, the data are generated from a standard normal with mean 0 and standard deviation 1. You can use the `mean` and *sd* arguments to *rnorm* to specify a different normal distribution:
	
	normalSat <- rnorm(25, mean=450, sd=100)
	normalSat

	[1] 373.3390 594.3363 534.4879 410.0630 307.2232
	[6] 307.8008 417.1772 478.4570 521.9336 493.2416
	[11] 414.8075 479.7721 423.8568 580.8690 451.5870
	[16] 406.8826 488.2447 454.1125 444.0776 320.3576
	[21] 236.3024 360.6385 511.2733 508.2971 449.4118

Similarly, you use the *runif* function to generate random data from a uniform distribution:

	uniformDat <- runif(25)
	uniformDat

	[1] 0.03105927 0.18295065 0.96637386 0.71535963
	[5] 0.16081450 0.15216891 0.07346868 0.15047337
	[9] 0.49408599 0.35582231 0.70424152 0.63671421
	[13] 0.20865305 0.20167994 0.37511929 0.54082887
	[17] 0.86681824 0.23792988 0.44364083 0.88482396
	[21] 0.41863803 0.42392873 0.24800036 0.22084038
	[25] 0.48285406

The default uniform distribution is that over the interval 0 to 1; you can specify alternatives by setting the *min* and *max* arguments:

	uniformPerc <- runif(25, min=0, max=100)
	uniformPerc

	[1] 66.221400 12.270863 33.417174 21.985229
	[5] 92.767213 17.911602  1.935963 53.551991
	[9] 75.110760 22.436347 63.172258 95.977501
	[13] 79.317351 56.767608 89.416080 79.546495
	[17]  8.961152 49.315612 43.432128 68.871867
	[21] 73.598221 63.888835 35.261694 54.481692
	[25] 37.575176

Another commonly used vector is the *sequence*, a uniformly-spaced run of numbers. For the common case of a run of integers, you can use the infix operator, :, as follows:

	1:10

	 [1]  1  2  3  4  5  6  7  8  9 10

For more general sequences, use the *seq* function:

	seq(length = 11, from = 10, to = 30)

	[1] 10 12 14 16 18 20 22 24 26 28 30

	seq(from = 10,length = 20, by = 4)

	[1] 10 14 18 22 26 30 34 38 42 46 50 54 58 62 66 70 74
	[18] 78 82 86

> Big Data Big Analytics Tip: If you are working with big data, you’ll use vectors regularly to manipulate parameters and information about your data.  However, you’ll typically want to store your big data sets in the RevoScaleR high-performance .xdf file format.

#### Exploratory Data Analysis

Once you have some data, you will want to explore it graphically. For most small data sets, the place to begin is with the plot function, which provides a default graphical view of the data:

	plot(michelson)
	plot(normalDat)
	plot(uniformPerc)
	plot(1:10)

For *numeric* vectors such as ours, the default view is a scatter plot of the observations against their index, resulting in the following plots:

![](media/rserver-getting-started/image5.jpeg)

For an exploration of the shape of the data, the usual tools are *stem* (to create a stemplot) and *hist* (to create a histogram):

	stem(michelson)

	 The decimal point is 2 digit(s) to the right of the |
	  6 | 5
	  7 | 46
	  8 | 1558
	  9 | 033566888
	 10 | 0007

	hist(michelson)

The resulting histogram is shown as the left plot below. We can make the histogram look more like the stemplot by specifying the *nclass* argument to *hist*:

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

>*Big Data Big Analytics Tip*: These plots are great if you have a small data set in memory.  When working with big data, some plot types may not be very informative when working directly with the data (e.g., scatter plots can produce a big blob of ink) and others may be computational intensive (e.g., require sorting data).  A good starting place is the *rxHistogram* function in RevoScaleR that efficiently computes and renders histograms for large data sets.  And remember that RevoScaleR functions such as *rxCube* can provide summary information that is easily amenable to the impressive plotting capabilities provided by R packages.

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

> *Big Data Big Analytics Tip*: The *rxSummary* function in RevoScaleR will efficiently compute summary statistics for a data frame in memory or a large data file stored on disk.

### Creating Multivariate Data Sets

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

You can then read the data into R using the *read.table* function. The argument *header=TRUE* specifies that the first line is a header of variable names:

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

(Notice how *read.table* changed the names of our original “2B" and “3B" columns to be valid R names; R names cannot begin with a numeral.)

Most small R data sets in daily use are data frames. The built-in package, *datasets,* is a rich source of data frames for further experimentation. In the next section, we turn our attention to the built-in data set *attitude*, part of the *datasets* package.

>*Big Data Big Analytics Tip*: Check out the *rxImport* function for an efficient and flexible way to bring data stored in a variety of data formats (e.g., text, SQL Server, ODBC, SAS, SPSS, Teradata) into a data frame in memory or an .xdf file.

### Linear Models

The *attitude* data set is a data frame with 30 observations on 7 variables, measuring the percent proportion of favorable responses to seven survey questions in each of 30 departments. The survey was conducted in a large financial organization; there were approximately 35 respondents in each department.

We mentioned that the *plot* function could be used with virtually any data set to get an initial visualization; let’s see what it gives for the *attitude* data:

	plot(attitude)

The resulting plot is a pairwise scatter plot of the numeric variables in the data set.

![](media/rserver-getting-started/image9.jpeg)

The first two variables (*rating* and *complaints*) show a strong linear relationship. To model that relationship, we use the lm function:

	attitudeLM1 <- lm(rating ~ complaints, data=attitude)

To view a summary of the model, we can use the summary function:

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

We can also try to visualize the model using plot:

	plot(attitudeLM1)

The default plot for a fitted linear model is a set of four plots; by default they are shown one at a time, and you are prompted before each new plot is displayed. To view them all at once, use the *par* function with the *mfrow* parameter to specify a 2 x 2 layout:

	par(mfrow=c(2,2))
	plot(attitudeLM1)

![](media/rserver-getting-started/image10.jpeg)

>**Big Data Big Analytics Tip**: The *rxLinMod* function is a full-featured alternative to lm that can efficiently handle large data sets.  Also look at *rxLogit* and *rxGlm* as alternatives to *glm*, *rxKmeans* as an alternative to *kmeans*, and *rxDTree* as an alternative to *rpart*.

### Matrices and apply

A *matrix* is a two-dimensional data array. Unlike data frames, which can have different data types in their columns, matrices may contain data of only one type. Most commonly, matrices are used to hold numeric data. You create matrices with the *matrix* function:

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

Matrix multiplication in the linear algebra sense requires a special operator, *%*%:*

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

When you need to manipulate the rows or columns of a matrix, an incredibly useful tool is the apply function. With *apply*, you can apply a function to all the rows or columns of matrix at once. For example, to find the column products of *A*, you could use *apply* as follows:

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

### Lists and *lapply*

A *list* in R is a very flexible data object that can be used to combine data of different types and different lengths for almost any purpose. Arbitrary lists can be created with either the list function or the c function; many other functions, especially the statistical modeling functions, return their output as list objects.

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

The function *lapply* can be used to apply the same function to each component of a list in turn:

	lapply(list1, length)

	$x
	[1] 10

	$y
	[1] 3

	$z
	[1] 4

>*Big Data Big Analytics Tip*: You will regularly use lists and functions that manipulate them when handling input and output for your big data analyses.  

### Packages

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

Other packages are available through the Comprehensive R Archive Network (CRAN); to obtain a package you use the *install.packages* function:

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

>*Big Data Big Analytics Tip*: The RevoScaleR package is included with every distribution of Microsoft R Server and R Client, and is automatically loaded into memory when you start the program.  So all of the “rx” functions mentioned in these tips are at your fingertips.  You can get information on them by using the ? at the command line, for example: *?rxLinMod*

### Using Microsoft R via Rscript and R CMD BATCH

Microsoft R Server and R Client are intended for high-performance computing and analytics, and some users are accustomed to running their analyses via batch mode and command-line scripting. *R CMD BATCH* generally works with Microsoft R Server and R Client with no modifications needed, but to get full advantage of the Microsoft R Server and R Client extensions with other command line invocations, you need to know a little bit about how Microsoft R Server and R Client work. Microsoft R is 100% R, with the standard R BLAS and LAPACK libraries substituted out for the Intel Math Kernel Libraries, and with a number of additional packages. Some of these packages are added to the default package list by the *Rprofile.site* file distributed with Microsoft R Server and R Client. If you use *Rscript* (or, on some systems, the equivalent Revoscript) with a Microsoft R Server or R Client script, be sure to add the flag *–default-packages=* to your call; this ensures that the Microsoft R default packages are loaded (including the *methods* package from base R).

Similarly, you should avoid the –vanilla construction for invoking Microsoft R Server or R Client; this method of invocation avoids evaluating the *Rprofile.site* file, so that this is equivalent to calling R without the Microsoft R Server or R Client extensions (except the MKL BLAS and LAPACK libraries).


## Tips on Computing with Big Data in R

Big data is ubiquitous. The good news is that it provides great opportunities for the data analyst. With more data comes more information and more insight. You can relax assumptions required with smaller data sets and let the data speak for itself. But big data also presents problems. The size of data sets is now increasing much more rapidly than the speed of single cores, of RAM, and of hard drives. Most software tools aren’t handling this well; many of us have experienced that moment when our data has grown just a little too big—and our software has stopped working.

The use of the R statistical programming language has seen phenomenal growth because of R’s flexibility and power. Cutting-edge algorithms are written in R, and analysts love it for quick prototyping. But R users are faced with two big problems when scaling to big data: capacity and speed. Data sets typically must fit into memory, and even if they can, there are limits to what types of analyses can be done. Even without a capacity limit, computation may be too slow to be useful.

If you are used to working with smaller data sets in R, you will want to think differently about how you perform your analyses when using big data. If you are used to working in a High Performance Computing (HPC) environment, you will also want to think differently when you add analysis of big data to the picture. High Performance Computing is CPU centric, typically focusing on using many cores to perform lots of processing on small amounts of data. High Performance Analytics (HPA) is data centric. The focus is on feeding data to the cores—on disk I/O, data locality, efficient threading, and data management in RAM.

The **RevoScaleR** package that is included with Microsoft R Server and Microsoft R Client provides tools and examples for addressing the speed and capacity issues involved in High Performance Analytics. It provides data management and analysis functionality that scales from small, in-memory data sets to huge data sets stored on disk. The analysis functions are threaded to use multiple cores, and computations can be distributed across multiple computers (nodes) on a cluster or in the cloud.

Here are some tips for handling big data with R:

### Upgrade Your Hardware

It is always best to start with the easiest things first, and in some cases getting a better computer, or improving the one you have, can help a great deal. Usually the most important consideration is memory. If you are analyzing data that just about fits in R on your current system, getting more memory will not only let you finish your analysis, it is also likely to speed things up by a lot. This is because your operating system starts to “thrash” when it gets low on memory, removing some things from memory to let others continue to run. This can slow your system to a crawl. Getting more cores can also help, but only up to a point. R itself can generally only use one core at a time internally. In addition, for many data analysis problems the bottlenecks are disk I/O and the speed of RAM, so efficiently using more than 4 or 8 cores on commodity hardware can be difficult.

### Upgrade Your Software

Some software is simply more optimized for use with big data. For instance, getting better math libraries can greatly speed some computations. R allows its core math libraries to be replaced, and in Microsoft R Server and R Client they are replaced with extremely fast, threaded libraries. And, of course, the ***RevoScaleR*** package provides the underlying high-performance compute engine used by its Big Data Big Analytics algorithms.

### Minimize Copies of Data

When working with small data sets, an extra copy is not a problem. With big data it can slow the analysis, or even bring it to a screeching halt. Be aware of the ‘automatic’ copying that occurs in R. For example, if a data frame is passed into a function, a copy is only made if the data frame is modified. But if a data frame is put into a list, a copy is automatically made. In many of the basic analysis algorithms, such as *lm* and *glm*, multiple copies of the data set are made as the computations progress, resulting in serious limitations in processing big data sets. The RevoScaleR analysis functions (for instance, *rxSummary*, *rxCube*, *rxLinMod*, *rxLogit,* *rxGlm*, *rxKmeans*) are all implemented with a focus on efficient use of memory; data is not copied unless absolutely necessary. The plot below shows an example of how reducing copies of data and tuning algorithms can dramatically increase speed and capacity.

![](media/rserver-getting-started/image11.jpeg)

### Process Data in Chunks

Processing your data a chunk at a time is the key to being able to scale your computations without increasing memory requirements. External memory (or “out-of-core”) algorithms don’t require that all of your data be in RAM at one time. Data is processed a chunk at time, with intermediate results updated for each chunk. When all of the data is processed, final results are computed. The core functions provided with **RevoScaleR** all process data in chunks. So, if the number of rows of your data set doubles, you can still perform the same data analyses—it will just take longer, typically scaling linearly with the number of rows. Your analysis is not bound by memory constraints. The plot below shows how data chunking allows unlimited rows in limited RAM.

![](media/rserver-getting-started/image12.jpeg)

The *biglm* package, available on CRAN, also estimates linear and generalized linear models using external memory algorithms, although they are not parallelized.

### Compute in Parallel Across Cores or Nodes

Using more cores and more computers (nodes) is the key to scaling computations to really big data. Since data analysis algorithms tend to be I/O bound when data cannot fit into memory, the use of multiple hard drives can be even more important than the use of multiple cores.

The **RevoScaleR** analysis functions are written to automatically compute in parallel on available cores, and can also be used to automatically distribute computations across the nodes of a cluster. These functions combine the advantages of external memory algorithms (see [Process Data in Chunks](#process-data-in-chunks) above) with the advantages of High Performance Computing. That is, these are Parallel External Memory Algorithm’s (PEMA’s)—external memory algorithms that have been parallelized. Such algorithms process data a chunk at a time in parallel, storing intermediate results from each chunk and combining them at the end. Iterative algorithms repeat this process until convergence is determined. Any external memory algorithm that is not “inherently sequential” can be parallelized; results for one chunk of data cannot depend upon prior results. Dependence on data from a prior chunk is OK, but must be handled specially. The plot below shows an example of how using multiple computers can dramatically increase speed, in this case taking advantage of memory caching on the nodes to achieve super-linear speedups.

![](media/rserver-getting-started/image13.jpeg)

Microsofts’ *foreach* package, which is open source and available on CRAN, provides very easy- to-use tools for executing R functions in parallel, both on a single computer and on multiple computers. This is particularly useful for “embarrassingly parallel” types of computations such as simulations, which do not involve lots of data and do not involve communication among the parallel tasks.

### Take Advantage of Integers

In R the two choices for “continuous” data are *numeric*, which is an 8 byte (double) floating point number and *integer*, which is a 4 byte integer. If your data can be stored and processed as an integer, there can be big advantages to doing so. First, it will only take half of the memory. Second, in some cases integers can be processed much faster than doubles. For example, if you have a variable whose values are integral numbers in the range from 1 to 1000 and you want to find the median, it is much faster to count all the occurrences of the integers than it is to sort the variable. A tabulation of all the integers, in fact, can be thought of as a way to compress the data with no loss of information. The resulting tabulation can be converted into an exact empirical distribution of the data by dividing the counts by the sum of the counts, and all of the empirical quantiles including the median can be obtained from this. The R function *tabulate* can be used for this, and is very fast. The following code illustrates this:

	nd = sample(as.numeric(1:1000),size = 1e+8, replace = TRUE)
	system.time(nit <- tabulate(as.integer(nd)))
	system.time(nds <- sort(nd))

A vector of 100 million doubles is created, with randomized integral values in the range from 1 to 1,000. Sorting this vector takes about 15 times longer than converting to integers and tabulating, and 25 times longer if the conversion to integers is not included in the timing (this is relevant if you convert to integers once and then operate multiple times on the resulting vector).

Sometimes decimal numbers can be converted to integers without losing information. An example is temperature measurements of the weather, such as 32.7, which can be multiplied by 10 to convert them into integers.

**RevoScaleR** provides several tools for the fast handling of integral values. For instance, in formulas for linear and generalized linear models and other analysis functions, the “F()” function can be used to virtually “convert” numeric variables into factors, with the levels represented by integers. The *rxCube* function allows rapid tabulations of factors and their interactions (for example, age by state by income) for arbitrarily large data sets.

### Store Your Data Efficiently

If your data doesn’t easily fit into memory, you’ll want to store it efficiently for fast access from disk. If you use appropriate data types, you can save on storage space and access time. Take advantage of integers, and store data in 32-bit floats not 64-bit doubles. A 32-bit float can represent 7 decimal digits of precision, which is more than enough for most data, and it takes up half the space of doubles. (Save the 64-bit doubles for computations).

Recognize that relational databases are not always optimal for storing data for analysis. Even with the best indexing they are typically not designed to provide fast sequential reads of blocks of rows for specified columns, which is the key to fast access to data on disk. **RevoScaleR** provides an efficient .xdf file format that provides storage of a wide variety of data types, and is designed for fast sequential reads of blocks of data. There are tools for rapidly accessing data in .xdf files from R and for importing data into this format from SAS, SPSS, and text files and SQL Server, Teradata and ODBC connections.

### Only Read in The Data That Is Needed

Even though a data set may have many thousands of variables, typically not all of them are being analyzed at one time. If you have your entire data set in memory, you can easily run into out-of-memory problems. By just reading from disk the actual variables and observations you will use in analysis, you can speed up the analysis considerably. **RevoScaleR** functions provide this automatically. For example, when estimating a model, only the variables used in the model are read from the .xdf file.

### Avoid Loops when Transforming Data

It is well-known that processing data in loops in R can be very slow compared with vector operations. For example, if you compare the timings of adding two vectors, one with a loop and the other with a simple vector operation, you will find the vector operation to be orders of magnitude faster:

	n <- 1000000
	x1 <- 1:n
	x2 <- 1:n
	y <- vector()
	system.time( for(i in 1:n){ y[i] <- x1[i] + x2[i] })
	system.time( y <- x1 + x2)

On a good laptop the loop over the data was timed at about 430 seconds, while the vectorized add is barely timeable. In R the core operations on vectors are typically written in C, C++ or FORTRAN, and these compiled languages can provide much greater speed for this type of code than can the R interpreter.

### Use C, C++, or FORTRAN for Critical Functions

One of the best features of R is its ability to integrate easily with other languages, including C, C++, and FORTRAN. You can pass R data objects to other languages, do some computations, and return the results in R data objects. It is typically the case that only small portions of an R program can benefit from the speedups that compiled languages like C, C++, and FORTRAN can provide. Indeed, much of the code in the base and recommended packages in R is written in this way—the bulk of the code is in R but a few core pieces of functionality are written in C, C++, or FORTRAN. The type of code that benefits the most from this involves loops over data vectors. The package *Rcpp,* which is available on CRAN, provides tools that make it very easy to convert R code into C++ and to integrate C and C++ code into R. Of course, before writing code in another language, it pays to do some research to see if the type of functionality you want is already available in R, either in the base and recommended packages or in a 3<sup>rd</sup> party package. For example, all of the core algorithms for the **RevoScaleR** package are written in optimized C++ code. It also pays to do some research to see if there is publically-available code in one of these compiled languages that does what you want.

### Process Data Transformations in Batches

When working with small data sets, it is common to perform data transformations one at a time. For instance, one line of code might create a new variable, and the next line might multiply that variable by 10. Each of these lines of code processes all rows of the data. With a big data set that cannot fit into memory, there can be substantial overhead to making a pass through the data. With **RevoScaleR**’s *rxDataStep* function you can specify multiple data transformations that can be performed in just one pass through the data, processing the data a chunk at a time. A little planning ahead can save a lot of time.

### User Row-Oriented Data Transformations where Possible

When data is processed in chunks, basic data transformations for a single row of data should in general not be dependent on values in other rows of data. The key is that your transformation expression should give the same result even if only some of the rows of data are in memory at one time. Data manipulations using lags can be done but require special handling.

### Handle Categorical Variables Efficiently and with Care

Categorical or factor variables are extremely useful in visualizing and analyzing big data, but they need to be handled efficiently with big data because they are typically expanded when used in modeling. For example, if you use a factor variable with 100 categories as an independent variable in a linear model with *lm*, behind the scenes 100 dummy variables are created when estimating the model. The analysis modeling functions in **RevoScaleR** use special handling of categorical data to minimize the use of memory when processing them; they do not generally need to explicitly create dummy variable to represent factors.

Creating factor variables also often takes more careful handling with big data sets. This is because not all of the factor levels may be represented in a single chunk of data. For example, if you want to use the *factor* function in a data transformation used on chunks of data, you must explicitly specify the levels or you might end up with incompatible factor levels from chunk to chunk. The *rxImport* and *rxFactors* functions in **RevoScaleR** provide functionality for creating factor variables in big data sets.

### Be Aware of Output with the Same Number of Rows as Your Data

Most analysis functions return a relatively small object of results that can easily be handled in memory. But occasionally, output will have the same number of rows as your data, for example, when computing predictions and residuals from a model. In order for this to scale, you will want the output written out to a file rather than kept in memory. For this reason, the **RevoScaleR** modeling functions such as *rxLinMod*, *rxLogit*, and *rxGlm* do not automatically compute predictions and residuals. The *rxPredict* function provides this functionality and can add predicted values to an existing .xdf file.

### Think Twice Before Sorting

When working with small data sets, it is common to sort data at various stages of the analysis process. Although **RevoScaleR**’s *rxSort* function is very efficient for .xdf files and can handle data sets far too large to fit into memory, sorting is by nature a time-intensive operation, especially on big data.

One of the major reasons for sorting is to compute medians and other quantiles. As noted above in the section on taking advantage of integers, when the data consists of integral values, a tabulation of those values is generally much faster than sorting and gives exact values for all empirical quantiles. Even when the data is not integral, scaling the data and converting to integers can give very fast and accurate quantiles. As an example, if the data consists of floating point values in the range from 0 to 1,000, converting to integers and tabulating will bound the median or any other quantile to within two adjacent integers. Interpolation within those values can get you closer, as can a small number of additional iterations. If the original data falls into some other range (for example, 0 to 1), scaling to a larger range (for example, 0 to 1,000) can accomplish the same thing. The *rxQuantile* function uses this approach to rapidly compute approximate quantiles for arbitrarily large data.

Another major reason for sorting is to make it easier to compute aggregate statistics by groups. If the data are sorted by groups, then contiguous observations can be aggregated. However, it is often possible, and much faster, to make a single pass through the original data and to accumulate the desired statistics by group. The *aggregate* function can do this for data that fits into memory, and **RevoScaleR**’s *rxSummary*, *rxCube*, and *rxCrossTabs* provide extremely fast ways to do this on large data.

The **RevoScaleR** functions *rxRoc*, and *rxLorenz* are other examples of ‘big data’ alternatives to functions that traditionally rely on sorting.

In summary, by using the tips and tools outlined above you can have the best of both worlds: the ability to rapidly extract information from big data sets using R and the flexibility and power of the R language to manipulate and graph this information.

## Getting Started with Big Data in R

The **RevoScaleR** package, included in Microsoft R Server and R Client, provides a framework for quickly writing start-to-finish, scalable R code for data analysis. Even if you are relatively new to R, you can get started with just a few basic functions.

### Step 1: Accessing Your Data with *rxImport*

The *rxImport* function allows you to import data from fixed or delimited text files, SAS files, SPSS files, or a SQL Server, Teradata or ODBC connection. There’s no need to have SAS or SPSS installed on your system to import those file types, but you will need an ODBC driver for your data base to access it. Let’s use an example of a delimited text file available in the sample data directory of the **RevoScaleR** package, a small data set containing simulated mortgage default data. We’ll store the location of the file in a character string, then import the data into an in-memory data set (data frame) using the default settings:

	inDataFile <- file.path(rxGetOption("sampleDataDir"),
		"mortDefaultSmall2000.csv")

	mortData <- rxImport(inData = inDataFile)

If we think that we may want to do the same analysis on a larger data set later, we can prepare for that by putting placeholders in our code for output files. If we set these values to a file path, the data will be stored on disk in the efficient .xdf file format, rather than storing it in memory. The output object returned from *rxImport* would be a small object representing the .xdf file (an *RxXdfData* object), rather than a data frame containing all of the data. But for the time being, we will continue to work with the data in memory. We can do this by setting the *outFile* parameters to NULL. The following code will accomplish the same importing task of that above:

	outFile <- NULL
	outFile2 <- NULL
	mortData <- rxImport(inData = inDataFile, outFile = outFile)

### Step 2: A Quick Look at the Data

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

### Step 3: Data Selection and Transformations with *rxDataStep*

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

### Step 4: Visualizing Your Data with *rxHistogram*, *rxCube*, and *rxLinePlot*

The *rxHistogram* function will show us the distribution of any of the variables in our data set. For example, let’s look at credit score:

	rxHistogram(~creditScore, data = mortDataNew )

![](media/rserver-getting-started/image14.png)

The *rxCube* function will compute category counts, and can operate on the interaction of categorical variables. Using the *F()* notation to convert a variable into an on-the-fly categorical factor variable (with a level for each integer value), we can compute the counts for each credit score for the two groups who have low and high credit card debt:

	mortCube <- rxCube(~F(creditScore):catDebt, data = mortDataNew)

The *rxLinePlot* function is a convenient way to plot output from *rxCube*. We use the *rxResultsDF* helper function to convert cube output into a data frame convenient for plotting:

	rxLinePlot(Counts~creditScore|catDebt, data=rxResultsDF(mortCube))

![](media/rserver-getting-started/image15.png)

### Step 5: Analyzing Your Data with *rxLogit*

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

### Step 6: Scaling Your Analysis

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


## Optimized Math Libraries

One feature of Microsoft R Server and R Client is its inclusion of optimized libraries for linear algebra. These libraries are used throughout R’s modeling applications, including linear models, principal components analysis, and others.

Matrix multiplication, eigenvalue calculations, and singular value decompositions are significantly faster using these optimized libraries. For example, we ran the following computations, first with R built from source using the standard R BLAS, then with Microsoft R Server and R Client built with the optimized math libraries:

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

### A Note on R Numerics

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

For a good introduction to the pitfalls involving numeric computation, see the excellent article by Goldberg .

### Performance Optimization and Numerics

Many of the most widely-used and compute-intensive linear algebra and arithmetic operations performed by R are computed by low-level numerics libraries, including the basic linear algebra subroutine (BLAS) library. Microsoft R includes highly-tuned low-level numerics libraries that are optimized for speed on a wide variety of x86, x86-64 and IA-64 processor architectures.

Many of the performance optimizations try to

- Make use of available SIMD-style vector registers and operations

- Make efficient re-use of speedy processor memory caches.

Both approaches often require blocking or otherwise re-ordering computations, for example to fit in a small cache.

Because floating-point arithmetic always involves approximation, and the exact nature of the approximation depends upon the particular algorithms used, it is possible that the results from the highly-tuned numerics available in Microsoft R Server and R Client differ from results computed by other implementations of R, just as results can differ across system architectures. These computational differences typically manifest themselves on the order of machine epsilon.

Some high-performance numerics routines, including those that use x86 SIMD vector instructions (SSE, etc.), require that their input data be loaded into memory addresses divisible by 16 bytes. Unfortunately, R does not presently guarantee alignment of data on 16-byte boundaries. Therefore, it is possible that, depending on data alignment, parts of some computations may be grouped off differently (for less efficient computation). This alignment-dependent blocking of some computations can also result in differences from run to run on the order of machine epsilon.

## R Memory Limits in Windows

There are several tools available in the Windows versions of R to help you monitor and manage memory usage.

### 64-bit R Memory Limits

The primary advantage 64-bit architectures bring to R is an increase in the amount of memory available to a given R process. The first benefit of that increase is an increase in the size of data objects you can create. For example, on most 32-bit versions of R, the largest data object you can create is roughly 3GB; attempts to create 4GB objects result in errors with the message “cannot allocate vector of length *xxxx*.” On 64-bit versions of R, you can generally create larger data objects. From R 3.0.0 onward, the old limitation of \(2^{31} - 1\) elements in a vector (about 2 billion elements) has been removed (except for character vectors).

The functions *memory.size* and *memory.limit* help you manage the memory used by Windows versions of R. In 64-bit Microsoft R Server and R Client, R sets the memory limit by default to the amount of physical RAM minus half a gigabyte, so that, for example, on a machine with 8GB of RAM, the default memory limit is 7.5GB:

	memory.limit()

	[1] 7678.328

You can also use *memory.limit* to increase the amount of memory used by R, but not decrease it:

	memory.limit(size = 15000)

	NULL

Objects larger than physical memory can be allocated and used, as long as the total memory used does not exceed *memory.limit()*. However, using such large objects may be unacceptably slow.

The *memory.size* function returns the amount of memory currently being used by R, or, if you specify *max=TRUE*, the maximum amount of memory used in the current R session:

	memory.size()

	[1] 8207.29

	memory.size(max=TRUE)

	[1] 8214.75

The result is in MB.

Another useful function for managing memory, which is available on all platforms and not just Windows, is gc. The gc function causes R to perform garbage collection and report the current state of memory usage. The report gives information on the number of “Ncells” and “Vcells” currently in use; very roughly, the “Ncells” can be thought of the memory used by the language itself and “Vcells” the memory used for the data. The “Ncells” occupy 28 bytes on a 32-bit platform and typically 56 bytes on a 64-bit platform. “Vcells” occupy 8 bytes on all platforms. Typical output from gc is as follows:

	> gc()
		used (Mb) gc trigger   (Mb) limit (Mb)  max used   (Mb)
	Ncells 137107  7.4     350000   18.7     229376    350000   18.7
	Vcells 142395  1.1  451286991 3443.1      32768 537014530 4097.1

### Using the Memory Tools Together

The following function combines *memory.limit*, *memory.size*, and *gc* into a single function with friendly formatting; it can be used only on Windows platforms

	reportMem <- function()
	{
		# Perform garbage collection
		gc(verbose=FALSE)
		#How much memory is being used by malloc
		intvar <- round(memory.size())
		cat("Memory currently allocated is", intvar, "Mb\n")
		#Maximum amount of memory that has been obtained from the OS
		intvar <- round(memory.size(max=TRUE))
		cat("Maximum amount of memory allocated during this session is",
		intvar, "Mb\n")
		#Memory limit
		intvar <- round(memory.limit())
		cat("Current limit for total allocation is", intvar, "Mb\n")
	}

The *reportObjSize* function defined below is a simple wrapper for *object.size* that prints the size of an object in MB:

	reportObjSize <- function(x=x)
	{
		bm_conv <- 1024*1024
		objsize <- round(object.size(x)/bm_conv)
		cat("Object Size =", objsize, "Mb", "\n")
	}

Together, we can use these functions to show how memory use changes as we allocate vectors of different sizes. (We used a 64-bit Windows machine for our examples; you can do similar things on 32-bit Windows, but obviously with smaller objects sizes.) For example, here we create a vector of size roughly 1GB, and see its size and effect on the memory profile:

	reportMem()

		Memory currently allocated is 21 Mb
		Maximum amount of memory allocated during this session is 23 Mb
		Current limit for total allocation is 7678 Mb

	xlen1gb <- 2^27
	x <- rnorm(xlen1gb)
	reportMem()

		Memory currently allocated is 1039 Mb
		Maximum amount of memory allocated during this session is 1047 Mb
		Current limit for total allocation is 7678 Mb

To delete an object, use the *rm* function. The *reportMem* function handles the garbage collection to ensure that the deleted object’s memory is in fact freed:

	rm(x)
	reportMem()

	 Memory currently allocated is 15 Mb
	 Maximum amount of memory allocated during this session is 1047 Mb
	 Current limit for total allocation is 7678 Mb

To create a 5GB object, make sure that the memory limit is high enough. (Allocating a 5GB object can be done even on systems with less than 5GB of physical memory, but it can be very slow.)

	x <- rnorm(xlen1gb*5)
	reportObjSize(x)

	 Object Size = 5120 Mb

	reportMem()

	 Memory currently allocated is 5135 Mb
	 Maximum amount of memory allocated during this session is 5143 Mb
	 Current limit for total allocation is 7678 Mb

## Next Steps: A Roadmap to Documentation

Having completed the tutorials, you are now ready to dive right in and start using R for your own purposes. While the tutorials have given you the basic tools to begin exploring, you may still want more guidance for your specific tasks. Luckily, there is a huge library of Microsoft R, open-source R, and S documentation that can help you perform almost any task with R. This brief roadmap points you toward some of the most useful documentation that Microsoft is aware of. (If you find other useful resources, drop us a line at revodoc@microsoft.com!)

The obvious place to start is with the rest of the **Microsoft R** document set, which includes documentation on the **RevoScaleR** package for scalable data analysis (on all platforms). You can find this documentation on this site using the table of contents on this page.

Next, you should be aware of the R Core Team manuals, which are part of every R distribution, including *An Introduction to R*, *The R Language Definition*, *Writing R Extensions* and so on. 

Beyond the standard R manuals, there are many other resources. [Learn about them here](microsoft-r-more-resources.md). 