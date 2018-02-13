--- 
 
# required metadata 
title: "rx_privacy_control: Changes the opt-in state for anonymous usage collection (revoscalepy)" 
description: "Used for opting out of telemetry data collection, which is enabled by default." 
keywords: "privacy, telemetry" 
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

# rx_privacy_control


 


## Usage



```
revoscalepy.rx_privacy_control(enable: bool = None)
```





## Description

Used for opting out of telemetry data collection, which is enabled by default.


## Arguments


### opt_in

A bool value that specifies whether to opt in (True) or out (False) of anonymous data usage collection.
If not specified, the value is returned.


## Example



```
import revoscalepy
revoscalepy.rx_privacy_control(False)
```

