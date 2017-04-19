--- 
 
# required metadata 
title: "Download a snapshot from the R server." 
description: " Downloads the specified snapshot from the R server in zip format. " 
keywords: "mrsdeploy, downloadSnapshot" 
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
 
 
 
 
 #`downloadSnapshot`: Download a snapshot from the R server.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Downloads the specified snapshot from the R server in zip format.
 
 
 ##Usage

```   
  downloadSnapshot(snapshot_id, file = NULL)
 
```
 
 ##Arguments

   
  
 ### `snapshot_id`
 Identifier of the snapshot to load. 
  
  
  
 ### `file`
 Name of a file to write the contents of the snapshot.  If `NULL`, (the default), then the raw vector of bytes will be returned. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
raw vector of bytes if `file=NULL` if successful.  If `file` not equal `NULL`,
then `TRUE` will be returned.
 
 ##See Also
 
[createSnapshot](createSnapshot.md)

[deleteSnapshot](deleteSnapshot.md)

[listSnapshots](listSnapshots.md)

[loadSnapshot](loadSnapshot.md)
   
 ##Examples

 ```
   
  ## Not run:
 
downloadSnapshot("8ce7eb47-3aeb-4c5a-b0a5-a2025f07d9cd")
downloadSnapshot("8ce7eb47-3aeb-4c5a-b0a5-a2025f07d9cd", file="snapshot.zip")
 ## End(Not run) 
  
 
```
 
 
