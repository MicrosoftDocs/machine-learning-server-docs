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

# Configure SSL for R Server's Operationalization (HTTPS)

>[!IMPORTANT] 
>For security reasons, we strongly recommend that HTTPS be enabled in **all production environments.**  Since we cannot ship certificates for you, HTTPS protocols are disabled by default.


You can use HTTPS within a connection encrypted by TLS and/or SSL.  To enable HTTPS, you'll need some or all of these certificates.

||HTTPS Certificates|Description|Front-end|Back-end|
|---|------------------------------------|------------------------------------------------------------------------|--------------|---------------|
|1|API certificate|Secures communication between client applications and front-end.|Yes, with private key|No|
|2|Back-end certificate|_Note: If a back-end is inside the front-end's trust boundary, then this certificate isn't needed._ <br>Encrypts the traffic between the front-end and back-end. You can use a unique certificate for each back-end, or you can use one common Multi-Domain (SAN) certificate for all back-ends.|No|Yes, with private key|
|3|Authentication certificate|_Note: If a back-end is inside the front-end's trust boundary, then this certificate isn't needed._<br>Authenticates the front-end with the back-end so that only the front-end can communicate with the back-end.|Yes, with private and a public key|No|

<br />

## Encrypt the Traffic between Client Applications and R Server

>[!IMPORTANT] 
>We strongly recommend that HTTPS be enabled in **all production environments.**  

This section walks you through the steps for securing the connections between the client application and the front-end. Doing so will encrypt the communication between client and front-end to prevent traffic from being modified or read.

#### Using Windows IIS to Encrypt

> Make sure the name of the certificate matches the domain name of the front-end URL. 

1. On each front-end machine, install the trusted, signed ** API HTTPS certificate** with a private key in the certificate store on the front-end machine.
<a name="iis"></a>
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
1. Run the [diagnostic tool](admin-utility.md#test) to send a test HTTPs request.
   > If satisfied with the new HTTPS binding, consider removing the "HTTP" binding to prevent any access via HTTP.

#### Using your Default .ASP Core Web Server to Encrypt

1. On each front-end machine, install the trusted, signed ** API HTTPS certificate** with a private key in the certificate store on the front-end machine.
   > Make sure the name of the certificate matches the domain name of the front-end URL. 
   >
   > Also, take note of the `Subject` name of the certificate as you'll need this info later.

1. Update the external JSON configuration file, `appsettings.json` to enable configure the HTTPS port for the front-end:
   1. Open `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server installation directory. If you don't know where that directory is, launch an R console and enter `normalizePath(R.home())`.

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
1. Launch the utility script with administrator privileges and:
   1. [Restart the front-end](admin-utility.md#startstop).
   1. Run the [diagnostic tool](admin-utility.md#test) to send a test HTTPs request.

<br />

## Encrypt Communication between the Front-end and Back-end

This section walks you through the steps for encrypting the traffic between the front-end and each of its back-ends. 

> If a back-end is inside the front-end's trust boundary, then encryption of this piece isn't needed. However, if the back-end resides outside of the trust boundary, consider using the back-end certificate to encrypt the traffic between the front-end and back-end. 

When encrypting, you have the choice of using one of the following **back-end HTTPS certificates**:
+ One unique certificate per back-end machine.
+ One common Multi-Domain (SAN) certificate with all back-end names declared in the single certificate

#### Using Windows IIS to Encrypt

> Make sure the name of the certificate matches the domain name of the back-end URL. 

1. On each back-end machine, install the trusted, signed **back-end HTTPS certificate** with a private key in the certificate store on the back-end machine.
1. Launch IIS and follow the [instructions above](#iis).

#### Using your Default .ASP Core Web Server to Encrypt

1. On each back-end machine, install the trusted, signed **back-end HTTPS certificate** with a private key in the certificate store on the back-end machine.
   > Make sure the name of the certificate matches the domain name of the back-end URL. 
   >
   > Also, take note of the `Subject` name of the certificate as you'll need this info later.

1. Update the external JSON configuration file, `appsettings.json` to enable configure the HTTPS port for the back-end:
   1. Open `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server installation directory. If you don't know where that directory is, launch an R console and enter `normalizePath(R.home())`.

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
   1. Run the [diagnostic tool](admin-utility.md#test) to send a test HTTPs request.


<br />

## Authenticate the Front-end with the Back-end

This section walks you through the steps for authenticating the front-end with the back-end so that only the front-end can communicate with the back-end.

> If a back-end is inside the front-end's trust boundary, then this certificate isn't needed. However, if the back-end resides outside of the trust boundary, consider using the back-end certificate to encrypt the traffic between the front-end and back-end. 


1. On each front-end:

    1. Install the trusted, signed **HTTPS authentication certificate** with both private and public keys in the certificate store.
       > Make sure the name of the certificate matches the domain name of the back-end URL. 
       > Also, take note of the `Subject` name of the certificate as you'll need this info later.

    1. Open the external JSON configuration file, `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json`, where `<MRS_home>` is the path to the Microsoft R Server installation directory. If you don't know where that directory is, launch an R console and enter `normalizePath(R.home())`.

    1. In the file, search for the section starting with `"BackEndConfiguration": {` .
    1. Uncomment characters in that section and update the properties to match the values for the **Authentication certificate**:
       ```
       "BackEndConfiguration": {
           "ClientCertificate": {
               "StoreName": "My",
               "StoreLocation": "LocalMachine",
               "SubjectName": "<name-of-certificate-subject>"
       ```

    1. Close and save the file.
    1. Launch the administrator's utility and [restart the back-end](admin-utility.md#startstop).
    1. Repeat on each front-end.

1. On each back-end:
    > These steps assume the trusted, signed **HTTPS authentication certificate**  is already installed on the front-end machine with a _public_ key.

    1. Open the external JSON configuration file, `appsettings.json` file.
    1. In the file, search for the section starting with `"BackEndConfiguration": {` .
    1. Uncomment characters in that section and update the properties to match the values for the **Authentication certificate**:
       ```
       "ClientCertificate": {
           "Issuer": "<certificate issuer name>",
           "Subject": "<certificate subject name>"
       },   
       ```

    1. Close and save the file.
    1. Launch the administrator's utility and [restart the back-end](admin-utility.md#startstop).
    1. Repeat on each back-end.