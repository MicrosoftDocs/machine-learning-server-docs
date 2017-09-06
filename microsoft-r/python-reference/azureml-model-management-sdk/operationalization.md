--- 
 
# required metadata 
title: "Operationalization,authentication,delete_service,deploy_realtime,deploy_service,destructor,get_service,initializer,list_services,realtime_service,redeploy_realtime,redeploy_service,service: " 
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

# Operationalization



```
azureml.deploy.operationalization.Operationalization
```




*Operationalization* is designed to be a low-level abstract foundation class
from which other service operationalization attribute classes in the
*mldeploy* package can be derived. It provides a standard template for
creating attribute-based operationalization lifecycle phases providing a
consistent  *__init()__*, *__del()__* sequence that chains initialization
(initializer), authentication (authentication), and destruction (destructor)
methods for the class hierarchy.



```
authentication(context)
```




Authentication lifecycle method. Invokes the authentication entry-point
for the class hierarchy.

An optional _noonp_ method where subclass implementers MAY provide this
method definition by overriding.



```
delete_service(name)
```




Sub-class should override.



```
deploy_realtime(name, **opts)
```




return a new service instance.



```
deploy_service(name, **opts)
```




Sub-class should override.



```
destructor()
```




Destroy lifecycle method. Invokes destructors for the class hierarchy.

An optional _noonp_ method where subclass implementers MAY provide this
method definition by overriding.



```
get_service(name)
```




Retrieve service meta-data from the name source and return an new
service instance.

Sub-class should override.



```
initializer(api_client, config)
```




Init lifecycle method, invoked during construction. Sets up attributes
and invokes initializers for the class hierarchy.

An optional _noonp_ method where subclass implementers MAY provide this
method definition by overriding.

Object with configuration property name/value pairs



```
list_services(name=None, **opts)
```




Sub-class should override.



```
realtime_service(name)
```




Begin fluent API for defining a realtime web service.

**Example:**

>>> client.realtime_service('scoring')
      .description('A new realtime web service')
      .version('v1.0.0')


# Arguments


## name

The web service name.


# Returns

A [`RealtimeDefinition`](realtime-definition.md) for fluent API.



```
redeploy_realtime(name, force=False, **opts)
```




return a new service instance.



```
redeploy_service(name, force=False, **opts)
```




Sub-class should override.



```
service(name)
```




Begin fluent API for defining a web service.

**Example:**

>>> client.service('scoring')
      .description('A new web service')
      .version('v1.0.0')


# Arguments


## name

The web service name.


# Returns

A [`ServiceDefinition`](service-definition.md) for fluent API.
