---

# required metadata
title: "Import data from Azure SQL Database and SQL Server (Microsoft R)"
description: "How to import relational data from Azure SQL and SQL Server databases in RevoScaleR"
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

# Import SQL Server relational data (Microsoft R)

This article explains how to import relational data from a SQL table or view into a data frame or .xdf file in Microsoft R. Source data can originate from Azure SQL Database or an on-premises SQL Server instance.

## Prerequisites

+ [Azure subscription, Azure SQL Database, AdventureWorksLT	sample database](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)
+ SQL Server (any supported version and edition) with the AdventureWorks sample database	
+ Microsoft R Server 9.1 for Windows or Linux	
+ R console application (RGui.exe on Windows, or Revo64 on Linux)

Windows users, remember that R is case-sensitive and that file paths use the forward slash as a delimiter.

## How to import from Azure SQL Database

The following script demonstrates how to import data from Azure SQL database into a local XDF. This script sets the connection string, defines the SQL query, the .xdf file, and the **rxImport** command for loading data and saving the output:

	sConnString <- "Driver={ODBC Driver 13 for SQL Server}; Server=tcp:<your-server-name>.database.windows.net,1433; Database=AdventureWorksLT; Uid=<your-user-name>; Pwd=<your-password>; Encrypt=yes; TrustServerCertificate=no; Connection Timeout=30;"

	sQuery <- "select ProductDescriptionID, Description, ModifiedDate from SalesLT.ProductDescription"

	sDataSet <- RxOdbcData(sqlQuery=sQuery, connectionString=sConnString)

	sDataFile <- RxXdfData("c:/users/temp/mysqldata.xdf")

	rxImport(sDataSet, sDataFile, overwrite=TRUE)

	rxGetInfo(sDataFile, getVarInfo=TRUE, numRows=50)

To understand what this script is doing, lets explore it step by step and learn how to obtain valid information for each command.

### Step 1: Connect

Create the connection object using information from the Azure portal and ODBC Data Source Administrator.

	> sConnString <- "Driver={ODBC Driver 13 for SQL Server}; Server=tcp:<your-server-name>.database.windows.net,1433; Database=AdventureWorksLT; Uid=<your-user-name>; Pwd=<your-password>; Encrypt=yes; TrustServerCertificate=no; Connection Timeout=30;"

The driver used on the connection is an ODBC driver. On Windows, the ODBC driver is provided by the operating system. You can run the **ODBC Data Source Administrator (64-bit)** app to verify the driver is listed in the **Drivers** tab. On Linux, the ODBC driver manager and individual drivers must be installed manually. For pointers, see [How to import relational data using ODBC](sqaler-data-odbc.md).

All of the remaining connection properties are obtained from the Azure portal.

1. Sign in to the [Azure portal](https://ms.portal.azure.com) and locate AdventureWorksLT. 

2. In **Overview > Essentials > Connection strings**, click **Show database connection strings**.

3. On the **ODBC** tab, copy the connection information. It should look similar to the following string, except the server name and user name will be valid for your database. The password is always a placeholder, which you must replace with a valid value.

	Driver={ODBC Driver 13 for SQL Server}; Server=tcp:<your-server-name>.database.windows.net,1433; Database=AdventureWorksLT; Uid=<your-user-name>; Pwd=<your-password>; Encrypt=yes; TrustServerCertificate=no; Connection Timeout=30;

4. Replace the *sConnString* in the example script with a valid connection string having the correct server name, user name, and password, and then run the command in your R console application to create the connection object.

### Step 2: Firewall

On Azure SQL Database, access is controlled through firewall rules for specific IP addresses.

1. From your client machine running Microsoft R, sign in to the [Azure portal](https://ms.portal.azure.com) and locate AdventureWorksLT. 

2. In **Overview**, click **Set server firewall > Add client IP** to create a rule for the local client. Click **Save** once the rule is created.

On a small private network, IP addresses are most likely static, and the IP address detected in the portal is probably correct. To confirm, you can use **IPConfig** on Windows or **hostname -I** on Linux to get your IP address. 

On corporate networks, IP addresses can change on computer restarts, through network address translations, or other reasons described in [this article](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).
  
One way to get the right IP address is from the error message reported on a connection failure. If you get the "ODBC Error in SQLDisconnect" error message, it will include this text: "Client with IP address *<ip-address>* is not allowed to access the server". The IP address reported in the message can be specified as the start and ending IP range in your firewall rule. 

Producing this error is easy. Just run the entire example script (with a valid connection string), concluding with **rxImport**. When **rxImport** fails, copy the IP address reported in the message, and use it to set the firewall rule in the Azure portal. Wait a few minutes, and then retry the script.

### Step 3: Query

In the R console application, create the SQL query object. The example query consists of columns from a single table, but any valid T-SQL query providing a rowset is acceptable. This table was chosen because it includes numeric data.

	> squery <-"SELECT SalesOrderID, SalesOrderDetailID, OrderQty, UnitPrice, UnitPriceDiscount, LineTotal FROM SalesLT.SalesOrderDetail"

Before attempting unqualified *SELECT * FROM* queries, review the columns in your database for unhandled data types in R. In AdventureWorksLT, the *rowguid(uniqueidentifier)* column is not handled. Other unsupported data types are [listed here](https://docs.microsoft.com/sql/advanced-analytics/r/r-libraries-and-data-types#data-types-not-supported-by-r).

Queries with unsupported data types produce the error below. If you get this error and cannot immediately detect the unhandled data type, incrementally add fields to the query to isolate the problem.
  
		Unhandled SQL data type!!!
		Could not open data source.
		Error in doTryCatch(return(expr), name, parentenv, handler)

### Step 4: RxOdbcData

Create the **RxOdbcData** data object using the connection and query object specifying which data to retrieve. This exercise uses only a few arguments, but to learn more about data sources, see [Data sources in Microsoft R](scaler-user-guide-data-source.md).

		> sDataSet <- RxOdbcData(sqlQuery=sQuery, connectionString=sConnString)

### Step 5: XDF

Create the XDF file to save the data to disk. Check the folder permissions for write access. 

		> sDataFile <- RxXdfData("c:/users/temp/mysqldata.xdf")

By default, Windows does not allow external writes by non-local users. You might want to create a single folder, such as C:/Users/Temp, and give *Everyone* write permissions. Once the XDF is created, you can move the file elsewhere and delete the folder, or revoke the write permissions you just granted.

On Linux, you can use this alternative path:

		> sDataFile <- RxXdfData(/tmp/mysqldata.xdf")

### Step 6: rxImport

Run **rxImport** with *inData* and *outFile* arguments. Include *overwrite* so that you can rerun the query with different queries without having to the delete the file each time.

		> rxImport(sDataSet, sDataFile, overwrite=TRUE)

### Step 7: rxGetInfo

Use **rxGetInfo** to return information about the XDF data source, plus the first 50 rows:

		> rxGetInfo(sDataFile, getVarInfo=TRUE, numRows=50)

This command shows you 



## Read data using rxImport

## Write data using rxDataStep

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


### RxOdbcData Example for SQL Server and Oracle (with arguments) 


**Working with SQL Server**

The previous example showed the connection using a connection string. The next example shows how to pass connection information through server, dsn, user, and password arguments:

	claimsSQL = "SELECT * FROM claims"
	claimsDS2<- RxOdbcData(sqlQuery = claimsSQL, 
		connectionString = "DSN=MSSQLDSN;Uid=mktest;Pwd=passwd;")
	claimsFile <- RxXdfData("claimsFromODBC.xdf")
	rxImport(claimsDS2, claimsFile, overwrite=TRUE)
	rxGetInfo(claimsFile, getVarInfo=TRUE, numRows=10) 

As before, we get the claims data:

	File name: claimsFromODBC.xdf 
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

**Working with Oracle Express**

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

+ [XDF files](scaler-data-xdf.md)	
+ [Data Sources](scaler-user-guide-data-source.md)	
+ [Import text data](scaler-user-guide-data-import.md)
+ [Import and consume data on HDFS](scaler-data-hdfs.md)

## See Also
   
 [RevoScaleR Functions](scaler/scaler.md)   
 [Tutorial: data import and exploration](scaler-getting-started-data-import-exploration.md)
 [utorial: data manipulation and statistical analysis](scaler-getting-started-data-manipulation.md) 
 