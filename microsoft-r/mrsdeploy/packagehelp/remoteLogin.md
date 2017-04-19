--- 
 
# required metadata 
title: "Remote login to the Microsoft R Server" 
description: " Authenticates the user and creates a remote R session. " 
keywords: "mrsdeploy, remoteLogin" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 
 
 #`remoteLogin`: Remote login to the Microsoft R Server

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Authenticates the user and creates a remote R session.
 
 
 ##Usage

```   
  remoteLogin(deployr_endpoint, session = TRUE, diff = TRUE,
    commandline = TRUE, prompt = "REMOTE> ", username = NULL,
    password = NULL)
 
```
 
 ##Arguments

   
  
 ### `deployr_endpoint`
 The Microsoft R Server HTTP/HTTPS endpoint, including the port number. 
  
  
  
 ### `session`
 If `TRUE`,  create a remote session. 
  
  
  
 ### `diff`
 If `TRUE`, creates a 'diff' report showing differences between the local and remote sessions. Parameter is only valid if `session` parameter is `TRUE`. 
  
  
  
 ### `commandline`
 If `TRUE`,  creates a "REMOTE' command line in the R console. 'REMOTE' command line is used to interact with the remote R session.  Parameter is only  valid if `session` parameter is `TRUE`. 
  
  
  
 ### `prompt`
 The command prompt to be used for the remote session 
  
  
  
 ### `username`
 if `NULL`, the user will be prompted to enter username and password 
  
  
  
 ### `password`
 if `NULL`, the user will be prompted to enter username and password 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##See Also
 
[remoteLogout](remoteLogout.md)

[remoteCommandLine](remoteCommandLine.md)
   
 ##Examples

 ```
   
  ## Not run:
 
remoteLogin("https://localhost:1280", session=TRUE, diff=TRUE, commandline=TRUE)
 ## End(Not run) 
  
 
```
 
 
