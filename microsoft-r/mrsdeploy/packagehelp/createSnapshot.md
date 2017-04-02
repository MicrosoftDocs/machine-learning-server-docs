--- 
 
# required metadata 
title: "Create a snapshot of the remote session (workspace and working directory)." 
description: " Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)  " 
keywords: "mrsdeploy, createSnapshot" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 
 
 #`createSnapshot`: Create a snapshot of the remote session (workspace and working directory).

 Applies to version 1.0 of package mrsdeploy.
 
 ##Description
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Usage

```   
  createSnapshot(name)
 
```
 
 ##Arguments

   
  
 ### `name`
 Name describing the snapshot to be saved. 
  
 
 
 ##Details
 
Creates a snapshot of the remote session and saves it on the server. Both the workpace
and the files in the working directory are saved.
 
 
 ##Value
 
`snapshot_id` if successful.
 
 ##See Also
 
[deleteSnapshot](deleteSnapshot.md)

[listSnapshots](listSnapshots.md)

[loadSnapshot](loadSnapshot.md)

[downloadSnapshot](downloadSnapshot.md)
   
 ##Examples

 ```
   
  ## Not run:
 
snapshot_id<-createSnapshot()
 ## End(Not run) 
  
 
```
 
 
