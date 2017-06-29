--- 
 
# required metadata 
title: " Get Path to Package Repositories " 
description: " `getRevoRepos` returns the path to either the Revolution Analytics CRAN mirror or the versioned package source repository maintained by Microsoft. " 
keywords: ", getRevoRepos,  ~kwd1 ,  ~kwd2 " 
author: "richcalaway"
ms.author: "richcala" 
manager: "jhubbard" 
ms.date: "03/27/2017" 
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
 
 
 
 #getRevoRepos:  Get Path to Revolution Analytics Package Repositories  
 ##Description
 
`getRevoRepos` returns the path to either the Revolution Analytics CRAN mirror or the
versioned package source repository maintained by Revolution Analytics.
 
 
 ##Usage

```   
  getRevoRepos(version = paste(unlist(unclass(getRversion()))[1:2], collapse = "."), CRANmirror = FALSE)
 
```
 
 ##Arguments

   
    
 ### version
  A string or floating-point literal representing an R version from 2.9 or later. (2.10 is a valid version, but the versioned package source repository does not exist for this version of R.) This argument is ignored if `CRANmirror=TRUE`.  
  
    
 ### CRANmirror
  A logical flag. If `TRUE`, the path to the Revolution Analytics CRAN mirror is returned.  
  
 
 
 ##Value
 
A character string containing the path to the versioned package source repository
or the Revolution Analytics CRAN mirror.
 
 
 ##Author(s)
 
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
 
 
 
 ##Examples

 ```
   
    getRevoRepos(CRANmirror=TRUE)
 
```
 
 
 
