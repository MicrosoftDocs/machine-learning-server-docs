---

# required metadata
title: "Use RevoScaleR with SQL Server (Machine Learning Server) "
description: "Overview and tutorial to using RevoScaleR in SQL Server databases."
keywords: 
author: "chuckheinzelman"
ms.author: "charlhe"
ms.manager: cgronlun
ms.date: 08/11/2016
ms.topic: "how-to"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# Using RevoScaleR with SQL Server

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

By using the **RevoScaleR** functions in your R solutions, you can create high-performance big data analytics that use SQL Server data, or that run in the context of a SQL Server database server.

This introduction provides general knowledge that you need to get started with analyzing SQL Server data, or moving your models into production in SQL Server 2016. It is primarily intended for users who are already have a basic familiarity with the R language and would like to understand more about how to use the **RevoScaleR** package with SQL Server. 

We recommend that you read this introduction to understand the requirements and basic workflows, and refer to SQL Server Books Online. In particular, the walkthrough that was previously included in this guide is now published as a tutorial for **SQL Server R Services**.

+ [Data Science End-to-End Walkthrough](https://msdn.microsoft.com/library/mt612857.aspx): Load, explore, and analyze the New York City taxi dataset. Build models and deploy them to SQL Server for production.
+ [Using the RevoScaleR Packages](https://msdn.microsoft.com/library/mt637368.aspx): Deep dive on the anaytical sunctions provided by the **RevoScaleR** package. Demonstrates how to create and use compute contexts, how to move data between local and server compute contexts using XDF files, and creates a simulation using R code that runs in SQL Server.
+ [Data Science Scenarios and Solution Templates](https://msdn.microsoft.com/library/mt693423.aspx): Includes all the R and T-SQL code you need for fraud detection, churn analysis, predictive maintenance, and demand forecasting.

All tutorials include sample code, along with explanations of how to work with SQL and R.


## Basic Operations


The **RevoScaleR** package is included when you install **SQL Server R Services** on an instance of SQL Server 2016. **RevoScaleR** is also included in all installations of **Machine Learning Server**.  

### Development
You can develop your R solutions in any R IDE you prefer. We recommend that you use a standalone R IDE when testing your code and exploring your data, rather than trying to write R code inside of T-SQL. 

To run R code within the context of SQL Server, you have two options:
+ Run code from your laptop or any remote workstation, but set the compute context to SQL Server..
+ Copy R code into the @script argument of a special SQL stored procedure, [sp_execute_external_script](https://msdn.microsoft.com/library/mt604368.aspx). You can provide input data as a parameter of the stored procedure, call the stored procedure from any application that supports SQL calls, and easily export results to any application that can consume SQL data. 


### Getting SQL Server Data

To work with SQL Server data in R, you must create a SQL Server data source in your R code. to do this, use the `RxSqlServerData` function, which defines the connection string and the data you want to use. 

The data source is saved as a variable that points to the data. The data is not actually loaded or moved until you use a function that uses the data, such as the `rxDataStep` (which can work with multiple data sources) or `rxGetVarInfo`.

The ability to save the data source object in a variable makes it very easy to re-use connection information with different functions, create multiple variables for different tables in a single source, and so forth. Most of the rx functions also support moving or analyzing data in chunks.

Additionally, with SQL Server data you can stream data, sending a predefined number of rows in each batch, or, if the function supports it, process the data in parallel. 

In contrast, when you read data from a database using RODBC, all the data is read into memory, making it difficult to work with large datasets.

### Using SQL Server as a Compute Context

A defining feature of the **RevoScaleR** package is that most of its functions support execution of R code in different compute contexts. To run your R code on the SQL Server computer:

1. Define a compute context that points to the database server. 
2. Change the *active compute context* to use the SQL Server computer. 
3. To switch back to local execution, reset the compute context to the default.

If you run R code within T-SQL code, the server is always used as the compute context. In this scenario, your code will call the R libraries installed on the SQL Server instance, and use secure connections to get data from the database. You can also save models in a database table, load models from a table to use for scoring, and save your results ot a databse table, all without leaving the context of SQL Server.

For more information, see SQL Server 2016 Books Online:
 + [Data Exploration and Predictive Modeling with R](https://msdn.microsoft.com/library/mt590947.aspx) 
 + [R Interoperability in SQL Server R Services](https://docs.microsoft.com/sql/advanced-analytics/r/r-interoperability-in-sql-server)

### Deploying, Managing, and Optimizing Solutions

One of the primary goals of providing R Services in SQL Server is to make it easier to deploy R code in production. Typically you'll deploy your R code to production by wrapping it in a stored procedure. Stored procedures in SQL Server support parameterization, making it easy to pass in a SQL query as input, and save the results to the database.

Because the syntax for calling stored procedures is supported by many applications, you do not need to write any extra code to call your R code from an external application -- just pass in the data and handle the results that are returned.

You can also generate visualizations and archive them locally, export them to other applications such as Reporting Services or Power BI, or send them back to your local workstation for review. 

Finally, because R Services is integrated with SQL Server, you can use database server tools for monitoring code execution and managing and balancing resources.

For more information, see SQL Server 2016 Books Online:
 + [Operationalizing Your R Code](https://msdn.microsoft.com/library/mt590864.aspx)
 + [Resource Governance for R Services](https://msdn.microsoft.com/library/mt703708.aspx)
 + [SQL Server R Services Performance Tuning](https://msdn.microsoft.com/library/mt723573.aspx)

## Related Guides

For information on other distributed computing compute contexts, see:

- [How to use RevoScaleR on Hadoop MapReduce](how-to-revoscaler-hadoop.md)
- [How to use RevoScaleR on Spark](how-to-revoscaler-spark.md)

More information on **RevoScaleR** can be found here:

- [Tutorial: Data Import](tutorial-revoscaler-data-import-transform.md)
- [RevoScaleR Function Reference](../r-reference/revoscaler/revoscaler.md)
- [Distributed Computing](how-to-revoscaler-distributed-computing.md)
- [RevoScaleR ODBC Data Import](how-to-revoscaler-data-odbc.md)