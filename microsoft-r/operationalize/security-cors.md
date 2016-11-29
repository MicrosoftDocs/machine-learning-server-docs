---

# required metadata
title: "Cross-Origin Resource Sharing"
description: "Cross-Origin Resource Sharing - R Server"
keywords: "Security DeployR CORS Cross-Origin Resource Sharing"
author: "j-martens"
manager: "jhubbard"
ms.date: "05/06/2016"
ms.topic: "get-started-article"
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

# Security: Cross-Origin Resource Sharing

Cross-Origin Resource Sharing (CORS) enables your client application to freely communicate and make cross-site HTTP requests for resources from a domain other than where the web node is hosted. 

CORS can be enabled or disabled in the external configuration file, `appsettings.json`. Support for CORS is disabled by default.  

**To enable CORS support:**

@@Web or Compute Node??

1. Open `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server install directory. To find this path, enter `normalizePath(R.home())` in your R console.

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