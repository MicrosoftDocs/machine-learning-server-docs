Microsoft R Server is R for the Enterprise and solves the problem of deployment and operationalization of R code. Microsoft R Server offers big-data capable R distributions for servers, Hadoop clusters, and data warehouses.

Microsoft R Server is the fastest, most cost-effective enterprise-class big data analytics platform available today. It supports a variety of big data statistics, predictive modeling and machine learning capabilities, and it provides you with analytics that are fully compatible with the R language, the de facto standard for modern analytics users.

Microsoft R Server is your flexible choice for analyzing data at scale, building intelligent apps, and discovering valuable insights across your business.

It is also possible for students, educators and organizations to have access to these same big data analytics capabilities with the Dreamspark program.

Performance of R solutions in Microsoft R Server is expected to generally be better than any conventional R implementation, given the same hardware, because R can be run using server resources and sometimes distributed to multiple processes. Depending on the input data and the processing of it, queries can be distributed across multiple partitions for faster processing.

Users can also expect to see considerable differences in performance and scalability for the same RevoScaleR functions if run in Microsoft R Server versus being run locally in Microsoft R Client. Reasons include support for chunking, increased threads available for R worker processing and parallel processing not only with standard R packages, but also with ScaleR functions. R Server offers optimized performance and scalability through parallelization and streaming.

However, performance even on identical hardware can be affected by many factors outside the R code, including competing demands on server resources, the type of query plan that is created, schema changes, the need to update statistics or create a new query plan, fragmentation and so on. It is possible that a stored procedure containing R code might run in seconds under one workload, but take minutes when there are other services running. We recommend that you monitor multiple aspects of server performance, including networking for remote compute contexts, when quantifying R job performance.

In many enterprises, the final step is to deploy an interface to the underlying analysis to a broader audience within the organization. The DeployR package, available for Microsoft R Server only, provides the tools for doing just that; it is a full-featured web services software development kit for R that allows programmers to use Java, JavaScript or .NET to integrate the R analysis output with a third party package. [Learn more about DeployR...](../../deployr-about.md)


[Watch the R Server technology overview video.](https://www.microsoft.com/en-us/cloud-platform/r-server) <a href="" target="_blank">>></a>


|Microsoft R Server Editions|Description                                                          |Install|ScaleR Get Started|
|---------------------------|---------------------------------------------------------------------|:-------:|:------------------:|
|R Server for Hadoop        |Scale your analysis transparently by distributing work across nodes without complex programming|[Doc](../../rserver-install-hadoop.md)|[Doc](../../scaler-hadoop-getting-started.md)|
|R Server for Teradata DB   |Run advanced analytics in-database for seamless data analysis|[Doc](../../rserver-install-teradata-server.md)|[Doc](../../scaler-teradata-getting-started.md)|
|R Server for Linux         |Bring predictive and prescriptive analytics power to your Linux environments|[Doc](../../rserver-install-linux-server.md)|[Doc](../../scaler-getting-started.md)|

For R Server on Windows, learn more about [SQL Server R Services](https://msdn.microsoft.com/en-us/library/mt604845.aspx) and R Server (Standalone) on the SQL Server documentation site on MSDN.

<br />
For a list of supported operating systems, see [Supported platforms in Microsoft R Server](../../rserver-install-supported-platforms.md).