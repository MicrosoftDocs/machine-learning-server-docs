--- 
 
# required metadata 
title: "Wrapper function for remote script execution." 
description: " A simple wrapper function for executing a remote R script. " 
keywords: "mrsdeploy, remoteScript" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 
 
 #`remoteScript`: Wrapper function for remote script execution.

 Applies to version 1.0 of package mrsdeploy.
 
 ##Description
 
A simple wrapper function for executing a remote R script.
 
 
 ##Usage

```   
  remoteScript(name, inputs = NULL, outputs = NULL, displayPlots = TRUE,
    writePlots = TRUE)
 
```
 
 ##Arguments

   
  
 ### `name`
 The R Script file to be executed. 
  
  
  
 ### `inputs`
 JSON encoded string of R objects that are loaded into the Remote R session's workspace prior to excution.  Only R objects of type: primitives, vectors and dataframes are supported via this parameter.  Alternatively the [putLocalObject](putLocalObject.md) can be used, prior to a call to this function, to move any R object from the local workspace into the  remote R session. 
  
  
  
 ### `outputs`
 Character vector of the names of the objects to retreive.  Only primitives, vectors and dataframes can be retrieved using this function.  Use [getRemoteObject](getRemoteObject.md)to get any type of R object from the remote session. 
  
  
  
 ### `displayPlots`
 If `TRUE`, plots generated during execution are displayed in the local plot window. **NOTE** This capability requires that the '`png`' package is installed on the local machine. 
  
  
  
 ### `writePlots`
 If `TRUE`, plots generated during execution are copied to the working directory of the local session. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
A list containing the results of the execution.
 
 ##See Also
 
[remoteExecute](remoteExecute.md)
   
 ##Examples

 ```
   
  ## Not run:
 
resp<-remoteExecute("myScript.R")
resp<-remoteScript("scaleRtest.R", displayPlots = FALSE, writePlots = FALSE)
 
#specify 2 inputs to the script and retrieve 2 outputs.  The supported types for inputs and 
#outputs are: primitives, vectors and dataframes.
data<-'[{"name":"x","value":120}, {"name":"y","value":130}]'
resp<-remoteExecute("c:/myScript.R", inputs=data, outputs=c("out1", "out2"), writePlots=TRUE)

#alternative using 4 calls
x<-120;y<-130
putLocalObject(c("x","y"))
resp<-remoteExecute("c:/myScript.R", script=TRUE)
getRemoteObject(c("out1", "out2"))
 ## End(Not run) 
  
 
```
 
 
