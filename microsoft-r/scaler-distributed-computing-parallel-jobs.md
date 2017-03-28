---

# required metadata
title: "Parallel computing in Microsoft R"
description: "Microsoft R Server in-database and cluster computing using the ScaleR engine and RevoScaleR package."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/23/2017"
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

# Running jobs in parallel (ScaleR in Microsoft R)

ScaleR functions can be used to distribute computations over more than one R Server or R Client instance, allowing you to run multiple workloads in parallel on multiple computers. To get distributed computations, you create one or more *compute contexts*, and then shift script execution to a ScaleR engine on a different computer or platform. We call this flexibility *Write Once, Deploy Anywhere*, or *WODA*. 

A *compute context* specifies the computing resources to be used by ScaleR’s distributable computing functions. ScaleR functions like RxSpark, RxHadoopMR, or RxInSQLServer are used to set the compute contenxt. Typically, you will specify different parameters depending on whether commands are issued locally or remotely.

ScaleR's distributed computing capabilities vary by platform and the details for creating a compute context vary depending upon the specific framework used to support those distributed computing capabilities. However, once you have established a computing context, you can use the same **RevoScaleR** commands to manage your data, analyze data, and control computations in all frameworks.

> [!NOTE]
> RevoScaleR is available in both R Server and R Client. You can develop script in R Client for execution on R Server.  However, because R Client is limited to two threads for processing and in-memory datasets, scripts might require deeper customizations if the scope of operations involve much larger datasets that introduce dependencies on chunking. Chunking is not supported in R Client. In R Client, the `blocksPerRead` argument is ignored and all data is read into memory. Large datasets that exceed memory must be pushed to a compute context of a Microsoft R Server instance.
>

<a name="parallel-computing-with-rxexec"></a>
## Parallel Computing with rxExec

While the **RevoScaleR** HPA functions are engineered to work in parallel automatically, other R functions always run sequentially. As we have seen, the `rxExec` function allows you to take an arbitrary function and run it in parallel on your distributed computing resources. This in turn allows you to tackle a large variety of parallel computing problems, in particular those of the *high-performance computing class*. 

In this article, you will learn how `rxExec` can be used to perform parallel computations. To demonstate, you'll use script to simulate a dice-rolling game, determine the probability that any two persons in a given group size share a birthday, create a plot of the Mandelbrot set, and perform naive k-means clustering.

In general, the only required arguments to `rxExec` are the function to be run and any required arguments of that function. Additional arguments can be used to control the computation.

>[!IMPORTANT]
> Before trying the examples, set your compute context to one of the following: `RxLocalParallel`, `RxSpark`, `RxHadoopMR`, or `RxInTeradata` (depending on the compute resources available to you) and be sure that your compute context has the option *wait=TRUE*.

### Playing Dice: A Simulation

A familiar casino game consists of rolling a pair of dice. If you roll a 7 or 11 on your initial roll, you win. If you roll 2, 3, or 12, you lose. Roll a 4, 5, 6, 8, 9, or 10, NS that number becomes your *point* and you continue rolling until you either roll your point again (in which case you win) or roll a 7, in which case you lose. The game is easily simulated in R using the following function:

	playDice <- function()
	{
		result <- NULL
		point <- NULL
		count <- 1
		while (is.null(result))
		{
			roll <- sum(sample(6, 2, replace=TRUE))

			if (is.null(point))
			{
				point <- roll
			}
			if (count == 1 && (roll == 7 || roll == 11))
			{
				result <- "Win"
			}
	 		else if (count == 1 && (roll == 2 || roll == 3 || roll == 12))
			{
				result <- "Loss"
			}
			else if (count > 1 && roll == 7 )
			{
				result <- "Loss"
			}
			else if (count > 1 && point == roll)
			{
				result <- "Win"
			}
			else
			{
				count <- count + 1
			}
		}
		result
	}

We will now use `rxExec` to play thousands of games to help determine the probability of a win. Using a five-node HPC Server cluster, we play the game 10000 times, 2000 times on each node:

	z <- rxExec(playDice, timesToRun=10000, taskChunkSize=2000)
	table(unlist(z))

	Loss  Win
	5087 4913

We expect approximately 4929 wins in 10000 trials, and our result of 4913 wins is pretty close.

### The Birthday Problem

The birthday problem is an old standby in introductory statistics classes because its result seems counterintuitive. In a group of about 25 people, the chances are better than 50-50 that at least two people in the room will share a birthday. Put 50 people in a room and you are practically guaranteed there will be a birthday-sharing pair. Since 50 is so much less than 365, most people are surprised by this result.

We can use the following function to estimate the probability of at least one birthday-sharing pair in groups of various sizes (the first line of the function is what allows us to obtain results for more than one value at a time; the remaining calculations are for a single n):

	"pbirthday" <- function(n, ntests=5000)
	{   
		if (length(n) > 1L) return(sapply(n, pbirthday, ntests = ntests))

		daysInYear <- seq.int(365)
		anydup <- function(i)
		{
			any(duplicated(sample(daysInYear, size = n, replace = TRUE)))
		}

		prob <- sum(sapply(seq.int(ntests), anydup)) / ntests
		names(prob) <- n
		prob
	}

We can test that it works in a sequential setting, estimating the probability for group sizes 3, 25, and 50 as follows:

	pbirthday(c(3,25,50))

For each group size, 5000 random tests were performed. For this run, the following results were returned:

	     3     25     50
	0.0078 0.5710 0.9726

Make sure your compute context is set to a “waiting” context.  Then distribute this computation for groups of 2 to 100 using `rxExec` as follows, using *rxElemArg* to specify a different argument for each call to pbirthday, and then using the *taskChunkSize* argument to pass these arguments to the nodes in chunks of 20:

	z <- rxExec(pbirthday, n=rxElemArg(2:100), taskChunkSize=20)

The results will be returned in a list, with one element for each node. We can use *unlist* to convert the results into a single vector:

	probSameBD <- unlist(z)

We can make a colorful plot of the results by constructing variables for the party sizes and the nodes where each computation was performed:

	partySize <- 2:100
	nodes <- as.factor( rep(1:5, each=20)[2:100])
	levels(nodes) <- paste("Node", levels(nodes))
	birthdayData <- data.frame(probSameBD, partySize, nodes)

	rxLinePlot( probSameBD~partySize, groups = nodes, data=birthdayData,
		type = "p",
		xTitle = "Party Size",
		yTitle = "Probability of Same Birthday",
		title = "Our Rockin Soiree!")

The resulting plot is shown below:

![Birthday Problem Plot](media/rserver-scaler-distributed-computing/birthday_plot.png)

### Plotting the Mandelbrot Set

Computing the Mandelbrot set is a popular parallel computing example because it involves a simple computation performed independently on an array of points in the complex plane. For any point *z=x+yi* in the complex plane, *z* belongs to the Mandelbrot set if and only if *z* remains bounded under the iteration *z_(n+1)=z_n^2+z_n*. If we are associating a point (*x_0,y_0*) in the plane with a pixel on a computer screen, the following R function returns the number of iterations before the point becomes unbounded, or the maximum number of iterations. If the maximum number of iterations is returned, the point is assumed to be in the set:

	mandelbrot <- function(x0,y0,lim)
	{
		x <- x0; y <- y0
		iter <- 0
		while (x^2 + y^2 < 4 && iter < lim)
		{
			xtemp <- x^2 - y^2 + x0
			y <- 2 * x * y + y0
			x <- xtemp
			iter <- iter + 1
		}
		iter
	}

The following function retains the basic computation but returns a vector of results for a given y value:

	vmandelbrot <- function(xvec, y0, lim)
	{
		unlist(lapply(xvec, mandelbrot, y0=y0, lim=lim))
	}

We can then distribute this computation by computing several rows at a time on each compute resource. In the following, we create an input x vector of length 240, a y vector of length 240, and specify the iteration limit as 100. We then call `rxExec` with our *vmandelbrot* function, giving 1/5 of the y vector to each computational node in our five node HPC Server cluster. This should be done in a compute context with *wait=TRUE*. Finally, we put the results into a 240x240 matrix and create an image plot that shows the familiar Mandelbrot set:

	x.in <- seq(-2.0, 0.6, length.out=240)
	y.in <- seq(-1.3, 1.3, length.out=240)
	m <- 100
	z <- rxExec(vmandelbrot, x.in,y0=rxElemArg(y.in), m, taskChunkSize=48,
			execObjects="mandelbrot")
	z <- matrix(unlist(z), ncol=240)
	image(x.in, y.in, z, col=c(rainbow(m), '#000000'), useRaster=TRUE)

The resulting plot is shown below (not all graphics devices support the useRaster argument; if your plot is empty, try omitting that argument):

![Mandelbrot Plot](media/rserver-scaler-distributed-computing/mandelbrot_plot.png)

### Naïve Parallel k-Means Clustering

**RevoScaleR** has a built-in analysis function, `rxKmeans`, to perform distributed k-means, but in this section we see how the regular R kmeans function can be put to use in a distributed context.

The kmeans function implements several *iterative relocation* algorithms for clustering. An iterative relocation algorithm starts from an initial classification and then iteratively moves data points from one cluster to another to reduce sums of squares. One possible starting point is to simply pick cluster centers at random and then assign points to each cluster so that the sum of squares is minimized. If this procedure is repeated many times for different sets of centers, the set with the smallest error can be chosen.

We can do this with the ordinary kmeans function, which has a parameter *nstart* that tells it how many times to pick the starting centers, and also to pick the set of centers that returns a result with smallest error:

	x <- matrix(rnorm(250000), nrow = 5000, ncol = 50)
	system.time(kmeans(x, centers=10, iter.max = 35, nstart = 400))

On a Dell XPS laptop with 8GB of RAM, this takes about a minute and a half.

To parallelize this efficiently, we should do the following:

- Pass the data to a specified number of computing resources just once.
- Split the work into smaller tasks for passing to each computing resource.
- Combine the results from all the computing resources so the best result is returned.

In the case of *kmeans*, we can ask for the computations to be done by *cores*, rather than by *nodes*. (Currently, the *elemType* argument is honored only for *RxHpcServer* compute contexts. See the *rxExec* help file for details.) And because we are distributing the computation, we can do fewer repetitions (*nstarts*) on each compute element. We can do all of this with the following function (again, this should be run with a compute context for which *wait=TRUE*):

	kMeansRSR <- function(x, centers=5, iter.max=10, nstart=1)
	{
		numTimes <- 20
		results <- rxExec(FUN = kmeans, x=x, centers=centers, iter.max=iter.max,
			nstart=nstart, elemType="cores", timesToRun=numTimes)
		best <- 1
		bestSS <- sum(results[[1]]$withinss)
		for (j in 1:numTimes)
		{
		      jSS <- sum(results[[j]]$withinss)
		      if (bestSS > jSS)
		      {
		              best <- j
		             bestSS <- jSS
			  }
		}
		results[[best]]
	}

Notice that in our *kMeansRSR* function we are letting the underlying *kmeans* function find nstart sets of centers per call and the choice of “best” is done in our function after we have called *kmeans numTimes*. No parallelization is done to *kmeans* itself.

With our *kMeansRSR* function, we can then repeat the computation from before:

	system.time(kMeansRSR(x, 10, 35, 20))

With our 5-node HPC Server cluster, this reduces the time from a minute and a half to about 15 seconds.

### Sharing Data Efficiently Between Parallel Processes 

Data can be shared between `rxExec` parallel processes by copying it to the environment of each process through the `execObjects` option to `rxExec`, or by specifying the data as arguments to each function call. For small data, this works well but as the data objects get larger this can create a significant performance penalty due to the time needed to do the copy. In such cases, it can be much more efficient to share the data by storing it in a location accessible by each of the parallel processes, such as a local or network file share.  

The following example shows how this can be done when parallelizing the computation of statistics on subsets of a larger data table.  

We’ll start by creating some sample data using the **data.table** package.  

~~~~
	library(data.table)

	nrows <- 5e6
	test.data  <- data.table(tag=sample(LETTERS[1:7], nrows, replace=TRUE), 
							a=runif(nrows), b=runif(nrows))

	# get the unique tags that we'll subset by 
	tags <- unique(test.data$tag)
	tags
~~~~

Next we’ll setup for use of the **doParallel** backend. 

~~~~
	library(doParallel) 
	(nCore <- parallel::detectCores(all.tests = TRUE, logical = FALSE) )
	cl <- makeCluster(nCore, useXDR = F)
	registerDoParallel(cl)
	rxSetComputeContext(RxForeachDoPar())

	# set MKL thread to 1 to avoid contention 
	setMKLthreads(1)  
~~~~

Now we’ll create a function for computing statistics on a selected subset of the data table by passing in both the data table and the tag value to subset by. We’ll then run the function using `rxExec` for each tag value.

~~~~
	filterTest <- function(d, ztag) {
	sum(subset(d, tag==ztag, select=a)) 
	}
	system.time({
	res1 <- rxExec(filterTest, test.data, rxElemArg(tags))
	}) 
	##   user  system elapsed 
	##   9.67    7.40   22.05 
~~~~

To see the impact of sharing the file via a common storage location rather than passing it to each parallel process we’ll save the data table into an RDS file, which is an efficient way of storing individual R objects, create a new version of the function that reads the data table from the RDS file, and then re-run `rxExec` using the new function. 

~~~~
	saveRDS(test.data, 'c:/temp/tmp.rds', compress=FALSE)
	filterTest <- function(ztag) {
	d <- readRDS('c:/temp/tmp.rds')
	sum(subset(d, tag==ztag, select=a)) 
	}

	system.time({
	res2 <- rxExec(filterTest, rxElemArg(tags))
	}) 
	##   user  system elapsed 
	##   0.03    0.00   11.01 

	# make sure the results match 
	identical(res2, res2)  
	## [1] TRUE 

	# close up shop 
	stopCluster(cl)
~~~~

Although results may vary, in this case we’ve reduced the elapsed time by 50% by sharing the data table rather than passing it as an in-memory object to each parallel process.   

<a name="parallel-random-number-generation"></a>
### Parallel Random Number Generation

When generating random numbers in parallel computation, a frequent problem is the possibility of highly correlated random number streams. High quality parallel random number generators avoid this problem. RevoScaleR includes several high quality parallel random number generators and these can be used with rxExec to improve the quality of your parallel simulations.

By default, a parallel version of the Mersenne-Twister random number generator is used that supports 6024 separate substreams. We can set it to work on our dice example by setting a non-null seed:

	z <- rxExec(playDice, timesToRun=10000, taskChunkSize=2000, RNGseed=777)
	table(unlist(z))

This makes our simulation repeatable:

	Loss  Win
	5104 4896

	z <- rxExec(playDice, timesToRun=10000, taskChunkSize=2000, RNGseed=777)
	table(unlist(z))

	Loss  Win
	5104 4896

This random number generator can be asked for explicitly by specifying RNGkind="MT2203":

	z <- rxExec(playDice, timesToRun=10000, taskChunkSize=2000, RNGseed=777,
	    RNGkind="MT2203")
	table(unlist(z))

	Loss  Win
	5104 4896

We can build reproducibility into our naïve k-means example as follows:

	kMeansRSR <- function(x, centers=5, iter.max=10, nstart=1, numTimes = 20, seed = NULL)
	{
		results <- rxExec(FUN = kmeans, x=x, centers=centers, iter.max=iter.max,
			nstart=nstart, elemType="cores", timesToRun=numTimes, RNGseed = seed)
		best <- 1
		bestSS <- sum(results[[1]]$withinss)
		for (j in 1:numTimes)
		{
		      jSS <- sum(results[[j]]$withinss)
		      if (bestSS > jSS)
		      {
		              best <- j
		             bestSS <- jSS
			  }
		}
		results[[best]]
	}
	km1 <- kMeansRSR(x, 10, 35, 20, seed=777)
	km2 <- kMeansRSR(x, 10, 35, 20, seed=777)
	all.equal(km1, km2)

	  [1] TRUE

To obtain the default random number generators without setting a seed, specify "auto" as the argument to either RNGseed or RNGkind:

	x3 <- rxExec(runif, 500, timesToRun=5, RNGkind="auto")
	x4 <- rxExec(runif, 500, timesToRun=5, RNGseed="auto")

To verify that we are actually getting uncorrelated streams, we can use runif within rxExec to generate a list of vectors of random vectors, then use the cor function to measure the correlation between vectors:

	x <- rxExec(runif, 500, timesToRun=5, RNGkind="MT2203")
	x.df <- data.frame(x)
	corx <- cor(x.df)
	diag(corx) <- 0
	any(abs(corx)> 0.3)

None of the correlations is above 0.3; in repeated runs of the code, the maximum correlation seldom exceeded 0.1.

Because the MT2203 generator offers such a rich array of substreams, we recommend its use. You can, however, use several other generators, all from Intel’s Vector Statistical Library, a component of the Intel Math Kernel Library. The available generators are as follows: "MCG31", "R250", "MRG32K3A", "MCG59", "MT19937", "MT2203", "SFMT19937" (all of which are pseudo-random number generators which can be used to generate uncorrelated random number streams) plus "SOBOL" and "NIEDERR", which are quasi-random number generators that do not generate uncorrelated random number streams. Detailed descriptions of the available generators can be found in the [Vector Statistical Library Notes](http://software.intel.com/sites/products/documentation/doclib/mkl_sa/11/vslnotes/vslnotes.pdf).

#### A Note on Reproducibility

For distributed compute contexts, rxExec starts the random number streams on a per-worker basis; if there are more tasks than workers, you may not obtain completely reproducible results because different tasks may be performed by randomly chosen workers. If you need completely reproducible results, you can use the taskChunkSize argument to force the number of task *chunks* to be less than or equal to the number of workers—this will ensure that each chunk of tasks is performed on a single random number stream. You can also define a custom function that includes random number generation control within it; this moves the random number control into each task. See the help file for rxRngNewStream for details.

### Working with Results from Non-Blocking Jobs

So far, all of our examples have required a blocking, or waiting, compute context so that we could make immediate use of the results returned by rxExec. But as we saw in the section on non-blocking jobs, some computations will be so time consuming that it is not practical to wait on the results. In such cases, it is probably best to divide your analysis into two or more pieces, one of which can be structured as a non-blocking job, and then use the pending job (or more usefully, the job results, when available) as input to the remaining pieces.

For example, let’s return to the birthday example, and see how to restructure our analysis to use a non-blocking job for the distributed computations. The pbirthday function itself requires no changes, and our variable specifying the number of ntests can be used as is:

	"pbirthday" <- function(n, ntests=5000)
	{   
		if (length(n) > 1L) return(sapply(n, pbirthday, ntests = ntests))

		daysInYear <- seq.int(365)
		anydup <- function(i)
		{
			any(duplicated(sample(daysInYear, size = n, replace = TRUE)))
		}

		prob <- sum(sapply(seq.int(ntests), anydup)) / ntests
		names(prob) <- n
		prob
	}
	ntests <- 2000

However, when we call rxExec, the return object will no longer be the results list, but a jobInfo object:

	z <- rxExec(pbirthday, n=rxElemArg(2:100), ntests=ntests, taskChunkSize=20)

We check the job status:

	rxGetJobStatus(z)

	  [1] "finished"

We can then proceed almost as before:

	probSameBD <- unlist(rxGetJobResults(z))
	partySize <- 2:100
	nodes = as.factor( rep(1:5, each=20)[2:100])
	levels(nodes) <- paste("Node", levels(nodes))
	birthdayData <- data.frame(probSameBD, partySize, nodes)

	rxLinePlot( probSameBD~partySize, groups = nodes, data=birthdayData,
		type = "p",
		xTitle = "Party Size",
		yTitle = "Probability of Same Birthday",
		title = "Our Rockin Soiree!")

The other examples are a bit trickier, in that the result of the calls to rxExec were embedded in functions. But again, dividing the computations into distributed and non-distributed components can help—the distributed computations can be non-blocking, and the non-distributed portions can then be applied to the results. Thus the kmeans example can be rewritten thus:

	genKmeansClusters <- function(x, centers=5, iter.max=10, nstart=1)
	{
		numTimes <- 20
		rxExec(FUN = kmeans, x=x, centers=centers, iter.max=iter.max,
			nstart=nstart, elemType="cores", timesToRun=numTimes)
	}

	findKmeansBest <- function(results){
		numTimes <- length(results)
		best <- 1
		bestSS <- sum(results[[1]]$withinss)
		for (j in 1:numTimes)
		{
		      jSS <- sum(results[[j]]$withinss)
		      if (bestSS > jSS)
		      {
		              best <- j
		             bestSS <- jSS
			  }
		}
		results[[best]]
	}

To run this in our non-blocking cluster context, we do the following:

	x <- matrix(rnorm(250000), nrow = 5000, ncol = 50)
	z <- genKmeansClusters(x, 10, 35, 20)

Once we see that z’s job status is “finished”, we can run findKmeansBest on the results:

	findKmeansBest(rxGetJobResults(z))

### Calling HPA Functions from rxExec

To this point, none of the functions we have called with rxExec has been a RevoScaleR function, because our intent has been to show how rxExec can be used to address the large class of traditional high-performance computing problems. However, there is no inherent reason why rxExec cannot be used with RevoScaleR’s HPA functions, and many times it can be extremely useful to do so. For example, if you are running a cluster on which every node has two or more cores, you can use rxExec to start an independent analysis on each node, and each of those analyses can take advantage of the multiple cores on its node. The following simulation simulates data from a Poisson distribution and then fits a generalized linear model to the simulated data:

	"SimAndEstimatePoisson" <- function(nobs, trials)
	{
	    "SimulatePoissonData" <- function(nobs)
	    {
		    x1 <- log(runif(nobs, min=.5, max=1.5))
		    x2 <- log(runif(nobs, min=.5, max=1.5))

		    b0 <- 0
		    b1 <- 1
		    b2 <- 2
		    lambda <- exp(b0 + b1*x1 + b2*x2)
	        count <- rpois(nobs,lambda)
			pSim <- data.frame(count=count, x1=x1, x2=x2)
	    }

	    cf <- NULL
	    rxOptions(reportProgress = 0)
	    for (i in 1:trials)
	    {
			simData <- SimulatePoissonData(nobs)
			result1 <- rxGlm(count~x1+x2,data=simData,family=poisson())
		    cf <- rbind(cf,as.double(coefficients(result1)))
	    }
	    cf       
	}

If we call the above function with rxExec on a five-node cluster compute context, we get five simulations running simultaneously, and can easily produce 1000 simulations as follows:

	rxExec(SimAndEstimatePoisson, nobs=50000, trials=10, elemType = "nodes",
	    taskChunkSize=5, timesToRun=100)

It is important to recognize the distinction between running an HPA function with a distributed compute context, and calling an HPA function using rxExec with a distributed compute context. In the former case, we are fitting just one model, using the distributed compute context to farm out portions of the computations, but ultimately returning just one model object. In the latter case, we are calculating one model per task, the tasks being farmed out to the various nodes or cores as desired, and a list of models is returned.

### Using rxExec in the Local Compute Context

By default, if you call `rxExec` in the local compute context, your computation runS sequentially on your local machine. However, you can incorporate parallel computing on your local machine using the special compute context `RxLocalParallel` as follows:

	rxSetComputeContext(RxLocalParallel())

This allows the ParallelR package doParallel to distribute the computation among the available cores of your computer.

If you are using random numbers in the local parallel context, be aware that `rxExec` chooses a number of workers based on the number of tasks and the current value of `rxGetOption("numCoresToUse")`. to guarantee each task runS with a separate random number stream, set `rxOptions(numCoresToUse)` equal to the number of tasks, and explicitly set *timesToRun* to the number of tasks. For example, if we want a list consisting of five sets of uniform random numbers, we could do the following to obtain reproducible results:

	rxOptions(numCoresToUse=5)
	x1 <- rxExec(runif, 500, timesToRun=5, RNGkind="MT2203", RNGseed=14)
	x2 <- rxExec(runif, 500, timesToRun=5, RNGkind="MT2203", RNGseed=14)
	all.equal(x1, x2)

**numCoresToUse** is a scalar integer specifying the number of cores to use. If you set this parameter to either -1 or a value in excess of available cores, ScaleR will use however many cores are available. Increasing the number of cores also increases the amount of memory required for ScaleR analysis functions.

>[!NOTE]
> HPA functions are not affected by the `RxLocalParallel` compute context; they will run locally and in the usual internally distributed fashion when the `RxLocalParallel` compute context is in effect.

### Using rxExec with foreach Back Ends

If you do not have access to a Hadoop cluster or enterprise database, but do have access to a cluster via PVM, MPI, socket, or NetWorkSpaces connections or a multicore workstation, you can use `rxExec` with an arbitrary foreach backend (doParallel, doSNOW, doMPI, etc.) Simply register your parallel backend as usual and then set your RevoScaleR compute context using the special compute context `RxForeachDoPar`:

	rxSetComputeContext(RxForeachDoPar())

For example, here is how you might start a SNOW-like cluster connection with the doParallel back end:

	library(doParallel)
	cl <- makeCluster(4)
	registerDoParallel(cl)
	rxSetComputeContext(RxForeachDoPar())

You then call `rxExec` as usual. The computations are automatically directed to the registered foreach back end.

>[!WARNING]
> HPA functions are not usually affected by the `RxForeachDoPar` compute context; they will run locally and in the usual internally distributed fashion when the RxForeachDoPar compute context is in effect. The one exception is when HPA functions are called within `rxExec`; in this case it is possible that the internal threading of the HPA functions can be affected by the launch mechanism of the parallel backend workers. The doMC backend and the multicore-like backend of doParallel both use forking to launch their workers; this is known to be incompatible with the HPA functions.

### Controlling rxExec Computations

As we have seen in these examples, there are several arguments to `rxExec` that allow you to fine-tune your `rxExec` commands. Both the birthday example and the Mandelbrot example used the *taskChunkSize* argument to specify how many tasks should go to each worker. The Mandelbrot example also used the *execObjects* argument, which can be used to pass either a character vector or an environment containing objects—the objects specified by the vector or contained in the environment are added to the environment of the function specified in the FUN argument, unless that environment is locked, in which case they are added to the parent frame in which *FUN* is evaluated. (If you use an environment, it should be one you create with *parent=emptyenv();* this allows you to pass only those objects you need to the function’s environment.) These two examples also show the use of *rxElemArg* in passing arguments to the workers. In the kmeans example, we met the *elemType* and *timesToRun* arguments. The *packagesToLoad* argument allows you to specify packages to load on each worker. The *consoleOutput* and *autoCleanup* flags serve the same purpose as their counterparts in the compute context constructor functions—that is, they can be used to specify whether console output should be displayed or the associated task files should be cleaned up on job completion for an individual call to 'rxExec'.

Two additional arguments remain to be introduced: *oncePerElem* and *continueOnFailure*. The *oncePerElem* argument restricts the called function to be run just once per allotted node; this is frequently used with the *timesToRun* argument to ensure that each occurrence is run on a separate node. The *oncePerElem* argument, however, can only be set to *TRUE* if *elemType="nodes"*. It must be set to *FALSE* if *elemType="cores"*.

If *oncePerElem* is *TRUE* and *elemType="nodes"*, *rxExec*’s results are returned in a list with components named by node. If a given node does not have a valid R syntactic name, its name is mangled to become a valid R syntactic name for use in the return list.

The *continueOnFailure* argument is used to say that a computation should continue even if one or more of the compute elements fails for some reason; this is useful, for example, if you are running several thousand independent simulations and It doesn’t matter if you get results for all of them. Using *continueOnFailure=TRUE* (the default), you will get results for all compute elements that finish the simulation and error messages for the compute elements that fail.

>[!NOTE]
> The arguments *elemType*, *consoleOutput*, *autoCleanup*, *continueOnFailure*, and *oncePerElem* are ignored by the special compute contexts 'RxLocalParallel' and 'RxForeachDoPar`.

## Using RevoScaleR with foreach: Package doRSR

The foreach package provides a for-loop-like approach to parallel computing that has proven quite popular. Developed by Microsoft, foreach is an open source package that is bundled with Microsoft R but is also available on the Comprehensive R Archive Network, CRAN. Parallel backends have been written for a variety of parallel computing packages, including nws, snow, and rmpi. If you need to share parallel code with users of other R distributions, writing that code using foreach provides considerable flexibility. To execute that code in Microsoft R using your distributed computing resources, you can use the doRSR package.

The doRSR package is a parallel backend for RevoScaleR, built on top of rxExec, and included with all RevoScaleR distributions. To get started using it, simply load the doRSR package and register the backend:

	library(doRSR)
	registerDoRSR()

The doRSR package uses your current compute context to determine how to run your job. In most cases, the job is run via rxExec, sequentially in the local compute context and in parallel in a distributed compute context. In the special case where you are in the local compute context and have set rxOptions(useDoParallel=TRUE), doRSR will pass your foreach jobs to the doParallel package for execution in parallel using multiple cores on your machine.

A simple example is this one from the foreach help file:

	foreach(i=1:3) %dopar% sqrt(i)

This returns, as expected, a list containing the square roots of 1, 2, and 3:

	[[1]]
	[1] 1

	[[2]]
	[1] 1.414214

	[[3]]
	[1] 1.732051

Another example is what the help file reports as a “simple (and inefficient) parallel matrix multiply”:

	a <- matrix(1:16, 4, 4)
	b <- t(a)
	foreach(b=iter(b, by='col'), .combine=cbind) %dopar%
	  (a %*% b)

This returns the multiplied matrix:

	     [,1] [,2] [,3] [,4]
	[1,]  276  304  332  360
	[2,]  304  336  368  400
	[3,]  332  368  404  440
	[4,]  360  400  440  480

### A Simple Simulation: Playing Dice

We introduced the simulation function playDice in the previous section. It simulates a single game of dice rolling. We then used rxExec to play 10000 games. Now we will use foreach to play 10000 games:

	z1 <- foreach(i=1:10000, .options.rsr=list(chunkSize=2000)) %dopar% playDice()
	table(unlist(z1))		

	Loss  Win
	5079 4921

Again, we get about the expected number of wins. If you time the rxExec version versus the foreach version using doRSR, you will find the rxExec version several times faster. This is to be expected; foreach is a high-level interface allowing access to many different back ends, including RevoScaleR’s rxExec. It will necessarily be slower than calling those back ends directly.

### Another Version of kmeans

Also in the previous section, we created a function kmeansRSR to perform a naïve parallelization of the standard R kmeans function. We can do the same thing with foreach directly as follows:

	kMeansForeach <- function(x, centers=5, iter.max=10, nstart=1)
	{
		numTimes <- 20
		results <- foreach(i=1:numTimes) %dopar% kmeans(x=x, centers=centers, iter.max=iter.max,
			nstart=nstart)
		best <- 1
		bestSS <- sum(results[[1]]$withinss)
		for (j in 1:numTimes)
		{
		      jSS <- sum(results[[j]]$withinss)
		      if (bestSS > jSS)
		      {
		              best <- j
		             bestSS <- jSS
			  }
		}
		results[[best]]
	}

Recall that the idea was to run a specified number of kmeans fits, then find the best set of results, where “best” is the result with the lowest within-group sum of squares. We can run this function as follows:

	x <- matrix(rnorm(250000), nrow = 5000, ncol = 50)
	kMeansForeach(x, 10, 35, 20)

<!--## Managing Distributed Data

There are several basic approaches to data management in distributed computing:

1.	On systems with traditional file systems, you can either put all the data on all the nodes or distribute only the data that a node requires for its computations to that particular node. In such file systems, it is important that the data be local to the nodes rather than accessed over a network; for large data sets, the computation time for network-accessed data can be many times slower than for local data. In these systems, we recommend using standard .xdf files or “split” .xdf files (see ["Distributing Data with rxSplit"](#distributing-data-with-rxsplit)).

2.	In the Hadoop Distributed File System, the data is distributed automatically, typically to a subset of the nodes, and the computations are also distributed to the nodes containing the required data. On this system, we recommend “composite” .xdf files, which are specialized files designed to be managed by HDFS.

3.	In a Teradata Distributed Data Warehouse, you can perform distributed computations in-database using the RxInTeradata compute context.

For distributing high volumes of data over large networks, custom-engineered network/file-server solutions are probably appropriate.

## Distributing Data with rxSplit  

For some computations, such as those involving distributed prediction, it is most efficient to perform the computations on a distributed data set, one in which each node sees only the data it is supposed to work on. You can split an .xdf file into portions suitable for distribution using the function *rxSplit*. For example, to split the large airline data into five files for distribution on a five node cluster, you could use *rxSplit* as follows:

	rxOptions(computeContext="local")
	bigAirlineData <- "C:/data/AirlineData87to08.xdf"
	rxSplit(bigAirlineData, numOutFiles=5)

By default, *rxSplit* simply appends a number in the sequence from 1 to *numOutFiles* to the base file name to create the new file names, and in this case the resulting file names, for example, “AirlineData87to081.xdf”, are a bit confusing. You can exercise greater control over the output file names by using the *outFilesBase* and *outFilesSuffixes* arguments. With *outFilesBase*, you can specify either a single character string to be used for all files or a character vector the same length as the desired number of files. The latter option is useful, for example, if you would like to create four files with the same file name, but different paths:

	nodepaths <- paste("compute", 10:13, sep="")
	basenames <- file.path("C:", nodepaths, "DistAirlineData")
	rxSplit(bigAirlineData, outFilesBase=basenames)

This creates the four directories C:/compute10, etc., and creates a file named “DistAirlineData.xdf” in each directory. You will want to do something like this when using distributed data with the standard **RevoScaleR** analysis functions such as rxLinMod and rxLogit.

You can supply the *outFilesSuffixes* arguments to exercise greater control over what is appended to the end of each file. Returning to our first example, we can add a hyphen between our base file name and the sequence 1 to 5 using *outFilesSuffixes* as follows:

	rxSplit(bigAirlineData, outFileSuffixes=paste("-", 1:5, sep=""))

The splitBy argument specifies whether to split your data file row-by-row or block-by-block. The default is *splitBy="rows"*, to split by blocks instead, specify *splitBy="blocks"*. The *splitBy* argument is ignored if you also specify the *splitByFactor* argument as a character string representing a valid factor variable. In this case, one file is created per level of the factor variable.

The *rxSplit* function works in the local compute context only; once you’ve split the file you need to distribute the resulting files to the individual nodes using the techniques of the previous sections. You should then specify a compute context with the flag *dataDistType* set to *"split"*. Once you have done this, HPA functions such as *rxLinMod* will know to split their computations according to the data on each node.

### Data Analysis with Split Data

To use split data in your distributed data analysis, the first step is generally to split the data using rxSplit, which as we have seen is a local operation. So the next step is then to copy the split data to your cluster nodes. For the HPA functions such as rxLinMod, the split data must be found somewhere in your specified *dataPath* on each node. For example, to perform this example, we copied the split airline data DistAirlineData.xdf to the C:\data\distributed directory on each of the nodes compute10, compute11, compute12, and compute13. We could, however, have placed the split data in a different place on each node, so long as each of the locations was somewhere in the list of directories in *dataPath*.

Next, create a compute context that specifies *dataDistType="split"*. For example, here is our original HPC Server cluster compute context, with this flag added and with the distributed data folder added to the data path:

	myCluster <- RxHpcServer(
	headNode="cluster-head2",
	revoPath="C:\\Program Files\\Microsoft\\MRO-for-RRE\\8.0\\R-3.2.2\\bin\\x64\\",
	    shareDir="\\AllShare\\myName",
	dataPath=c("C:\\data","C:\\data\\distributed"),
	dataDistType="split")
	rxSetComputeContext(myCluster)

We are now ready to fit a simple linear model:

	AirlineLmDist <- rxLinMod(ArrDelay ~ DayOfWeek,
		data="DistAirlineData.xdf",  cube=TRUE, blocksPerRead=30)

When we print the object, we see that we obtain the same model as when computed with the full data on all nodes:

	Call:
	rxLinMod(formula = ArrDelay ~ DayOfWeek, data = "DistAirlineData.xdf",
	    cube = TRUE, blocksPerRead = 30)

	Cube Linear Regression Results for: ArrDelay ~ DayOfWeek
	File name: C:\data\distributed\DistAirlineData.xdf
	Dependent variable(s): ArrDelay
	Total independent variables: 7
	Number of valid observations: 120947440
	Number of missing observations: 2587529

	Coefficients:
	                    ArrDelay
	DayOfWeek=Monday    6.669515
	DayOfWeek=Tuesday   5.960421
	DayOfWeek=Wednesday 7.091502
	DayOfWeek=Thursday  8.945047
	DayOfWeek=Friday    9.606953
	DayOfWeek=Saturday  4.187419
	DayOfWeek=Sunday    6.525040

With data in the .xdf format, you have your choice of using the full data set or a split data set on each node. For other data sources, you must have the data split across the nodes. For example, the airline data’s original form is a set of .csv files, one for each year from 1987 to 2008. (Additional years are now available, but have not been included in our big airline data.) If we copy the year 2000 data to compute10, the year 2001 data to compute11, the year 2002 data to compute12, and the year 2003 data to compute13 with the file name SplitAirline.csv, we can analyze the data as follows:

	textDS <- RxTextData( file = "C:/data/distributed/SplitAirline.csv",
	              varsToKeep = c( "ArrDelay", "CRSDepTime", "DayOfWeek" ),
	              colInfo = list(ArrDelay   = list( type = "integer" ),
	                        CRSDepTime = list( type = "integer" ),
	                        DayOfWeek  = list( type = "integer" ) )
	           )
	rxSummary( ArrDelay ~ F( DayOfWeek, low = 1, high = 7 ), textDS )

We can then perform an rxLogit model to classify flights as “Late” as follows:

	computeLate <- function( dataList )
	{
	    dataList$Late <- dataList$ArrDelay>15
	    return( dataList )
	}

	rxLogitFitSplitCsv <- rxLogit( Late ~ CRSDepTime + F( DayOfWeek, low = 1,
	                          high = 7 ), data = textDS,
	                          transformVars = c( "ArrDelay" ),
	                          transformFunc=computeLate, verbose=1 )

### Distributed Prediction

You can predict (or score) from a fitted model in a distributed context, but in this case, your data *must* be split. For example, if we fit our distributed linear model with *covCoef=TRUE* (and *cube=FALSE*), we can compute standard errors for the predicted values:

	AirlineLmDist <- rxLinMod(ArrDelay ~ DayOfWeek,
		data="DistAirlineData.xdf",  covCoef=TRUE, blocksPerRead=30)
	rxPredict(AirlineLmDist, data="DistAirlineData.xdf", 	outData="errDistAirlineData.xdf",
		computeStdErrors=TRUE, computeResiduals=TRUE)

> [!NOTE]
> The `blocksPerRead` argument is ignored if script runs locally using R Client.
>

The output data is also split, in this case holding fitted values, residuals, and standard errors for the predicted values.

### Creating Split Training and Test Data Sets

One common technique for validating models is to break the data to be analyzed into training and test subsamples, then fit the model using the training data and score it by predicting on the test data. Once you have split your original data set onto your cluster nodes, you can split the data on the individual nodes by calling rxSplit again within a call to rxExec. If you specify the RNGseed argument to rxExec (see ["Parallel Random Number Generation"](#parallel-random-number-generation)), the split becomes reproducible:

	rxExec(rxSplit, inData="C:/data/distributed/DistAirlineData.xdf",
		outFilesBase="airlineData",
		outFileSuffixes=c("Test", "Train"),
		splitByFactor="testSplitVar",
		varsToKeep=c("Late", "ArrDelay", "DayOfWeek", "CRSDepTime"),
		overwrite=TRUE,
		transforms=list(testSplitVar = factor( sample(c("Test", "Train"),
			size=.rxNumRows, replace=TRUE, prob=c(.10, .9)),
			levels= c("Test", "Train"))), rngSeed=17, consoleOutput=TRUE)

The result is two new data files, airlineData.testSplitVar.Train.xdf and airlineData.testSplitVar.Test.xdf, on each of your nodes. We can fit the model to the training data and predict with the test data as follows:

	AirlineLmDist <- rxLinMod(ArrDelay ~ DayOfWeek,
		data="airlineData.testSplitVar.Train.xdf",  covCoef=TRUE, blocksPerRead=30)
	rxPredict(AirlineLmDist, data="airlineData.testSplitVar.Test.xdf",
		computeStdErrors=TRUE, computeResiduals=TRUE)

> [!NOTE]
> The `blocksPerRead` argument is ignored if script runs locally using R Client.
>

### Performing Data Operations on Each Node

To create or modify data on each node, use the data manipulation functions within rxExec. For example, suppose that after looking at the airline data we decide to create a “cleaner” version of it by keeping only the flights where: there is information on the arrival delay,  the flight did not depart more than one hour early, and the actual and scheduled flight time is positive. We can put a call to *rxDataStep* (and any other code we want processed) into a function to be processed on each node via *rxExec*:

	newAirData <-  function()
	{
	    airData <- "AirlineData87to08.xdf"
	    rxDataStep(inData = airData, outFile = "C:\\data\\airlineNew.xdf",
		   rowSelection = !is.na(ArrDelay) &
	        (DepDelay > -60) & (ActualElapsedTime > 0) & (CRSElapsedTime > 0),
			blocksPerRead = 20, overwrite = TRUE)
	}
	rxExec( newAirData )
-->