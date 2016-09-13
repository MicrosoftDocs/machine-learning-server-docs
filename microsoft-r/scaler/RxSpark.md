---

# required metadata
title: "RxSpark function (ScaleR)"
description: "ScaleR Functions: RxSqlServerData"
keywords: "RevoScaleR, ScaleR, RxSpark"
author: "HeidiSteen"
manager: "paulettm"
ms.date: "06/23/2016"
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

#RxSpark

Creates a compute context for use with a Spark cluster.

This function is new in RevoScaleR package version 8.0.5. See [Supported platforms](./rserver-install-supported-platforms.md) for supported versions of Apache Spark.

## Usage
~~~~
RxSpark(
object,
hdfsShareDir = paste( "/user/RevoShare", Sys.info()[["user"]], sep="/" ),
shareDir = paste( "/var/RevoShare", Sys.info()[["user"]], sep="/" ),
clientShareDir = rxGetDefaultTmpDirByOS(),
sshUsername = Sys.info()[["user"]],
sshHostname = NULL,
sshSwitches = "",
sshProfileScript = NULL,
sshClientDir = "",
nameNode = rxGetOption("hdfsHost"),
jobTrackerURL = NULL,
port = rxGetOption("hdfsPort"),
onClusterNode = NULL,
wait = TRUE,
numExecutors = 65535,
executorCores = 2,
executorMem = "4g",
driverMem = "4g",
executorOverheadMem = "4g",
extraSparkConfig = "",
persistentRun = FALSE,
idleTimeout = 3600,
suppressWarning = TRUE,
consoleOutput = FALSE,
showOutputWhileWaiting = TRUE,
autoCleanup = TRUE,
workingDir = NULL,
dataPath = NULL,
outDataPath = NULL,
fileSystem = NULL,
packagesToLoad = NULL,
resultsTimeout = 15,
...)
~~~~

## Arguments

The following table shows the arguments to RxSpark in order and their default values.

|Parameter | Description|
| --------- | --------- |
|object| Object of class RxSpark. This argument is optional. If supplied, the values of the other specified arguments are used to replace those of ‘object’ and the modified object is returned.|
|hdfsShareDir| Character string specifying the file sharing location within HDFS. You must have permissions to read and write to this location.|
|shareDir|Character string specifying the directory on the master (perhaps edge) node that is shared among all the nodes of the cluster and any client host. You must have permissions to read and write in this directory.|
|clientShareDir|Character string specifying the absolute path of the temporary directory on the client. Defaults to /tmp for POSIX-compliant non-Windows clients. For Windows and non-compliant POSIX clients, defaults to the value of the TEMP environment variable if defined, else to the TMP environment variable if defined, else to ‘NULL’. If the default directory does not exist, defaults to NULL. UNC paths ("‘\\host\dir’") are not supported.|
|sshUsername|Character string specifying the username for making an ssh connection to the Spark cluster. This is not needed if you are running your R client directly on the cluster. Defaults to the username of the user running the R client (that is, the value of ‘Sys.info()[["user"]]’).|
|sshHostname|Character string specifying the hostname or IP address of the Spark cluster node or edge node that the client will log into for launching Spark jobs and for copying files between the client machine and the Spark cluster. Defaults to the hostname of the machine running the R client (that is, the value of ‘Sys.info()[["nodename"]]’) This field is only used if ‘onClusterNode’ is ‘NULL’ or ‘FALSE’.|
|sshSwitches|Character string specifying any switches needed for making an ssh connection to the Spark cluster. This is not needed if one is running one's R client directly on the cluster.|
|sshProfileScript|Optional character string specifying the absolute path to a profile script on the sshHostname host. This is used when the target ssh host does not automatically read in a .bash_profile, .profile or other shell environment configuration file for the definition of requisite variables.|
|sshClientDir|Character string specifying the Windows directory where Cygwin's ssh.exe and scp.exe or PuTTY's plink.exe and pscp.exe executables can be found. Needed only for Windows. Not needed if these executables are on the Windows Path or if Cygwin's location can be found in the Windows Registry. Defaults to the empty string.|
|nameNode|Character string specifying the Spark name node hostname or IP address. Typically you can leave this at its default value. If set to a value other than "default" or the empty string (see below), this must be an address that can be resolved by the data nodes and used by them to contact the name node. Depending on your cluster, it may need to be set to a private network address such as ‘"master.local"’. If set to the empty string, "", then the master process will set this to the name of the node on which it is running, as returned by ‘Sys.info()[["nodename"]]’. This is likely to work when the sshHostname points to the name node or the sshHostname is not specified and the R client is running on the name node. Defaults to rxGetOption("hdfsHost").|
|jobTrackerURL|Character scalar specifying the full URL for the jobtracker web interface. This is used only for the purpose of loading the job tracker web page from the `rxLaunchClusterJobManager` convenience function. It is never used for job control, and its specification in the compute context is completely optional. See the `rxLaunchClusterJobManager` page for more information.|
|port|Numeric scalar specifying the port used by the name node for HDFS. Needs to be able to be cast to an integer. Defaults to rxGetOption("hdfsPort"). |
|onClusterNode|Logical scalar or NULL specifying whether the user is initiating the job from a client that will connect to either an edge node or an actual cluster node, directly from either an edge node or node within the cluster. If set to ‘FALSE’ or ‘NULL’, then ‘sshHostname’ must be a valid host.|
|wait|Logical scalar. If ‘TRUE’ or if ‘persistentRun’ is ‘TRUE’, the job will be blocking and the invoking function will not return until the job has completed or has failed. Otherwise, the job will be non-blocking and the invoking function will return, allowing you to continue running other R code. The object ‘rxgLastPendingJob’ is created with the job information. You can pass this object to the ‘rxGetJobStatus’ function to check on the processing status of the job. ‘rxWaitForJob’ will change a non-waiting job to a waiting job. Conversely, pressing ESC changes a waiting job to a non-waiting job, provided that the scheduler has accepted the job. If you press ESC before the job has been accepted, the job is canceled.|
|numExecutors|Numeric scalar specifying the number of executors in Spark, which is equivalent to parameter -num-executors in spark-submit app. If not specified, the default behavior is to launch as many executors as possible, which may use up all resources and prevent other users from sharing the cluster.|
|executorCores|Numeric scalar specifying the number of cores per executor, which is equivalent to parameter -executor-cores in spark-submit app.|
|executorMem|Character string specifying memory per executor (e.g., 1000M, 2G), which is equivalent to parameter -executor-memory in spark-submit app.|
|driverMem|Character string specifying memory for driver (e.g., 1000M, 2G), which is equivalent to parameter -driver-memory in spark-submit app.|
|executorOverheadMem|Character string specifying memory overhead per executor (e.g. 1000M, 2G), which is equivalent to setting spark.yarn.executor.memoryOverhead in YARN. Increasing this value will allocate more memory for the R process and the ScaleR engine process in the YARN executors, so it may help resolve job failure or executor lost issues.|
|extraSparkConfig|Character string specifying extra arbitrary Spark properties (e.g., ‘"--conf spark.speculation=true --conf spark.yarn.queue=aQueue"’), which is equivalent to additional parameters passed into spark-submit app.|
|persistentRun|XPERIMENTAL. logical scalar. If ‘TRUE’, the Spark application (and associated processes) will persist across jobs until the idleTimeout is reached or the ‘rxStopEngine’ function is called explicitly. This avoids the overhead of launching a new Spark application for each job. If ‘FALSE’, a new Spark application will be launched when a job starts and will be terminated when the job completes.|
|idleTimeout|Numeric scalar specifying the number of seconds of being idle (i.e., not running any Spark job) before system kills the Spark process. Setting a value greater than 600 is recommended.|
|suppressWarning|Logical scalar. If ‘TRUE’, suppress warning message regarding missing Spark application parameters.|
|consoleOutput|Logical scalar. If ‘TRUE’, causes the standard output of the R processes to be printed to the user console.|
|showOutputWhileWaiting|Logical scalar. If ‘TRUE’, causes the standard output of the remote primary R and Spark job process to be printed to the user console while waiting for (blocking on) a job.|
|autoCleanup|Logical scalar. If ‘TRUE’, the default behavior is to clean up the temporary computational artifacts and delete the result objects upon retrieval.  If ‘FALSE’, then the computational results are not deleted, and the results may be acquired using ‘rxGetJobResults’, and the output via ‘rxGetJobOutput’ until the ‘rxCleanupJobs’ is used to delete the results and other artifacts. Leaving this flag set to ‘FALSE’ can result in accumulation of compute artifacts which you may eventually need to delete before they fill up your hard drive.|
|workingDir|Character string specifying a working directory for the processes on the master node.|
|dataPath|NOT YET IMPLEMENTED. character vector defining the search path(s) for the data source(s).|
|outDataPath|NOT YET IMPLEMENTED. ‘NULL’ or character vector defining the search path(s) for new output data file(s). If not ‘NULL’, this overrides any specification for ‘outDataPath’ in rxOptions’.|
|fileSystem|‘NULL’ or an ‘RxHdfsFileSystem’ to use as the default file system for data sources when created when this compute context is active.|
|packagesToLoad|Optional character vector specifying additional packages to be loaded on the nodes when jobs are run in this compute context.|
|resultsTimeout|A numeric value indicating for how long attempts should be made to retrieve results from the cluster. Under normal conditions, results are available immediately. However, under certain high load conditions, the processes on the nodes have reported as completed, but the results have not been fully committed to disk by the operating system. Increase this parameter if results retrieval is failing on high load clusters.|
|...|Additional arguments to be passed directly to the Microsoft R Services Compute Engine.|

## Return Value
An object of class RxSpark.

## Example

~~~~
        ## Not run:

        ##############################################################################
        # Running client on edge node
        ##############################################################################

        hadoopCC <- RxSpark()

        ##############################################################################
        # Running from a Windows client
        # (requires Cygwin and/or PuTTY)
        ##############################################################################
        mySshUsername <- "user1"
        mySshHostname <- "12.345.678.90"  #public facing cluster IP address
        mySshSwitches <- "-i /home/yourName/user1.pem" #use .ppk file with PuTTY
        myShareDir <- paste("/var/RevoShare", mySshUsername, sep ="/")
        myHdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
        mySparkCluster <- RxSpark(
        hdfsShareDir = myHdfsShareDir,
        shareDir = myShareDir,
        sshUsername = mySshUsername,
        sshHostname = mySshHostname,
        sshSwitches = mySshSwitches)
        ## End(Not run)

~~~~

## See Also
[Comparison of rx Functions and CRAN R Functions](compare-base-r-scaler-functions.md)

[ScaleR Functions for Working with SQL Server Data](functions-for-sql-server-data.md)
