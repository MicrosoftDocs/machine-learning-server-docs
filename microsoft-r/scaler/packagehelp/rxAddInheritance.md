--- 
 
# required metadata 
title: "Add Inheritance to Compatible Objects" 
description: "Add a specific S3 class to the class of a compatible object." 
keywords: "RevoScaleR, rxAddInheritance, rxAddInheritance.default, rxAddInheritance.rxDTree, models, tree, classif, regression" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "04/03/2017" 
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
 
 
 
 
 #`rxAddInheritance`: Add Inheritance to Compatible Objects

 Applies to version 9.0.1 of package RevoScaleR.
 
 ##Description
 
Add a specific S3 class to the `"class"` of a compatible object.
 
 
 ##Usage

```   
  
  rxAddInheritance(x, inheritanceClass, ...)
  
 ## S3 method for class `rxDTree':
rxAddInheritance  (x, inheritanceClass = "rpart",    ...  )
  
 
```
 
 ##Arguments

   
    
 ### `x`
  the object to which inheritance is to be added. Specifically, for `rxAddInheritance.rxDTree`, an object of class `rxDTree`. 
  
    
 ### `inheritanceClass`
  the class to be inherited from. Specifically, for `rxAddInheritance.rxDTree`, the class `"rpart"`. 
  
    
 ### ` ...`
  additional arguments to be passed directly to other methods. 
  
 
 
 
 ##Details
 
The `rxAddInheritance` function is designed to be a general way to add inheritance to S3 objects that are compatible
with the class for which inheritance is to be added. Note, however, that this approach is exactly as dangerous and error-prone
as simply calling `class(x) <- c(class(x), inheritanceClass)`. 

It is expected that classes that are designed to mimic existing R classes, but not rely on them, will have their own
specific `rxAddInheritance` methods to add the R class inheritance on request.

In the case of `rxDTree` objects, these were designed to be compatible with the `"rpart"` class. so adding **rpart**
inheritance is guaranteed to work.
 
 
 ##Value
 
The original object, modified to include `inheritanceClass` as part of its `"class"` attribute. 

In the case of `rxDTree` objects, the object may also be modified to include the `functions` component if it was
previously `NULL`.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[rxDTree](rxDTree.md).
   
 ##Examples

 ```
   
  mtcarTree <- rxDTree(mpg ~ disp, data=mtcars)
  mtcarRpart <- rxAddInheritance(mtcarTree)
  require(rpart)
  printcp(mtcarRpart)
 
```
 
 
 
 
 
