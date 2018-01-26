--- 
 
# required metadata 
title: "rx_serialize_model: Serialize a python model (revoscalepy)" 
description: "Serialize the given python model." 
keywords: "serialization" 
author: "HeidiSteen" 
manager: "cgronlun" 
ms.date: "01/26/2018" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# rx_serialize_model


 


## Usage



```
revoscalepy.rx_serialize_model(model, realtime_scoring_only=False) -> bytes
```





## Description

Serialize the given python model.


## Arguments


### model

Supported models
are “rx_logit”, “rx_lin_mod”, “rx_dtree”, “rx_btrees”, “rx_dforest” and MicrosoftML models.


### realtime_scoring_only

Boolean flag indicating if model serialization is for real-time only.
Default to False


## Returns

Bytes of the serialized model.


## Example



```
import os
from revoscalepy import RxOptions, RxXdfData, rx_serialize_model, rx_lin_mod
sample_data_path = RxOptions.get_option("sampleDataDir")
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
linmod = rx_lin_mod("ArrDelay~DayOfWeek", ds)
s_linmod = rx_serialize_model(linmod, realtime_scoring_only = False)
```

