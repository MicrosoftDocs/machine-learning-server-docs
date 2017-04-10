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

Microsoft R Server installation on CDH is enhanced in 9.1.0. This release adds support custom service descriptors so that you can manage R Server from within Cloudera Manager.

After installing the parcels, the next step is to download, install, and run the Microsoft R Server Custom Service Descriptor. In Cloudera Manager, installation and activation are decoupled so that you can have multiple versions of a software package in the cluster. Only version can be active at any time.

1. Download the CSD file.
1. Copy the CSD file MRS-9.1.0-CONFIG.jar to the Cloudera CSD directory, typically /opt/cloudera/csd.
2.	Modify the permissions of CSD file as follows: 
        sudo chmod 644 /opt/cloudera/csd/MRS-9.1.0-CONFIG.jar
        sudo chown cloudera-scm:cloudera-scm /opt/cloudera/csd/MRS-9.1.0-CONFIG.jar
3.	Stop and restart the cloudera-scm-server service using the following shell commands:
        sudo service cloudera-scm-server stop
        sudo service cloudera-scm-server start
4.	Confirm the CSD is installed by checking the Custom Service Descriptor list in Cloudera Manager at http://<cloudera-manager-server>:7180/cmf/csd/list
5.	On the Cloudera Manager home page, click the dropdown beside the cluster name and click **Add a Service**.
6.	From the Add Service Wizard, select **Microsoft R Server** and click **Continue**.
7.	Select **all hosts**, and click **Continue**.
8.	Accept defaults through the remainder of the wizard.

## Script inputs

~~~~
    -j or --location                    Folder location where CSD jar file is located
    -c or --clustername                 Cloudera Cluster Name
    -u or --username                    Username for connection to cloudera manager server
    -p or --password                    Password for connection to cloudera manager server
~~~~

## See Also

[Install R Server 9.1.0 on the Cloudera distribution of Apache Hadoop (CDH)](rserver-install-cloudera.md)
