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


1. Create or call a model in Python that you'll publish as a web service. 

   In this example, we train a SciKit-Learn support vector machines (SVM) model on the Iris Dataset on a remote R Server.  

   ```python
   #Import SVM and datasets from the SciKit-Learn library
   execute_request = deployrclient.models.ExecuteRequest("from sklearn import svm\nfrom sklearn import datasets")
   execute_response = client.execute_code(session_id,execute_request, headers)
   #Report if it was a success
   execute_response.success
   
   #Define the untrained Support Vector Classifier (SVC) object and the dataset to be preloaded
   execute_request = deployrclient.models.ExecuteRequest("clf=svm.SVC()\niris=datasets.load_iris()")
   #Now, go create the object and preload Iris Dataset in R Server
   execute_response = client.execute_code(session_id,execute_request, headers)
   #Report if it was a success
   execute_response.success
   
   #Define two rows from the Iris Dataset as a sample for scoring
   workspace_object = deployrclient.models.WorkspaceObject("variety_1",[7,3.2,4.7,1.4])
   workspace_object_2 = deployrclient.models.WorkspaceObject("variety_2",[3,2.6,3,2.5])

   #Define how to train the classifier model; what to predict; what to return
   execute_request = deployrclient.models.ExecuteRequest("clf.fit(iris.data, iris.target)\n"+
                                                      "result=clf.predict(variety_1)\n"+
                                                      "other_result=clf.predict(variety_2)"
                                                      ,[workspace_object,workspace_object_2], #Input Parameters
                                                      ["result", "other_result"]) #Output parameter names
   #Now, go train that model on the Iris Dataset in R Server
   execute_response = client.execute_code(session_id,execute_request, headers)

   #If successful, print name and result of each output parameter. Else, print error.
   if(execute_response.success):
       for result in execute_response.output_parameters:
           print("{0}: {1}".format(result.name,result.value))
   else:
       print (execute_response.error_message)
   ```

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
