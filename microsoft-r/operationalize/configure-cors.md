---

# required metadata
title: "Cross-Origin Resource Sharing CORS in Machine Learning Server "
description: "Enterprise-Grade Security: CORS with Machine Learning Server"
keywords: ""
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "conceptual"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""
---

# Cross-Origin Resource Sharing 

**Applies to:  Machine Learning Server, Microsoft R Server 9.x**

Cross-Origin Resource Sharing (CORS) enables your client application to freely communicate and make cross-site HTTP requests for resources from a domain other than where the web node is hosted. You can enable or disable CORS in the external configuration file, appsettings.json. Support for CORS is disabled by default.  

**To enable CORS support:**

1. On each web node, open the configuration file, \<web-node-install-path>/appsettings.json. (Find the [install path](../operationalize/configure-find-admin-configuration-file.md) for your version.)

2. Enable CORS in the `"CORS": {` section of the  appsettings.json file:
   1. Set CORS `"Enabled": true`

   2. Enter a comma-separated list of allowed `"Origins"` for your policy.  In this example, the policy allows cross-origin requests from "<http://www.contoso.com>", "<http://www.microsoft.com>", and no other origins.
      ```
      "CORS": {
      "Enabled": true,
      "Origins": ["http://www.contoso.com", "http://www.microsoft.com"]
      }
      ```

3. Launch the administrator's utility and [restart the web node](configure-admin-cli-stop-start.md).
