---

# required metadata
title: "Manage threads in RevoScaleR"
description: ""
keywords: "How ScaleR establishes and manages thread pools for parallel processing."
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "10/17/2016"
ms.topic: "get-started-article"
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

# Managing Threads in RevoScaleR

A process is an instance of a software program, running under an operating system. RevoScaleR runs in two separate processes, one running R and R-related code, and one running RevoScaleR’s underlying compute engine.

In order to perform parallel computations, a process must provide blocks of executable code to the operating system that can be run in parallel.

There are two primary approaches to this problem. One approach is to use threads. Threads can be thought of as sub-programs that are still bound to the original process that creates them. They communicate with the creating process, while still being autonomous enough to allow the operating system to schedule their operation independently, thus allowing parallelization.

By way of contrast, forking a process (a technique only available on Unix-based systems) creates a complete copy of one process that differs only by a flag available within each of the resulting processes.

Unfortunately, these two methods of parallelization are not completely compatible unless steps are taken to ensure that they do not interfere with each other and cause the software to malfunction. Specifically, program failures can be caused by forking a Unix process while the objects used to control communication and synchronization between threads are in use.

In order to optimize performance, RevoScaleR is capable of establishing and maintaining a pool of threads. This way, there is no overhead in creating these threads to parallelize a computation. However, given the above issues, this functionality is, by default, disabled on Linux (this is not an issue on Windows, as Windows does not use fork; hence, the thread pool is always instantiated and ready for use on Windows).

If you know that you will not be forking your R process (such as spawning it using nohup or creating multiple R processes with the multicore package), or if you know exactly when you might fork a process, you can turn the thread pool on and off.  RevoScaleR provides an interface to activate the thread pool:

	rxSetEnableThreadPool(TRUE)

Similarly, the thread pool may be disabled as follows:

	rxSetEnableThreadPool(FALSE)

Enabling the thread pool will improve performance, but should not be done if there is any chance you will fork your R process. If you want to ensure that the thread pool is always enabled, you can add the above command to a *.First* function defined in either your own Rprofile startup file, or the system Rprofile.site file. For example, you can add the following lines after the closing right parenthesis of the existing Rprofile.site file:

	.First <- function()
	{
		.First.sys()
		invisible(rxSetEnableThreadPool(TRUE))
	}


The *.First.sys* function is normally run after all other initialization is complete, including the evaluation of the *.First* function. We need the call to *rxSetEnableThreadPool* to occur after RevoScaleR is loaded, and that is done by *.First.sys*, so we call *.First.sys* first.

R has extensive facilities for managing random number generation, and these are all fully supported in RevoScaleR. In addition, RevoScaleR provides an interface to random number generators supplied by the Vector Statistical Library that is part of Intel’s Math Kernel Library. To use one of these generators, call the RevoScaleR function *rxRngNewStream*, specifying the desired generator (the default is a version of the Mersenne-Twister, MT-2203), the desired substream (if applicable), and a seed (if desired). See the help file for a complete list of the available generators and examples of their use.

The VSL random number generators are most useful in a distributed setting, where they allow parallel processes to generate uncorrelated random number streams. See the [RevoScaleR Distributed Computing Guide](../scaler-distributed-computing.md)
for complete details.

RevoScaleR depends on a number of packages that are specified as “default packages” in Microsoft R Server and R Client. To ensure these packages are loaded when using Rscript, you must include the flag *–-default-packages=* (with nothing on the right-hand side), and not use the flag *–-vanilla* when invoking Rscript. In particular, the methods package is required for correct operation of RevoScaleR.

The RevoScaleR package includes comprehensive help for all of its functions and data sets, along with an overview topic describing the package as a whole. To view this overview topic, use the R ? operator at the R prompt as follows:

	?RevoScaleR

To obtain help on a RevoScaleR function, use the R *?* operator at the R prompt together with the function name. For example, the following command displays help about *rxLinMod*:

	?rxLinMod

We will often refer to help topic pages as a function’s help file, as in “see the *rxLinMod* help file for details.” This always means to type the *?* operator followed by the function name.

## See Also

[Introduction to Microsoft R](../microsoft-r-getting-started.md)

[Diving into data analysis in Microsoft R](../data-analysis-in-microsoft-r.md)

[RevoScaleR Functions](../revoscaler.md)
