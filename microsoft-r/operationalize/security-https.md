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

<br />

### Encrypt the Traffic between the Client Application and DeployR

>[!IMPORTANT] 
>We strongly recommend that HTTPS be enabled in **all production environments.**  

This section walks you through the steps for securing the connections between the client application and the DeployR front-end. Doing so will encrypt the communication between client and front-end to prevent traffic from being modified or read.

### Using Windows IIS to Encrypt**

> Make sure the name of the certificate matches the domain name of the front-end URL. 

**On each front-end machine:**

1. Install the trusted, signed HTTPS "API certificate" with a private key in the certificate store on the front-end machine.
<a name=iis></a>
1. Launch IIS.
1. In the "Connections" pane on the left, expand the "Sites" folder and select the website.
1. Click on "Bindings" under the "Actions" pane on the right.
1. Click on "Add".
1. Choose "HTTPS" as the type and enter the "Port", which is 443 by default. Take note of the port number. 
1. Select the SSL certificate you installed previously. 
1. Click "OK" to create the new HTTPS binding.
1. Back in the "Connections" pane, select the website name.
1. Click the "SSL Settings" icon in the center of the screen to open the dialog. 
1. Select the checkbox to "Require SSL" and require a client certificate.
1. Run the [DeployR diagnostic tool](admin-diagnostics-troubleshooting.md) to send a test HTTPs request.
   > If satisfied with the new HTTPS binding, consider removing the "HTTP" binding to prevent any access via HTTP.

### Using your Default .ASP Core Web Server to Encrypt

**On each front-end machine:**

1. Install the trusted, signed HTTPS "API certificate" with a private key in the certificate store on the front-end machine.
   > Make sure the name of the certificate matches the domain name of the front-end URL. 
   >
   > Also, take note of the `Subject` name of the certificate as you'll need this info later.

   > @@@@@@@@ HOW DO WE DO THIS ON LINUX?? SUPPORTED FLAVORS OF LINUX????

1. Update the DeployR external JSON configuration file, `appsettings.json` to enable configure the HTTPS port for the front-end:
   1. In the `appsettings.json` file, search for the section starting with `"Kestrel": {` .
   1. Update and add properties in the `Kestrel` section to match the values for the API certificate. The `Subject` name can be found as a property of your certificate in the certificate store.
      ```
      {
          "Kestrel": {
              "Port": <https-port-number>,
              "HttpsEnabled": true,
              "HttpsCertificate": {
                  "StoreName": "My",        
                  "StoreLocation": "LocalMachine",
                  "SubjectName": "CN=<certificate-subject-name>"
              }
          },
      ```
   1. Close and save the file.
1. Launch the administrator's utility and:
   1. [Restart the front-end](admin-utility.md#startstop).
   1. Run the [DeployR diagnostic tool](admin-diagnostics-troubleshooting.md) to send a test HTTPs request.

<br />

### Encrypt Communication between the Front-end and Back-end

This section walks you through the steps for encrypting the traffic between the DeployR's front-end and each of its back-ends. 

> If a back-end is inside the front-end's trust boundary, then this certificate isn't needed. However, if the back-end resides outside of the trust boundary, consider using the back-end certificate to encrypt the traffic between the front-end and back-end. 

With this option, you have the choice of using:
+ One unique certificate per back-end machine.
+ One common Multi-Domain (SAN) certificate with all back-end names declared in the single certificate

### Using Windows IIS to Encrypt**

> Make sure the name of the certificate matches the domain name of the back-end URL. 

**On each back-end machine:**

1. Install the trusted, signed HTTPS "back-end certificate" with a private key in the certificate store on the back-end machine.
1. Launch IIS and follow the [instructions above](#iis).

### Using your Default .ASP Core Web Server to Encrypt

**On each back-end machine:**

1. Install the trusted, signed HTTPS "back-end certificate" with a private key in the certificate store on the back-end machine.
   > Make sure the name of the certificate matches the domain name of the back-end URL. 
   >
   > Also, take note of the `Subject` name of the certificate as you'll need this info later.

   > @@@@@@@@ HOW DO WE DO THIS ON LINUX?? SUPPORTED FLAVORS OF LINUX????

1. Update the DeployR external JSON configuration file, `appsettings.json` to enable configure the HTTPS port for the back-end:
   1. In the `appsettings.json` file, search for the section starting with `"Kestrel": {` .
   1. Update and add properties in that section to match the values for the back-end certificate. The `Subject` name can be found as a property of your certificate in the certificate store.
      ```
      {
          "Kestrel": {
              "Port": <https-port-number>,
              "HttpsEnabled": true,
              "HttpsCertificate": {
                  "StoreName": "My",        
                  "StoreLocation": "LocalMachine",
                  "SubjectName": "CN=<certificate-subject-name>"
              }
          },
      ```
   1. Close and save the file.
1. Launch the administrator's utility and:
   1. [Restart the back-end](admin-utility.md#startstop).
   1. Run the [DeployR diagnostic tool](admin-diagnostics-troubleshooting.md) to send a test HTTPs request.


<br />

### Authenticate the Front-end with the Back-end

This section walks you through the steps for authenticating the front-end with the back-end so that only the front-end can communicate with the back-end.

> If a back-end is inside the front-end's trust boundary, then this certificate isn't needed. However, if the back-end resides outside of the trust boundary, consider using the back-end certificate to encrypt the traffic between the front-end and back-end. 

1. On each front-end:

    > These steps assume the trusted, signed HTTPS certificate is already installed on each front-end with a _private_ key AND PUBLIC KEY!!!!!!.

    > WHAT DO WE NEED TO DO ON THE FRONT-END?

    1. install the private key certificate. 

    1. 3:25pm 

    1. edit `appsettings.json`  

    Uncomment it and specify how the front-end will find the certificate in the certificate store.

    "BackEndConfiguration": {

        "ClientCertificate": {

            "StoreName": "My",

            "StoreLocation": "CurrentUser",

            "SubjectName": "CN=be-auth.deployr.mrs.microsoft-tst.com"

"BackEndConfiguration": {

        // Uncomment this section if your backend(s) require certificate authentication

        //"ClientCertificate": {

        //    "StoreName": "My",

        //    "StoreLocation": "LocalMachine",

        //    "SubjectName": "<subject name>"

        //},

    1. Restart the server....

    1. if multople fronts repeat    

1. On each back-end, require a client certificate with a public key. 

    > These steps assume the trusted, signed HTTPS certificate is already installed on the back-end machine with a _public_ key.

    > WHAT DO WE NEED TO DO ON EACH BACK-END?


    1. edit `appsettings.json`  


"ClientCertificate": {

        "Issuer": "CN=Microsoft IT SSL SHA2, OU=Microsoft IT, O=Microsoft Corporation, L=Redmond, S=Washington, C=US",

        "Subject": "CN=be-auth.deployr.mrs.microsoft-tst.com"

    },



// Uncomment this section if you want to enforce certificate authentication for front-end to back-end

    //"ClientCertificate": {

    //    "Issuer": "<issuer name>",

    //    "Subject": "<subject name>"

    //},   

 1. restart the backend.

 1. repeat on all back-ends.
