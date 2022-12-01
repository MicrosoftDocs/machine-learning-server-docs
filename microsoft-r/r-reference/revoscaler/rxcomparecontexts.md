--- 

# required metadata 
title: "rxCompareContexts function (revoAnalytics) | Microsoft Docs" 
description: " Determines if two compute contexts are equivalent. " 
keywords: "(revoAnalytics), rxCompareContexts, IO" 
author: "chuckheinzelman"
ms.author: "charlhe" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "sql-non-specified"
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 
ms.technology: machine-learning-server

--- 


 # rxCompareContexts:  Compare Two Compute Context Objects  
 ## Description

Determines if two compute contexts are equivalent.



 ## Usage

```   
  rxCompareContexts(context1, context2, exactMatch = FALSE)

```


 ## Arguments



 ### `context1`
 The first compute context to be compared. 


 ### `context2`
 The second compute context to be compared. 


 ### `exactMatch`
 Determines if compute contexts are matched simply by the location specified for the compute context, or by all fields in the full compute context. The location is specified by the `headNode` (if available) and `shareDir` parameters. See Details for more information. 




 ## Details

If `exactMatch=FALSE`, only the shared directory `shareDir` and the cluster 
head name `headNode` (if available) are compared.  Otherwise, all slots are compared. However, if the
`nodes` slot in either compute context is `NULL`, that slot is also
omitted from the comparison.  Note also that case is ignored for node name comparisons, and for LSF node lists, 
near matching of node names and partial domain information will be used when comparing node names.


 ## Author(s)

Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



 ## Examples

 ```

  ## Not run:

rxCompareContexts( myContext1, myContext2 )
 ## End(Not run) 
```


