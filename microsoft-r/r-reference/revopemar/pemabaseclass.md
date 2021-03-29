--- 

# required metadata 
title: " Generator for a PemaBaseClass reference class object " 
description: " Generator for a PemaBaseClass reference class object to be used with pemaCompute. " 
keywords: "RevoPemaR, PemaBaseClass,  models " 
author: "dphansen"
ms.author: "davidph"
ms.author: "davidph" 
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


 # PemaBaseClass:  Generator for a PemaBaseClass reference class object 

 Applies to version 8.0.3 of package RevoPemaR.

 ## Description

Generator for a PemaBaseClass reference class object to be used with pemaCompute.


 ## Usage

```   
  PemaBaseClass(...)

```


 ## Arguments



 ###  ...
  Arguments used in the constructor. Not used by the `PemaBaseClass` base class.  



 ## Details

This is a base reference class for the construction of reference classes
to be used for parallel external memory algorithms for use in
[pemaCompute](pemacompute.md). See [PemaMean](pemamean.md) for a simple
example.


 ## Value

A `PemaBaseClass` reference class object.








 ## See Also

setRefClass,
[setPemaClass](setpemaclass.md),
[pemaCompute](pemacompute.md),
[PemaMean](pemamean.md)


 ## Examples

 ```


  # Show help for methods
  PemaBaseClass$help(initialize)
  PemaBaseClass$help(processData)
  PemaBaseClass$help(updateResults)
  PemaBaseClass$help(processResults)
```




