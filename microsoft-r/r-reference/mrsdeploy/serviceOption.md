--- 

# required metadata 
title: "serviceOption function (mrsdeploy) | Microsoft Docs" 
description: " Retrieve, set, and list the different service options. " 
keywords: "(mrsdeploy), serviceOption" 
author: "dphansen" 
manager: "cgronlun" 
ms.date: 07/15/2019
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




 # serviceOption: Retrieve, set, and list the different service options. 
 ## Description

Retrieve, set, and list the different service options.


 ## Usage

```   
  serviceOption()

```

 ## Arguments



 ### `name`
 The option name. 



 ### `value`
 The option value to be set. 



 ## Details

Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)



 ## Value

A list of the current service options.

 ## See Also

Other service methods: [deleteService](deleteService.md),
[getService](getService.md), [listServices](listServices.md),
[print.serviceDetails](print.serviceDetails.md),
[publishService](publishService.md),
[summary.serviceDetails](summary.serviceDetails.md),
[updateService](updateService.md)

 ## Examples

 ```

  ## Not run:


# --- set option name|value
opts <- serviceOption()
opts$set('base_dir', getwd())

# --- get option
opts <- serviceOption()
base_dir <- opts$get('base_dir')

# --- list options
options <- serviceOption()$get()
 ## End(Not run) 
```

