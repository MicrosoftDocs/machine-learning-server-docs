---

# required metadata
title: "Manage threads in RevoScaleR (Machine Learning Server) "
description: ""
keywords: "How ScaleR establishes and manages thread pools for parallel processing."
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

# Managing threads in RevoScaleR

RevoScaleR provides functions for managing the thread pool used for parallel execution.

On Windows, thread pool management is enabled and should not be turned off. 

On Linux, thread pool management is turned off by default to avoid interferring with how Unix systems fork processes. Process forking is only available on Unix-based systems. You can turn the thread pool on if you do not fork your R process. RevoScaleR provides an interface to activate the thread pool:

	rxSetEnableThreadPool(TRUE)

Similarly, the thread pool may be disabled as follows:

	rxSetEnableThreadPool(FALSE)

If you want to ensure that the RevoScaleR thread pool is always enabled on Linux, you can add the preceding command to a *.First* function defined in either your own Rprofile startup file, or the system Rprofile.site file. For example, you can add the following lines after the closing right parenthesis of the existing Rprofile.site file:

	.First <- function()
	{
		.First.sys()
		invisible(rxSetEnableThreadPool(TRUE))
	}

The *.First.sys* function is normally run after all other initialization is complete, including the evaluation of the *.First* function. We need the call to *rxSetEnableThreadPool* to occur after RevoScaleR is loaded. That is done by *.First.sys*, so we call *.First.sys* first.


## See Also

+ [Machine Learning Server](../what-is-machine-learning-server.md)
+ [How-to guides in Machine Learing Server](how-to-introduction.md)
+ [RevoScaleR Functions](~/r-reference/revoscaler/revoscaler.md)
