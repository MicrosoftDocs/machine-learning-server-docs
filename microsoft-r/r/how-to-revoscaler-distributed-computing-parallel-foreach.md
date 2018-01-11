---

# required metadata
title: "Parallel execution using foreach and doRSR for script containing RevoScaleR and foreach constructs"
description: "Parallel execution using the doRSR package for script containing RevoScaleR and foreach constructs."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "01/03/2018"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---

# Parallel execution using doRSR for script containing RevoScaleR and foreach constructs

Machine Learning Server includes the open-source [foreach package](https://CRAN.R-project.org/package=foreach) in case you need to substitute the built-in parallel execution methodology of RevoScaleR with a custom implementation. 

To execute code that leverages foreach, you will need a parallel backend engine, similar to **NetWorkSpaces**, **snow**, and **rmpi**. For integration with script leveraging RevoScaleR, you can use the **doRSR** package. The **doRSR** package is a parallel backend for RevoScaleR, built on top of [rxExec](../r-reference/revoscaler/rxexec.md), and included with all RevoScaleR distributions.

## Example script using doRSR

To get started with doRSR, load the package and register the backend:

	library(doRSR)
	registerDoRSR()

The doRSR package uses your current compute context to determine how to run your job. In most cases, the job is run via rxExec, sequentially in the local compute context and in parallel in a distributed compute context. In the special case where you are in the local compute context and have set `rxOptions(useDoParallel=TRUE)`, doRSR will pass your foreach jobs to the **doParallel** package for execution in parallel using multiple cores on your machine.

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

### Use case: simulation

In [Running jobs in parallel](how-to-revoscaler-distributed-computing-parallel-jobs.md), we introduced the simulation function `playDice` to simulate a single game of dice rolling, using [rxExec](../r-reference/revoscaler/rxexec.md) to play 10000 games. You can do the same thing with foreach:

	z1 <- foreach(i=1:10000, .options.rsr=list(chunkSize=2000)) %dopar% playDice()
	table(unlist(z1))		

	Loss  Win
	5079 4921

Although outcomes are equivalent in both approaches, the rxExec approach is several times faster because you can call it directly.

### Use case: naïve parallelization of the standard R kmeans function

Also in the [previous article](how-to-revoscaler-distributed-computing-parallel-jobs.md), we created a function kmeansRSR to perform a naïve parallelization of the standard R kmeans function. We can do the same thing with foreach directly as follows:

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

