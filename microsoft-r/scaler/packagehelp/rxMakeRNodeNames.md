--- 
 
# required metadata 
title: " Converts valid computer names into valid R variable names. " 
description: " Converts valid computer names into valid R variable names.  Should only be used when you want to guarantee that host  names are usable as variable names. " 
keywords: "RevoScaleR, rxMakeRNodeNames, IO" 
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
 
 
 #`rxMakeRNodeNames`:  Converts valid computer names into valid R variable names. 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Converts valid computer names into valid R variable names.  Should only be used when you want to guarantee that host 
names are usable as variable names.
 
 
 
 ##Usage

```   
  rxMakeRNodeNames( nodeNames )
 
```
 
 
 ##Arguments

   
  
 ### `nodeNames`
 character vector of node names to be converted. 
  
 
 
 
 ##Details
 
rxMakeRNodeNames will preform the following transformations on each element of the character vector passed:


1 
 Perform `toupper` on the name.

1 
 Remove all white space from the name.

1 
 If one exists, removes the first dot and all characters following it from the name.  (This has the effect of stripping 
the domain name off, if one exists.)

1 
 Changes any '-' (dash) characters to '_' (underscore) characters so that node names used as variables do not have to be quoted.



The names returned by this function are valid R names, that is, symbols, but they may no longer be valid computer node names. Do **not**
use these names, or this function, in any context where the name may be used to query or control the actual computer; use the original computer
name for that.  This function is intended to be used only to generate R variable names for processing or storing distributed computing results
from the associated computer. Note also that once a host name has been converted into a guaranteed acceptable R variable name, 
it is impossible to guarantee the reverse conversion.
 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##Examples

 ```
   
  ## Not run:
 
rxMakeRNodeNames(rxGetNodes(myCluster))
rxMakeRNodeNames( c("cluster-head","worker1.foo.edu") )
 ## End(Not run) 
  
 
```
 
 
