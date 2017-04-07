---

# required metadata
title: "Publish and consume Python web services | Microsoft R Server Docs"
description: "Publish and consume Python web services with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "03/25/2017"
ms.topic: "article"
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
ms.technology: 
  - deployr
  - r-server
ms.custom: ""
---

# Publish and consume Python web services

**Applies to:  Microsoft R Server 9.1**

This article is for data scientists who wants to learn how to publish Python code/models as web services hosted in R Server and how to consume them. This article assumes you are proficient in Python.


1. Create a snapshot of this Python session so you can recall it later to reproduce the session. 

   @@ARE SNAPSHOTS ONLY FOR REMOTE EXECUTION SCENARIOS?  

   Snapshots are very useful for remote script execution scenarios when you need a prepared environment that includes certain libraries, objects, files and artifacts. Snapshots save the whole workspace and working directory so that you can pick up from exactly where you left last time. 

   Snapshots are only accessible to the user who creates them and cannot be shared across users.

   For example, suppose you want to execute a script that needs three libraries, a reference data file, and a model object.  Instead of loading these items each time you want to execute the script, create a snapshot of an R session containing them. Then, you can save time later by retrieving this snapshot using its ID to get the session contents exactly as they were at the time the snapshot was created.

   > [!IMPORTANT] 
   > While snapshots can also be used when publishing a web service for environment dependencies, it may have an impact on the performance of the consumption time.  For optimal performance, consider the size of the snapshot carefully especially when publishing a service. Before creating a snapshot, ensure that you keep only those workspace objects you need and purge the rest.  And, in the event that you only need a single object, consider passing that object alone itself instead of using a snapshot.

   ```python
   #Create a snapshot of the current session
   client.create_snapshot(session_id, deployrclient.models.CreateSnapshotRequest("Iris Snapshot"), headers)
   #List every snapshot
   for snapshot in client.list_snapshots(headers):
       print(snapshot)
   ```
