--- 
 
# required metadata 
title: "Tweedie Generalized Linear Models" 
description: " Produces a dummy generalized linear model family object that can be used with `rxGlm` to fit  Tweedie generalized linear regression models. This does NOT produce a full family object, but gives  `rxGlm` enough information to call a C++ implementation that fits a Tweedie model. The excellent  R package **tweedie** by Gordon Smyth does provide a full Tweedie family object, and that can also be  used with `rxGlm`. " 
keywords: "RevoScaleR, rxTweedie, regression" 
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
 
 
 #`rxTweedie`: Tweedie Generalized Linear Models

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Produces a dummy generalized linear model family object that can be used with `rxGlm` to fit 
Tweedie generalized linear regression models. This does NOT produce a full family object, but gives 
`rxGlm` enough information to call a C++ implementation that fits a Tweedie model. The excellent 
R package **tweedie** by Gordon Smyth does provide a full Tweedie family object, and that can also be 
used with `rxGlm`.
 
 
 ##Usage

```   
  rxTweedie(var.power = 0, link.power = 1 - var.power)
 
```
 
 ##Arguments

   
    
 ### `var.power`
 index of power variance function. 
  
    
 ### `link.power`
 index of power link function. Setting `link.power` to `0`  produces a `log` link function. Setting it to `1` is the identity link.  The default is a canonical link  equal to `1 - var.power` 
  
   
 
 
 ##Details
 
This provides a way to specify the var.power and the link.power arguments to the Tweedie distribution for `rxGlm`.
 
 
       
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##References
 
Peter K Dunn (2011). tweedie: Tweedie exponential family models. R package version 2.1.1.  
 
 
 
 ##See Also
 
[rxGlm](rxglm.md)
   
 
 ##Examples

 ```
   
  # In the claims data, the cost is set to NA if no claim was made
  # Convert NA to 0 for the cost, and read data into a data frame
  
  claimsXdf <- file.path(rxGetOption("sampleDataDir"),"claims.xdf")
  claims <- rxDataStep(inData = claimsXdf, 
             transforms = list(cost = ifelse(is.na(cost), 0, cost)))
    
  # Estimate using a Tweedie family                           
  claimsTweedie <- rxGlm(cost ~ age + car.age + type , 
      data=claims, family = rxTweedie(var.power = 1.5)) 
  summary(claimsTweedie)
  
  # Re-estimate using a Tweedie family setting link.power to 0,
  # resulting in a log link function                          
  claimsTweedie <- rxGlm(cost ~ age + car.age + type , 
      data=claims, family = rxTweedie(var.power = 1.5, link.power = 0)) 
  summary(claimsTweedie)
 
```
 
   
   
