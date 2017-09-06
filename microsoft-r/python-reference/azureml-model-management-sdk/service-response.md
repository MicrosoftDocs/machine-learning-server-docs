--- 
 
# required metadata 
title: "ServiceResponse,api,artifact,artifacts,console_output,error,output,outputs,raw_outputs: " 
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

# ServiceResponse



```
azureml.deploy.server.service.ServiceResponse(api, response, output_schema)
```




Create a new Response Object by service name and raw service metadata.



```
api
```




Gets the api endpoint.



```
artifact(artifact_name, decode=True, encoding=None)
```




A convenience function to look up a file artifact by name and optionally
base64 decode it.


# Arguments


## artifact_name

The name of the file artifact.


## decode

Whether to decode the Base64 encoded artifact string.


## encoding


# Returns

The file artifact as a Base64 encoded string if *decode=False*
otherwise the decoded string.



```
artifacts
```




Gets the response outputs if present.



```
console_output
```




Gets the console output if present.



```
error
```




Gets the error if present.



```
output(output)
```




    A convenience function to look up a output values by name.


# Arguments


## output


# Returns

The service output.



```
outputs
```




Gets the response outputs if present.



```
raw_outputs
```




Gets the raw response outputs if present.
