--- 
 
# required metadata 
title: "BatchResponse,api,completed_item_count,execution_id,total_item_count: " 
description: "" 
keywords: "" 
author: "Microsoft" 
manager: "Microsoft" 
ms.date: "09/05/2017" 
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

# BatchResponse



```
azureml.deploy.server.service.BatchResponse(api, execution_id, response,
    output_schema)
```




Create a new Response Object by service name and raw service metadata.



```
api
```




Gets the api endpoint.



```
completed_item_count
```




Gets the number of completed batch results processed thus far.



```
execution_id
```




Gets this batch’s *execution id* if currently started, otherwise *None*.



```
total_item_count
```




Gets the total number of batch results processed in any state.
