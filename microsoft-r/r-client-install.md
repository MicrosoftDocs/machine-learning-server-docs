---

# required metadata
title: "Install Microsoft R Client on Windows"
description: "Windows installation of Microsoft R Client."
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/17/2016"
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
ms.technology: "r-client"
ms.custom: ""

---

#Install Microsoft R Client

[!include[Microsoft R Client](./includes/r-client/r-client-intro.md)]

###Installing R Client

[Download Microsoft R Client](http://aka.ms/rclient/download), available through Visual Studio Dev Essentials, and install it today.

[!include[Install](./includes/r-client/r-client-install.md)]
<br>

> [!NOTE]
> By default, telemetry data is collected during your usage of R Client. To turn this feature off, use the RevoScaleR package function `rxPrivacyControl(FALSE)`. To turn it back on, change the setting to `TRUE`. For more information, see [Opting out of usage data collection](rserver-optout-telemetry.md).

<br>

###What's Next

Once you've installed R Client, the next step is to [configure your favorite R integrated development environment (IDE)](r-client-get-started.md#configure-ide) to point to the R Client R executable.

<br>

###Learn More

You can learn more with these guides:

+ [Get Started with Microsoft R Client](r-client-get-started.md)

+ [Microsoft R Getting Started](microsoft-r-getting-started.md)

+ [RevoScaleR Getting Started](scaler-getting-started.md)
