--- 
 
# required metadata 
title: "OAuth2 Token request" 
description: " Method to create parameter list to be used for getting an OAuth2 token. " 
keywords: "RevoScaleR, rxOAuthParameters, file, connection" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 #rxOAuthParameters: OAuth2 Token request

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Method to create parameter list to be used for getting an OAuth2 token.
 
 
 ##Usage

```   
      rxOAuthParameters(authUri = NULL, tenantId = NULL, clientId = NULL, resource = NULL, username = NULL, password = NULL, authToken= NULL, useWindowsAuth = FALSE)
  	
 
```
 
 ##Arguments

   
  
    
 ### authUri
 Optional string containing OAuth Authentication URI - default NULL  
   
    
 ### tenantId
 Optional string containing OAuth Tenant ID - default NULL  
  
    
 ### clientId
 Optional string containing OAuth ClientID - default NULL  
  
    
 ### resource
 Optional string containing OAuth Resource  - default NULL  
  
    
 ### username
 Optional string containing OAuth Username - default NULL  
  
    
 ### password
 Optional string containing OAuth Password - default NULL  
  
    
 ### authToken
 Optional string containing a valid OAuth token to be used for WebHdfs requests - default NULL  
  
    
 ### useWindowsAuth
 Optional Flag indicating if Windows Authentication should be used for obtaining the OAuth token (applicable only on Windows) - default NULL  
  
  
 
 
 ##Details
 
Reading from HDFS file system via WebHdfs can only be done by first obtaining a OAuth2 token. This function
allows the specification of parameters that can be set to retreive a token.
 
 
 
 ##Value
 
An rxOAuthParameters list object. This object may be used in
[RxHdfsFileSystem](rxhdfsfilesystem.md) to set the OAuth request method for WebHdfs usage.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[RxHdfsFileSystem](rxhdfsfilesystem.md)
   
 ##Examples

 ```
   
  # Setup to run analyses to use HDFS with access via WebHdfs and OAuth2
  ## Not run:
 
oAuth <- rxOAuthParameters(authUri = "https://login.windows.net/",
            tenantId = "mytest.onmicrosoft.com",
            clientId = "872cd9fa-d31f-45e0-9eab-6e460a02d1e2", 
            resource = "https://KonaCompute.net/", 
            username = "me@mytest.onmicrosoft.com", 
            password = "password")

myHdfsFileSystem <- RxHdfsFileSystem(hostName = "myHost", port = 443, useWebHdfs = TRUE, oAuthParameters = oAuth)
rxSetFileSystem(fileSystem = myHdfsFileSystem )
 ## End(Not run) 
  
 
```
 
 
 
