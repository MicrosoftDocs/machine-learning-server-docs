#Microsoft R

R is the world’s most powerful, and preferred, programming language for statistical computing, machine learning, and graphics, and is supported by a thriving global community of users, developers, and contributors. Developers frequently provide tools incorporating their expertise in the form of R packages. Traditionally, using R in an enterprise setting has presented certain challenges, especially as the volume of data rises, or when faced with a need to deploy solutions to production environments. 

The Microsoft R product family includes:
+ <a href="#mrs">Microsoft R Server</a>, R for the Enterprise
+ <a href="#mrc">Microsoft R Client</a>, high performance analytics run locally
+ <a href="#mro">Microsoft R Open</a>, the enhanced distribution of R

See a comparison of these products [here](#compare-prods).

<br>
<a name="mrs"></a>
##Microsoft R Server

Microsoft R Server is the most broadly deployable enterprise-class analytics platform for R available today. Supporting a variety of big data statistics, predictive modeling and machine learning capabilities, R Server supports the full range of analytics – exploration, analysis, visualization and modeling based on open source R. 

By using and extending open source R, Microsoft R Server is fully compatible with R scripts, functions and CRAN packages, to analyze data at enterprise scale. We also address the in-memory limitations of open source R by adding parallel and chunked processing of data in Microsoft R Server, enabling users to run analytics on data much bigger than what fits in main memory. And since R Server is built on top of Microsoft R Open, you can use any open source R packages to build your analytics. 

Microsoft R Server delivers enterprise class performance and scalability for your R-based applications with libraries that allow you to write once and deploy across multiple platforms with minimal effort, whether on-premises or in the cloud.

Get started with Microsoft R Server now and read this [Getting Started](microsoft-r-getting-started.md) guide.

<a name="mrc"></a>
##Microsoft R Client

Microsoft R Client is a free, data science tool that runs locally on your computer and allows you to do high performance analytics.  R Client is built on top of Microsoft R Open so you can use any open source R packages to build your analytics. Additionally, R Client introduces the powerful ScaleR technology and its proprietary functions to benefit from parallelization and remote computing. 

R Client allows you to work with production data locally using the full set of ScaleR functions, but there are some constraints.  On its own, the data to be processed must fit in local memory, and processing is limited up to two threads for ScaleR functions. To benefit from disk scalability, performance and speed, you can push the compute context to a production instance of Microsoft R Server such as SQL Server R Services and R Server for Hadoop. R client is optimized to work with all Microsoft R Server versions. 

Learn how to [install Microsoft R Client](install-r-client-windows.md) and check out this [Getting Started](microsoft-r-getting-started.md) guide.


<a name="mro"></a>
##Microsoft R Open

Microsoft R Open is the enhanced distribution of R from Microsoft Corporation. It is a complete open source platform for statistical analysis and data science. Being based on the open source R engine makes Microsoft R Open fully compatibility with all R packages, scripts and applications that work with that version of R. Microsoft R Open delivers [performance boosts](https://mran.microsoft.com/documents/rro/multithread/#mt-bench), in comparison to the standard R distribution, since R Open leverages high-performance, multi-threaded math libraries. Like open source R from CRAN, Microsoft R Open is open source and free to download, use, and share.   
Microsoft R Open provides limited performance and scalability in comparison to Microsoft R Server and Microsoft R Client Editions. Specifically, none of the proprietary ScaleR functions and packages included with Microsoft R Server and Microsoft R Client are available in standalone Microsoft R Open. Also, data that can be processed is limited to the data that can fit in server memory.

Visit the [MRAN Website](https://mran.microsoft.com/) to learn more about Microsoft R Open and download it.

<a name="compare-prods"></a>
##The Microsoft R Family

The feature set provided by Microsoft R Server, Microsoft R Client, and Microsoft R Open can be categorized as follows:

|Feature   |Microsoft R Open|Microsoft R Client|Microsoft R Server|
|----------|----------------|------------------|-----------|
|Big Data  |In-memory bound<br>Can only process datasets that fit into the available memory|In-memory bound<br>Can process datasets that fit into the available memory<br>Operates on large volumes when connected to R Server|Disk scalability<br>Operates on bigger volumes & factors|
|Speed of<br>Analysis|Multi-threaded when MKL is installed for non-ScaleR functions|Multi-threaded with MKL for non-ScaleR functions<br>Up to 2 threads for ScaleR functions with a local compute context|Full parallel threading & processing|
|Enterprise<br>Readiness|Community support|Commercial support|Commercial support|
|Analytic<br>Breadth <br>& Depth|7000+ open source packages|Leverage & optimize open source R packages plus 'Big Data'-ready ScaleR packages|Leverage & optimize open source R packages plus 'Big Data'-ready + Multithreaded ready ScaleR packages|
|Commercial<br>Viability|Risk of deployment to open source|Free for everyone|Commercial licenses|
|[DeployR Enterprise](microsoft-r-getting-started.md#deployr-intro)   |Not available|Not available|Included|



##Learn more

Start learning about Microsoft R today:
+ [Microsoft R Getting Started](microsoft-r-getting-started.md)

+ [DeployR Getting Started](deployr-about.md)

+ [Microsoft R Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr)

+ [Product Web Page](https://www.microsoft.com/en-us/server-cloud/products/r-server/) 

+ [Additional Resources](microsoft-r-more-resources.md)

+ [Documentation Archives](microsoft-r-old-versions.md)