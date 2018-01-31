---

# required metadata
title: "Encrypt credentials and secrets - Machine Learning Server "
description: "We recommend that you encrypt strings in the appsettings.json configuration file."
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "2/16/2018"
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

# Encrypt credentials and secrets

**Applies to:  Machine Learning Server, Microsoft R Server**

For security purposes, we strongly recommend that you encrypt strings in the appsettings.json configuration file. For example, you should encrypt any remote database connection strings and/or LDAP/LDAP-S passwords rather than store strings in plain text. 

The encryption function available in the administration utility relies on the RSA algorithm for encryption. 

**To encrypt in Machine Learning Server 9.3:**
@Heidi

**To encrypt in earlier versions:**
       
1. On the web node, install a credential encryption certificate with a private key into the default certificate store on the local machine. That location is in the Windows certificate store or in the Linux-based PFX file. 

   The length of the encryption key depends on the certificate, however, we recommend a length of at least 1048 bits.

   Ensure that your certificate is secured properly. On Windows, for example, you can use Bitlocker to encrypt the disk.  

1. [Launch the administration utility](configure-admin-cli-launch.md) with administrator privileges (Windows) or root/sudo privileges (Linux).

      1. From the main menu, choose the option **Encrypt Credentials**.

      1. Specify where is the encryption certificate installed: 
         + Local machine (Computer account)
         + Current user (My user account)

         The list of available certificates appears.

      1. Specify which encryption certificate to use.

      1. Enter information you want to encrypt.  The tool returns an encrypted string.

1. Open the configuration file, \<web-node-install-path>/appsettings.json. (Find the [install path](../operationalize/configure-find-admin-configuration-file.md) for your version.) 

1. In that file, update the appropriate section for a [remote database connection](configure-remote-database-to-operationalize.mdconfigure-admin-cli-encrypt-credentials.md) or the [authentication password](configure-authentication.mdconfigure-admin-cli-encrypt-credentials.md) strings. 

>[!NOTE]
>You can bypass script interface using the argument '-encryptsecret encryptSecret encryptSecretCertificateStoreName encryptSecretCertificateStoreLocation encryptSecretCertificateSubjectName'. See the table at the end of this topic, [here](configure-admin-cli-launch.md#switch).
