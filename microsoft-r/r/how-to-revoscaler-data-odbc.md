---

# required metadata
title: "Import relational data using ODBC (Microsoft R)"
description: "How to import relational data using ODBC and rxImport in RevoScaleR"
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "05/25/2017"
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

# Import relational data using ODBC (Microsoft R)

RevoScaleR allows you to read or write data from virtually any database for which you can obtain an ODBC driver, a standard software interface for accessing relational data. ODBC connections are enabled through drivers and a driver manager. Drivers handle the translation of requests from an application to the database. The ODBC Driver Manager sets up and manages the connection between them.

Both drivers and an ODBC Driver Manager must be installed on the computer running Microsoft R. On Windows, the driver manager is built in. On Linux systems, RevoScaleR supports [unixODBC](http://www.unixodbc.org/), which you will need to install. Once the manager is installed, you can proceed to install individual database drivers for all of the data sources you need to support.

To import data from a relational database management system, do the following:

1. Install an ODBC Driver Manager (Linux only)
2. Install ODBC drivers
3. Create an **RxOdbcData** data source
4. For read operations, use **rxImport** to read data
5. For write operations, use **rxDataStep** to write data back to the data source

## About ODBC  in Microsoft R

In Microsoft R, an ODBC and **RxOdbcData** dependency exists for data imported from relational database systems like Oracle, PostGreSQL, and MySQL, to name a few. ODBC is primarily required for **RxOdbcData** data sources, but it is also used internally for DBMS-specific connections to SQL Server. If you delete the ODBC driver for SQL Server and then try to use **RxSqlServerData**, the connection fails. 

Although ODBC drivers exist for text data, Microsoft R does not use ODBC for sources accessed as file-based reads. To be specific, it's not used for accessing text files, SPSS database files, or SAS database files.

For Teradata, you should avoid ODBC and create an **RxTeradata** data source instead (see [RevoScaleR Teradata Getting Started Guide](https://msdn.microsoft.com/en-us/microsoft-r/scaler-teradata-getting-started) for details).

For SQL Server, you can use **RxSqlServerData** or **RxOdbcData** interchangeably, unless you are accessing [R objects stored in a SQL Server table](https://docs.microsoft.com/sql/advanced-analytics/r/save-and-load-r-objects-from-sql-server-using-odbc), in which case **RxOdbdData** is required. For examples and instructions on using SQL Server data in Microsoft R, see [Import SQL data from Azure SQL Database and SQL Server](how-to-revoscaler-data-sql.md).

## How to configure ODBC for relational data access

Follow these steps to configure ODBC for loading data from an external relational database.

### 1 - Install unixODBC 

**Linux only**

An ODBC Driver Manager manages communication between applications and drivers. On Linux, an ODBC Driver Manager is not typically bundled in a Linux distribution. Although several driver managers are available, Microsoft R supports the [unixODBC Driver Manager](http://www.unixodbc.org/). 

To first check whether unixODBC is installed, do the following:

+ On RHEL: `rpm –qa | grep unixODBC`	
+ On Ubuntu: `apt list --installed | grep unixODBC`	
+ On SLES: `zypper se unixODBC`	

If unixODBC is not installed, issue the following commands to add it:

+ On RHEL: `sudo yum install unixodbc-devel unixodbc-bin unixodbc`
+ On Ubuntu: `sudo apt-get install unixodbc-devel unixodbc-bin unixodbc`
+ On SLES: `sudo zypper install unixODBC`

For more information, SQL Server documentation provides in-depth instructions for [installing unixODBC](https://docs.microsoft.com/sql/connect/odbc/linux/installing-the-driver-manager) on a variety of Linux operating systems. Alternatively, you can go to the [unixODBC web site](http://www.unixodbc.org/), download the software, and follow instructions on the site.

### 2 - Install drivers

ODBC drivers must be installed on the machine running R Server or R Client. You will need a driver that corresponds to the database version you plan to use. To check which drivers are installed, use the instructions below.

**On Linux**

1. To list existing drivers: `odbcinst -q -d`
2. Driver version information is in the odbcinst.ini file. To find the location of that file: `odbcinst -j`
3. Typically, driver information is in /etc/odbcinst.ini. To view its contents: `gedit /etc/odbcinst.ini`
4. You can also list any existing ODBC data sources already on the system: `odbcinst -q -s`

**On Windows**

1. Search for *odbc data sources* to find the ODBC Data Source Administrator (64-bit) application.
2. In ODBC Data Source Administrator > Drivers tab, review the currently installed drivers.

If the driver you need is missing, download and install the driver using instructions provided by the vendor. A partial list of download links is provided below.

**ODBC download sites for commonly used databases**

Drivers for commonly used databases might be pre-installed with the operating system or database management system, but if not, most can be downloaded from database vendor web sites. The following table provides links to download pages for several data platforms.

Data platform | Driver download pages and instructions|
-------------|---------------------------------|
SQL Server on Linux | [Installing the Microsoft ODBC Driver for SQL Server on Linux](https://docs.microsoft.com/sql/connect/odbc/linux/installing-the-microsoft-odbc-driver-for-sql-server-on-linux) |
Oracle Instant Client | [Oracle Instant Client Downloads](http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html) |
MySQL | [Download Connector/ODBC](https://dev.mysql.com/downloads/connector/odbc) |
PostgreSQL | [psqlODBC](https://odbc.postgressql.org) |
Cloudera | [Cloudera ODBC Driver for Hive](https://www.cloudera.com/downloads/connectors/hive/odcbc/2-5-12.html) |

### 3 - Connect and import

This step uses examples to illustrate connection strings, query strings, **RxOdbcData** data source objects, XDF objects, and import commands for various platform configurations.

Query strings must consist of data extraction queries (SELECT and SHOW statements) that populate a single data frame or .xdf file. As long as a single extracted table is produced, you can use queries linked by AND, OR, and multiple FROM clauses. Unsupported syntax includes INSERT queries or multiple query strings separated by a semicolon.

Recall that **RxOdbcData** provides local compute context only, which means that when you create the object, any read or write operations are executed by R Server on the local machine.

#### Using SQL Server

This example uses a connection string to connect to a local SQL Server instance and the [RevoClaimsDB database](sample-built-in-data.md). For simplicity, the connection is further scoped to a single table, but you could write T-SQL to select a more interesting data set. 

	sConnectStr <- "Driver={ODBC Driver 13 for SQL Server};Server=(local);Database=RevoClaimsDB;Trusted_Connection=Yes"
	sQuery = "Select * from dbo.claims"
	sDS <-RxOdbcData(sqlQuery=sQuery, connectionString=sConnectStr)
	sXdf <- RxXdfData("c:/users/temp/claimsFromODBC.xdf")
	rxImport(sDS, sXdf, overwrite=TRUE)

This returns the following:

	> rxGetInfo(sXdf, getVarInfo=TRUE)
	File name: c:\Users\TEMP\claimsFromODBC.xdf 
	Number of observations: 128 
	Number of variables: 6 
	Number of blocks: 1 
	Compression type: zlib 
	Variable information: 
	Var 1: "RowNum", Type: character
	Var 2: "age", Type: character
	Var 3: "car age", Type: character
	Var 4: "type", Type: character
	Var 5: "cost", Type: character
	Var 6: "number", Type: character
	
> [!Tip]
> If you are selecting an entire table or view, a simpler alternative for the query string is just the name of that object specified as an argument on **RxOdbcData**. For example: `sDS <-RxOdbcData(tabble=dbo.claims, connectionString=sConnectStr)`. With this shortcut, you can omit the sQuery object from your script.

By amending the **RxOdbcData** object with additional arguments, you can use *colClasses* to recast numeric "character" data as integers or float, and use *stringsAsFactors* to convert all unspecified character variables to factors:

	sDS <-RxOdbcData(sqlQuery=sQuery, connectionString=sConnectStr, colClasses=c(RowNum="integer", number="integer"), stringsAsFactors=TRUE)
	sXdf <- RxXdfData("c:/users/temp/claimsFromODBC.xdf")
	rxImport(sDS, sXdf, overwrite=TRUE)

Data values for age and car.age are expressed as ranges in the original data, so those columns (as well as the type column) are appropriately converted to factors through the *stringsAsFactors* argument. Retaining the character data type for cost is also appropriate for the existing data. While cost has mostly numeric values, it also has multiple instances of "NA". 

After re-import, variable metadata should be as follows:

	> rxGetInfo(sXdf, getVarInfo=TRUE)
	Variable information: 
	Var 1: RowNum, Type: integer, Low/High: (1, 128)
	Var 2: age
		8 factor levels: 17-20 21-24 25-29 30-34 35-39 40-49 50-59 60+
	Var 3: car age
		4 factor levels: 0-3 4-7 8-9 10+
	Var 4: type
		4 factor levels: A B C D
	Var 5: cost
		100 factor levels: 289.00 282.00 133.00 160.00 372.00 ... 119.00 385.00 324.00 192.00 123.00
	Var 6: number, Type: integer, Low/High: (0, 434)

#### Using Oracle Express 

Oracle Express is a free version of the popular Oracle database management system intended for evaluation and education. It uses the same ODBC drivers as the commercial offerings. The follow example demonstrates an Oracle SQL statement to show all the tables in a database (this differs from standard SQL implementations):

	tablesDS <- RxOdbcData(sqlQuery="select * from user_tables", connectionString = "DSN=ORA10GDSN;Uid=system;Pwd=X8dzlkjWQ")
	OracleTableDF <- rxImport(tablesDS, overwrite=TRUE)
	OracleTableDF[,1]

This yields a list of tables similar to the following (showing partial results for brevity, with the first 10 tables):

	  [1] "MVIEW$_ADV_WORKLOAD"           "MVIEW$_ADV_BASETABLE"         
	  [3] "MVIEW$_ADV_SQLDEPEND"          "MVIEW$_ADV_PRETTY"            
	  [5] "MVIEW$_ADV_TEMP"               "MVIEW$_ADV_FILTER"            
	  [7] "MVIEW$_ADV_LOG"                "MVIEW$_ADV_FILTERINSTANCE"    
	  [9] "MVIEW$_ADV_LEVEL"              "MVIEW$_ADV_ROLLUP"            

#### Using MySQL on Red Hat Enterprise Linux

As a first step, specify the name of your DSN. On Linux, this is the same name specified for the ODBC configuration.

	### Test of import from 'centos-database01' ###
	sConnectionStr = "DSN=ScaleR-ODBC-test"
	sUserSQL = "SELECT * FROM airline1987"
	airlineXDFName <- file.path(getwd(), "airlineimported.xdf")
	airlineODBCSource <- RxOdbcData(sqlQuery = sUserSQL, connectionString = sConnectionStr, useFastRead = TRUE)
	rxImport(airlineODBCSource, airlineXDFName, overwrite = TRUE)

## Next Steps

Continue on to the following data import articles to learn more about XDF, data source objects, and other data formats:

+ [Import SQL data](how-to-revoscaler-data-sql.md)	
+ [Import text data](how-to-revoscaler-data-import.md)
+ [Import and consume data on HDFS](how-to-revoscaler-data-hdfs.md)	
+ [XDF files](concept-what-is-xdf.md)	
+ [Data Sources](how-to-revoscaler-data-source.md)	

## See Also
   
 [RevoScaleR Functions](~/r-reference/revoscaler/revoscaler.md)   
 [Tutorial: data import and exploration](tutorial-revoscaler-data-import-transform.md)
 [Tutorial: data manipulation and statistical analysis](tutorial-revoscaler-data-model-analysis.md) 
 