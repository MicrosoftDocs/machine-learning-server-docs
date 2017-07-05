---

# required metadata
title: "RevoScaleR with Sparklyr example (Microsoft R)"
description: "Spark interoperability with Microsoft R Server 9.1"
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "04/17/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology:
  - r-client
  - r-server
#ms.custom: ""

---

# RevoScaleR with sparklyr step-by-step examples

Micorosft R Server (MRS) 9.1 supports the [sparklyr package from RStudio](https://cran.r-project.org/package=sparklyr). Microsoft R Server and sparklyr can now be used in tandem within a single Spark session. This walkthrough shows you two approaches for using these technologies together:

+ [Example 1: sparklyr data and MRS analytics](#example1)
+ [Example 2: MRS data and sparklyr analytics](#example2)

## Prerequisites

To run the example code, your environment must provide the following:

+ A Hadoop cluster with Spark and valid installation of Microsoft R Server
+ Microsoft R Server configured for Hadoop and Spark
+ Microsoft R Server SampleData loaded into HDFS
+ gcc and g++ installed on the edgenode that the example runs on
+ Write permissions to the R Library Directory
+ Read/Write permissions to HDFS directory /user/RevoShare
+ An internet connection or the ability to download and manually install sparklyr


> [!NOTE]
> For more background in using Microsoft R Server with Spark, see [Get started with R Server and ScaleR on Spark](how-to-revoscaler-spark.md).

## Install the sparklyr package

How you install the sparklyr R package depends on whether or not you are on HDI.

If on HDI, use:
```R
options(repos = “https://mran.microsoft.com/snapshot/2017-05-01“)

install.packages(“sparklyr”)
```

If not, use:

```R
install.packages(“sparklyr”)
```


## Load data into HDFS

To load SampleData into HDFS, please run these commands from within an edge node with MRS installed:

````
	hadoop fs -mkdir /share
	hadoop fs -mkdir /share/SampleData
	# Standard RevoScaleR path is: /usr/lib64/microsoft-r/3.3/lib64/R/library/RevoScaleR/
	# this will vary if you installed via Cloudera Parcels, to find the correct path
	# please use command: sudo find / -name SampleData
	hadoop fs -copyFromLocal <Path-To-RevoScaleR>/SampleData/* /share/SampleData
	# To validate everything was copied, compare the outputs of:
	hadoop fs -ls /share/SampleData
	# and:
	ls -la <Path-To-RevoScaleR>/SampleData/
````
<a name="example1"></a>

## Example 1: sparklyr data with MRS modeling

This example assumes dployr and sparklyr data structures, with model training and predictive analysis using Microsoft R Server.

1. Create a connection to Spark using `rxSparkConnect()`, specifying a sparklyr interop; using sparklyr and its interfaces to connect to Spark.
2. Call `rxGetSparklyrConnection()` on the compute context to get a sparklyr connection object.
3. Use dplyr to load mtcars into a Spark DataFrame via the sparklyr connection object.
4. Partition the data in-Spark into a training and scoring set using dplyr.
5. After partitioning, register the training set DataFrame in Spark to a Hive table.
6. Train a model in ScaleR using `rxLinMod()` on an `RxHiveData()` object.
7. With the trained model, run a toy prediction using `rxPredict()` on the test partition.
8. After prediction, take the root mean square to determine accuracy.

> [!NOTE]
> Data is the Standard R dataset mtcars for this example. For more information, see [Motor Trend Car Road Tests](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/mtcars.html).

### Sample Code

```
	# Install sparklyr
	install.packages("sparklyr")
	

	# Load required libraries
	library(RevoScaleR)
	library(sparklyr)
	library(dplyr)
	

	# Connect to Spark using rxSparkConnect, specifying 'interop = "sparklyr"'
	# this will create a sparklyr connection to spark, and allow you to use
	# dplyr for data manipulation. Using rxSparkConnect in this way will use
	# default values and rxOptions for creating a Spark connection, please
	# see "?rxSparkConnect" for how to define parameters specific to your setup
	cc <- rxSparkConnect(reset = TRUE, interop = "sparklyr")
	

	# The returned Spark connection (sc) provides a remote dplyr data source 
	# to the Spark cluster using SparlyR within rxSparkConnect.
	sc <- rxGetSparklyrConnection(cc)
	

	# Next, load mtcars in to Spark using a dplyr pipeline
	mtcars_tbl <- copy_to(sc, mtcars)
	

	# Now, partition the data into Training and Test partitions using dplyr
	partitions <- mtcars_tbl %>%
	  filter(hp >= 100) %>%
	  mutate(cyl8 = cyl == 8) %>%
	  sdf_partition(training = 0.5, test = 0.5, seed = 1099)
	

	# Register the partitions as DataFrames using sparklyr
	sdf_register(partitions$training, "cars_training")
	sdf_register(partitions$test, "cars_test")

	

	# Create a RxHiveData Object for each
	cars_training_hive <- RxHiveData(table = "cars_training", 
	                        colInfo = list(cyl = list(type = "factor")))
	cars_test_hive <- RxHiveData(table = "cars_test", 
	                    colInfo = list(cyl = list(type = "factor")))

	# Use the Training set to train a model with rxLinMod()
	lmModel <- rxLinMod(mpg ~ wt + cyl, cars_training_hive)
	

	# Take a summary of the trained model (this step is optional)
	summary(lmModel)
	

	# Currently, for rxPredict(), only XDF files are supported as an output
	# The following command will create the directory "/user/RevoShare/MTCarsPredRes" 
	# to hold the composite XDF.
	pred_res_xdf <- RxXdfData("/user/RevoShare/MTCarsPredRes", fileSystem = RxHdfsFileSystem())
	

	# Run rxPredict, specifying the outData as our RxXdfData() object, write
	# the model variables into the results object so we can analyze out accuracy
	# after the prediction completes
	pred_res_xdf <- rxPredict(lmModel, data = cars_test_hive, outData = pred_res_xdf, 
	             overwrite = TRUE, writeModelVars = TRUE)
	

	# Now, import the results from HDFS into a DataFrame so we can see our error
	pred_res_df <- rxImport(pred_res_xdf)
	

	# Calculate the Root Mean Squared error of our prediction
	sqrt(mean((pred_res_df$mpg - pred_res_df$mpg_Pred)^2, na.rm = TRUE))
	

	# When you are finished, close the connection to Spark
	rxSparkDisconnect(cc)
```

### Sample Output, Comments Removed

```
	> library(RevoScaleR)
	> library(sparklyr)
	> library(dplyr)
	>
	> cc <- rxSparkConnect(reset = TRUE, interop = "sparklyr")
	No Spark applications can be reused. It may take 1 to 2 minutes to launch a new Spark application.
	>
	> sc <- rxGetSparklyrConnection(cc)
	>
	> mtcars_tbl <- copy_to(sc, mtcars)
	>
	> partitions <- mtcars_tbl %>%
	+   filter(hp >= 100) %>%
	+   mutate(cyl8 = cyl == 8) %>%
	+   sdf_partition(training = 0.5, test = 0.5, seed = 1099)
	>
	> sdf_register(partitions$training, "cars_training")
	Source:   query [8 x 12]
	Database: spark connection master=yarn-client app=sparklyr-scaleR-spark-won2r0FHOA-azureuser-37220-CB63BD4AD48648F79996CA7B24E1493D local=FALSE
	

	    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb  cyl8
	  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <lgl>
	1  10.4     8 460.0   215  3.00 5.424 17.82     0     0     3     4  TRUE
	2  14.3     8 360.0   245  3.21 3.570 15.84     0     0     3     4  TRUE
	3  15.0     8 301.0   335  3.54 3.570 14.60     0     1     5     8  TRUE
	4  15.8     8 351.0   264  4.22 3.170 14.50     0     1     5     4  TRUE
	5  16.4     8 275.8   180  3.07 4.070 17.40     0     0     3     3  TRUE
	6  18.7     8 360.0   175  3.15 3.440 17.02     0     0     3     2  TRUE
	7  21.0     6 160.0   110  3.90 2.875 17.02     0     1     4     4 FALSE
	8  21.4     4 121.0   109  4.11 2.780 18.60     1     1     4     2 FALSE
	> sdf_register(partitions$test, "cars_test")
	Source:   query [15 x 12]
	Database: spark connection master=yarn-client app=sparklyr-scaleR-spark-won2r0FHOA-azureuser-37220-CB63BD4AD48648F79996CA7B24E1493D local=FALSE
	

	     mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb  cyl8
	   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <lgl>
	1   10.4     8 472.0   205  2.93 5.250 17.98     0     0     3     4  TRUE
	2   13.3     8 350.0   245  3.73 3.840 15.41     0     0     3     4  TRUE
	3   14.7     8 440.0   230  3.23 5.345 17.42     0     0     3     4  TRUE
	4   15.2     8 275.8   180  3.07 3.780 18.00     0     0     3     3  TRUE
	5   15.2     8 304.0   150  3.15 3.435 17.30     0     0     3     2  TRUE
	6   15.5     8 318.0   150  2.76 3.520 16.87     0     0     3     2  TRUE
	7   17.3     8 275.8   180  3.07 3.730 17.60     0     0     3     3  TRUE
	8   17.8     6 167.6   123  3.92 3.440 18.90     1     0     4     4 FALSE
	9   18.1     6 225.0   105  2.76 3.460 20.22     1     0     3     1 FALSE
	10  19.2     6 167.6   123  3.92 3.440 18.30     1     0     4     4 FALSE
	11  19.2     8 400.0   175  3.08 3.845 17.05     0     0     3     2  TRUE
	12  19.7     6 145.0   175  3.62 2.770 15.50     0     1     5     6 FALSE
	13  21.0     6 160.0   110  3.90 2.620 16.46     0     1     4     4 FALSE
	14  21.4     6 258.0   110  3.08 3.215 19.44     1     0     3     1 FALSE
	15  30.4     4  95.1   113  3.77 1.513 16.90     1     1     5     2 FALSE
	>
	> cars_training_hive <- RxHiveData(table = "cars_training",
	+                         colInfo = list(cyl = list(type = "factor")))
	> cars_test_hive <- RxHiveData(table = "cars_test",
	+                     colInfo = list(cyl = list(type = "factor")))
	>
	> lmModel <- rxLinMod(mpg ~ wt + cyl, cars_training_hive)
	>
	> summary(lmModel)
	Call:
	rxLinMod(formula = mpg ~ wt + cyl, data = cars_training_hive)
	

	Linear Regression Results for: mpg ~ wt + cyl
	Data: cars_training_hive (RxSparkData Data Source)
	Dependent variable(s): mpg
	Total independent variables: 5 (Including number dropped: 1)
	Number of valid observations: 8
	Number of missing observations: 0
	

	Coefficients: (1 not defined because of singularities)
	            Estimate Std. Error t value Pr(>|t|)
	(Intercept)  28.8015     3.4673   8.307  0.00115 **
	wt           -2.6624     1.0436  -2.551  0.06323 .
	cyl=8.0      -3.3873     2.3472  -1.443  0.22246
	cyl=6.0      -0.1471     2.6869  -0.055  0.95897
	cyl=4.0      Dropped    Dropped Dropped  Dropped
	---
	Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
	

	Residual standard error: 1.899 on 4 degrees of freedom
	Multiple R-squared: 0.8462
	Adjusted R-squared: 0.7309
	F-statistic: 7.338 on 3 and 4 DF,  p-value: 0.04199
	Condition number: 9.7585
	>
	> pred_res_xdf <- RxXdfData("/user/RevoShare/MTCarsPredRes", fileSystem = RxHdfsFileSystem())
	>
	> pred_res_xdf <- rxPredict(lmModel, data = cars_test_hive, outData = pred_res_xdf,
	+              overwrite = TRUE, writeModelVars = TRUE)
	>
	> pred_res_df <- rxImport(pred_res_xdf)
	>
	> sqrt(mean((pred_res_df$mpg - pred_res_df$mpg_Pred)^2, na.rm = TRUE))
	[1] 2.295512
	>
	> rxSparkDisconnect(cc)
	We have shut down the current Spark application and switched to local compute context.
	>
```
<a name="example2"></a>

## Example 2: MRS data with sparklyr and dplyR modeling

This example uses the airline data set. It includes multiple Microsoft R Server approaches for loading data, such as `RxTextData` for CSV files, and `RxOrcData` or `RxParquetData` for those formats. You can try different approaches by commenting out inactive paths. Given this data, the examples shows you how to parition and train a model with sparklyr.

1. Create a connection to Spark using `rxSparkConnect()`, specifying a sparklyr interop; using sparklyr and its interfaces to connect to Spark.
2. Call `rxGetSparklyrConnection()` on the compute context to get a sparklyr connection object.
3. Use Microsoft R Server to load data from many sources.
4. Partition the data in-Spark into a training and scoring set using dplyr.
5. After partitioning, register the training set DataFrame in Spark to a Hive table.
6. Train a model using sparklyr to call Spark ML algorithms.
7. Take a summary of the trained model to see estimates and errors.

###Sample Code

```
	# # # # # 
	# It is assumed at this point that you have installed sparklyr
	# Proceed to load the required libraries
	library(RevoScaleR)
	library(sparklyr)
	library(dplyr)
	

	# Connect to Spark using rxSparkConnect
	cc <- rxSparkConnect(reset = TRUE, interop = "sparklyr")
	

	# The returned Spark connection (sc) provides a remote dplyr data source 
	# to the Spark cluster using SparlyR within rxSparkConnect.
	sc <- rxGetSparklyrConnection(cc)
	

	# Bind RxHdfsFileSystem to a variable, this will reduce code clutter
	hdfsFS <- RxHdfsFileSystem()
	

	# # # #
	# There are many data sources which we can use in MRS, in the this section
	# we will go through 4 different file based data sources and how to import 
	# data for use in Spark. If you wish to use any of these data sources, simply
	# comment out line number 40
	#
	# One data source is XDF files stored in HDFS. Here we create an Data Object from
	# a composite XDF from HDFS, this can then be held in memory as a DataFrame, or 
	# loaded into a Hive Table.
	AirlineDemoSmall <- RxXdfData(file="/share/SampleData/AirlineDemoSmallComposite", fileSystem = hdfsFS)
	# Another option is CSV files stored in HDFS. To create a CSV Data Object from HDFS 
	# we would use RxTextData(). This can also be used for othere plain text type formats
	AirlineDemoSmall <- RxTextData("/share/SampleData/AirlineDemoSmall.csv", fileSystem = hdfsFS)
	# A third option is Parquet data using RxParquetData()
	AirlineDemoSmall <- RxParquetData("/share/SampleData/AirlineDemoSmallParquet", fileSystem = hdfsFS)
	# Lastly, ORC  Data using RxOrcData()
	AirlineDemoSmall <- RxOrcData("/share/SampleData/AirlineDemoSmallOrc", fileSystem = hdfsFS)
	# # # #
	

	# Continuing with our example, first, prepare your data for loading, for this, 
	# I'll proceed with XDF data, but any of the previously specified data sources 
	# are valid.
	AirlineDemoSmall <- RxXdfData(file="/share/SampleData/AirlineDemoSmallComposite", fileSystem = hdfsFS)
	

	# Regardless of which data source used, to work with it using dplyr, we need to 
	# write it to a Hive table, so, Next, create a Hive Data Object using RxHiveData()
	AirlineDemoSmallHive <- RxHiveData(table="AirlineDemoSmall")
	

	# Use rxDataStep to load the data into the table
	# this depends on data
	rxDataStep(inData = AirlineDemoSmall, outFile = AirlineDemoSmallHive,
	         overwrite = TRUE) # takes about 90 seconds on 1 node cluster
	

	###
	#
	# If you wanted the data as a data frame in Spark, you would use
	# rxDataStep() like so:
	#
	# AirlineDemoSmalldf <- rxDataStep(inData = AirlineDemoSmall, 
	#            overwrite = TRUE) 
	#
	# But data must be in a Hive Table for use with dplyr as an In-Spark 
	# object
	###
	

	# To see that the table was created list all Hive tables
	src_tbls(sc)
	

	# Next, define a dplyr data source referencing the Hive table
	# This caches the data in Spark
	tbl_cache(sc, "airlinedemosmall")
	flights_tbl <- tbl(sc, "airlinedemosmall")
	

	# Print out a few rows of the table
	flights_tbl
	

	# Filter the data to remove missing observations
	model_data <- flights_tbl %>%
	  filter(!is.na(ArrDelay)) %>%
	  select(ArrDelay, CRSDepTime, DayOfWeek)
	

	# Now, partition data into training and test sets using dplyr
	model_partition <- model_data %>% 
	  sdf_partition(train = 0.8, test = 0.2, seed = 6666)
	

	# Fit a linear model using Spark's ml_linear_regression to attempt
	# to predict CRSDepTime by Day of Week and Delay
	ml1 <- model_partition$train %>%
	  ml_linear_regression(CRSDepTime ~ DayOfWeek + ArrDelay)
	

	# Run a summary on the model to see the estimated CRSDepTime per Day of Week
	summary(ml1)
	

	# When you are finished, close the connection to Spark
	rxSparkDisconnect(cc)
```

###Sample Output, Comments Removed

```
	> library(RevoScaleR)
	> library(sparklyr)
	> library(dplyr)
	

	Attaching package: ‘dplyr’
	

	The following objects are masked from ‘package:stats’:
	

	    filter, lag
	

	The following objects are masked from ‘package:base’:
	

	    intersect, setdiff, setequal, union
	

	> 
	> cc <- rxSparkConnect(reset = TRUE, interop = "sparklyr")
	No Spark applications can be reused. It may take 1 to 2 minutes to launch a new Spark application.
	> 
	> sc <- rxGetSparklyrConnection(cc)
	> 
	> hdfsFS <- RxHdfsFileSystem()
	>
	> AirlineDemoSmall <- RxXdfData(file="/share/SampleData/AirlineDemoSmallComposite", fileSystem = hdfsFS)
	> 
	> AirlineDemoSmall <- RxTextData("/share/SampleData/AirlineDemoSmall.csv", fileSystem = hdfsFS)
	> 
	> AirlineDemoSmall <- RxParquetData("/share/SampleData/AirlineDemoSmallParquet", fileSystem = hdfsFS)
	> 
	> AirlineDemoSmall <- RxOrcData("/share/SampleData/AirlineDemoSmallOrc", fileSystem = hdfsFS)
	>
	> AirlineDemoSmall <- RxXdfData(file="/share/SampleData/AirlineDemoSmallComposite", fileSystem = hdfsFS)
	>
	> AirlineDemoSmallHive <- RxHiveData(table="AirlineDemoSmall")
	> 
	> rxDataStep(inData = AirlineDemoSmall, outFile = AirlineDemoSmallHive,
	+          overwrite = TRUE) # takes about 90 seconds on 1 node cluster
	

	>
	> src_tbls(sc)
	[1] "airlinedemosmall" "mtcarspredres"
	>
	> tbl_cache(sc, "airlinedemosmall")
	> flights_tbl <- tbl(sc, "airlinedemosmall")
	> 
	> flights_tbl
	Source:   query [6e+05 x 3]
	Database: spark connection master=yarn-client app=sparklyr-scaleR-spark-won2r0FHOA-azureuser-39513-2B056E9B34EA4304A8088EB4EE2281D1 local=FALSE
	

	   ArrDelay CRSDepTime DayOfWeek
	      <int>      <dbl>     <chr>
	1        -4   6.666667    Friday
	2        43   8.000000    Friday
	3       -10   6.666667    Sunday
	4       -13   8.000000    Sunday
	5         3   6.666667    Monday
	6        -9   8.000000    Monday
	7       -34  10.249999    Sunday
	8       -15  10.249999    Monday
	9        82  10.249999   Tuesday
	10       13  10.249999 Wednesday
	# ... with 6e+05 more rows
	> 
	> model_data <- flights_tbl %>%
	+   filter(!is.na(ArrDelay)) %>%
	+   select(ArrDelay, CRSDepTime, DayOfWeek)
	> 
	> model_partition <- model_data %>%
	+   sdf_partition(train = 0.8, test = 0.2, seed = 6666)
	> 
	> ml1 <- model_partition$train %>%
	+   ml_linear_regression(CRSDepTime ~ DayOfWeek + ArrDelay)
	* No rows dropped by 'na.omit' call
	> 
	> summary(ml1)
	Call: ml_linear_regression(., CRSDepTime ~ DayOfWeek + ArrDelay)
	

	Deviance Residuals: (approximate):
	     Min       1Q   Median       3Q      Max
	-18.5909  -3.9953  -0.1025   3.8427  11.4574
	

	Coefficients:
	                       Estimate  Std. Error  t value Pr(>|t|)
	(Intercept)         13.29014268  0.01861252 714.0432  < 2e-16 ***
	DayOfWeek_Monday    -0.01653608  0.02504743  -0.6602  0.50913
	DayOfWeek_Saturday  -0.27756084  0.02580578 -10.7558  < 2e-16 ***
	DayOfWeek_Sunday     0.38942350  0.02514204  15.4889  < 2e-16 ***
	DayOfWeek_Thursday   0.06014716  0.02616861   2.2984  0.02154 *
	DayOfWeek_Tuesday   -0.00214295  0.02663735  -0.0804  0.93588
	DayOfWeek_Wednesday  0.04691710  0.02638572   1.7781  0.07538 .
	ArrDelay             0.01315263  0.00016836  78.1235  < 2e-16 ***
	---
	Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
	

	R-Squared: 0.01439
	Root Mean Squared Error: 4.67
	> 
	> rxSparkDisconnect(cc)
	We have shut down the current Spark application and switched to local compute context.
	>
```

## Conclusion

The ability to use both Microsoft R Server and sparklyr from within one Spark session will allow Microsoft R Server users to quickly and seamlessly utilize features provided by sparklyr within their solutions.


## See Also

[What's new in R Server](../whats-new-in-r-server.md)

[Tips on computing with big data](tutorial-large-data-tips.md)

[Diving into Data Analysis](how-to-introduction.md)