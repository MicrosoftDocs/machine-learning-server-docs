--- 
 
# required metadata 
title: "Utility Functions for rxDForest" 
description: "     Utility Functions for rxDForest. " 
keywords: "RevoScaleR, rxDForestUtils, rxVarImpPlot, rxLeafSize, rxTreeDepth, rxTreeSize, rxVarUsed, rxGetTree, models, tree, classif, regression, classification" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 
 
 
 
 
 #`rxDForestUtils`: Utility Functions for rxDForest

 Applies to version 9.1.0 of package RevoScaleR.
 
 
 ##Description
 
Utility Functions for rxDForest.
 
 
 ##Usage

```   
  rxVarImpPlot(x, sort = TRUE, n.var = 30, main = deparse(substitute(x)),   ...  )
  rxLeafSize(x, use.weight = TRUE)
  rxTreeDepth(x)
  rxTreeSize(x, terminal = TRUE)
  rxVarUsed(x, by.tree = FALSE, count = TRUE)
  rxGetTree(x, k = 1)	   
 
```
 
 ##Arguments

   
    
 ### `x`
  an object of class [rxDForest](rxdforest.md) or [rxDTree](rxdtree.md). 
  
  
    
 ### `sort`
  logical value. If `TRUE`, the variables will be sorted in decreasing importance. 
  
    
 ### `n.var`
  an integer specifying the number of variables to show when `sort=FALSE`. 
  
    
 ### `main`
  a character string specifying the main title for the plot. 
  
    
 ### ` ...`
  other arguments to be passed on to dotchart. 
  
  
    
 ### `use.weight`
  logical value. If `TRUE`, the leaf size is measured by the total weight of its observations  instead of the total number of its observations. 
  
  
    
 ### `terminal`
  logical value. If `TRUE`, only the terminal nodes will be counted. 
  
  
    
 ### `by.tree`
  logical value. If `TRUE`, the list of variables used will be broken down by trees. 
  
    
 ### `count`
  logical value. If `TRUE`, the frequencies that variables appear in trees will be returned. 
  
  
    
 ### `k`
  an integer specifying the index of the tree to be extracted. 
  
 
 
 ##Value
 


* `rxVarImpPlot` -  plots a dotchart of the variable importance as measured by the decision forest.


* `rxLeafSize` -  returns the size of the terminal nodes in the decision forest.


* `rxTreeDepth` -  returns the depth of trees in the decision forest.


* `rxTreeSize` -  returns the size of trees in terms of the number of nodes in the decision forest.


* `rxVarUsed` -  finds out the variables actually used in the decision forest.


* `rxGetTree` -  extracts a single tree from the decision forest.



 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##References
 
[`randomForest`](http://cran.r-project.org/web/packages/randomForest/index.html)
.
 
 
 ##See Also
 
[rxDForest](rxdforest.md), [rxDTree](rxdtree.md), [rxBTrees](rxbtrees.md).
   
 ##Examples

 ```
   
  set.seed(1234)
  
  # classification
  iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
  iris.dforest <- rxDForest(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width, 
      data = iris[iris.sub, ], importance = TRUE)
      
  rxVarImpPlot(iris.dforest)
  rxTreeSize(iris.dforest)
  rxVarUsed(iris.dforest)
  rxGetTree(iris.dforest)
 
```
 
 
 
 
 
 
