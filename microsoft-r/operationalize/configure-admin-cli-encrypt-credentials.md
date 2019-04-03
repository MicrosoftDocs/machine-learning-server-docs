---

# required metadata
title: "Encrypt credentials and secrets - Machine Learning Server "
description: "You should encrypt strings in the appsettings.json configuration file."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
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

# Encrypt credentials and secrets

**Applies to:  Machine Learning Server, Microsoft R Server**

For security purposes, we strongly recommend that you encrypt strings in the appsettings.json configuration file. For example, you should encrypt any remote database connection strings and/or LDAP/LDAP-S passwords rather than store strings in plain text. 

The encryption function available in the administration utility relies on the RSA algorithm for encryption. 

       
**To encrypt credentials or secrets:**

1. On the web node, install a credential encryption certificate with a private key into the default certificate store on the local machine. That location is in the Windows certificate store or in the Linux-based PFX file. 

   The length of the encryption key depends on the certificate, however, we recommend a length of at least 1048 bits.

   Ensure that your certificate is secured properly. On Windows, for example, you can use Bitlocker to encrypt the disk.  

1. Open the administration tool to encrypt the secret:
   + For Machine Learning Server 9.3, use `admin` extension of the Azure Command Line Interface ([Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)) to encrypt your credentials.

     You do not need an Azure subscription to use this CLI. It is installed as part of Machine Learning Server and runs locally.

     1. On the machine hosting the node, launch a command-line window or terminal  with administrator (Windows) or root/sudo (Linux) privileges.

     1. Run the command to encrypt:
        ```azurecli
        # With elevated privileges, run the following commands.
        # Get help on the commands
        az ml admin credentials --help

        # Display list of certificates installed on this machine
        az ml admin credentials list
        
        # For example:
        # [
        #   "A. DC=Windows Azure CRP Certificate Generator (private key)"
        # ]
        
        
        # Encrypt a secret
        az ml admin credentials set ---cert-store-name <CERT_STORE_NAME> --cert-store-location <CERT_STORE_LOCATION> --cert-subject_name <CERT_SUBJECT_NAME> --secret <secret>
        ```
        |CLI&nbsp;options|Description|
        |:----------:|----------------|
        |list|Returns the list of certificates found on the machine.|
        |set|Returns an encrypted string when you specify a certificate and a secret to be encrypted.|
        |--cert-store-name| The certificate store name. In Windows, it is usually one of "My", "Root", "TrustedPeople" etc.|
        |--cert-store-location | The certificate store location. In Windows, it is either "CurrentUser" or "LocalMachine".|
        |--cert-subject_name | The subject name of certificate. You could check it from "az ml admin credentials list". In the above example, the subject name is "DC=Windows Azure CRP Certificate Generator". For multiple entries on the subject name, escape the subject name as follows: `az ml admin credentials --cert-subject_name "\"CN=myhost.mydomain.local, O=myCompany, L=Local, C=MyCountry\""`|
        |--secret|Enter information you want to encrypt. |

        The CLI returns an encrypted string.

   + For versions 9.0 - 9.2: [Launch the administration utility](configure-admin-cli-launch.md) with administrator privileges (Windows) or root/sudo privileges (Linux).

      1. From the main menu, choose the option **Encrypt Credentials**.

      1. Specify where is the encryption certificate installed: 
         + Local machine (Computer account)
         + Current user (My user account)

      1. Specify which encryption certificate to use.

      1. Enter information you want to encrypt.  The tool returns an encrypted string.

1. Open the configuration file, [\<web-node-install-path>](../operationalize/configure-find-admin-configuration-file.md)/appsettings.json.  

1. In that file, update the appropriate section for a [remote database connection](configure-remote-database-to-operationalize.md) or the [authentication password](configure-authentication.md#encrypt) strings. 

>[!NOTE]
>You can bypass script interface using the argument '-encryptsecret encryptSecret encryptSecretCertificateStoreName encryptSecretCertificateStoreLocation encryptSecretCertificateSubjectName'. See the table at the end of this topic, [here](configure-admin-cli-launch.md#switch).
