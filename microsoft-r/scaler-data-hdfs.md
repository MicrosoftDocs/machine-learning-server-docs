---

# required metadata
title: "Import and consume HDFS data files in Microsoft R"
description: "Load data from Hadoop Distributed File System into Microsoft R."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/23/2017"
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

# Import and consume HDFS data files in Microsoft R

**Applies to: Microsoft R Server**

## Importing data into multiple XDF files for Hadoop

The .xdf file format has been modified for analyses on Hadoop to store data on HDFS in a composite set of files rather than a single file. The composite set consists of a named directory with two subdirectories, ‘data’ and ‘metadata’, containing split ‘.xdfd’ files and a metadata ‘.xdfm’ files respectively. Data is split into individual ‘.xdfd’ files such that each file remains within a single HDFS block. (The HDFS block size varies from installation to installation but is typically either 64MB or 128MB). The ‘.xdfm’ file contains the metadata for all of the .xdfd files. For more in depth information about the composite XDF format and its use within a Hadoop compute context see the [*RevoScaleR MapReduce Getting Started Guide*](scaler-hadoop-getting-started.md).

In most cases, a single .xdf file will be the most efficient way to store and analyze your data in a local compute context, unless you are using data from HDFS as input. Even so, you can still read, write and analyze a set of composite XDF files in a local compute context.

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


## See Also

 [Introduction to R Server](rserver.md) 
 [Install R Server on Windows](rserver-install-windows.md)  
 [Install R Server on Linux](rserver-install-linux-server.md)  
 [Install R Server on Hadoop](rserver-install-hadoop.md)