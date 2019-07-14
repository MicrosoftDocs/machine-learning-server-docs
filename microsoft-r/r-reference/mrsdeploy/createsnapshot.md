--- 

# required metadata 
title: "createSnapshot function (mrsdeploy) | Microsoft Docs" 
description: " Creates a snapshot of the remote R session and saves it on the server. Both the workspace and the files in the working directory are saved. " 
keywords: "(mrsdeploy), createSnapshot" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: 07/15/2019
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




 # createSnapshot: Create a snapshot of the remote R session. 
 ## Description

Creates a snapshot of the remote R session and saves it on the server. Both the workspace
and the files in the working directory are saved.


 ## Usage

```   
  createSnapshot(name)

```

 ## Arguments



 ### `name`
 Name describing the snapshot to be saved. 



 ## Details

Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)



 ## Value

`snapshot_id` if successful.

 ## See Also

[deleteSnapshot](deleteSnapshot.md)

[listSnapshots](listSnapshots.md)

[loadSnapshot](loadSnapshot.md)

[downloadSnapshot](downloadSnapshot.md)

 ## Examples

 ```

  ## Not run:

snapshot_id<-createSnapshot()
 ## End(Not run) 
```

