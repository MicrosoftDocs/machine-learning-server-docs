---

# required metadata
title: "Enterprise-Grade Security: CORS | Microsoft R Server Docs"
description: "Enterprise-Grade Security: CORS with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "12/08/2016"
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

# Cross-Origin Resource Sharing for Operationalization

**Applies to:  Microsoft R Server 9.0.1**

Cross-Origin Resource Sharing (CORS) enables your client application to freely communicate and make cross-site HTTP requests for resources from a domain other than where the web node is hosted. You can enable or disable CORS in the external configuration file, `appsettings.json`. Support for CORS is disabled by default.  

To enable CORS support:

1. On each web node, open the `appsettings.json` configuration file.

   + On Windows, this file is under `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\` where `<MRS_home>` is the path to the Microsoft R Server installation directory. To find this path, enter `normalizePath(R.home())` in your R console.

   + On Linux, this file is under `/usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/`.

1. Enable CORS in the `"CORS": {` section of the  `appsettings.json` file:
   1.  Set CORS `"Enabled": true`

   1. Enter a comma-separated list of allowed `"Origins"` for your policy.  In this example, the policy allows cross-origin requests from “http://www.contoso.com”, “http://www.microsoft.com”, and no other origins.
   ```
   "CORS": {
      "Enabled": true,
      "Origins": [“http://www.contoso.com”, “http://www.microsoft.com”]
   }
   ```

3. Launch the administrator's utility and [restart the web node](admin-utility.md#startstop).