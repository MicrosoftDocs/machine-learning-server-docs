---

# required metadata
title: "Introducing SQL Server R Services"
description: "SQL Server R Services introduction"
keywords: "SQL Server R Services"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "12/66/2016"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology:
  - r-server
#ms.custom: ""

---

# Introducing Machine Learning with SQL Server

SQL Server provides a platform for developing intelligent applications that uncover new insights. You can use the rich and powerful R language and the many open source packages to create models and generate predictions using your SQL Server data. 

In SQL Server 2016, Microsoft introduced SQL Server R Services, a feature that supports enterprise-scale data science by integrating the R language with SQL Server database engine.

In SQL Server 2017, machine learning becomes even more powerful, with addition of support for the popular Python language. To reflect the support for multiple languages, as of CTP 2.0, SQL Server R Services has also been renamed as Machine Learning Services (In-Database). To read up on the latest changes in CTP 2.0 release of SQL Server 2017, see [What's new for R in SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r-services/what-s-new-in-sql-server-r-services) in the SQL Server product documentation. 

SQL Server Machine Learning Services is an installation of R that runs alongside SQL Server (operating as a SQL Server service) and communicates securely with SQL Server.  During the installation process, Microsoft R Open and ScaleR libraries are installed onto SQL Server so that you can integrate your server data wth SQL Server and Microsoft’s BI tools. Because SQL Server Machine Learning Services integrates the R and Python languages with SQL Server, you can keep analytics close to the data and eliminate the costs and security risks associated with data movement.


>[!WARNING]
>SQL Server Machine Learning Services is built atop of specific versions of Microsoft R Open. When installing, always use the R Open version provided by the install (or described in the installation guides) to avoid compatibility errors. You can also install and use Microsoft R Open on its own by downloading the [R Open latest version](https://mran.microsoft.com).

SQL Server Machine Learning Services combines R and Python with a comprehensive set of SQL Server tools and technologies that offer superior performance, security, reliability and manageability. You can deploy R and Python solutions using convenient, familiar tools, and your production applications can call the R runtime and retrieve predictions and visuals using Transact-SQL. With Enterprise Edition, you also get the ScaleR libraries to overcome R’s inherent performance and scale limitations.

The **RevoScaleR** and **MicrosoftML** packages are included when you install **SQL Server R Services** on an instance of SQL Server 2016. RevoScaleR and MicrosoftML are also included in all installations of **Microsoft R Server**.  

## What&#39;s New in Machine Learning with SQL Server
 
In SQL Server 2016, Microsoft introduced SQL Server R Services, a feature that supports enterprise-scale data science by integrating the R language with SQL Server database engine.  
 
In SQL Server 2017, machine learning becomes even more powerful, with addition of support for the popular Python language. To reflect the support for multiple languages, as of CTP 2.0, SQL Server R Services has also been renamed as **Machine Learning Services (In-Database)**.
 
SQL Server 2017 includes many new features to make it easier to build and deploy machine learning solutions that use SQL Server data. Learn more about [what's new in this release.](https://docs.microsoft.com/en-us/sql/advanced-analytics/r-services/what-s-new-in-sql-server-r-services)
 

## How to Work with R in SQL Server


### Getting and Moving SQL Server Data

To work with SQL Server data in R, create a SQL Server data source in your R code using the `RxSqlServerData` function, which defines the connection string and the data you want to use.

The data source is saved as a variable that points to the data. The data is not actually loaded or moved until you use a function that uses the data, such as `rxDataStep`, which can work with multiple data sources, or `rxGetVarInfo`.

The ability to save the data source object in a variable makes it very easy to re-use connection information with different functions, create multiple variables for different tables in a single source, and so forth. Most of the RevoScaleR functions, also referred to as rx functions, support moving or analyzing data in chunks.

Additionally, with SQL Server data you can stream data, sending a predefined number of rows in each batch, or, if the function supports it, process the data in parallel. In contrast, when you read data from a database using RODBC, all the data is read into memory, making it difficult to work with large datasets.

### Using SQL Server as a Compute Context

A defining feature of the **RevoScaleR** package is that most of its functions support execution of R code in different compute contexts. To run your R code on the SQL Server computer:

1. Define a compute context that points to the database server.
1. Change the *active compute context* to use the SQL Server computer.
1. To switch back to local execution, reset the compute context to the default.

If you run R code within T-SQL code, the server is always used as the compute context. In this scenario, your code will call the R libraries installed on the SQL Server instance, and use secure connections to get data from the database. You can also save models in a database table, load models from a table to use for scoring, and save your results ot a databse table, all without leaving the context of SQL Server.

For more information, see [Data Exploration and Predictive Modeling with R](https://docs.microsoft.com/en-us/sql/advanced-analytics/r/data-exploration-and-predictive-modeling-with-r)
 
### Using Development Tools
You can develop your R solutions in your preferred R IDE. We recommend that you use a standalone R IDE when testing your code and exploring your data, rather than trying to write R code inside of T-SQL. For R Services in particular, [R Tools for Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) is a good choice, because it has strong support for both R code and integration with SQL Server, but you can use any IDE that supports database connectivity.

To run R code within the context of SQL Server, you have two options:
+ Run code from your laptop or any remote workstation, but set the compute context to SQL Server.
+ Copy R code into the @script argument of a special SQL stored procedure, [sp_execute_external_script](https://msdn.microsoft.com/library/mt604368.aspx). You can provide input data as a parameter of the stored procedure, call the stored procedure from any application that supports SQL calls, and easily export results to any application that can consume SQL data.

### Deploying, Managing, and Optimizing Solutions

One of the primary goals of providing Machine Learning Services in SQL Server is to make it easier to deploy R code in production. Typically you'll deploy your R code to production by wrapping it in a stored procedure. Stored procedures in SQL Server support parameterization, making it easy to pass in a SQL query as input, and save the results to the database.

Because the syntax for calling stored procedures is supported by many applications, you do not need to write any extra code to call your R code from an external application -- just pass in the data and handle the results that are returned.

You can also generate visualizations and archive them locally, export them to other applications such as Reporting Services or Power BI, or send them back to your local workstation for review.

Finally, because Machine Learning Services is integrated with SQL Server, you can use database server tools for monitoring code execution and managing and balancing resources.

For more information, see these SQL Server:
 + [Operationalizing Your R Code](https://docs.microsoft.com/en-us/sql/advanced-analytics/r/operationalizing-your-r-code)
 + [Resource Governance](https://docs.microsoft.com/en-us/sql/advanced-analytics/r/resource-governance-for-r-services)
 + [Performance Tuning](https://docs.microsoft.com/en-us/sql/advanced-analytics/r/sql-server-r-services-performance-tuning)


## More Resources

Learn more about SQL Server Machine Learning Services here:

+ In the [SQL Server Documentation Set](https://docs.microsoft.com/en-us/sql/advanced-analytics/r/sql-server-r-services) site, which also includes a tutorial for **SQL Server Machine Learning Services**.
+ [Data Science End-to-End Walkthrough](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough): Load, explore, and analyze the New York City taxi dataset. Build models and deploy them to SQL Server for production.
+ [Using the RevoScaleR Packages](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages): Deep dive on the analytical functions provided by the **ScaleR** package. Demonstrates how to create and use compute contexts, how to move data between local and server compute contexts using XDF files, and creates a simulation using R code that runs in SQL Server.
+ [Data Science Scenarios and Solution Templates](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/data-science-scenarios-and-solution-templates): Includes all the R and T-SQL code you need for fraud detection, churn analysis, predictive maintenance, and demand forecasting.
+ [In-Database Advanced Analytics for SQL Developers (Tutorial)](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/sqldev-in-database-advanced-analytics-for-sql-developers)


Learn more about the [RevoScaleR package and its function here](~/r-reference/revoscaler/revoscaler.md).
