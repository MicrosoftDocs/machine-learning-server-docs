---

# required metadata
title: "Import data from Azure SQL Database and SQL Server (Microsoft R)"
description: "How to import relational data from Azure SQL and SQL Server databases in RevoScaleR"
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

# Import SQL Server relational data (Microsoft R)

This article shows you how to import relational data from SQL Server into a data frame or .xdf file in Microsoft R. Source data can originate from Azure SQL Database, or SQL Server on premises or on [an Azure virtual machine](https://docs.microsoft.com/sql/linux/sql-server-linux-azure-virtual-machine#a-idconnecta-connect-to-the-linux-vm).

## Prerequisites

+ [Azure subscription, Azure SQL Database, AdventureWorksLT	sample database](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)
+ [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) (any supported version and edition) with the [AdventureWorksDW sample database](https://www.microsoft.com/download/details.aspx?id=49502)	
+ Microsoft R Server 9.1 for Windows or Linux	
+ R console application (RGui.exe on Windows or Revo64 on Linux)

> [!Note]
> Windows users, remember that R is case-sensitive and that file paths use the forward slash as a delimiter.

## How to import from Azure SQL Database

The following script imports sample data from Azure SQL database into a local XDF. The script sets the connection string, SQL query, XDF file, and the **rxImport** command for loading data and saving the output:

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

3. On the **ODBC** tab, copy the connection information. It should look similar to the following string, except the server name and user name will be valid for your database. The password is always a placeholder, which you must replace with the actual password used for accessing your database.

		Driver={ODBC Driver 13 for SQL Server}; Server=tcp:<your-server-name>.database.windows.net,1433; Database=AdventureWorksLT; Uid=<your-user-name>; Pwd=<your-password>; Encrypt=yes; TrustServerCertificate=no; Connection Timeout=30;

4. Replace the *sConnString* in the example script with a valid connection string having the correct server name, user name, and password.

### Step 2: Firewall

On Azure SQL Database, access is controlled through firewall rules created for specific IP addresses. Creating a firewall rule is a requirement for accessing Azure SQL Database from a client application.

1. On your client machine running Microsoft R, sign in to the [Azure portal](https://ms.portal.azure.com) and locate AdventureWorksLT. 

2. Use **Overview > Set server firewall > Add client IP** to create a rule for the local client. 

3. Click **Save** once the rule is created.

On a small private network, IP addresses are most likely static, and the IP address detected by the portal is probably correct. To confirm, you can use **IPConfig** on Windows or **hostname -I** on Linux to get your IP address. 

On corporate networks, IP addresses can change on computer restarts, through network address translations, or other reasons described in [this article](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure). It can be hard to get the right IP address, even through **IPConfig** and **hostname -I**.
  
One way to get the right IP address is from the error message reported on a connection failure. If you get the "ODBC Error in SQLDisconnect" error message, it will include this text: "Client with IP address *<ip-address>* is not allowed to access the server". The IP address reported in the message is that one actually used on the connection, and it should be specified as the start and ending IP range in your firewall rule. 

Producing this error is easy. Just run the entire example script (assuming a valid connection string and write permissions to create the XDF), concluding with **rxImport**. When **rxImport** fails, copy the IP address reported in the message, and use it to set the firewall rule in the Azure portal. Wait a few minutes, and then retry the script.

### Step 3: Query

In the R console application, create the SQL query object. The example query consists of columns from a single table, but any valid T-SQL query providing a rowset is acceptable. This table was chosen because it includes numeric data.

	> squery <-"SELECT SalesOrderID, SalesOrderDetailID, OrderQty, UnitPrice, UnitPriceDiscount, LineTotal FROM SalesLT.SalesOrderDetail"

Before attempting unqualified *SELECT * FROM* queries, review the columns in your database for unhandled data types in R. In AdventureWorksLT, the *rowguid(uniqueidentifier)* column is not handled. Other unsupported data types are [listed here](https://docs.microsoft.com/sql/advanced-analytics/r/r-libraries-and-data-types#data-types-not-supported-by-r).

Queries with unsupported data types produce the error below. If you get this error and cannot immediately detect the unhandled data type, incrementally add fields to the query to isolate the problem.
  
		Unhandled SQL data type!!!
		Could not open data source.
		Error in doTryCatch(return(expr), name, parentenv, handler)

Queries should be data extraction queries (SELECT and SHOW statements) for reading data into a data frame or .xdf file. INSERT queries are not supported. Because queries are used to populate a single data frame or .xdf file, multiple queries (that is, queries separated by a semicolon “;”) are not supported. Compound queries, however, producing a single extracted table (such as queries linked by AND or OR, or involving multiple FROM clauses) are supported.

### Step 4: RxOdbcData

Create the **RxOdbcData** data object using the connection and query object specifying which data to retrieve. This exercise uses only a few arguments, but to learn more about data sources, see [Data sources in Microsoft R](scaler-user-guide-data-source.md).

		> sDataSet <- RxOdbcData(sqlQuery=sQuery, connectionString=sConnString)

You could substitute **RxSqlServerData** for **RxOdbcData** if you want the option of setting the compute context to a remote SQL Server instance (for example, if you want to run **rxMerge**, **rxImport**, or **rxSort** on a remote SQL Server that also has an R Server installation on the same machine). Otherwise, **RxOdbcData** is local compute context only.

### Step 5: XDF

Create the XDF file to save the data to disk. Check the folder permissions for write access. 

		> sDataFile <- RxXdfData("c:/users/temp/mysqldata.xdf")

By default, Windows does not allow external writes by non-local users. You might want to create a single folder, such as C:/Users/Temp, and give *Everyone* write permissions. Once the XDF is created, you can move the file elsewhere and delete the folder, or revoke the write permissions you just granted.

On Linux, you can use this alternative path:

		> sDataFile <- RxXdfData(/tmp/mysqldata.xdf")

### Step 6: rxImport

Run **rxImport** with *inData* and *outFile* arguments. Include *overwrite* so that you can rerun the script with different queries without having to the delete the file each time.

		> rxImport(sDataSet, sDataFile, overwrite=TRUE)

### Step 7: rxGetInfo

Use **rxGetInfo** to return information about the XDF data source, plus the first 50 rows:

		> rxGetInfo(sDataFile, getVarInfo=TRUE, numRows=50)

### Step 8: rxSummary

Use **rxSummary** to produce summary statistics on the data. The `~.` is used to compute summary statistic on numeric fields.

		> rxSummary(~., sDataFile)

Output shows the central tendencies of the data, including mean values, standard deviation, minimum and maximum values, and whether any observations are missing. From the output, you can tell that orders are typically of small quantities and prices.

## How to import from SQL Server

The following script demonstrates how to import data from a SQL Server relational database into a local XDF. This script is slightly different from the one for Azure SQL Database. This one uses the AdventureWorks data warehouse (AdventureWorksDW) for its normalized data. 

	sConnString <- "Driver={SQL Server}; Server=(local); Database=AdventureWorksDW2016; Connection Timeout=30;"
	squery <-"SELECT * FROM dbo.vDMPrep"
	sDataSet <- RxOdbcData(sqlQuery=squery, connectionString=sConnString)
	sDataFile <- RxXdfData("c:/users/temp/mysqldata.xdf")
	rxImport(sDataSet, sDataFile, overwrite=TRUE)
	rxGetInfo(sDataFile, getVarInfo=TRUE)

As with the previous exercise, modifications are necessary before you can run this script successfully.

### Step 1: Connect

Create the connection object using the SQL Server database driver a local server and the sample database.

	> sConnString <- "Driver={SQL Server}; Server=(local); Database=AdventureWorksDW2016; Connection Timeout=30;"

The driver used on the connection is an ODBC driver that is installed by SQL Server. You could use the default database driver provided with operating system, but SQL Server Setup also installs drivers. 

On Windows, ODBC drivers can be listed in the **ODBC Data Source Administrator (64-bit)** app on the **Drivers** tab. On Linux, the ODBC driver manager and individual drivers must be installed manually. For pointers, see [How to import relational data using ODBC](sqaler-data-odbc.md).

The Server=(local) refers to a local default instance connected over TCP. A named instance is specified as computername$instancename. A remote server has the same syntax, but you should verify that that remote connections are enabled. The defaults for this setting vary depending on which edition is installed.

### Step 2: Query

In the R console application, create the SQL query object. The example query consists of columns from a single view, but any valid T-SQL query providing a rowset is acceptable. This unqualified query works because all columns in this view are supported data types.

	> squery <-"SELECT * FROM dbo.vDMPrep"

> [!Tip]
> You could skip this step and specify the query information through **RxOdbcData** via the *table* argument. Specifically, you could write `sDataSet <- RxOdbcData(table=dbo.vDMPrep, connectionString=sConnString)`.

### Step 3: RxOdbcData

Create an **RxOdbcData** data source object based on query results. The first example is the simple case.

	> sDataSet <- RxOdbcData(sqlQuery=squery, connectionString=sConnString)

The **RxOdbcData** data source takes arguments that can be used to [modify the data set](#drilldown-change-datatype). A revised object includes syntax that converts characters to factors via *stringsAsFactors*, specifies ranges for ages using *colInfo*, and *colClasses* sets the data type to convert to:

	> colInfoList <- list("Age" = list(type = "factor", levels = c("20-40", "41-50", "51-60", "61+")))
	> sDataSet <- RxOdbcData(sqlQuery=squery, connectionString=sConnString, colClasses=c(Model="character", OrderNumber="character"), colInfo=colInfoList, stringsAsFactors=TRUE)

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

After the modifications, the variable information includes default and custom factor levels. Additionally, for Model and OrderNumber, we preserved the original "character" data type using the *colClasses* argument. We did this because *stringsAsFactors* globally converts character data to factors (levels), and for *OrderNumber* and *Model*, the number of levels was overkill.

	Variable information: 
	Var 1: EnglishProductCategoryName
		3 factor levels: Bikes Accessories Clothing
	Var 2: Model, Type: character
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
	Var 10: OrderNumber, Type: character
	Var 11: LineNumber, Type: integer, Storage: int16, Low/High: (1, 8)
	Var 12: Quantity, Type: integer, Storage: int16, Low/High: (1, 1)
	Var 13: Amount, Type: numeric, Low/High: (2.2900, 3578.2700)

### Step 4: XDF

Create the XDF file to save the data to disk. Check the folder permissions for write access. 

		> sDataFile <- RxXdfData("c:/users/temp/mysqldata.xdf")

By default, Windows does not allow external writes by non-local users. You might want to create a single folder, such as C:/Users/Temp, and give *Everyone* write permissions. Once the XDF is created, you can move the file elsewhere and delete the folder, or revoke the write permissions you just granted.

On Linux, you can use this alternative path:

		> sDataFile <- RxXdfData(/tmp/mysqldata.xdf")

### Step 5: rxImport

Run **rxImport** with *inData* and *outFile* arguments. Include *overwrite* so that you can rerun the script with different queries without having to the delete the file each time.

		> rxImport(sDataSet, sDataFile, overwrite=TRUE)

### Step 7: rxGetInfo

Use **rxGetInfo** to return information about the XDF data source:

		> rxGetInfo(sDataFile, getVarInfo=TRUE)

### Step 7: rxSummary

Use **rxSummary** to produce summary statistics on the data. The `~.` is used to compute summary statistic on numeric fields.

		> rxSummary(~., sDataFile)

Output shows the central tendencies of the data, including mean values, standard deviation, minimum and maximum values, and whether any observations are missing. From the output, you can see that Age was probably included in the view by mistake. There are no values for this variable in the observations retrieved from the data source.

<a name="drilldown-change-datatype"></a>
## Drill down: Changing data types

Data stored in databases may be stored differently from how you want to store the data in R. As noted in the previous example for on-premises SQL Server, you can add the `colClasses`, `colInfo`, and `stringsAsFactors` arguments to `RxOdbcData` to specify how columns are stored in R. 

+ The `stringsAsFactors` argument is the simplest to use. If specified, any character string column not otherwise accounted for by the `colClasses` or `colInfo` argument is stored as a factor in R, with levels defined according to the unique character strings found in the column. 	
+ The `colClasses` argument allows you to specify a particular R data type for a particular variable.	
+ The `colInfo` argument is similar, but it also allows you to specify a set of levels for a factor variable. 	

> [!Important]
> The data type must be supported, otherwise the unhandled data type error occurs before the conversion. For example, you can't cast a unique identifier as an integer as a bypass mechanism.

The following example uses the SalelOrderHeader table because it provides more columns, and combines all three arguments to modify the data types of several variables in the claims data:

	> colInfoList <- list("Age" = list(type = "factor", levels = c("20-40", "41-50", "51-60", "61+")))
	> sDataSet <- RxOdbcData(sqlQuery=squery, connectionString=sConnString, colClasses=c(Model="character", OrderNumber="character"), colInfo=colInfoList, stringsAsFactors=TRUE)

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
 