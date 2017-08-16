--- 
 
# required metadata 
title: "createSnapshot (mrsdeploy package for R, Microsoft Machine Learning Server) | Microsoft Docs" 
description: "Creates a snapshot of the remote R session and saves it on the server. Both the workspace and the files in the working directory are saved. " 
keywords: "mrsdeploy, createSnapshot" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 # createSnapshot

 Applies to: [mrsdeploy package for R](./r-reference/mrsdeploy/mrsdeploy-package.md), version 1.1.0  
 
 ##Description (unchanged)
 
Creates a snapshot of the remote R session and saves it on the server. Both the workspace
and the files in the working directory are saved.
 
This is what the metadata looks like:

 ```
title: "createSnapshot (mrsdeploy package for R, Microsoft Machine Learning Server) | Microsoft Docs" 
description: "Creates a snapshot of the remote R session and saves it on the server. Both the workspace and the files in the working directory are saved. " 
 ```
 
 ##Usage (unchanged)

```   
  createSnapshot(name)
 
```
 
 ##Arguments  (unchanged)

   
  
 ### name
 Name describing the snapshot to be saved. 
  
 
 
 ##Details  (unchanged)
 
Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)

 
 
 ##Value  (unchanged)
 
`snapshot_id` if successful.
 
 ##See Also
 
[deleteSnapshot](deletesnapshot.md)

[listSnapshots](listsnapshots.md)

[loadSnapshot](loadsnapshot.md)

[downloadSnapshot](downloadsnapshot.md)
   
 ##Examples  (unchanged)

 ```
   
  ## Not run:
 
snapshot_id<-createSnapshot()
 ## End(Not run) 
  
 
```
 
 
