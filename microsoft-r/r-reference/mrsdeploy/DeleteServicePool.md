--- 

# required metadata 
title: "deleteServicePool function (mrsdeploy) | Microsoft Docs" 
description: " Delete an existing dedicated pool for a published web service running on Machine Learning Server. " 
keywords: "(mrsdeploy), deleteServicePool" 
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




 # deleteServicePool: Delete an existing dedicated pool for a published web service 
 ## Description

Delete an existing dedicated pool for a published web service running on 
Machine Learning Server.


 ## Usage

```   
  deleteServicePool(name, version)

```

 ## Arguments



 ### `name`
 A string representing the web service name. To create or update  a dedicated pool, you must provide the specific name and version of the  service that the pool is associated with. 



 ### `version`
 A string representing the web service version. To create or  update a dedicated pool, you must provide the specific name and version of the  service that the pool is associated with. 



 ## See Also

Other dedicated pool methods: [configureServicePool](ConfigureServicePool.md),
[getPoolStatus](GetPoolStatus.md)

 ## Examples

 ```

  ## Not run:


deleteServicePool(
   name = "myService",
   version = "v1"
)
 ## End(Not run) 
```

