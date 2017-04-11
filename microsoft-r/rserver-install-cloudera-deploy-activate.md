---

# required metadata
title: "Deploy and activate MRS parcels and CSDs on CDH"
description: "Deploy and activate Microsoft R Server parcels and CSDs on the Cloudera distribution of Apache Hadoop (CDH)."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "03/29/2017"
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
ms.technology: "r-server"
ms.custom: ""
---

# Deploy and activate the MRS parcel and custom service descriptor (CSD)

**Applies to:** R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)

After [generating a parcel and Custom Service Descriptor (CSD) for Microsoft R Server (MRS) 9.1.0](rserver-install-cloudera-generate-parcel.md), you can use Cloudera Manager to deploy the parcel. You can also add MRS as a service for administration with in Cloudera Manager.

## Distribute the MRS parcel

1. In Cloudera Manager, click the parcel icon on the top right menu bar.

   [parcel icon in cloudera manager][media/rserver-install-cloudera/cloudera-manager-parcel-icon.png]

2. Find **MRS** in the parcel list. If you don't see it, check the parcel repo folder (by default, /opt/cloudera/parcel-repo) for `MRS-9.1.0-el7.parcel` and `MRS-9.1.0-el7.parcel.sha`. The machine should be the master node of the cluster. 

   [parcel list in cloudera manager][media/rserver-install-cloudera/cloudera-manager-parcel-list.png]

3. In the parcel details page, **MRS** should have a status of *Download* with an option to *Distribute*. Click **Distribute** to roll out MRS on available data nodes.

   [parcel details in cloudera manager][media/rserver-install-cloudera/cloudera-manager-parcel-detail.png]

4. Status changes to *distributed*. Click **Activate** on the button to make MRS operational in the cluster.

   [Activate button in parcel detial][media/rserver-install-cloudera/cloudera-manager-activate-button.png]

You are finished with this task when status is "distributed, activated" and the next available action is *Deactivate*.

## Add MRS as a service

1. In Cloudera Manager home page, click the down arrow by the cluster name and choose **Add Service**.

   [add service command in cloudera manager][media/rserver-install-cloudera/cloudera-manager-add-service.png]

2. Find and select **Microsoft R Server** and click **Continue**.

   [add Microsoft R Server][media/rserver-install-cloudera/cloudera-manager-add-mrs-service.png]

3. In the next page, add role assignments by choosing data nodes on which to run the service. Click **Continue**.

4. On the last page, click **Finish** to start the service.

R Server should now be available to use.

## Next Steps

Review the following walkthroughs to move forward with using R Server and the RevoScaleR package in Spark and MapReduce processing models.

+ [Get started with ScaleR on Spark](scaler-spark-getting-started.md)
+ [Get started with ScaleR on MapReduce](scaler-hadoop-getting-started.md)

## See Also

[R Server installation on Hadoop overview](rserver-install-hadoop.md)
