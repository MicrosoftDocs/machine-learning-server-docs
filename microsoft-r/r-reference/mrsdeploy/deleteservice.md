--- 

# required metadata 
title: "deleteService function (mrsdeploy) | Microsoft Docs" 
description: " Delete a web service on an R Server instance. " 
keywords: "(mrsdeploy), deleteService" 
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
ms.technology: "r-server" 
ms.custom: "" 

--- 




 # deleteService: Delete a web service.
 ## Description

Delete a web service on an R Server instance.


 ## Usage

```   
  deleteService(name, v)

```

 ## Arguments



 ### `name`
 The web service name. 



 ### `v`
 The web service version. 



 ## Details

Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)



 ## See Also

Other service methods: [getService](getService.md),
[listServices](listServices.md),
[print.serviceDetails](print.serviceDetails.md),
[publishService](publishService.md),
[serviceOption](serviceOption.md),
[summary.serviceDetails](summary.serviceDetails.md),
[updateService](updateService.md)

 ## Examples

 ```

  ## Not run:

# Delete the `add-service` version `1.0.1`
deleteService("add-service", "1.0.1")
 ## End(Not run) 
```

