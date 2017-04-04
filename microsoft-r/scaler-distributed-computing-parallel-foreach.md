---

# required metadata
title: "Parallel execution using doRSR for script containing ScaleR and foreach constructs"
description: "Parallel execution using the doRSR package for script containing RevoScaleR and foreach constructs."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/02/2017"
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

# Parallel execution using doRSR for script containing ScaleR and foreach constructs

The open-source [foreach package](https://CRAN.R-project.org/package=foreach) combines a looping structure for R script 
that can execute in parallel. When you need to loop through repeated operations, and you have multiple processors or nodes to work with, you can use **foreach** in your script to execute a forloop in parallel. Developed by Microsoft, foreach is an open source package that is bundled with R Server but is also available on the Comprehensive R Archive Network, CRAN.  

To execute code that leverages foreach, you will need a parallel backend engine, similar to **NetWorkSpaces**, **snow**, and **rmpi**. For integration with script leveraging ScaleR, you can use the **doRSR** package. The **doRSR** package is a parallel backend for RevoScaleR, built on top of `rxExec`, and included with all RevoScaleR distributions.

To get started with doRSR, load the package and register the backend:

	library(doRSR)
	registerDoRSR()

The doRSR package uses your current compute context to determine how to run your job. In most cases, the job is run via `rxExec`, sequentially in the local compute context and in parallel in a distributed compute context. In the special case where you are in the local compute context and have set `rxOptions(useDoParallel=TRUE)`, doRSR will pass your foreach jobs to the **doParallel** package for execution in parallel using multiple cores on your machine.

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

In [Running jobs in parallel](scaler-distributed-computing-parallel-jobs.md), we introduced the simulation function `playDice` in the previous section. It simulates a single game of dice rolling. We then used `rxExec` to play 10000 games. Now we will use foreach to play 10000 games:

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

