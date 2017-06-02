---

# required metadata
title: "Import relational data using ODBC (Microsoft R)"
description: "How to import relational data using ODBC and rxImport in RevoScaleR"
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "05/25/2017"
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

For SQL Server, you can use **RxSqlServerData** or **RxOdbcData** interchangeably, unless you are accessing [R objects stored in a SQL Server table](https://docs.microsoft.com/sql/advanced-analytics/r/save-and-load-r-objects-from-sql-server-using-odbc), in which case **RxOdbdData** is required. For examples and instructions on using SQL Server data in Microsoft R, see [Import SQL data from Azure SQL Database and SQL Server](scaler-data-sql.md).

## How to configure ODBC for relational data access

Follow these steps to configure ODBC for loading data from an external relational database.

### Step 1: Install unixODBC

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

### Step 2: Install drivers

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


### Step 3: Connect

This step explains how to specify the connection and the **RxOdbcData** data source object that wraps additional properties around a database connection. You can create an object for almost any relational database, including SQL Server. 

An **RxOdbcData** object supports local compute context only, which means that when you create the object, any read or write operations are executed by R Server on the local machine.



## Examples

This example uses a connection string to connect to a local SQL Server instance and the AdventureWorks database. For simplicity, the connection is further scoped to a single table, but you could write T-SQL to select a more interesting data set. 

~~~~
sConnectionStr \<- "Driver={SQL Server};Server=win-database01;
 Database=TestData;Uid=mktest;Pwd=sqlpwd;"

	sConnectionStr <- "Driver={SQL Server};Server=win-database01; 
		Database=TestData;Uid=mktest;Pwd=sqlpwd;"
	claimsSQL = "SELECT * FROM claims"
	claimsDS <- RxOdbcData(sqlQuery = claimsSQL,
		connectionString=sConnectionStr)
	claimsFile <- RxXdfData("claimsFromODBC.xdf")
	rxImport(claimsDS, claimsFile, overwrite=TRUE)
	rxGetInfo(claimsFile, getVarInfo=TRUE, numRows=10)
~~~~

This returns the following:

~~~~	
	File name: claimsFromODBC.xdf 
	Number of observations: 128 
	Number of variables: 6 
	Number of blocks: 1 
	Variable information: 
	  Var 1: RowNum, Type: integer, Low/High: (1, 128)
	  Var 2: age, Type: character, Storage: fixed, Width: 10
	  Var 3: car.age, Type: character, Storage: fixed, Width: 10
	  Var 4: type, Type: character, Storage: fixed, Width: 10
	  Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
	  Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)
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
~~~~	

Since we are extracting all the data from the table with our SQL query, it can be simpler to just provide the table argument to `RxOdbcData`:

~~~~
		sConnectionStr <- "Driver={SQL Server};Server=win-database01; 
			Database=TestData; Uid=mktest; Pwd=sqlpwd;"
	claimsDS2 <- RxOdbcData(table="claims",	connectionString=sConnectionStr)
		claimsFile <- RxXdfData("claimsFromODBC.xdf")
		rxImport(claimsDS2, claimsFile, overwrite=TRUE)
		rxGetInfo(claimsFile, getVarInfo=TRUE, numRows=10) 
~~~~

This gives the same results as before.

This last example is unrelated to previous examples, but it demonstrates writing a table back to the database. This operation includes use of the `RxOdbcData` data source and the `rxDataStep` function. 

~~~~
myTable <- RxOdbcData(table="mtcars", connectionString = connectionString)
rxDataStep(mtcars, myTable, overwrite=TRUE) 
head(myTable) 
rxGetInfo(myTable, getVarInfo=TRUE) 
myCars <- rxDataStep(myTable) 
head(myCars)
~~~~


### RxOdbcData Example for Oracle Express 

Oracle Express is a free version of the popular Oracle database management system intended for evaluation and education. It uses the same ODBC drivers as the commercial offerings. The follow example demonstrates an Oracle SQL statement to show all the tables in a database (this differs from standard SQL implementations):

	tablesDS <- RxOdbcData(sqlQuery="select * from user_tables", connectionString = "DSN=ORA10GDSN;Uid=system;Pwd=X8dzlkjWQ")
	OracleTableDF <- rxImport(tablesDS, overwrite=TRUE)
	OracleTableDF[,1]

This yields a list of tables similar to the following:

	  [1] "MVIEW$_ADV_WORKLOAD"           "MVIEW$_ADV_BASETABLE"         
	  [3] "MVIEW$_ADV_SQLDEPEND"          "MVIEW$_ADV_PRETTY"            
	  [5] "MVIEW$_ADV_TEMP"               "MVIEW$_ADV_FILTER"            
	  [7] "MVIEW$_ADV_LOG"                "MVIEW$_ADV_FILTERINSTANCE"    
	  [9] "MVIEW$_ADV_LEVEL"              "MVIEW$_ADV_ROLLUP"            
	 [11] "MVIEW$_ADV_AJG"                "MVIEW$_ADV_FJG"               
	 [13] "MVIEW$_ADV_GC"                 "MVIEW$_ADV_CLIQUE"            
	 [15] "MVIEW$_ADV_ELIGIBLE"           "MVIEW$_ADV_OUTPUT"            
	 [17] "MVIEW$_ADV_EXCEPTIONS"         "MVIEW$_ADV_PARAMETERS"        
	 [19] "MVIEW$_ADV_INFO"               "MVIEW$_ADV_JOURNAL"           
	 [21] "MVIEW$_ADV_PLAN"               "AQ$_QUEUE_TABLES"             
	 [23] "AQ$_QUEUES"                    "AQ$_SCHEDULES"                
	 [25] "AQ$_INTERNET_AGENTS"           "AQ$_INTERNET_AGENT_PRIVS"     
	 [27] "DEF$_AQCALL"                   "DEF$_AQERROR"                 
	 [29] "DEF$_ERROR"                    "DEF$_DESTINATION"             
	 [31] "DEF$_CALLDEST"                 "DEF$_DEFAULTDEST"             
	 [33] "DEF$_LOB"                      "DEF$_TEMP$LOB"                
	 [35] "DEF$_PROPAGATOR"               "DEF$_ORIGIN"                  
	 [37] "DEF$_PUSHED_TRANSACTIONS"      "OL$"                          
	 [39] "OL$HINTS"                      "OL$NODES"                     
	 [41] "LOGMNR_SESSION_EVOLVE$"        "LOGMNR_HEADER1$"              
	 [43] "LOGMNR_HEADER2$"               "LOGMNR_UID$"                  
	 [45] "LOGMNRC_DBNAME_UID_MAP"        "LOGMNR_DICTSTATE$"            
	 [47] "LOGMNR_DICTIONARY$"            "LOGMNR_OBJ$"                  
	 [49] "LOGMNR_USER$"                  "LOGMNRC_GTLO"                 
	 [51] "LOGMNRC_GTCS"                  "LOGMNRC_GSII"                 
	 [53] "LOGMNR_TAB$"                   "LOGMNR_COL$"                  
	 [55] "LOGMNR_ATTRCOL$"               "LOGMNR_TS$"                   
	 [57] "LOGMNR_IND$"                   "LOGMNR_TABPART$"              
	 [59] "LOGMNR_TABSUBPART$"            "LOGMNR_TABCOMPART$"           
	 [61] "LOGMNR_TYPE$"                  "LOGMNR_COLTYPE$"              
	 [63] "LOGMNR_ATTRIBUTE$"             "LOGMNR_LOB$"                  
	 [65] "LOGMNR_CDEF$"                  "LOGMNR_CCOL$"                 
	 [67] "LOGMNR_ICOL$"                  "LOGMNR_LOBFRAG$"              
	 [69] "LOGMNR_INDPART$"               "LOGMNR_INDSUBPART$"           
	 [71] "LOGMNR_INDCOMPART$"            "LOGMNRP_CTAS_PART_MAP"        
	 [73] "LOGMNRT_MDDL$"                 "LOGMNR_LOG$"                  
	 [75] "LOGMNR_PROCESSED_LOG$"         "LOGMNR_SPILL$"                
	 [77] "LOGMNR_AGE_SPILL$"             "LOGMNR_RESTART_CKPT_TXINFO$"  
	 [79] "LOGMNR_ERROR$"                 "LOGMNR_RESTART_CKPT$"         
	 [81] "LOGMNR_FILTER$"                "LOGMNR_PARAMETER$"            
	 [83] "LOGMNR_SESSION$"               "LOGSTDBY$PARAMETERS"          
	 [85] "LOGSTDBY$EVENTS"               "LOGSTDBY$APPLY_PROGRESS"      
	 [87] "LOGSTDBY$APPLY_MILESTONE"      "LOGSTDBY$SCN"                 
	 [89] "LOGSTDBY$PLSQL"                "LOGSTDBY$SKIP_TRANSACTION"    
	 [91] "LOGSTDBY$SKIP"                 "LOGSTDBY$SKIP_SUPPORT"        
	 [93] "LOGSTDBY$HISTORY"              "REPCAT$_REPCAT"               
	 [95] "REPCAT$_FLAVORS"               "REPCAT$_REPSCHEMA"            
	 [97] "REPCAT$_SNAPGROUP"             "REPCAT$_REPOBJECT"            
	 [99] "REPCAT$_REPCOLUMN"             "REPCAT$_KEY_COLUMNS"          
	[101] "REPCAT$_GENERATED"             "REPCAT$_REPPROP"              
	[103] "REPCAT$_REPCATLOG"             "REPCAT$_DDL"                  
	[105] "REPCAT$_REPGROUP_PRIVS"        "REPCAT$_PRIORITY_GROUP"       
	[107] "REPCAT$_PRIORITY"              "REPCAT$_COLUMN_GROUP"         
	[109] "REPCAT$_GROUPED_COLUMN"        "REPCAT$_CONFLICT"             
	[111] "REPCAT$_RESOLUTION_METHOD"     "REPCAT$_RESOLUTION"           
	[113] "REPCAT$_RESOLUTION_STATISTICS" "REPCAT$_RESOL_STATS_CONTROL"  
	[115] "REPCAT$_PARAMETER_COLUMN"      "REPCAT$_AUDIT_ATTRIBUTE"      
	[117] "REPCAT$_AUDIT_COLUMN"          "REPCAT$_FLAVOR_OBJECTS"       
	[119] "REPCAT$_TEMPLATE_STATUS"       "REPCAT$_TEMPLATE_TYPES"       
	[121] "REPCAT$_REFRESH_TEMPLATES"     "REPCAT$_USER_AUTHORIZATIONS"  
	[123] "REPCAT$_OBJECT_TYPES"          "REPCAT$_TEMPLATE_REFGROUPS"   
	[125] "REPCAT$_TEMPLATE_OBJECTS"      "REPCAT$_TEMPLATE_PARMS"       
	[127] "REPCAT$_OBJECT_PARMS"          "REPCAT$_USER_PARM_VALUES"     
	[129] "REPCAT$_TEMPLATE_SITES"        "REPCAT$_SITE_OBJECTS"         
	[131] "REPCAT$_RUNTIME_PARMS"         "REPCAT$_TEMPLATE_TARGETS"     
	[133] "REPCAT$_EXCEPTIONS"            "REPCAT$_INSTANTIATION_DDL"    
	[135] "REPCAT$_EXTENSION"             "REPCAT$_SITES_NEW"            
	[137] "SQLPLUS_PRODUCT_PROFILE"       "HELP"                         
	[139] "TestFile"                      "1987"    


**Working with MySQL Files on Red Hat Enterprise Linux**

You first need to specify the name of your DSN. On Linux, this will be the same as the name you specify for the ODBC configuration.

	### Test of import from 'centos-database01' ###
	
	sConnectionStr = "DSN=ScaleR-ODBC-test"
	sUserSQL = "SELECT * FROM airline1987"
	airlineXDFName <- file.path(getwd(), "airlineimported.xdf")
	airlineODBCSource <- RxOdbcData(sqlQuery = sUserSQL, connectionString = sConnectionStr, useFastRead = TRUE)
	rxImport(airlineODBCSource, airlineXDFName, overwrite = TRUE)

**Limitations on SQL Queries**

The sqlQuery argument to `RxOdbcData` is limited to data extraction queries (SELECT and SHOW statements, primarily) because *RxOdbcData* is currently intended to be used for reading data from the database into a .xdf file. In particular, INSERT queries are not supported. Also, because each query is used to populate a single .xdf file, multiple queries (that is, queries separated by a semicolon “;”) are not supported. Compound queries, however, that produce a single extracted table, (that is, queries linked by AND or OR, or involving multiple FROM clauses) are supported.

## Specifying Variable Data Types

Data stored in databases may be stored differently from how you want to store the data in R. You can use the `colClasses`, `colInfo`, and `stringsAsFactors` arguments to `RxOdbcData` to specify how columns are stored in R. The `stringsAsFactors` argument is the simplest to use; if specified, any character string column not otherwise accounted for by the `colClasses` or `colInfo` argument is stored as a factor in R, with levels defined according to the unique character strings found in the column. The `colClasses` argument allows you to specify a particular class for a particular variable; `colInfo` is similar, but it also allows you to specify a set of levels for a factor variable. The following example shows all three arguments being combined to allow us to modify the data types of several variables in the claims data:

	sConnectionStr <- "Driver={SQL Server};Server=win-database01; Database=TestData;Uid=mktest;Pwd=sqlpwd;";
	colInfoList <- list("car.age" = list(type = "factor", levels = c("0-3", 
		"4-7", "8-9", "10+")))
	claimsFile <- RxXdfData("claimsFromODBC_Modified.xdf")
	claimsSQL = "SELECT * FROM claims"
	claimsDS <- RxOdbcData(sqlQuery = claimsSQL, connectionString=sConnectionStr,
		colClasses = c(number="integer"), colInfo = colInfoList, 
		stringsAsFactors=TRUE)
	rxImport(claimsDS, claimsFile, overwrite=TRUE)
	rxGetInfo(claimsFile, getVarInfo = TRUE) 

This returns the following:

	File name: claimsFromODBC_Modified.xdf 
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
	Var 6: number, Type: integer, Low/High: (0, 434)


## Next Steps

Continue on to the following data import articles to learn more about XDF, data source objects, and other data formats:

+ [Write back to an ODBC data source using rxDataStep](scaler-data-writeback.md)
+ [XDF files](scaler-data-xdf.md)	
+ [Data Sources](scaler-user-guide-data-source.md)	
+ [Import text data](scaler-user-guide-data-import.md)
+ [Import and consume data on HDFS](scaler-data-hdfs.md)

## See Also
   
 [RevoScaleR Functions](scaler/scaler.md)   
 [Tutorial: data import and exploration](scaler-getting-started-data-import-exploration.md)
 [Tutorial: data manipulation and statistical analysis](scaler-getting-started-data-manipulation.md) 
 