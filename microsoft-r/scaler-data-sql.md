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

This article shows you how to import relational data from SQL Server into a data frame or .xdf file in Microsoft R. Source data can originate from Azure SQL Database or an on-premises SQL Server instance.

## Prerequisites

+ [Azure subscription, Azure SQL Database, AdventureWorksLT	sample database](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)
+ [SQL Server (any supported version and edition)](https://www.microsoft.com/sql-server/sql-server-downloads) with the [AdventureWorksDW sample database](https://www.microsoft.com/download/details.aspx?id=49502)	
+ Microsoft R Server 9.1 for Windows or Linux	
+ R console application (RGui.exe on Windows or Revo64 on Linux)

> [!Note]
> Windows users, remember that R is case-sensitive and that file paths use the forward slash as a delimiter.

## How to import from Azure SQL Database

The following script demonstrates how to import data from Azure SQL database into a local XDF. This script sets the connection string, SQL query, XDF file, and **rxImport** command for loading data and saving the output:

	sConnString <- "Driver={ODBC Driver 13 for SQL Server}; Server=tcp:<your-server-name>.database.windows.net,1433; Database=AdventureWorksLT; Uid=<your-user-name>; Pwd=<your-password>; Encrypt=yes; TrustServerCertificate=no; Connection Timeout=30;"
	sQuery <- "select ProductDescriptionID, Description, ModifiedDate from SalesLT.ProductDescription"
	sDataSet <- RxOdbcData(sqlQuery=sQuery, connectionString=sConnString)
	sDataFile <- RxXdfData("c:/users/temp/mysqldata.xdf")
	rxImport(sDataSet, sDataFile, overwrite=TRUE)
	rxGetInfo(sDataFile, getVarInfo=TRUE, numRows=50)

You can run this script in an R console application, but a few modifications are necessary before you can do it successfully. Before running the script, review it line by line to see what needs changing.

### Step 1: Connect

Create the connection object using information from the Azure portal and ODBC Data Source Administrator.

	> sConnString <- "Driver={ODBC Driver 13 for SQL Server}; Server=tcp:<your-server-name>.database.windows.net,1433; Database=AdventureWorksLT; Uid=<your-user-name>; Pwd=<your-password>; Encrypt=yes; TrustServerCertificate=no; Connection Timeout=30;"

The driver used on the connection is an ODBC driver. On Windows, the ODBC driver is provided by the operating system. You can run the **ODBC Data Source Administrator (64-bit)** app to verify the driver is listed in the **Drivers** tab. On Linux, the ODBC driver manager and individual drivers must be installed manually. For pointers, see [How to import relational data using ODBC](sqaler-data-odbc.md).

All of the remaining connection properties are obtained from the Azure portal.

1. Sign in to the [Azure portal](https://ms.portal.azure.com) and locate AdventureWorksLT. 

2. In **Overview > Essentials > Connection strings**, click **Show database connection strings**.

3. On the **ODBC** tab, copy the connection information. It should look similar to the following string, except the server name and user name will be valid for your database. The password is always a placeholder, which you must replace with a valid value.

		Driver={ODBC Driver 13 for SQL Server}; Server=tcp:<your-server-name>.database.windows.net,1433; Database=AdventureWorksLT; Uid=<your-user-name>; Pwd=<your-password>; Encrypt=yes; TrustServerCertificate=no; Connection Timeout=30;

4. Replace the *sConnString* in the example script with a valid connection string having the correct server name, user name, and password.

### Step 2: Firewall

On Azure SQL Database, access is controlled through firewall rules for specific IP addresses. Creating a firewall rule is a requirement for accessing Azure SQL Database from a client application.

1. On your client machine running Microsoft R, sign in to the [Azure portal](https://ms.portal.azure.com) and locate AdventureWorksLT. 

2. Use **Overview > Set server firewall > Add client IP** to create a rule for the local client. 

3. Click **Save** once the rule is created.

On a small private network, IP addresses are most likely static, and the IP address detected by the portal is probably correct. To confirm, you can use **IPConfig** on Windows or **hostname -I** on Linux to get your IP address. 

On corporate networks, IP addresses can change on computer restarts, through network address translations, or other reasons described in [this article](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure). It can be hard to get the right IP address, even through **IPConfig** and **hostname -I**.
  
One way to get the right IP address is from the error message reported on a connection failure. If you get the "ODBC Error in SQLDisconnect" error message, it will include this text: "Client with IP address *<ip-address>* is not allowed to access the server". The IP address reported in the message can be specified as the start and ending IP range in your firewall rule. 

Producing this error is easy. Just run the entire example script (assuming a valid connection string and write permissions to create the XDF), concluding with **rxImport**. When **rxImport** fails, copy the IP address reported in the message, and use it to set the firewall rule in the Azure portal. Wait a few minutes, and then retry the script.

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

You could substitute **RxOdbcData** with **RxSqlServerData** if you want the option of setting the compute context to be the remote SQL Server instance (for example, if you want to run **rxMerge**, **rxImport**, or **rxSort** on a remote SQL Server that also has an R Server installation on the same machine). Otherwise, **RxOdbcData** is local compute context only.

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

### Step 8: rxSummary

Use **rxSummary** to produce summary statistics on the data. The `~.` is used to compute summary statistic on numeric fields.

		> rxSummary(~., sDataFile)

Output shows the central tendencies of the data, including mean values, standard deviation, minimum and maximum values, and whether any observations are missing.

## How to import from SQL Server on-premises

The following script demonstrates how to import data from a SQL Server relational database into a local XDF. This script uses the AdventureWorks data warehouse (AdventureWorksDW) for its normalized data. 

	sConnString <- "Driver={SQL Server}; Server=(local); Database=AdventureWorksDW2016; Connection Timeout=30;"
	squery <-"SELECT * FROM dbo.vDMPrep"
	sDataSet <- RxOdbcData(sqlQuery=squery, connectionString=sConnString)
	sDataFile <- RxXdfData("c:/users/temp/mysqldata.xdf")
	rxImport(sDataSet, sDataFile, overwrite=TRUE)
	rxGetInfo(sDataFile, getVarInfo=TRUE)

As with the previous exercise, modifications are necessary before you can do run this script successfully.

### Step 1: Connect

Create the connection object using the SQL Server database driver a local server and the sample database.

	> sConnString <- "Driver={SQL Server}; Server=(local); Database=AdventureWorksDW2016; Connection Timeout=30;"

The driver used on the connection is an ODBC driver that is installed by SQL Server. You could use the default database driver provided with operating system, but SQL Server also installed drivers. 

On Windows, the ODBC driver is provided by the operating system. You can run the **ODBC Data Source Administrator (64-bit)** app to verify the driver is listed in the **Drivers** tab. On Linux, the ODBC driver manager and individual drivers must be installed manually. For pointers, see [How to import relational data using ODBC](sqaler-data-odbc.md).

The Server=(local) refers to a local default instance connected over TCP. A named instance is specified as computername$instancename. On a remote computer, the syntax is the same. Verify that remote connections are enabled. The defaults vary depending on which edition is installed.

### Step 2: Query

In the R console application, create the SQL query object. The example query consists of columns from a single view, but any valid T-SQL query providing a rowset is acceptable. This unqualified query works because all columns in this view are supported data types.

	> squery <-"SELECT * FROM dbo.vDMPrep"

### Step 3: RxOdbcData

Create an **RxOdbcData** data source object based on query results. The first example is the simple case.

	> sDataSet <- RxOdbcData(sqlQuery=squery, connectionString=sConnString)

The **RxOdbcData** data source takes arguments that can be used to [modify the data set](#example-change-datatype). A revised object includes syntax that converts strings into factors, specifies ranges for ages using `colInfo`, and `colClass` sets the data type to convert to:

	> colInfoList <- list("Age" = list(type = "factor", levels = c("20-40", "41-50", "51-60", "61+")))
	> sDataSet <- RxOdbcData(sqlQuery=squery, connectionString=sConnString, colClasses=c(number="integer"), colInfo=colInfoList, stringsAsFactors=TRUE)

Compare before-and-after variable information. On the original data set, the variable information is as follows:

	Variable information: 
	Var 1: EnglishProductCategoryName, Type: character
	Var 2: Model, Type: character
	Var 3: CustomerKey, Type: integer, Low/High: (11000, 29483)
	Var 4: Region, Type: character
	Var 5: Age, Type: integer, Low/High: (30, 101)
	Var 6: IncomeGroup, Type: character
	Var 7: CalendarYear, Type: integer, Storage: int16, Low/High: (2010, 2014)
	Var 8: FiscalYear, Type: integer, Storage: int16, Low/High: (2010, 2013)
	Var 9: Month, Type: integer, Storage: int16, Low/High: (1, 12)
	Var 10: OrderNumber, Type: character
	Var 11: LineNumber, Type: integer, Storage: int16, Low/High: (1, 8)
	Var 12: Quantity, Type: integer, Storage: int16, Low/High: (1, 1)
	Var 13: Amount, Type: numeric, Low/High: (2.2900, 3578.2700)

After the modifications, the variable information includes default and custom factor levels. There are also some unwanted changes as a result of *stringsAsFactors* converting character data to factors (levles) for *OrderNumber* and *Model*. We can rerun the script without *stringsAsFactors* to roll this back.

	Variable information: 
	Var 1: EnglishProductCategoryName
		3 factor levels: Bikes Accessories Clothing
	Var 2: Model
		40 factor levels: Road-150 Mountain-100 Road-650 Road-550-W Road-250 ... Classic Vest LL Mountain Tire Bike Wash Hitch Rack - 4-Bike Women's Mountain Shorts
	Var 3: CustomerKey, Type: integer, Low/High: (11000, 29483)
	Var 4: Region
		3 factor levels: North America Europe Pacific
	Var 5: Age
		4 factor levels: 20-40 41-50 51-60 61+
	Var 6: IncomeGroup
		3 factor levels: High Low Moderate
	Var 7: CalendarYear, Type: integer, Storage: int16, Low/High: (2010, 2014)
	Var 8: FiscalYear, Type: integer, Storage: int16, Low/High: (2010, 2013)
	Var 9: Month, Type: integer, Storage: int16, Low/High: (1, 12)
	Var 10: OrderNumber
		27659 factor levels: SO43697 SO43698 SO43699 SO43700 SO43701 ... SO75119 SO75120 SO75121 SO75122 SO75123
	Var 11: LineNumber, Type: integer, Storage: int16, Low/High: (1, 8)
	Var 12: Quantity, Type: integer, Storage: int16, Low/High: (1, 1)
	Var 13: Amount, Type: numeric, Low/High: (2.2900, 3578.2700)

### Step 4: XDF

	> sDataFile <- RxXdfData("c:/users/temp/mysqldata.xdf")

### Step 5: rxImport

	> rxImport(sDataSet, sDataFile, overwrite=TRUE)

### Step 6: rxGetInfo

	> rxGetInfo(sDataFile, getVarInfo=TRUE)

### Step 7: rxSummary

	> rxSummary(~., sDataFile)

## Write data using rxDataStep

The sqlQuery argument to `RxOdbcData` is limited to data extraction queries (SELECT and SHOW statements, primarily) because *RxOdbcData* is currently intended to be used for reading data from the database into a .xdf file. In particular, INSERT queries are not supported. Also, because each query is used to populate a single .xdf file, multiple queries (that is, queries separated by a semicolon “;”) are not supported. Compound queries, however, that produce a single extracted table, (that is, queries linked by AND or OR, or involving multiple FROM clauses) are supported.

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





<a name="example-change-datatype"></a>
## Example: Changing data types

Data stored in databases may be stored differently from how you want to store the data in R. You can use the `colClasses`, `colInfo`, and `stringsAsFactors` arguments to `RxOdbcData` to specify how columns are stored in R. 

+ The `stringsAsFactors` argument is the simplest to use. If specified, any character string column not otherwise accounted for by the `colClasses` or `colInfo` argument is stored as a factor in R, with levels defined according to the unique character strings found in the column. 	
+ The `colClasses` argument allows you to specify a particular class for a particular variable.	
+ The `colInfo` argument is similar, but it also allows you to specify a set of levels for a factor variable. 	

The data type must be supported, otherwise the unhandled data type error occurs before the conversion. For example, you can't cast a unique identifier as an integer as a bypass mechanism.

The following example uses the SalelOrderHeader table because it provides more columns, and combines all three arguments to modify the data types of several variables in the claims data:

	sConnectionStr <- "Driver={SQL Server};Server=win-database01; Database=TestData;Uid=mktest;Pwd=sqlpwd;";
	colInfoList <- list("car.age" = list(type = "factor", levels = c("0-3", "4-7", "8-9", "10+")))
	claimsSQL = "SELECT * FROM claims"
	claimsDS <- RxOdbcData(sqlQuery = claimsSQL, connectionString=sConnectionStr,
		colClasses = c(number="integer"), colInfo = colInfoList, stringsAsFactors=TRUE)
	claimsFile <- RxXdfData("claimsFromODBC_Modified.xdf")
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
+ [Import ODBC data](scaler-data-odbc.md)
+ [Import and consume data on HDFS](scaler-data-hdfs.md)

## See Also
   
 [RevoScaleR Functions](scaler/scaler.md)   
 [Tutorial: data import and exploration](scaler-getting-started-data-import-exploration.md)
 [Tutorial: data manipulation and statistical analysis](scaler-getting-started-data-manipulation.md) 
 