--- 

# required metadata 
title: "summary.serviceDetails function (mrsdeploy) | Microsoft Docs" 
description: " Defines the R summary generic for serviceDetails during a  listServices(). " 
keywords: "(mrsdeploy), summary.serviceDetails" 
author: "dphansen"
ms.author: "davidph" 
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
ms.technology: "" 
ms.custom: "" 

--- 




 # summary.serviceDetails: The summary generic for `serviceDetails`. 
 ## Description

Defines the R summary generic for `serviceDetails` during a 
`listServices()`.


 ## Usage

```   
 ## S3 method for class `serviceDetails':
summary  (o)

```

 ## Arguments



 ### `o`
 The `serviceDetails` list of S3 objects. 



 ## See Also

Other service methods: [deleteService](deleteService.md),
[getService](getService.md), [listServices](listServices.md),
[print.serviceDetails](print.serviceDetails.md),
[publishService](publishService.md),
[serviceOption](serviceOption.md), [updateService](updateService.md)

 ## Examples

 ```

  ## Not run:

# --- print all ---
summary(listServices())
 ## End(Not run) 
```

