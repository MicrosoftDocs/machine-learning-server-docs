---

# required metadata
title: "Enabling DeployR on AWS"
description: "How to Enable DeployR on AWS"
keywords: ""
author: "jmartens"
manager: "Paulette.McKay"
ms.date: "03/17/2016"
ms.topic: "article"
ms.prod: "deployr"
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

# Enabling DeployR on AWS

## DeployR on AWS

You can set up DeployR on **Amazon Web Services** (AWS).

For each [Amazon EC2 instance](http://docs.aws.amazon.com/general/latest/gr/rande.html), be sure to:

-   Have the minimum required disk space (or more) for the [installation of DeployR](deployr-installing-configuring.md) software and its database

-   Update the [server Web context](deployr-admin-diagnostics-troubleshooting.md#landing-page-cannot-be-reached) or else you will not be able to access to the DeployR landing page or other DeployR components after installation

    -   Set the IP to an external, public IP address.

    -   Disable the automatic IP detection

-   Set the appropriate security permissions on both the **internal** and **external** ports used by DeployR in the firewall:

    -   Open the external DeployR ports `8000`, `8001`, and `8006` through the AWS console.

    -   [Update the firewall](deployr-admin-configure-for-azure.md#updating-the-firewall).

-   Consider enabling HTTPS support for DeployR to secure the communications to the server. See these instructions for [enabling HTTPS](deployr-admin-security.md#enable-server-ssl-https).

>[!IMPORTANT]
>[Change all default DeployR user passwords](#change-pass), including the `admin` account, after installing DeployR.


