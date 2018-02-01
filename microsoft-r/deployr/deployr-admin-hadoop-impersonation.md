---

# required metadata
title: "Using Hadoop Impersonation - DeployR 8.x "
description: "DeployR and Hadoop Impersonation"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "11/10/2017"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Using Hadoop Impersonation and DeployR

**Applies to: DeployR 8.x**   (See [comparison between 8.x and 9.x](../whats-new-in-r-server.md#8vs9))

>Looking to deploy with Machine Learning Server? [Start here](../what-is-operationalization.md).

Typically when invoking system commands from R, those commands run as the user that started R. In the case of Hadoop, if user `abc` logs in to a node on a Hadoop cluster and starts R, any *system* commands, such as `system("hadoop fs -ls /")`, run as user `abc`. File permissions in HDFS are honored accordingly. For example, user `abc` cannot access files in HDFS if that user does not have proper permissions.

However when using DeployR, every R session is started as the same user. This is an artifact of the Rserve component that was used to create and interact with R sessions. In order to work around this circumstance, [Hadoop Impersonation](https://issues.apache.org/jira/browse/HADOOP-8561) is used. Hadoop impersonation is employed by standard Hadoop services like Hue and WebHDFS.

The idea behind impersonation is that R sets an environment variable that tells Hadoop to run commands as a different user than the user who started the R session. You can use one of two Hadoop-supported environment variables:

-   `HADOOP_USER_NAME` for non-kerberos secured clusters
-   `HADOOP_PROXY_USER` for clusters secured with Kerberos.

This document assumes we are working with a Kerberos secured cluster.

The rest of the document focuses on the steps needed to get Hadoop Impersonation to work with DeployR.

## Creating the 'rserve' User

Since the DeployR Rserve component runs by default as the `apache` user who **does not** have a `home` directory, we recommend that you create a new user that:

-   Is used when running Rserve
-   Has a `home` directory
-   Starts a bash shell

In this example, we create the user `rserve` and change which user used to run Rserve.

**Non Root Installation**

If DeployR was installed as non-root, then you must ensure that the user that starts DeployR has a `home` directory and starts a bash shell. Nothing else is required.


**Root Installation**

If DeployR was installed as user `root`, do the following:

1.  Create the Linux user `rserve`.

2.  Go to the directory where DeployR is installed. For example:

        cd /opt/deploy/<version>

3.  Go the `rserve` subdirectory and open `rserve.sh` in an editor. For example:

        vi /opt/deploy/<version>/rserve/rserve.sh

4.  Replace all instances of `apache` with `rserve`. 

    >There are two instances of the string `apache` in the file.

5.  Give full write permissions to directory `workdir`. For example:

        chmod 777 /opt/deployr/<version>/rserve/workdir

6.  Edit `/etc/group` and add `rserve` as a member of group `apache`. For example:

        apache:x:502:rserve

7.  Restart Rserve.

        cd /opt/deploy/<version>/rserve
        ./rserve.sh stop
        ./rserve.sh start


## Setting Up the Environment for the 'rserve' User


+ **ScaleR**: If you are using ScaleR inside Hadoop, add the following line to `.bash_profile` for user `rserve`. This ensures all environment variables needed by ScaleR are set properly.
  ```NA
  . ./etc/profile.d/Revolution/bash_profile_additions
  ```
  
+ **Kerberos**: If your Hadoop cluster is secured using Kerberos, obtain a Kerberos ticket for principal `hdfs`. This ticket acts as the proxy for all other users.

  Be sure you are the Linux user `rserve` when obtaining the ticket. For example:
  ```NA
  su - rserve
  kinit hdfs
  ```

>We recommend that you use a `cron` job or equivalent to renew this ticket periodically to keep it from expiring.

## Testing the Environment

1.  Create a private file in HDFS. In the following example, user `revo` owns the file:

        -rw-------   3 revo revo   14304763 2014-06-26 15:53 /share/AirlineDemoSmall/AirlineDemoSmall.csv

2.  Create a sample RScript that tries to access the file. For example:

        #in this case we read the file stored in HDFS into a dataframe

        filename<-"/share/AirlineDemoSmall/AirlineDemoSmall.csv"
        df<-read.table(pipe(paste('hadoop fs -cat ', filename, sep="")), sep=",", header=TRUE)
        summary(df)

    When you run this program from the R console as user `rserve`, it works because principal `hdfs` has a valid Kerberos ticket for the Linux user `rserve`.

3.  Modify the script to use the `HADOOP_PROXY_USER` environment variable. For example, add `Sys.setenv(HADOOP_PROXY_USER='rserve')`:

        #now we will run the script through the proxy

        filename<-"/share/AirlineDemoSmall/AirlineDemoSmall.csv"
        Sys.setenv(HADOOP_PROXY_USER='rserve')
        df<-read.table(pipe(paste('hadoop fs -cat ', filename, sep="")), sep=",", header=TRUE)
        summary(df)

    Unlike the previous step, when you run the script now, it will fail. This is due to the fact that the principal `hdfs` is no longer acting as a *superuser* for the command. Instead, it is acting as proxy for the user `rserve`. This is a subtle but important difference.

        > #now we will run the script through the proxy
        >
        > filename<-"/share/AirlineDemoSmall/AirlineDemoSmall.csv"
        > Sys.setenv(HADOOP_PROXY_USER='rserve')
        > df<-read.table(pipe(paste('hadoop fs -cat ', filename, sep="")), sep=",", header=TRUE)
        cat: Permission denied: user=rserve, access=READ, inode="/share/AirlineDemoSmall/AirlineDemoSmall.csv":revo:revo:-rw-------
        Error in read.table(pipe(paste("hadoop fs -cat ", filename, sep = "")),  :
         no lines available in input
        > summary(df)
        Error in object[[i]] : object of type 'closure' is not subsettable

4.  RScript for DeployR - We don't want to leave line of code that sets the HADOOP\_PROXY\_USER in our R Script. So we remove it and revert back to our original script. In addition, we pass the filename into the script from our DeployR client application. So, the script simply becomes:

        df<-read.table(pipe(paste('hadoop fs -cat ', filename, sep="")), sep=",", header=TRUE)
        summary(df)

We can now upload the script to DeployR using the Repository Manager. In our example, user `testuser` creates a directory called `demo` in the DeployR Repository Manager and name our RScript `piperead.R`.

<a name="create-deployr-client"></a>
## Creating a DeployR Client

Create a DeployR client application to control which user is running the script.

This example code is written in Visual Basic using the DeployR .NET client library. An equivalent example could be written in C\#, Java or JavaScript.

    Imports DeployR

    Module Module1

        Sub Main()

            hdfsRead(False, "revo", "/share/AirlineDemoSmall/AirlineDemoSmall.csv", "piperead.R")

        End Sub

        Sub hdfsRead(ByVal bDebug As Boolean, _
                     ByVal sProxyUser As String, _
                     ByVal sHDFSfile As String, _
                     ByVal sRscript As String)


            Dim client As RClient
            Dim user As RUser
            Dim project As RProject
            Dim execution As RProjectExecution
            Dim options As New ProjectExecutionOptions
            Dim sEnv As String

            Try

                'set debug mode if we need it
                RClientFactory.setDebugMode(bDebug)

                'create an deployR client
                client = RClientFactory.createClient("http://localhost:7300/deployr", 10)

                'login
                Dim basic As New RBasicAuthentication("testuser", "changeme")
                user = client.login(basic, False)

                'create a temporary project
                project = user.createProject()

                'set the Environment Variable
                sEnv = "Sys.setenv(HADOOP_PROXY_USER='" + sProxyUser + "')"
                project.executeCode(sEnv)

                'collect the inputs
                options.rinputs.Add(RDataFactory.createString("filename", sHDFSfile))

                'execute the script
                execution = project.executeScript(sRscript, "demo", "testuser", "", options)

                'write the R console output
                Console.WriteLine(execution.about.console)

            Catch ex As Exception

                'print exception
                Console.WriteLine("Exception:  " + ex.Message)

                If (TypeOf ex Is HTTPRestException) Then
                    Dim hex As HTTPRestException = DirectCast(ex, HTTPRestException)
                    'print R console text, if available
                    Console.WriteLine("Console:  " + hex.console)
                End If

            Finally

                'close the project
                project.close()

                'logout
                client.logout(user)

            End Try
        End Sub
    End Module

## Extending the Example

We can now extend this example to include two more RScripts for execution.

**Second RScript**

This script demonstrates how to copy a file from HDFS into the working directory of the R session. This RScript is uploaded to the DeployR repository as `copy_to_workspace.R`, in directory `demo` for user `testuser`.

    #copy a file from HDFS into the working directory

    system(paste('hadoop fs -copyToLocal', filename,  getwd(), sep=" "))
    df<-read.table(basename(filename), sep=",", header=TRUE)
    summary(df)

**Third RScript**

This script runs a ScaleR algorithm as a MapReduce job in Hadoop. This RScript is uploaded to the DeployR repository as `scaleR_hadoop.R`, in directory `demo` for user `testuser`.

    #run an rxSummary on 'filename'

    myNameNode <- "default"
    myPort <- 0

    myHadoopCluster <- RxHadoopMR(consoleOutput=TRUE, autoCleanup=TRUE, nameNode=myNameNode, port=myPort)
    rxSetComputeContext(myHadoopCluster)

    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    colInfo <- list(DayOfWeek = list(type = "factor",
          levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
          "Friday", "Saturday", "Sunday")))

    airDS <- RxTextData(file = filename, missingValueString = "M",
          colInfo  = colInfo, fileSystem = hdfsFS)

    adsSummary <- rxSummary(~ArrDelay+CRSDepTime+DayOfWeek, data = airDS)
    print(adsSummary)

**Update VB.NET code that executes all three examples**

This is an update of the [code shown here](#create-deployr-client) that now reads in and executes all three scripts.

    Imports DeployR

    Module Module1

        Sub Main()

            hdfsRead(False, "revo", "/share/AirlineDemoSmall/AirlineDemoSmall.csv", "piperead.R")
            hdfsRead(False, "revo", "/share/AirlineDemoSmall/AirlineDemoSmall.csv", "copy_to_workspace.R")
            hdfsRead(False, "revo", "/share/AirlineDemoSmall/AirlineDemoSmall.csv", "scaleR_hadoop.R")

        End Sub

        Sub hdfsRead(ByVal bDebug As Boolean, _
                     ByVal sProxyUser As String, _
                     ByVal sHDFSfile As String, _
                     ByVal sRscript As String)


            Dim client As RClient
            Dim user As RUser
            Dim project As RProject
            Dim execution As RProjectExecution
            Dim options As New ProjectExecutionOptions
            Dim sEnv As String

            Try

                'set debug mode if we need it
                RClientFactory.setDebugMode(bDebug)

                'create an deployR client
                client = RClientFactory.createClient("http://localhost:7300/deployr", 10)

                'login
                Dim basic As New RBasicAuthentication("testuser", "changeme")
                user = client.login(basic, False)

                'create a temporary project
                project = user.createProject()

                'set the Environment Variable
                sEnv = "Sys.setenv(HADOOP_PROXY_USER='" + sProxyUser + "')"
                project.executeCode(sEnv)

                'collect the inputs
                options.rinputs.Add(RDataFactory.createString("filename", sHDFSfile))

                'execute the script
                execution = project.executeScript(sRscript, "demo", "testuser", "", options)

                'write the R console output
                Console.WriteLine(execution.about.console)

            Catch ex As Exception

                'print exception
                Console.WriteLine("Exception:  " + ex.Message)

                If (TypeOf ex Is HTTPRestException) Then
                    Dim hex As HTTPRestException = DirectCast(ex, HTTPRestException)
                    'print R console text, if available
                    Console.WriteLine("Console:  " + hex.console)
                End If

            Finally

                'close the project
                project.close()

                'logout
                client.logout(user)

            End Try
        End Sub
    End Module
