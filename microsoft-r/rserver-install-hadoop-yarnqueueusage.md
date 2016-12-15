---
# required metadata
title: "Enforcing YARN Queue Usage (Microsoft R Server for Hadoop)"
description: "Enforce YARN queue usage for a  Microsoft R Server installation on a Hadoop cluster."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "12/14/2016"
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
# Enforcing YARN queue usage

R Server tasks running on Spark or MapReduce can be managed through use of YARN job queues. To direct a job to a specific queue, the end user must include the queue name in the MapReduce or Spark Compute Context.

## MapReduce
Use the "hadoopSwitches" option to direct jobs to a specific YARN queue.
````
RxHadoopMR(.., hadoopSwitches='-Dmapreduce.job.queuename=mrsjobs')
````
## Spark
Use the "extraSparkConfig" option to direct jobs to a specific YARN queue.
````
RxSpark(.., extraSparkConfig='-conf spark.yarn.queue=mrsjobs')

RxSparkConnect(..,
     extraSparkConfig='-conf spark.yarn.queue=mrsjobs')
````

## Overrides

Use of a specific queue can be enforced by the Hadoop system administrator by providing an installation override to the `RxSpark()`, `RxSparkConnect()`, and `RxHadoopMR()` compute context functions. A benefit is that you no longer have to explicitly specify the queue.  

This procedure involves creating a custom R package which contains the function overrides, installing that package on the nodes in use by end-users, and adding the package to the default search path on these nodes. The following code block provides an example. If you use this code as a template, remember to change the ‘mrsjobs’ YARN queue name to the queue name that's valid for your system.

1. Create a new R package, such as “abcMods” if your company abbreviation is ‘abc’, by installing the “devtools” package and running the following command, or use the `package.skeleton()` function in base R.  To learn more about creating R packages, see [Writing R Extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html) on the CRAN website, or this [online version of R Packages](http://r-pkgs.had.co.nz/) from Hadley Wickham..

  ~~~~
  > library(devtools)
  > create('/dev/abcMods',rstudio=FALSE)
   ~~~~

2. This will create the essential package files in the requested directory, in this case ‘/dev/abcMods’. Edit each of the following to fill in the relevant info.

  DESCRIPTION – text file containing the description of the R package:

  ~~~~
  Package: abcMods
  Date: 2016-09-01
  Title: Company ABC Modified Functions
  Depends: RevoScaleR
  Description: R Server functions modified for use by Company ABC
  License: file LICENSE
  ~~~~

  NAMESPACE – text file containing the list of overridden functions to be exported:

  ~~~~
  export("RxHadoopMR", "RxSpark", “RxSparkConnect”)
  ~~~~

  LICENSE – create a text file named ‘LICENSE’ in the package directory with a single line for the license associated with the R package:

  ~~~~
  This package is for internal Company ABC use only -- not for redistribution.
  ~~~~

3. In the package’s R directory add one or more `*.R` files with the code for the functions to be overridden. The following provides sample code for overriding `RxHadoopMR`, `RxSpark`, and `RxSparkConnect` that you might save to a file called "ccOverrides.r" in that directory.

  ~~~~
    # sample code to enforce use of YARN queues for RxHadoopMR, RxSpark,
    # and RxSparkConnect

  RxHadoopMR <- function(...) {
      dotargs <- list(...)
      hswitch <- dotargs$hadoopSwitches

      # remove any queue info that may already present
      y <- hswitch
      if (isTRUE(grep('queue',hswitch) > 0)) {
        hswitch <- gsub(' *= *','=',hswitch,perl=TRUE)
        hswitch <- gsub(' +',' ',hswitch)
        x <- unlist(strsplit(hswitch," "))
        i <- grep('queue',x)
        y <- paste(x[- i],collapse=' ')
      } 	

    # add in the required queue info
    dotargs$hadoopSwitches <- paste(y,'-Dmapreduce.job.queuename=mrsjobs')
    do.call( RevoScaleR::RxHadoopMR, dotargs )
  }

  RxSpark <- function(...) {
      dotargs <- list(...)
      hswitch <- dotargs$extraSparkConfig

      # remove any queue info that may already present
      y <- hswitch
      if (isTRUE(grep('queue',hswitch) > 0)) {
        hswitch <- gsub(' *= *','=',hswitch,perl=TRUE)
        hswitch <- gsub(' +',' ',hswitch)
        x <- unlist(strsplit(hswitch," "))
        i <- grep('queue',x)
        y <- paste(x[- i],collapse=' ')
      }

      # add in the required queue info
      dotargs$extraSparkConfig <- paste(y,'-conf spark.yarn.queue=mrsjobs')
      do.call( RevoScaleR::RxSpark, dotargs )
  } }

  RxSparkConnect <- function(...) {
      dotargs <- list(...)
      hswitch <- dotargs$extraSparkConfig

      # remove any queue info that may already present
      y <- hswitch
      if (isTRUE(grep('queue',hswitch) > 0)) {
        hswitch <- gsub(' *= *','=',hswitch,perl=TRUE)
        hswitch <- gsub(' +',' ',hswitch)
        x <- unlist(strsplit(hswitch," "))
        i <- grep('queue',x)
        y <- paste(x[- i],collapse=' ')
      }

      # add in the required queue info
      dotargs$extraSparkConfig <- paste(y,'-conf spark.yarn.queue=mrsjobs')
      do.call( RevoScaleR::RxSparkConnect, dotargs )
  }
~~~~

4. After editing the above components of the package, run the following Linux commands to build the package from the directory containing the **abcMods** directory:

  ~~~~
  R CMD build abcMods
  R CMD INSTALL abcmods_0.1-0    (if needed run this using sudo)
  ~~~~

  If you need to build the package for users on Windows then the equivalent commands would be as follows where ‘rpath’ is defined to point to the version of R Server to which you’d like to add the library.  

  ~~~~
  set rpath="C:\Program Files\Microsoft\R Client\R_SERVER\bin\x64\R.exe"
  %rpath% CMD build abcMods
  %rpath% CMD INSTALL abcMods_0.1-0.tar.gz
  ~~~~

5. To test it, start R, load the library, and make a call to `RxHadoopMR()`:

  ~~~~
  > library(abcMods)
  > RxHadoopMR(hadoopSwitches="-Dmapreduce.job.queuename=XYZ")
  ~~~~

  You should see the result come back with the queue name set to your override value (for example, `Dmapreduce.job.queuename=mrsjobs)`.

6. To automate the loading of the package so that users don’t need to specify "library(abcMods)", edit Rprofile.site and modify the line specifying the default packages to include **abcMods** as the last item:

  ~~~~
  options(defaultPackages=c(getOption("defaultPackages"), "rpart", "lattice", "RevoScaleR", "RevoMods", "RevoUtils", "RevoUtilsMath", "abcMods"))
  ~~~~

7. Once everything tests out to your satisfaction, install the package on all the edge nodes that your users will be logging into. To do this, copy "Rprofile.site" and R’s library/abcMods directory to each of these nodes, or install the package from the abcmods_0.1-0 tar file on each node and manually edit the "Rprofile.site" file on each node.    
