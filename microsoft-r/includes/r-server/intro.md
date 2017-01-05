Microsoft R Server is "R for the Enterprise" and solves the problem of deployment and operationalization of R code. It is also possible for students and educators to access these same big data analytics capabilities with the [Microsoft Imagine program](https://imagine.microsoft.com).

Microsoft R Server offers big-data capable R distributions for servers, Hadoop clusters, and data warehouses. It supports a variety of big data statistics, predictive modeling and machine learning capabilities, and provides you with analytics that are fully compatible with the R language, the de facto standard for modern analytics users.

Although generic R scripts tend to run faster on MRO via Intel MKL, the major benefit in terms of scale and performance comes from using ScaleR functions. ScaleR is available in both R Client and R Server, but only R Server adds support for remote execution, remote compute contexts, data chunking, additional threads for multithreaded processing, parallel processing, and streaming.

> [!Note]
> Performance can be affected by external factors outside the R code, including competing demands on server resources, the type of query plan that is created, schema changes, the need to update statistics or create a new query plan, fragmentation and so on. It is possible that a stored procedure containing R code might run in seconds under one workload, but take minutes when there are other services running. We recommend that you monitor multiple aspects of server performance, including networking for remote compute contexts, when quantifying R job performance.

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization. The operationalization feature (available in Microsoft R Server only) provides the tools for doing just that; it is a full-featured web services software development kit for R that allows programmers to use in any language to integrate the R analysis output with a third party package. [Learn more about Operationalization](../../operationalize/about.md)

[**Watch this 2-minute video introduction to Microsoft R Server.**](https://www.microsoft.com/en-us/cloud-platform/r-server)

|Microsoft R Server platforms|Description|Install|Get Started|
|----------------------------|-----------|:-----:|:----------------:|
|R Server for Hadoop        |Scale your analysis transparently by distributing work across nodes without complex programming|[Doc](../../rserver-install-hadoop.md)|[Doc](../../scaler-hadoop-getting-started.md)|
|R Server for Teradata DB   |Run advanced analytics in-database for seamless data analysis on Teradata|[Doc](../../rserver-install-teradata-server.md)|[Doc](../../scaler-teradata-getting-started.md)|
|R Server for Linux         |Bring predictive and prescriptive analytics power to your Linux environments|[Doc](../../rserver-install-linux-server.md)|[Doc](../../scaler-getting-started.md)|
|R Server for Windows|Bring predictive and prescriptive analytics power to your Windows environments|[Doc](../../rserver-install-windows.md)|[Doc](../../scaler-getting-started.md)|
|SQL Server R Services  |Run advanced analytics in-database for seamless data analysis on SQL Server|[Doc](https://msdn.microsoft.com/library/mt696069.aspx)|[Doc](https://msdn.microsoft.com/library/mt604885.aspx)|

<br />
For a list of supported operating systems, see [Supported platforms in Microsoft R Server](../../rserver-install-supported-platforms.md).
