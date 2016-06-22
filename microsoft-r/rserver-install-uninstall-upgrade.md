---
# required metadata
title: "Uninstall R Server on Linux or Hadoop to upgrade to a newer version"
description: "Upgrading Microsoft R Server and the RevoScaleR package is done by uninstalling the existing version and installing a newer version."
keywords: ""
author: "HeidiSteen"
manager: "paulettm"
ms.date: "06/20/2016"
ms.topic: ""
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
# Uninstall R Server to upgrade to a newer version

This article explains how to uninstall Microsoft R Server on Linux or Hadoop. Upgrading to any new version, whether it is a major or minor release, requires that you first uninstall the existing deployment so that you can install the new version.

If you upgrade the Hadoop cluster, you will also need to uninstall and reinstall Microsoft R Server. You can either uninstall R Server before upgrading your cluster, or create and run a user-defined script that repairs the R Server links as a post-upgrade operation. For simplicity, we recommend uninstalling R Server first, and then reinstalling R Server after the cluster is upgraded. We recommend [installing version 8.0.5](rserver-install-hadoop-805.md).
