--- 
 
# required metadata 
title: "remoteLoginAAD function (mrsdeploy) " 
description: " Authenticates the user and creates a remote R session. " 
keywords: "(mrsdeploy), remoteLoginAAD" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/18/2017" 
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
 
 
 
 
 #remoteLoginAAD: Remote login to Microsoft R Server using Azure Active directory. 
 ##Description
 
Authenticates the user and creates a remote R session.
 
 
 ##Usage

```   
  remoteLoginAAD(deployr_endpoint, authuri = NULL, tenantid = NULL,
    clientid = NULL, resource = NULL, session = TRUE, diff = TRUE,
    commandline = TRUE, prompt = "REMOTE> ", username = NULL,
    password = NULL)
 
```
 
 ##Arguments

   
  
 ### `deployr_endpoint`
 The Microsoft R Server HTTP/HTTPS endpoint, including the port number. 
  
  
  
 ### `authuri`
 The URI of the authentication service for Azure Active Directory. 
  
  
  
 ### `tenantid`
 The tenant ID of the Azure Active Directory account being used to authenticate. 
  
  
  
 ### `clientid`
 the client ID of the Application for the Azure Active Directory account. 
  
  
  
 ### `resource`
 The resource ID of the Application for the Azure Active Directory account. 
  
  
  
 ### `session`
 If `TRUE`,  create a remote session. 
  
  
  
 ### `diff`
 If `TRUE`, creates a 'diff' report showing differences between the local and remote sessions. Parameter is only valid if `session` parameter is `TRUE`. 
  
  
  
 ### `commandline`
 If `TRUE`,  creates a "REMOTE' command line in the R console. Parameter is only valid if `session` parameter is `TRUE`. 
  
  
  
 ### `prompt`
 The command prompt to be used for the remote session 
  
  
  
 ### `username`
 if `NULL`. the user will be prompted to enter username and password 
  
  
  
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
 
remoteLoginAAD("http://localhost:12800",
                   authuri = "https://login.windows.net",
                   tenantid = "myMRSServer.contoso.com",
                   clientid = "00000000-0000-0000-0000-000000000000",
                   resource = "00000000-0000-0000-0000-000000000000",
                   session=TRUE,
                   diff=TRUE,
                   commandline=TRUE)
 ## End(Not run) 
  
 
```
 
