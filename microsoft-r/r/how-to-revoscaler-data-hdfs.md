---

# required metadata
title: "Import HDFS data (Machine Learning Server) | Microsoft Docs"
description: "Load data from Hadoop Distributed File System (HDFS) into a RevoScaleR session in Machine Learning Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "06/02/2017"
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

# Import and consume HDFS data files using ReovScaleR

This article explains how to load data from the Hadoop Distributed File System (HDFS) into an R data frame or an .xdf file. Example script shows several use cases for using RevoScaleR functions with HDFS data.

## Load data from HDFS

If you are using RevoScaleR on a Linux system that is part of a Hadoop cluster, you can store and access text or .xdf data files on your system’s native file system or the Hadoop Distributed File System (HDFS). Assuming you already have an .xdf file on HDFS, you can load it by creating an **RxXdfData** object that takes the .xdf file as an input.

	mortDS <- RxXdfData("/share/SampleData/mortDefaultSmall.xdf")
	rxGetInfo(mortDS, numRows = 5)
	rxSummary(~., data = mortDS, blocksPerRead = 2)
	logitObj <- rxLogit(default~F(year) + creditScore + yearsEmploy + ccDebt,
               data = mortDS, blocksPerRead = 2,  reportProgress = 1)
	summary(logitObj)

By default, data is expected to be found on the native file system. If all your data is on HDFS, you can use **rxSetFileSystem** to specify this as a global option:

	rxSetFileSystem(RxHdfsFileSystem())

If only some files are on HDFS, keep the native file system default and use the *fileSystem* argument on **RxTextData** or **RxXdfData** data sources to specify which files are on HDFS. For example:

	hdfsFS <- RxHdfsFileSystem() 
	txtSource <- RxTextData("/test/HdfsData/AirlineCSV/CSVs/1987.csv", fileSystem=hdfsFS)
	xdfSource <- RxXdfData("/test/HdfsData/AirlineData1987", fileSystem=hdfsFS)

The **RxHdfsFileSystem** function creates a file system object for the HDFS file system. You can use **RxNativeFileSystem** function does the same thing for the native file system.

## Write XDF to HDFS

Once you load data from a text file or another source, you can save it as an .xdf file to either HDFS or the native file system. The compute context determines where the file can be saved. To get the current compute context, use **rxGetComputeContext()**. In a local compute context, out files must be written to the native file system. However, by setting the compute context to **RxHadoopMR** or **RxSpark**, you can write to HDFS.

The following example shows how to write a data frame as an .xdf file directly to HDFS using the built-in Iris data set.

1. Set the user name: 

    	> username <- `<your-user-name-here>` 

2. Set folder paths:

		hdfsDataDirRoot <- paste("/home/<user-dir>/<data-dir>/", username, sep="")
		localfsDataDirRoot <- paste("/home/<user-dir>/<data-dir>/", username, sep="")
		setwd(localfsDataDirRoot)
 
3. Set compute context: 

		port <- 8020 # KEEP IF USING THE DEFAULT
		host <- system("hostname", intern=TRUE)
		hdfsFS <- RxHdfsFileSystem(hostName=host, port=port)

		myHadoopCluster <- RxHadoopMR(

		nameNode= host, 
		port=port, 
		consoleOutput=TRUE)
		
		rxSetComputeContext(myHadoopCluster)

4. Write the XDF to a text file on HDFS:

		air7x <- RxXdfData(file='/user/RevoShare/revolution/AirOnTime7Pct', fileSystem = hdfsFS)
		air7t <- RxTextData(file='/user/RevoShare/revolution/AirOnTime7PctText', fileSystem = hdfsFS, createFileSet=TRUE)
		
		rxDataStep(air7x,air7t)

## Write a composite XDF

A *composite XDF* refers to a collection of .xdf files rather than a single .xdf out file. You can create a composite .xdf if you want to load, refresh, and analyze data as a collection of smaller files that can be managed independently or used collectively, depending on the need. 

A composite set consists of a named parent directory with two subdirectories, *data* and *metadata*, containing split data .xdfd files and metadata .xdfm files, respectively. The .xdfm file contains the metadata for all of the .xdfd files under the same parent folder. 

**RxXdfData** is used to specify a composite set, and **rxImport** is used to read in the data and output the generated .xdfd files. 

When the compute context is **RxHadoopMR**, a composite set of XDF is always created. In a local compute context, which you can use on HDFS, you must specify the option *createCompositeSet=TRUE* within the **RxXdfData** if you want the composite set.

**To create a composite file**

1. Use **RxGetComputeContext()** to determine whether you need to set *createCompositeSet*. In **RxHadoopMR** or **RxSpark**, you can omit the argument. For local compute context, add *createCompositeSet=TRUE* to force the composite set.
2. Use **RxTextData** to create a data source object based on a single file or a directory of files.
3. Use **RxXdfData** to create an XDF for use as the *outFile* argument.
3. Use **rxImport** function to read source files and save the output as a composite XDF.

The following example demonstrates creating a composite set of .xdf files within the native file system in a local compute context using a directory of .csv files as input. Prior to trying these examples yourself, copy the sample AirlineDemoSmallSplit folder from the sample data directory to /tmp. Create a second empty folder named testXdf under /tmp to hold the composite file set:

	# Run this command to verify source path. Output should be the AirlineDemoSmallPart?.csv file list.
	list.files("/tmp/AirlineDemoSmallSplit/")

	# Set folders and source and output objects
	AirDemoSrcDir <- "/tmp/AirlineDemoSmallSplit/"
	AirDemoSrcObj <- RxTextData(AirDemoSrcDir)
	AirDemoXdfDir <- "/tmp/testXdf/"
	AirDemoXdfObj <- RxXdfData(AirDemoXdfDir, createCompositeSet=TRUE)

	# Run rxImport to convert the data info XDF and save as a composite file
	rxImport(AirDemoSrcObj, outFile=AirDemoXdfObj)

This creates a directory named testXdf, with the data and metadata subdirectories, containing the split .xdfd and .xdfm files.

	list.files(AirDemoXdfDir, recursive=TRUE, full.names=TRUE)

Output should be as follows:

		[1] "/tmp/testXdf/data/testXdf_1.xdfd"  
		[2] "/tmp/testXdf/data/testXdf_2.xdfd" 
		[3] "/tmp/testXdf/data/testXdf_3.xdfd"   
		[4] "/tmp/testXdf/metadata/testXdf.xdfm"

Run **rxGetInfo** to return metadata, including the number of composite data files, and the first 10 rows.

	rxGetInfo(AirDemoXdfObj, getVarInfo=TRUE, numRows=10)
			File name: /tmp/testXdf 
			Number of composite data files: 3 
			Number of observations: 6e+05 
			Number of variables: 3 
			Number of blocks: 3 
			Compression type: zlib 
			Variable information: 
			Var 1: ArrDelay, Type: character
			Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
			Var 3: DayOfWeek, Type: character
			Data (10 rows starting with row 1): 
			ArrDelay CRSDepTime DayOfWeek
			1         6   9.666666    Monday
			2        -8  19.916666    Monday
			3        -2  13.750000    Monday
			4         1  11.750000    Monday
			5        -2   6.416667    Monday
			6       -14  13.833333    Monday
			7        20  16.416666    Monday
			8        -2  19.250000    Monday
			9        -2  20.833334    Monday
			10      -15  11.833333    Monday

## Control generated file output

Number of generated .xdfd files depends on characteristics of source data, but it is generally one file per HDFS block. However, if the original source data is distributed among multiple smaller files, each file counts as a block even if the file size is well below HDFS block size, thus the files The HDFS block size varies from installation to installation, but is typically either 64MB or 128MB. For more in depth information about the composite XDF format and its use within a Hadoop compute context, see [Get started with HadoopMR and RevoScaleR](how-to-revoscaler-hadoop.md).

Filenames are based on the parent directory name.

Rows per file are influenced by compute context: 

+ When compute context is local with *createCompositeSet=TRUE*, the number of blocks put into each .xdfd file in the composite set.
+ When compute context is HadoopMR, the number of rows in each .xdfd file is determined by the rows assigned to each MapReduce task, and the number of blocks per .xdfd file is therefore determined by *rowsPerRead*. 

## Load a composite XDF 

You can reference a composite XDF using the data source object used as the *outFile* for **rxImport**. To load a composite XDF residing on the HDFS file system, set **RxXdfData** to the parent folder having data and metadata subdirectories:

	rxSetFileSystem(RxHdfsFileSystem())
	TestXdfObj <- RxXdfData("/tmp/TestXdf/")
	rxGetInfo(TestXdfObj, numRows = 5)
	rxSummary(~., data = TestXdfObj)

Note that in this example we set the file system to HDFS globally so we did not need to specify the file system within the data source constructors.

## Using RevoScaleR with rhdfs

If you are using both RevoScaleR and the RHadoop connector package rhdfs, you need to ensure that the two do not interfere with each other. The rhdfs package depends upon the rJava package, which will prevent access to HDFS by RevoScaleR if it is called before RevoScaleR makes its connection to HDFS. 

To prevent this interaction, use the function rxHdfsConnect to establish a connection between RevoScaleR and HDFS. An install-time option on Linux can be used to trigger such a call from the Rprofile.site startup file. If the install-time option is not chosen, you can add it later by setting the REVOHADOOPHOST and REVOHADOOPPORT environment variables with the host name of your Hadoop name node and the name node’s port number, respectively. 

You can also call rxHdfsConnect interactively within a session, provided you have not yet attempted any other rJava or rhdfs commands. For example, the following call will fix a connection between the Hadoop host sandbox-01 and RevoScaleR; if you make a subsequent call to rhdfs, RevoScaleR can continue to use the previously established connection. Note that once rhdfs (or any other rJava call) has been invoked, you cannot change the host or port you use to connect to RevoScaleR:

	rxHdfsConnect(hostName = "sandbox-01", port = 8020)

## Next steps

Related articles include best practices for XDF file management, including managing split files, and compute context:

+ [XDF files in Microsoft R](concept-what-is-xdf.md)	
+ [Compute context in Microsoft R](concept-what-is-compute-context.md)	

To further your understanding of RevoScaleR usage with HadoopMR or Spark, continue with the following articles:

+ [Data import and exploration on Apache Spark](how-to-revoscaler-spark.md)	
+ [Data import and exploration on Hadoop MapReduce](how-to-revoscaler-hadoop.md)	

## See Also

 [Machine Learning Server](../what-is-machine-learning-server.md) 
 [Install Machine Learning Server on Linux](../install/machine-learning-serverl-linux-install.md)  
 [Install Machine Learning Server on Hadoop](../install/machine-learning-server-hadoop-install.md)