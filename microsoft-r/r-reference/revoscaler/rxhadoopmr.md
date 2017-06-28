--- 
 
# required metadata 
title: "Generate Hadoop Map Reduce Compute Context" 
description: " Creates a compute context for use with a Hadoop cluster. " 
keywords: "RevoScaleR, RxHadoopMR, IO" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
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
 
 
 #`RxHadoopMR`: Generate Hadoop Map Reduce Compute Context

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Creates a compute context for use with a Hadoop cluster.
 
 
 ##Usage

```   
  RxHadoopMR(object, 
                hdfsShareDir = paste( "/user/RevoShare", Sys.info()[["user"]], sep="/" ),
                shareDir = paste( "/var/RevoShare", Sys.info()[["user"]], sep="/" ),
                clientShareDir = rxGetDefaultTmpDirByOS(),
                hadoopRPath = rxGetOption("unixRPath"),
                hadoopSwitches = "",
                revoPath = rxGetOption("unixRPath"),
                sshUsername = Sys.info()[["user"]],
                sshHostname = NULL,
                sshSwitches = "",
                sshProfileScript = NULL,
                sshClientDir = "",
                usingRunAsUserMode = FALSE,
                nameNode = rxGetOption("hdfsHost"),
                jobTrackerURL = NULL,
                port = rxGetOption("hdfsPort"),
                onClusterNode = NULL,
                wait = TRUE,
                consoleOutput = FALSE,
                showOutputWhileWaiting = TRUE,
                autoCleanup = TRUE,
                workingDir = NULL,
                dataPath = NULL,
                outDataPath = NULL,
                fileSystem = NULL,
                packagesToLoad = NULL,
                resultsTimeout = 15,
                ...  )
 
```
 
 
 ##Arguments

   
  
    
 ### `object`
 object of class RxHadoopMR. This argument is optional. If supplied, the values of  the other specified arguments are used to replace those of `object` and the modified object is returned. 
  
  
    
 ### `hdfsShareDir`
 character string specifying the file sharing location within HDFS. You must  have permissions to read and write to this location. 
  
   
    
 ### `shareDir`
 character string specifying the directory on the master (perhaps edge) node that is  shared among all the nodes of the cluster and any client host. You must have permissions to read and write  in this directory.  
  
   
    
 ### `clientShareDir`
 character string specifying the absolute path of the temporary directory on the client.  Defaults to /tmp for POSIX-compliant non-Windows clients. For Windows and non-compliant POSIX clients,  defaults to the value of the TEMP environment variable if defined, else to the TMP environment variable  if defined, else to `NULL`. If the default directory does not exist, defaults to NULL.  UNC paths ("`\\host\dir`") are not supported. 
  
   
    
 ### `hadoopRPath`
 character string specifying the path to the directory on the cluster  compute nodes containing the files R.exe and Rterm.exe.  The invocation of R on each  node must be identical.  
  
  
    
 ### `revoPath`
 character string specifying the path to the directory on the master (perhaps edge) node  containing the files R.exe and Rterm.exe.   
  
  
    
 ### `hadoopSwitches`
 character string specifying optional generic Hadoop command line switches, for example `-conf myconf.xml`. See [`http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html`](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html)  for details on the Hadoop command line generic options.  
  
  
    
 ### `sshUsername`
 character string specifying the username for making an ssh connection to the Hadoop cluster. This is not needed if you are running your R client directly on the cluster. Defaults to the username of the user running the R client (that is, the value of `Sys.info()[["user"]]`). 
  
  
    
 ### `sshHostname`
 character string specifying the hostname or IP address of the Hadoop cluster  node or edge node that the client will log into for launching Hadoop jobs and for copying files  between the client machine and the Hadoop cluster. Defaults to the hostname of the machine running  the R client (that is, the value of `Sys.info()[["nodename"]]`)  This field is only used if  `onClusterNode` is `NULL` or `FALSE`.  If you are using PuTTY on a Windows system, this can be the name of a saved PuTTY session that can include the user name and authentication file to use.  
  
  
    
 ### `sshSwitches`
 character string specifying any switches needed for making an ssh connection to the Hadoop cluster. This is not needed if one is running one's R client directly on the cluster. 
  
  
    
 ### `sshProfileScript`
 Optional character string specifying the absolute path to a profile script that will exists on the sshHostname host.  This is used when the target ssh host does not automatically read in a .bash_profile, .profile or other shell environment configuration file for the definition of requisite variables such as HADOOP_STREAMING. 
  
  
    
 ### `sshClientDir`
 character string specifying the Windows directory where Cygwin's ssh.exe  and scp.exe or PuTTY's plink.exe and pscp.exe executables can be found. Needed only for Windows.  Not needed if these executables are on the Windows Path or if Cygwin's location can be found in  the Windows Registry. Defaults to the empty string. 
  
  
    
 ### `usingRunAsUserMode`
 logical scalar specifying whether run-as-user mode is being used on the Hadoop cluster. When using run-as-user mode, local R processes started by the map-reduce framework will run as the same user that started the job, and will have any allocated local permissions.  When not using run-as-user mode (the default for many Hadoop systems), local R processes will run as user mapred.  Note that when running as user mapred, permissions for files and directories will have to be more open in order to allow hand-offs between the user and mapred.  Run-as-user mode is controlled for the Hadoop map-reduce framework by the xml setting,  `mapred.task.tracker.task-controller`, in the `mapred-site.xml` configuration file.  If it is set to the value `org.apache.hadoop.mapred.LinuxTaskController`, then  run-as-user mode is in use.  If it is set to the value `org.apache.hadoop.mapred.DefaultTaskController`, then  run-as-user mode is not in use. 
  
  
    
 ### `nameNode`
 character string specifying the Hadoop name node hostname or IP address. Typically you can leave this at its default value. If set to a value other than "default" or the empty string (see below), this must be an address that can be resolved by the data nodes and used by them to contact the  name node. Depending on your cluster, it may need to be set to a private network address  such as `"master.local"`. If set to the empty string, "", then the master process will set  this to the name of the node on which it is running, as returned by `Sys.info()[["nodename"]]`.  This is likely to work when the sshHostname points to the name node or the sshHostname is not  specified and the R client is running on the name node. Defaults to rxGetOption("hdfsHost"). 
  
  
    
 ### `jobTrackerURL`
 character scalar specifying the full URL for the jobtracker web interface. This is used only for the purpose of loading the job tracker web page from the `rxLaunchClusterJobManager` convenience function.  It is never used for job control, and its specification in the compute context is completely optional.  See the [rxLaunchClusterJobManager](rxlaunchclustertaskmanager.md) page for more information. 
  
  
    
 ### `port`
 numeric scalar specifying the port used by the name node for hadoop jobs.  Needs to be able to be cast to an integer. Defaults to rxGetOption("hdfsPort"). 
  
  
    
 ### `onClusterNode`
 logical scalar or NULL specifying whether the user is initiating the job from a client that will connect to either an edge node or an actual cluster node, directly from either an edge node or node within the cluster.  If set to  `FALSE` or `NULL`, then `sshHostname` must be a valid host. 
  
  
    
 ### `wait`
 logical value.  If `TRUE`, the job will be blocking   and will not return until it has completed or has failed. If `FALSE`,   the job will be non-blocking return immediately,  allowing you to continue running other R code. The object `rxgLastPendingJob` is created with the job information. You can pass this object to the   [rxGetJobStatus](rxgetjobresults.md) function to check on the processing status of the job.  [rxWaitForJob](rxwaitforjob.md) will change a non-waiting job  to a waiting job. Conversely, pressing ESC changes a waiting job to a non-waiting job, provided that the HPC scheduler has accepted the job. If you press ESC before the job has been accepted, the job is canceled. 
  
  
    
 ### `consoleOutput`
 logical scalar. If `TRUE`, causes the standard output  of the R processes to be printed to the user console.  
  
  
    
 ### `showOutputWhileWaiting`
 logical scalar. If `TRUE`, causes the standard output  of the remote primary R and hadoop job process to be printed to the user console while waiting for (blocking on)  a job. 
  
  
    
 ### `autoCleanup`
 logical scalar. If `TRUE`, the default behavior is to clean up the  temporary computational artifacts and delete the result objects upon retrival.  If `FALSE`,  then the computational results are not deleted, and the results may be acquired using  [rxGetJobResults](rxgetjobresults.md), and the output via [rxGetJobOutput](rxgetjoboutput.md) until the  [rxCleanupJobs](rxcleanup.md) is used to delete the results and other artifacts. Leaving this flag set to `FALSE` can result in accumulation of compute artifacts which you may eventually need to delete before they fill up your hard drive. 
  
  
    
 ### `workingDir`
 character string specifying a working directory for the processes  on the master node. 
  
  
    
 ### `dataPath`
 NOT YET IMPLEMENTED. character vector defining the search path(s) for the data source(s). 
  
  
    
 ### `outDataPath`
 NOT YET IMPLEMENTED. `NULL` or character vector defining the search path(s) for   new output data file(s).  If not `NULL`, this overrides any specification for `outDataPath` in [rxOptions](rxoptions.md)  
   
  
    
 ### `fileSystem`
 `NULL` or an [RxHdfsFileSystem](rxhdfsfilesystem.md) to use as the default file system for data sources when created when this compute context is active. 
  
  
    
 ### `packagesToLoad`
 optional character vector specifying additional packages to be  loaded on the nodes when jobs are run in this compute context.  
  
  
    
 ### `resultsTimeout`
 A numeric value indicating for how long attempts should be made  to retrieve results from the cluster.  Under normal conditions, results are available immediately.   However, under certain high load conditions, the processes on the nodes have reported as completed, but the results have not been fully committed to disk by the operating system.  Increase this parameter if results retrievial is failing on high load clusters. 
  
    
 ### ` ...`
 additional arguments to be passed directly to the Microsoft R Services Compute Engine. 
  
  
 
 
 
 ##Details
 
This compute context is supported for Cloudera (CDH4 and CDH5), Hortonworks (HDP 1.3 and 2.x), and MapR (3.0.2, 3.0.3,
3.1.0, and 3.1.1) Hadoop distributions on Red Hat Enterprise Linux 5 and 6.
 
 
 
 ##Value
 
object of class RxHadoopMR.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxGetJobStatus](rxgetjobresults.md),
[rxGetJobOutput](rxgetjoboutput.md),
[rxGetJobResults](rxgetjobresults.md), 
[rxCleanupJobs](rxcleanup.md),
[RxSpark](rxspark.md),
[RxInSqlServer](rxinsqlserver.md),  
[RxInTeradata](rxinteradata.md), 
[RxComputeContext](rxcomputecontext.md),
[rxSetComputeContext](rxsetcomputecontext.md),
[RxHadoopMR-class](rxhadoopmr-class.md).
   
 
 ##Examples

 ```
   
  ## Not run:
 
##############################################################################
# Run hadoop on edge node
##############################################################################

hadoopCC <- RxHadoopMR() 

##############################################################################    
# Run hadoop from a Windows client
# (requires Cygwin and/or PuTTY)
##############################################################################
mySshUsername <- "user1"
mySshHostname <- "12.345.678.90"  #public facing cluster IP address
mySshSwitches <- "-i /home/yourName/user1.pem" #use .ppk file with PuTTY
myShareDir <- paste("/var/RevoShare", mySshUsername, sep ="/")
myHdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
myHadoopCluster <- RxHadoopMR(
hdfsShareDir = myHdfsShareDir,
shareDir = myShareDir,
sshUsername = mySshUsername,
sshHostname = mySshHostname,
sshSwitches = mySshSwitches)
  
    
 ## End(Not run) 
  
 
```
 
 
 
