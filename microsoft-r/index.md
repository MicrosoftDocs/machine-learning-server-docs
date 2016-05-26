#Microsoft R

R is the world’s most powerful, and preferred, programming language for statistical computing, machine learning, and graphics, and is supported by a thriving global community of users, developers, and contributors. Developers frequently provide tools incorporating their expertise in the form of R packages. Traditionally, using R in an enterprise setting has presented certain challenges, especially as the volume of data rises, or when faced with a need to deploy solutions to production environments. 

Microsoft R Server and Client, simply put, are R for the Enterprise. Microsoft provides the software, services, and support that combine to make the very popular R statistical computing environment a compelling tool not only for academia, exploration, and prototyping, but for deployment within an enterprise. 

Microsoft also offers Microsoft R Open, which provides high performance math libraries installed on top of a stable version of Open Source R including Base and Recommended Packages.

##The Microsoft R Family

The feature set provided by Microsoft R Server, Microsoft R Client, and Microsoft R Open can be categorized as follows:

|Feature   |Microsoft R Open|Microsoft R Client|Microsoft R Server|
|----------|----------------|------------------|-----------|
|Cores     |????|1-3|1-????|
|Big Data  |In-memory bound|In-memory bound<br>Operates on large volumes when connected to R Server|Hybrid memory & disk scalability<br>Operates on bigger volumes & factors|
|Speed of<br>Analysis|Multi-threaded when MKL is installed for non-ScaleR functions|Multi-threaded with MKL for non-ScaleR functions<br>Up to 2 threads for ScaleR functions with a local compute context|Full parallel threading & processing|
|Enterprise<br>Readiness|Community support|Commercial support|Commercial support|
|Analytic<br>Breadth <br>& Depth|7K+ analytic R packages|Leverage & optimize open source R packages plus 'Big Data'-ready ScaleR packages|Leverage & optimize open source R packages plus 'Big Data'-ready + Multithreaded ready ScaleR packages|
|Commercial<br>Viability|Risk of deployment to open source|Free for everyone|Commercial licenses|
|[DeployR](microsoft-r-getting-started.md#deployr-intro)   |Not available|Not available|Optional add-on|

<br>
##Microsoft R Server

Microsoft R Server is the most broadly deployable enterprise-class analytics platform for R available today. Supporting a variety of big data statistics, predictive modeling and machine learning capabilities, R Server supports the full range of analytics – exploration, analysis, visualization and modeling based on open source R. 

By using and extending open source R, Microsoft R Server is fully compatible with R scripts, functions and CRAN packages, to analyze data at enterprise scale. We also address the in-memory limitations of open source R by adding parallel and chunked processing of data in Microsoft R Server, enabling users to run analytics on data much bigger than what fits in main memory.

Microsoft R Server delivers enterprise class performance and scalability for your R-based applications with libraries that allow you to write once and deploy across multiple platforms with minimal effort, whether on-premises or in the cloud.

Get started with Microsoft R Server now and read this [Getting Started](getting-started.md) guide.

##Microsoft R Client

Microsoft R Client is a free tool to enable data scientists to connect to and explore production data as well as create models and algorithms using the powerful ScaleR APIs. When you install R Client, you get the same enhanced R packages and connectivity tools that are provided in Microsoft R Server. 

On its own, R Client is limited to three cores -- one thread for reading and two for processing RevoScaleR HPA functions. R Client is also in-memory bound, even with XDF files. And while R Client can be used on its own, you can benefit from the hybrid memory, disk scalability, performance and speed when you push the compute context to a production instance of Microsoft R Server. R client is optimized to work with all Microsoft R Server versions. 

Learn how to [install Microsoft R Client](install-r-client-windows.md) and check out this [Getting Started](getting-started.md) guide.


##Microsoft R Open

Microsoft R Open is the enhanced distribution of R from Microsoft Corporation. It is a complete open source platform for statistical analysis and data science. Based on the open source R engine, the most widely used statistics software in the world, makes Microsoft R Open fully compatibility with all packages, scripts and applications that work with that version of R. Like open source R from CRAN, Microsoft R Open is open source and free to download, use, and share.

Microsoft R Open delivers [performance boosts](https://mran.microsoft.com/documents/rro/multithread/#mt-bench), in comparison to the standard R distribution, since R Open leverages high-performance, multi-threaded math libraries.    

On its own, Microsoft R Open provides limited performance and scalability in comparison to Microsoft R Server and Microsoft R Client Editions. Specifically, none of the ScaleR functions and packages included with Microsoft R Server and Microsoft R Client are available in standalone Microsoft R Open. Also, data that can be processed is limited to the data that can fit in server memory unless connected with Microsoft R Server. Since Microsoft R Server and Microsoft R Client each connect to a custom version of Microsoft R Open, all of the amazing third-party packages available for that version of R should also work when you are in Microsoft R Server and Microsoft R Client. 

Visit the [MRAN Website](https://mran.microsoft.com/) to learn more about Microsoft R Open and download it.


##Learn more

Start learning about Microsoft R today:
+ [Microsoft R Getting Started](microsoft-r-getting-started.md)

+ [DeployR Getting Started](deployr-about.md)

+ [Microsoft R Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)

+ [Product Web Page](https://www.microsoft.com/en-us/server-cloud/products/r-server/) 

+ [Additional Resources](microsoft-r-more-resources.md)

+ [Documentation Archives](microsoft-r-old-versions.md)
