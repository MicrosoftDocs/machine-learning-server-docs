--- 
 
# required metadata 
title: "Predicted Values and Residuals for rx_lin_mod, rx_logit, rx_dtree," 
description: "Generic function to compute predicted values and residuals using" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
<<<<<<< HEAD
ms.date: "06/26/2017" 
=======
ms.date: "06/27/2017" 
>>>>>>> heidist-revoscalepy
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

<<<<<<< HEAD
# rx_predict
=======
## ``rx_predict``: Score using a Microsoft ML Machine Learning model


### Usage
>>>>>>> heidist-revoscalepy



```
revoscalepy.functions.RxPredict.rx_predict(model_object=None, data=None, output_data=None, **kwargs)
```




<<<<<<< HEAD
## Description
=======
### Description
>>>>>>> heidist-revoscalepy

Generic function to compute predicted values and residuals using
rx_lin_mod, rx_logit, rx_dtree, rx_dforest and rx_btrees objects.

For specific functions, see:
    rx_predict_default for rx_lin_mod and rx_logit objects.
    rx_predict_rx_dtree for rx_dtree object.
    rx_predict_rx_dforest for rx_dforest object.


<<<<<<< HEAD
## Parameters


### model_object
=======
### Arguments


##### model_object
>>>>>>> heidist-revoscalepy

object returned from a call to rx_lin_mod, rx_logit, rx_dtree,
rx_dforest and rx_btrees. Objects with multiple dependent variables are not
supported.


<<<<<<< HEAD
### data
=======
##### data
>>>>>>> heidist-revoscalepy

a data frame or an RxXdfData data source object to be used for predictions.


<<<<<<< HEAD
### output_data
=======
##### output_data
>>>>>>> heidist-revoscalepy

an RxXdfData data source object or existing data frame to store predictions.


<<<<<<< HEAD
### kwargs
=======
##### kwargs
>>>>>>> heidist-revoscalepy

additional parameters


<<<<<<< HEAD
## Returns
=======
### Returns
>>>>>>> heidist-revoscalepy

a data frame or a data source object of prediction results.


<<<<<<< HEAD
## Author
=======
### Author
>>>>>>> heidist-revoscalepy

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


<<<<<<< HEAD
## See also


## Example
=======
### See also


### Example
>>>>>>> heidist-revoscalepy



```
import os
from revoscalepy import RxOptions, RxXdfData, rx_lin_mod, rx_predict, rx_data_step

sample_data_path = RxOptions.get_option("sampleDataDir")
mort_ds = RxXdfData(os.path.join(sample_data_path, "mortDefaultSmall.xdf"))
mort_df = rx_data_step(mort_ds)

lin_mod = rx_lin_mod("creditScore ~ yearsEmploy", mort_df)
pred = rx_predict(lin_mod, data = mort_df)
<<<<<<< HEAD
pred.head()
=======
print(pred.head())
```


Output:



```
   creditScore_Pred
0        700.089114
1        699.834355
2        699.783403
3        699.681499
4        699.783403
>>>>>>> heidist-revoscalepy
```

