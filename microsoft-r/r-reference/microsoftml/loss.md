--- 

# required metadata 
title: "loss functions function (MicrosoftML) " 
description: " The loss functions for classification and regression. " 
keywords: "(MicrosoftML), loss functions, expLoss, hingeLoss, logLoss, poissonLoss, smoothHingeLoss, squaredLoss, loss" 
author: "dphansen" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
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

--- 










 # loss functions: Classification and Regression Loss functions 
 ## Description

The loss functions for classification and regression.


 ## Usage

```   
  expLoss(beta = 1, ...)

  hingeLoss(margin = 1, ...)

  logLoss(...)

  smoothHingeLoss(smoothingConst = 1, ...)

  poissonLoss(...)

  squaredLoss(...)

```

 ## Arguments



 ### `beta`
 Specifies the numeric value of beta (dilation). The default value  is 1. 



 ### `margin`
 Specifies the numeric margin value. The default value is 1. 



 ### `smoothingConst`
 Specifies the numeric value of the smoothing constant. The default value is 1. 



 ### ` ...`
 hidden argument. 



 ## Details

A loss function measures the discrepancy between the prediction
of a machine learning algorithm and the supervised output and represents the
cost of being wrong. 

The classification loss functions supported are:


* 
 `logLoss` 

* 
 `expLoss` 

* 
 `hingeLoss` 

* 
 `smoothHingeLoss`


The regression loss functions supported are:


* 
 `poissonLoss` 

* 
 `squaredLoss`.




 ## Value

A character string defining the loss function.

 ## Author(s)

Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)



 ## See Also

[rxFastLinear](rxFastLinear.md), [rxNeuralNet](rxNeuralNet.md)

 ## Examples

 ```

  train <- function(lossFunction) {

      result <- rxFastLinear(isCase ~ age + parity + education + spontaneous + induced,
                    transforms = list(isCase = case == 1), lossFunction = lossFunction,
                    data = infert,
                    type = "binary")
      coef(result)[["age"]]
  }

  age <- list()
  age$LogLoss <- train(logLoss())
  age$LogLossHinge <- train(hingeLoss())
  age$LogLossSmoothHinge <- train(smoothHingeLoss())
  age
```



