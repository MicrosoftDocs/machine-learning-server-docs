--- 
 
# required metadata 
title: "Predicted Values and Residuals for rx_lin_mod, rx_logit, rx_dtree," 
description: "Generic function to compute predicted values and residuals using" 
keywords: "predict" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
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

## ``rx_predict``


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
revoscalepy.rx_predict(model_object=None, data=None, output_data=None, **kwargs)
```




### Description

Generic function to compute predicted values and residuals using
rx_lin_mod, rx_logit, rx_dtree, rx_dforest and rx_btrees objects.

For specific functions, see:
    rx_predict_default for rx_lin_mod and rx_logit objects.
    rx_predict_rx_dtree for rx_dtree object.
    rx_predict_rx_dforest for rx_dforest object.


### Arguments


##### model_object

object returned from a call to rx_lin_mod, rx_logit, rx_dtree,
rx_dforest and rx_btrees. Objects with multiple dependent variables are not
supported.


##### data

a data frame or an RxXdfData data source object to be used for predictions.


##### output_data

an RxXdfData data source object or existing data frame to store predictions.


##### kwargs

additional parameters


### Returns

a data frame or a data source object of prediction results.


### See also

[``rx_predict_default``](rx_predict_default.md),
[``rx_predict_rx_dtree``](rx_predict_rx_dtree.md),
[``rx_predict_rx_dforest``](rx_predict_rx_dforest.md).


### Example



```
import os
from revoscalepy import RxOptions, RxXdfData, rx_lin_mod, rx_predict, rx_data_step

sample_data_path = RxOptions.get_option("sampleDataDir")
mort_ds = RxXdfData(os.path.join(sample_data_path, "mortDefaultSmall.xdf"))
mort_df = rx_data_step(mort_ds)

lin_mod = rx_lin_mod("creditScore ~ yearsEmploy", mort_df)
pred = rx_predict(lin_mod, data = mort_df)
print(pred.head())
```


Output:



```
Data will be read from C:\xadupre\microsoft\bigAnalytics\python\revoscalepy\revoscalepy\data\sample_data\mortDefaultSmall.xdf
Data will be written to C:\Users\xadupre\AppData\Local\Temp\rre414002.xdf File will be overwritten if it exists.

Rows Processed: 100000 
Rows Read: 100000, Total Rows Processed: 100000, Total Chunk Time: 0.002 seconds 
   creditScore_Pred
0        700.089114
1        699.834355
2        699.783403
3        699.681499
4        699.783403
```

