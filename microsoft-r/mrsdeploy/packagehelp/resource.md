--- 
 
# required metadata 
title: "A factory which creates a resource object that lets you interact with RESTful server-side data sources." 
description: " The intention is to provide high-level behaviors without the need to interact with the low level HTTP. This allows you to easily perform CRUD operations (create, read, update, delete) on server-side data. " 
keywords: "mrsdeploy, resource" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 
 
 #`resource`: A factory which creates a resource object that lets you interact with RESTful  server-side data sources.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
The intention is to provide high-level behaviors without the need to interact 
with the low level HTTP. This allows you to easily perform CRUD operations 
`(create, read, update, delete)` on server-side data.
 
 
 ##Usage

```   
  resource(url, param_defaults = list(), actions = NULL)
 
```
 
 ##Arguments

   
  
 ### `url`
 A parameterized URL template with parameters prefixed by `:`  for example `/services/:name/:version` 
  
  
  
 ### `param_defaults`
 Default values for url parameters. 
  
  
  
 ### `actions`
 A list with declaration of custom actions that will be  available in addition to the default set of resource actions (`get`,  `query`, `save`, `update`, `delete`). If a custom action has the same  key as a default action (e.g. `save`), then the default action will  be overwritten, and not extended. 
  
 
 
 ##Examples

 ```
   
  ## Not run:
 
# Create a User resource and get a user's first name:
User <- resource('/user/:id')
first_name <- User$get(list(id = 12345), fn = function(user) {
   return(user$first)
})
 ## End(Not run) 
  
 
```
 
 
