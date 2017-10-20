---

# required metadata
title: "Running background jobs using RevoScaleR (Machine Learning Server) "
description: "Machine Learning Server in-database and cluster computing using the RevoScaleR engine and RevoScaleR package."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "03/23/2017"
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

# Running background jobs using RevoScaleR

In Machine Learning Server, you can run jobs interactively, waiting for results before continuing on to the next operation, or you can run them asynchronously in the background if a job is long-running.

<a name="non-waiting-jobs"></a>
## Non-Waiting jobs

By default, all jobs are "waiting jobs" or "blocking jobs" (where control of the R prompt is not returned until the job is complete). As you can imagine, you might want a different interaction model if you are sending time-intensive jobs to your distributed compute context. Decoupling your current session from in-progress jobs will enable jobs to proceed in the background while you continue to work on your R Console for the duration of the computation. This can be useful if you expect the distributed computations to take a significant amount of time, and when such computations are managed by a job scheduler.

## Convert a Waiting Job to a Non-Waiting Job

Suppose you submit a job as a "waiting" job, and then realize that you’d prefer to be able to work in your R session on the local computer while it is running. 

In Windows, simply pressing the Esc will return the cursor to your screen. Depending on how quickly you press Esc, your job will either be canceled (if it has not yet been accepted by the job scheduler), or will continue to run on the cluster. 

Similarly, on Red Hat Enterprise Linux, pressing Ctrl-C will return the cursor to your screen, and either cancel the job or convert it to a non-waiting job.

For all jobs that run on the cluster, the object `rxgLastPendingJob` is automatically created. You can use the `rxgLastPendingJob` object to retrieve your results later or to cancel the job.

## Create Non-Blocking Jobs

To create non-waiting jobs, you simply set wait=FALSE in your compute context object:

	myNoWaitCluster <-  RxSpark(nameNode = "my-name-service-server", port = 8020), wait=FALSE)
	rxOptions(computeContext=myNoWaitCluster)

When *wait* is set to *FALSE*, a job information object rather than a job results object is returned from the submitted job. You should always *assign* this result so that you can use it to obtain job status while the job is running and obtain the job results when the job completes. For example, returning to our initial waiting job example, calling `rxExec` to get data set information, in the non-blocking case we augment our call to `rxExec` with an assignment, and then use the assigned object as input to the `rxGetJobStatus` and `rxGetJobResults` functions:

	airData <- "AirlineData87to08.xdf"
	job1 <- rxExec(rxGetInfo, data=airData)
	rxGetJobStatus(job1)

If you call `rxGetJobStatus` quickly, it may show us that the job is "running", but if called after a few seconds (or longer if another job of higher priority is ahead in the queue) it should report "finished", at which point we can ask for the results:

	rxGetJobResults(job1)

As in the case of the waiting job, we obtain the following results from our five-node cluster (note that the name of the head node is mangled here to be an R syntactic name):

	$CLUSTER_HEAD2
	File name: C:\data\AirlineData87to08.xdf
	Number of observations: 123534969
	Number of variables: 30
	Number of blocks: 832

	$COMPUTE10
	File name: C:\data\AirlineData87to08.xdf
	Number of observations: 123534969
	Number of variables: 30
	Number of blocks: 832

	$COMPUTE11
	File name: C:\data\AirlineData87to08.xdf
	Number of observations: 123534969
	Number of variables: 30
	Number of blocks: 832

	$COMPUTE12
	File name: C:\data\AirlineData87to08.xdf
	Number of observations: 123534969
	Number of variables: 30
	Number of blocks: 832

	$COMPUTE13
	File name: C:\data\AirlineData87to08.xdf
	Number of observations: 123534969
	Number of variables: 30
	Number of blocks: 832

Running a linear model on the airline data is more likely to show us the "running" status:

	delayArrJobInfo <- rxLinMod(ArrDelay ~ DayOfWeek,
		data=airData,  cube=TRUE, blocksPerRead=30)
	rxGetJobStatus(delayArrJobInfo)

This shows us the following:

	[1] "running"

Calling rxGetJobStatus again a few seconds later shows us that the job has completed:

	rxGetJobStatus(delayArrJobInfo)

	[1] "finished"

We can then call `rxGetJobResults` to obtain the actual computation results:

	delayArr <- rxGetJobResults(delayArrJobInfo)
	delayArr

As in the blocking case, this gives the following results:

	Call:
	rxLinMod(formula = ArrDelay ~ DayOfWeek, data = "AirlineData87to08.xdf",
	    cube = TRUE, blocksPerRead = 30)

	Cube Linear Regression Results for: ArrDelay ~ DayOfWeek
	File name: C:\data\AirlineData87to08.xdf
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

## Capture Job Information

If you forget to assign the job information object when you first submit your job, don’t panic. **RevoScaleR** saves the job information for the last pending job as the object `rxgLastPendingJob`. You can assign this value to a more specific name at any time until you submit another non-blocking job.

	rxOptions(computeContext=myNoWaitCluster)
	rxLinMod(ArrDelay ~ DayOfWeek, data="AirlineData87to08.xdf",  
		cube=TRUE, blocksPerRead=30)
	delayArrJobInfo <- rxgLastPendingJob
	rxGetJobStatus(delayArrJobInfo)

> [!NOTE]
> The `blocksPerRead` argument is ignored if script runs locally using R Client.
>

Also, as in all R sessions, the last value returned can be accessed as `.Last.value;` if you remember immediately that you forgot to assign the result, you can simply assign `.Last.value` to your desired job name and be done.

For jobs older than the last pending job, you can use `rxGetJobs` to obtain all the jobs associated with a given compute context. More details on `rxGetJobs` can be found in the next section.

## Cancel a Non-Waiting Job

Suppose you submit a job and realize you’ve mis-specified the formula. In the non-waiting case, it is easy to cancel your job simply by calling `rxCancelJob` with the job information object you saved when you submitted the job:

	rxCancelJob(job1)


## Non-Waiting Logistic Regression

Logistic regression uses an iteratively re-weighted least squares algorithm, and thus in general requires multiple passes through the data for successive iterations. This makes it a logical candidate for non-waiting distributed computing. For example, we replicated the large airline data set 8 times to create a data set with about one billion observations. We also added a variable "Late" to indicate which flights were at least fifteen minutes late. To find the probability of a late flight by day of week, we perform the following logistic regression:

	job2 <- rxLogit(Late ~ DayOfWeek, data = "AirlineData87to08Rep8.xdf")

This immediately returns control back to our R Console, and we can do some other things while this 1-billion observation logistic regression completes on our distributed computing resources. (Although even with one billion observations, the logistic regression completes in less than a minute.)

We verify that the job is finished and retrieve the results as follows:

	rxGetJobStatus(job2)
	logitResults <- rxGetJobResults(job2)
	summary(logitResults)

We obtain the following results:

	Call:
	rxLogit(formula = Late ~ DayOfWeek, data = "AirlineData87to08Rep8.xdf")

	Logistic Regression Results for: Late ~ DayOfWeek
	File name: C:\data\AirlineData87to08Rep8.xdf
	Dependent variable(s): Late
	Total independent variables: 8 (Including number dropped: 1)
	Number of valid observations: 967579520
	Number of missing observations: 20700232
	-2*LogLikelihood: 947605223.9911 (Residual deviance on 967579513 degrees of freedom)

	Coefficients:
	                      Estimate Std. Error  t value Pr(>|t|)    
	(Intercept)         -1.4691555  0.0002209 -6651.17 2.22e-16 ***
	DayOfWeek=Monday    -0.0083930  0.0003088   -27.18 2.22e-16 ***
	DayOfWeek=Tuesday   -0.0559740  0.0003115  -179.67 2.22e-16 ***
	DayOfWeek=Wednesday  0.0386048  0.0003068   125.82 2.22e-16 ***
	DayOfWeek=Thursday   0.1862203  0.0003006   619.41 2.22e-16 ***
	DayOfWeek=Friday     0.2388796  0.0002985   800.14 2.22e-16 ***
	DayOfWeek=Saturday  -0.1785315  0.0003285  -543.45 2.22e-16 ***
	DayOfWeek=Sunday       Dropped    Dropped  Dropped  Dropped    
	---
	Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

	Condition number of final variance-covariance matrix: 78.6309
	Number of iterations: 2

## Clean up job artifacts using rxGetJobs and rxCleanJobs (distributed computing)

Normally, whenever a waiting job completes or whenever you call `rxGetJobResults` to obtain the results of a non-waiting job, any artifacts created during the distributed computation are automatically removed. (This is controlled by the `autoCleanup` flag to the compute context constructor, which defaults to *TRUE*.) 

However, if a waiting job fails to complete for some reason, or you do not collect all the results from your non-waiting jobs, you may begin to accumulate artifacts on your distributed computing resources. Eventually, this could fill the storage space on these resources, causing system slowdown or malfunction. It is therefore a best practice to make sure you clean up your distributed computing resources from time to time. One way to do this is to simply use standard operating system tools to delete files from the various shared and working directories you specified in your compute context objects. But **RevoScaleR** also supplies a number of tools to help you remove any accumulated artifacts.

The first of these, `rxGetJobs`, allows you to get a list of all the jobs associated with a given compute context. By default, it matches just the head node (if available) and shared directory specified in the compute context; if you re-use these two specifications, ALL the jobs associated with that head node and shared directory are returned:

	myJobs <- rxGetJobs(myNoWaitCluster)

To restrict the matching to only those jobs associated with that specific compute context, specify *exactMatch=TRUE* when calling `rxGetJobs`.

	myJobs <- rxGetJobs(myNoWaitCluster, exactMatch=TRUE)

To obtain the jobs from a specified range of times, use the *startTime* and *endTime* arguments. For example, to obtain a list of jobs for a particular day, you could use something like the following:

	myJobs <- rxGetJobs(myNoWaitCluster,
		startTime=as.POSIXct("2013/01/16 0:00"),
		endTime=as.POSIXct("2013/01/16 23:59"))

Once you’ve obtained the list of jobs, you can try to clean them up using `rxCleanupJobs`:

	rxCleanupJobs(myJobs)

If any of the jobs is in a "finished" state, `rxCleanupJobs` will not clean up that job but instead warn you that the job is finished and that you can access the results with `rxGetJobResults`. This helps prevent data loss. You can, however, force the cleanup by specifying *force=TRUE* in the call to `rxCleanupJobs`:

	rxCleanupJobs(myJobs, force=TRUE)

You can also use `rxCleanupJobs` to clean up individual jobs:

	rxCleanupJobs(job1)


