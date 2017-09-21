--- 
 
# required metadata 
title: "rx_privacy_control: Changes the opt in state for anonymous usage collection" 
description: "rx_privacy_control(opt_in, bool=None)" 
keywords: "privacy, telemetry" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "09/11/2017" 
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

rx_privacy_control(opt_in, bool=None)


## Arguments


### opt_in

a bool value that specifies to opt in True or opt out False with anonymous data usage collection.
If not specified, the value is returned.


## Example



```
import revoscalepy
revoscalepy.rx_privacy_control(False)
```

