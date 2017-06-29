--- 
 
# required metadata 
title: "Display the REMOTE command prompt." 
description: " Displays the REMOTE command prompt and provides a remote execution context.  All R commands entered at the R console will be executed in the remote R session. " 
keywords: "mrsdeploy, remoteCommandLine" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
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
 
 
 
 
 #`remoteCommandLine`: Display the 'REMOTE>' command prompt.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Displays the 'REMOTE>' command prompt and provides a remote execution context.  All R commands
entered at the R console will be executed in the remote R session.
 
 
 ##Usage

```   
  remoteCommandLine(prompt = "REMOTE> ", displayPlots = TRUE,
    writePlots = FALSE, recPlots = TRUE)
 
```
 
 ##Arguments

   
  
 ### `prompt`
 The command prompt to be shown when in 'REMOTE' mode. 
  
  
  
 ### `displayPlots`
 If `TRUE`, plots generated during execution are displayed in the local plot window. **NOTE** This capability requires that the '`png`' package is installed on the local machine. 
  
  
  
 ### `writePlots`
 If `TRUE`, plots generated during execution are copied to the working directory of the local session. 
  
  
  
 ### `recPlots`
 If `TRUE`, plots will be created using the '`recordPlot`' function in R. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)


Once the 'REMOTE>' command prompt is shown, these commands become available:
`pause()` #when entered, switches back to the local R session command prompt and local execution context.  
`exit` #exit and logout from the remote session.
 
 
 ##See Also
 
[resume](resume.md)
   
 ##Examples

 ```
   
  ## Not run:
 
remoteCommandLine()

#switch from REMOTE command prompt back to the local command prompt.
REMOTE>pause()

#switch from local command prompt back to the REMOTE command prompt.
>resume()

#exit and logout from the remote session.
REMOTE>exit
 ## End(Not run) 
  
 
```
 
 
