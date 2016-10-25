---

# required metadata
title: "Security Authentication"
description: "Operationalization of R Analytics with Microsoft R Server"
keywords: ""
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

# Configuring DeployR for HTTPS

>[!IMPORTANT] 
>For security reasons, we strongly recommend that HTTPS be enabled in **all production environments.**  Since we cannot ship certificates for you, HTTPS protocols are disabled by default.


DeployR allows for HTTPS within a connection encrypted by TLS and/or SSL.  To enable HTTPS, you'll need some or all of these certificates.

||HTTPS Certificates|Description|Front-end|Back-end|
|---|------------------------------------|------------------------------------------------------------------------|--------------|---------------|
|1|API certificate|Secures communication between client applications and DeployR front-end.|Yes, with private key|No|
|2|Back-end certificate|_Note: If a back-end is inside the front-end's trust boundary, then this certificate isn't needed._ <br>Encrypts the traffic between the front-end and back-end. You can use a unique certificate for each back-end, or you can use one common Multi-Domain (SAN) certificate for all back-ends.|No|Yes, with private key|
|3|Authentication certificate|_Note: If a back-end is inside the front-end's trust boundary, then this certificate isn't needed._<br>Authenticates the front-end with the back-end so that only the front-end can communicate with the back-end.|Yes, with private key|Yes, with public key|

## Enabling HTTPS Support

Once enabled, your client applications can make API calls that connect over HTTPS.

<br />

### Encrypt the Traffic between the Client Application and DeployR

This section walks you through the steps for securing the connections between the client application and the DeployR front-end. Doing so will encrypt the communication between client and front-end to prevent traffic from being modified or read.

> The following steps assume the trusted, signed  HTTPS certificate is already installed on the front-end machine with a private key.

 
1. [Launch the DeployR Administrator Utility](admin-utility#launch) script with administrator privileges.

1. STEPS UNKNOWN @@@@@@@@@@@@@

1. When prompted to provide the full file path to the trusted SSL certificate file, type the full path to the file. 

1. [Restart the front-end](admin-utility.md#startstop) so that the changes can take effect.   

1. Test these changes by TRYING THIS @@@@@@.  

<br />

### Encrypt Communication between the Front-end and Back-end

This section walks you through the steps for encrypting the traffic between the DeployR's front-end and each of its back-ends. 

> If a back-end is inside the front-end's trust boundary, then this certificate isn't needed. However, if the back-end resides outside of the trust boundary, consider using the back-end certificate to encrypt the traffic between the front-end and back-end. 

With this option, you have the choice of using:
+ One unique certificate per back-end machine.
+ One common Multi-Domain (SAN) certificate with all back-end names declared in the certificate

> The following steps assume the trusted, signed HTTPS certificate is already installed on the back-end machine with a private key.

1. STEPS UNKNOWN @@@@@@

<br />

### Authenticate the Front-end with the Back-end

This section walks you through the steps for authenticating the front-end with the back-end so that only the front-end can communicate with the back-end.

> If a back-end is inside the front-end's trust boundary, then this certificate isn't needed. However, if the back-end resides outside of the trust boundary, consider using the back-end certificate to encrypt the traffic between the front-end and back-end. 

1. On each front-end:

    > These steps assume the trusted, signed HTTPS certificate is already installed on each front-end with a _private_ key.

    > WHAT DO WE NEED TO DO ON THE FRONT-END?

1. On each back-end, require a client certificate on each front-end. 

    > These steps assume the trusted, signed HTTPS certificate is already installed on the back-end machine with a _public_ key.

    > WHAT DO WE NEED TO DO ON EACH BACK-END?

    1. STEPS UNKNOWN



<br />



## Disabling HTTPS Support