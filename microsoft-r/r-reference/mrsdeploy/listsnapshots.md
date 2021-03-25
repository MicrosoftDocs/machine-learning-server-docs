--- 

# required metadata 
title: "listSnapshots function (mrsdeploy) | Microsoft Docs" 
description: " Get a list of all the snapshots on the R server that are available to the current user. " 
keywords: "(mrsdeploy), listSnapshots" 
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
ms.technology: "" 
ms.custom: "" 

--- 




 # listSnapshots: Get a list of snapshots for the current user. 
 ## Description

Get a list of all the snapshots on the R server that are available to the current user.


 ## Usage

```   
  listSnapshots()

```

 ## Details

Complete documentation: [`https://go.microsoft.com/fwlink/?linkid=836352`](https://go.microsoft.com/fwlink/?linkid=836352)



 ## Value

A character vector containing the snapshot IDs.

 ## See Also

[createSnapshot](createSnapshot.md)

[deleteSnapshot](deleteSnapshot.md)

[loadSnapshot](loadSnapshot.md)

[downloadSnapshot](downloadSnapshot.md)

 ## Examples

 ```

  ## Not run:

snapshots<-listSnapshots()
 ## End(Not run) 
```

