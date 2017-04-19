--- 
 
# required metadata 
title: "Load a snapshot from the R server into the remote session." 
description: " Loads the specified snapshot from the R server into the remote session.The workspace is updated with the objects saved in the snapshot, and saved files are restored to the working directory. " 
keywords: "mrsdeploy, loadSnapshot" 
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
 
 
 
 
 #`loadSnapshot`: Load a snapshot from the R server into the remote session.

 Applies to version 1.1.0 of package mrsdeploy.
 
 ##Description
 
Loads the specified snapshot from the R server into the remote session.The workspace
is updated with the objects saved in the snapshot, and saved files are restored to the working directory.
 
 
 ##Usage

```   
  loadSnapshot(snapshot_id)
 
```
 
 ##Arguments

   
  
 ### `snapshot_id`
 Identifier of the snapshot to load. 
  
 
 
 ##Details
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value
 
`TRUE` if successful.
 
 ##See Also
 
[createSnapshot](createSnapshot.md)

[deleteSnapshot](deleteSnapshot.md)

[listSnapshots](listSnapshots.md)

[downloadSnapshot](downloadSnapshot.md)
   
 ##Examples

 ```
   
  ## Not run:
 
loadSnapshot("8ce7eb47-3aeb-4c5a-b0a5-a2025f07d9cd")
 ## End(Not run) 
  
 
```
 
 
