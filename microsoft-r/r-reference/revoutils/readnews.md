--- 
 
# required metadata 
title: "Read R or Package NEWS Files" 
description: "Read the R NEWS file or a package's NEWS or NEWS.Rd file." 
keywords: ", readNews, utilities, documentation" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/27/2017" 
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
 
 
 #`readNews`: Read R or Package NEWS Files 
 ##Description
 Read the R NEWS file or a package's NEWS or NEWS.Rd file. 
 
 ##Usage

```   
  readNews(package)
 
```
 
 ##Arguments

   
    
 ### `package`
 a character string specifying the package for which you'd like to read the  news. If missing, the R NEWS is displayed. 
  
 
 
 
 ##Details
 
The R NEWS file is displayed using `RShowDoc` or `file.show`, depending
on whether the system appears capable of launching a browser. Similarly, package NEWS.Rd
files are displayed using `browseURL` or `file.show`.

This function should not be confused with readNEWS in the **tools** package, 
which performs a different kind of reading.
 
 
 ##Value
 
`NULL` is returned invisibly.
 
 
 
 
 
