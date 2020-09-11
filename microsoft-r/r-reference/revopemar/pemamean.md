--- 

# required metadata 
title: " Generator for an PemaMean reference class object " 
description: " Generator for an PemaMean reference class object, inheriting from PemaBaseClass, to be used with pemaCompute. " 
keywords: "RevoPemaR, PemaMean,  models " 
Author: "dphansen"
ms.author: "richcala" 
manager: "cgronlun" 
ms.date: "03/23/2017" 
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
#ms.technology: "" 
#ms.custom: "" 

--- 


 # PemaMean:  Generator for an PemaMean reference class object 

 Applies to version 8.0.3 of package RevoPemaR.

 ## Description

Generator for an PemaMean reference class object, inheriting from PemaBaseClass, to be used with pemaCompute.


 ## Usage

```   
  PemaMean(...)

```


 ## Arguments




 ###  ...
  Arguments used in the constructor. See Details section.  



 ## Details

This is a simple example of a parallel external memory algorithm for use with
[pemaCompute](pemacompute.md). The `varName`argument for the constructor is a
character string containing the name of the variable in the data set to use
for computations.


 ## Value

An `PemaMean` reference class object.








 ## See Also

setRefClass,
[setPemaClass](setpemaclass.md),
[PemaBaseClass](pemabaseclass.md)

 ## Examples

 ```


  # Show help for methods
  PemaMean$help(initialize)
  PemaMean$help(processData)
  PemaMean$help(updateResults)
  PemaMean$help(processResults)
```




