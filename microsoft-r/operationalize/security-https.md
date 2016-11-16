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

||HTTPS Certificates|Description|Web node|compute node|
|---|------------------------------------|------------------------------------------------------------------------|--------------|---------------|
|1|API certificate|Secures communication between client applications and web node.|Yes, with private key|No|
|2|compute node certificate|_Note: If a compute node is inside the web node's trust boundary, then this certificate isn't needed._ <br>Encrypts the traffic between the web node and compute node. You can use a unique certificate for each compute node, or you can use one common Multi-Domain (SAN) certificate for all compute nodes.|No|Yes, with private key|
|3|Authentication certificate|_Note: If a compute node is inside the web node's trust boundary, then this certificate isn't needed._<br>Authenticates the web node with the compute node so that only the web node can communicate with the compute node.|Yes, with private and a public key|No|

<br />

## Encrypt the Traffic between Client Applications and R Server

>[!IMPORTANT] 
>We strongly recommend that HTTPS be enabled in **all production environments.**  

This section walks you through the steps for securing the connections between the client application and the web node. Doing so will encrypt the communication between client and web node to prevent traffic from being modified or read.

#### Using Windows IIS to Encrypt

> Make sure the name of the certificate matches the domain name of the web node URL. 

1. On each machine hosting a web node, install the trusted, signed ** API HTTPS certificate** with a private key in the certificate store.
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

#### Using your Default .ASP Core web Server to Encrypt

1. On each machine hosting the web node, install the trusted, signed ** API HTTPS certificate** with a private key in the certificate store.
   > Make sure the name of the certificate matches the domain name of the web node URL. 
   >
   > Also, take note of the `Subject` name of the certificate as you'll need this info later.

1. Update the external JSON configuration file, `appsettings.json` to enable configure the HTTPS port for the web node:
   1. Open `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server install directory. To find this path, enter `normalizePath(R.home())` in your R console.

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
   1. [Restart the web node](admin-utility.md#startstop).
   1. Run the [diagnostic tool](admin-utility.md#test) to send a test HTTPs request.

<br />

## Encrypt Communication between the web node and Compute Node

This section walks you through the steps for encrypting the traffic between the web node and each of its compute nodes. 

> If a compute node is inside the web node's trust boundary, then encryption of this piece isn't needed. However, if the compute node resides outside of the trust boundary, consider using the compute node certificate to encrypt the traffic between the web node and compute node. 

When encrypting, you have the choice of using one of the following **compute node HTTPS certificates**:
+ One unique certificate per machine hosting a compute node.
+ One common Multi-Domain (SAN) certificate with all compute node names declared in the single certificate

#### Using Windows IIS to Encrypt

> Make sure the name of the certificate matches the domain name of the compute node URL. 

1. On each machine hosting a compute node, install the trusted, signed **compute node HTTPS certificate** with a private key in the certificate store.
1. Launch IIS and follow the [instructions above](#iis).

#### Using your Default .ASP Core web Server to Encrypt

1. On each machine hosting a compute node, install the trusted, signed **compute node HTTPS certificate** with a private key in the certificate store.
   > Make sure the name of the certificate matches the domain name of the compute node URL. 
   >
   > Also, take note of the `Subject` name of the certificate as you'll need this info later.

1. Update the external JSON configuration file, `appsettings.json` to enable configure the HTTPS port for the compute node:
   1. Open `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json` where `<MRS_home>` is the path to the Microsoft R Server install directory. To find this path, enter `normalizePath(R.home())` in your R console.

   1. In the `appsettings.json` file, search for the section starting with `"Kestrel": {` .

   1. Update and add properties in that section to match the values for the compute node certificate. The `Subject` name can be found as a property of your certificate in the certificate store.
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
   1. [Restart the compute node](admin-utility.md#startstop).
   1. Run the [diagnostic tool](admin-utility.md#test) to send a test HTTPs request.


<br />

## Authenticate the Web node with the Compute Node

This section walks you through the steps for authenticating the web node with the compute node so that only the web node can communicate with the compute node.

> If a compute node is inside the web node's trust boundary, then this certificate isn't needed. However, if the compute node resides outside of the trust boundary, consider using the compute node certificate to encrypt the traffic between the web node and compute node. 


1. On each web node:

    1. Install the trusted, signed **HTTPS authentication certificate** with both private and public keys in the certificate store.
       > Make sure the name of the certificate matches the domain name of the compute node URL. 
       > Also, take note of the `Subject` name of the certificate as you'll need this info later.

    1. Open the external JSON configuration file, `<MRS_home>\deployr\Microsoft.DeployR.Server.WebAPI\appsettings.json`, where `<MRS_home>` is the path to the Microsoft R Server install directory. To find this path, enter `normalizePath(R.home())` in your R console.

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
    1. Launch the administrator's utility and [restart the compute node](admin-utility.md#startstop).
    1. Repeat on each web node.

1. On each compute node:
    > These steps assume the trusted, signed **HTTPS authentication certificate**  is already installed on the machine hosting the web node with a _public_ key.

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
    1. Launch the administrator's utility and [restart the compute node](admin-utility.md#startstop).
    1. Repeat on each compute node.