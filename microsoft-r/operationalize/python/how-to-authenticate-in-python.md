---

# required metadata
title: "Connect to Machine Learning Server in Python with azureml-model-management-sdk - Machine Learning Server | Microsoft Docs"
description: "Publish and consume Python web services with Microsoft R Server"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "9/25/2017"
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

# Authenticate with Machine Learning Server in Python with azureml-model-management-sdk

**Applies to: Machine Learning Server 9.2**

The [azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) package, delivered with Machine Learning Server and provides functions for publishing and managing an R web service that is backed by the Python code block or script you provided.  


## Authentication

This section describes how to authenticate to Machine Learning Server on-premises or in the cloud using azureml-model-management-sdk.

In Machine Learning Server, every API call between the Web server and client must be authenticated. The azureml-model-management-sdk functions, which place API calls on your behalf, are no different. Authentication of user identity is handled via Active Directory. Machine Learning Server never stores or manages usernames and passwords.

By default, all azureml-model-management-sdk operations are available to authenticated users. Destructive tasks, such as deleting a web service, are available only to the user who initially created the service.  However, your administrator can also [assign role-based authorization](configure-roles.md) to further control the permissions around web services. 

azureml-model-management-sdk provides a client that supports several ways of authenticating against the Machine Learning Server. 


The function you use depends on the [type of authentication and deployment in your organization](configure-authentication.md). 

### On premises authentication

Use this approach if you are:
+ authenticating using Active Directory server on your network
+ using the [default administrator account](../configure-authentication.md#local) 

```Python 
# Import the DeployClient and MLServer classes from 
# the azureml-model-management-sdk package so you can 
# connect to Machine Learning Server (use=MLServer).

from azureml.deploy import DeployClient
from azureml.deploy.server import MLServer

# Define the location of the Machine Learning Server
# And provide your username and password
HOST = '{{https://YOUR_HOST_ENDPOINT}}'
context = ('{{YOUR_USERNAME}}', '{{YOUR_PASSWORD}}')
client = DeployClient(HOST, use=MLServer, auth=context)
```

|Argument|Description|
|--- | --- |
|host endpoint|The Machine Learning Server HTTP/HTTPS endpoint, including the port number.  You can find this on the first screen when you [launch the administration utility](../configure-use-admin-utility.md#launch).|
|username|If NULL, user is prompted to enter your AD or [local Machine Learning Server](../configure-authentication.md#local) username|
|password|If NULL, user is prompted to enter password|

If you do not know your connection settings, contact your administrator.

This code calls `/user/login` API, which requires a username and password. 


### Cloud authentication (AAD)

Use this approach if you are authenticating using Azure Active Directory in the cloud.

```Python 
# First, import the DeployClient and MLServer classes from 
# the azureml-model-management-sdk package so you can 
# connect to Machine Learning Server (use=MLServer).

from azureml.deploy import DeployClient
from azureml.deploy.server import MLServer

# Define the endpoint of the host Machine Learning Server
HOST = '{{YOUR_HOST_ENDPOINT}}'

# Pass in credentials for the AAD context as a dictionary 
context = {
    'authuri': 'https://login.windows.net',
    'tenant': '{{AAD_DOMAIN}}',
    'clientid': '{{NATIVE_APP_CLIENT_ID}}',
    'resource': '{{WEB_APP_CLIENT_ID}}',
    'username': '{{YOUR_USERNAME}}',  
    'password': '{{YOUR_PASSWORD}}' 
}
​
client = DeployClient(HOST, use=MLServer, auth=context)
```

|Argument|Description|
|--- | --- |
|host endpoint|The Machine Learning Server HTTP/HTTPS endpoint, including the port number.  This endpoint is the SIGN-ON URL value from the web application|
|authuri|The URI of the authentication service for Azure Active Directory.|
|tenant|The tenant ID of the Azure Active Directory account being used to authenticate is the domain of AAD account.|
|clientid|The numeric CLIENT ID of the AAD "native" application for the Azure Active Directory account.|
|resource|The numeric CLIENT ID from the AAD "Web" application for the Azure Active Directory account, also known by the `Audience` in the configuration file.|
|username|If NULL, user is prompted to enter username \<username>@<AAD-account-domain>. If you prefer not to provide the username/password in your script, you are prompted for it at runtime with a device code instead. You must enter that code at https://aka.ms/devicelogin to complete the authentication. |
|password|If NULL, user is prompted to enter password|

>[!NOTE]
>If you do not know your `tenant` id, `clientid`, or other details, contact your administrator. Or, if you have access to the Azure portal for the relevant Azure subscription, you can find [these authentication details](../configure-authentication.md#azure-active-directory). 



<br/>

### Access tokens

After you authenticate with Active Directory or Azure Active Directory, an [access token](../how-to-manage-access-tokens.md) is issued. This access token is then passed in the request header of every subsequent request. Once authenticated, the user does not need to provide credentials again as long as the token is still valid, and a header is submitted with every request. 

Keep in mind that each API call and every azureml-model-management-sdk function requires authentication with Machine Learning Server. If the user does not provide a valid login, an `Unauthorized` HTTP `401` status code is returned. 










Initiate DeployClient and Provide Authentication
The Client supports several ways of authenticating against the ML Server. Below, we are going to demonstrate examples using" Azure Active Directory AAD (username/password) and Azure Active Directory AAD (device code). Please fill your own credentials into the corresponding field.

Below, we are going to demonstrate examples using" Azure Active Directory AAD (username/password) and Azure Active Directory AAD (device code). Please fill your own credentials into the corresponding field.







This article is for data scientists who wants to learn how to publish Python code/models as web services hosted in R Server and how to consume them. This article assumes you are proficient in Python.

<a name="python-auth"></a>

### 1. Add authentication / header logic
Keep in mind that all APIs require authentication; therefore, all users must authenticate when making an API call using the `POST /login` API or through Azure Active Directory (AAD). 

To simplify this process, bearer access tokens are issued so that users need not provide their credentials for every single call.  This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's APIs. After a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. [Learn more about managing these tokens.](../how-to-manage-access-tokens.md) 

Before you interact with the core APIs, first authenticate, get the bearer access token using [the authentication method](../configure-authentication.md) configured by your administrator, and then include it in each header for each subsequent request:

1. Get started by importing the client library to make it accessible your preferred Python code editor, such as Jupyter, Visual Studio, VS Code, or iPython.

   Specify the parent directory of the client library. In our example, the Autorest generated client library is under `C:\Users\rserver-user\Documents\Python\deployrclient`:

   ```python
   # Import the generated client library. 
   import deployrclient
   ```   

1. Add the authentication logic to your application to define a connection from your local machine to R Server, provide credentials, capture the access token, add that token to the header, and use that header for all subsequent requests.  Use the authentication method defined by your R Server administrator: basic admin account, Active Directory/LDAP (AD/LDAP), or Azure Active Directory (AAD).

   **AD/LDAP or 'admin' account authentication**

   You must call the `POST /login` API in order to authenticate. You need to pass in the  `username` and `password` for the local administrator, or if Active Directory is enabled, pass the LDAP account information. In turn, R Server issues you a [bearer/access token](../how-to-manage-access-tokens.md). After authenticated, the user will not need to provide credentials again as long as the token is still valid, and a header is submitted with every request. If you do not know your connection settings, contact your administrator.

 