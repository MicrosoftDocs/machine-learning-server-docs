---

# required metadata
title: "Cross-Origin Resource Sharing CORS in Microsoft R Server | Microsoft Docs"
description: "Enterprise-Grade Security: CORS with Microsoft R Server"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "4/19/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""
---

# Cross-Origin Resource Sharing 

**Applies to:  Microsoft R Server 9.x**

Cross-Origin Resource Sharing (CORS) enables your client application to freely communicate and make cross-site HTTP requests for resources from a domain other than where the web node is hosted. You can enable or disable CORS in the external configuration file, appsettings.json. Support for CORS is disabled by default.  

**To enable CORS support:**

1. On each web node, [open the appsettings.json configuration file](configure-find-admin-configuration-file.md).

1. Enable CORS in the `"CORS": {` section of the  appsettings.json file:
   1.  Set CORS `"Enabled": true`

   1. Enter a comma-separated list of allowed `"Origins"` for your policy.  In this example, the policy allows cross-origin requests from “http://www.contoso.com”, “http://www.microsoft.com”, and no other origins.
   ```
   "CORS": {
      "Enabled": true,
      "Origins": [“http://www.contoso.com”, “http://www.microsoft.com”]
   }
   ```

3. Launch the administrator's utility and [restart the web node](configure-use-admin-utility.md#startstop).