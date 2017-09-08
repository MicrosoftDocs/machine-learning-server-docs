--- 
 
# required metadata 
title: "prune.rxDTree function (RevoScaleR) | Microsoft Docs" 
description: " 	Prune a decision tree created by rxDTree and return the smaller tree. " 
keywords: "(RevoScaleR), prune.rxDTree, models, tree, classif, regression" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
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
 
 
 
 #prune.rxDTree: Pruning an rxDTree Decision Tree 
 ##Description
 
Prune a decision tree created by `rxDTree` and return the smaller tree.
 
 
 ##Usage

```   
  
  prune.rxDTree(tree, cp,   ...  )
      
 
```
 
 ##Arguments

   
    
 ### `tree`
  object returned from a call to `rxDTree`. 
  
    
 ### `cp`
  a complexity parameter specifying the complexity at which to prune the tree. Generally, you should examine the `cptable` component of the `tree` object to determine a suitable value for `cp`. 
  
    
 ### ` ...`
  additional arguments to be passed to other methods. (There are, in fact, no other methods called by `prune.rxDTree`.) 
  
 
 
 
 
 ##Details
 
The `prune.rxDTree` function can be used as a `prune` method for objects of
class `rxDTree`, provided the **rpart** package is attached prior to attaching 
**RevoScaleR**.
 
 
 ##Value
 
an object of class `"rxDTree"` representing the pruned tree. 
It is a list with components similar to those of class `"rpart"` with the following distinctions:


* `where` -  A vector of integers indicating the node to which each point is allocated.  This information is always returned if the data source is a data frame.  If the data source is not a data frame and `outFile` is specified.  i.e., not `NULL`, the node indices are written/appended to  the specified file with a column name as defined by `outColName`.



For other components, see rpart.object for details.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##References
 
Breiman, L., Friedman, J. H., Olshen, R. A. and Stone, C. J. (1984)
*Classification and Regression Trees*.
Wadsworth.

Therneau, T. M. and Atkinson, E. J. (2011)
*An Introduction to Recursive Partitioning Using the RPART Routines*.
[`http://r.789695.n4.nabble.com/attachment/3209029/0/zed.pdf`](http://r.789695.n4.nabble.com/attachment/3209029/0/zed.pdf)
.

Yael Ben-Haim and Elad Tom-Tov (2010)
A streaming parallel decision tree algorithm.
*Journal of Machine Learning Research* **11**, 849--872. 
 
 
 ##See Also
 
rpart, rpart.control, rpart.object.
   
 ##Examples

 ```
   
  claimsData <- file.path(system.file("SampleData", package="RevoScaleR"), "claims.xdf")
  claimsTree <- rxDTree(type ~ cost + number, data=claimsData, minSplit=20)
  claimsTreePruned <- prune.rxDTree(claimsTree, cp=0.04)
 
```
 
 
 
 
 
