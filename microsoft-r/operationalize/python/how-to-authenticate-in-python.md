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

The [azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) package, delivered with Machine Learning Server, provides functions for publishing and managing a Python web service that is backed by the Python code block or script you provided.  

This section describes how to authenticate to Machine Learning Server on-premises or in the cloud using azureml-model-management-sdk so you can take advantage of these classes and functions for web services. Every API call between the Web server and client must be authenticated. The azureml-model-management-sdk functions, which place API calls on your behalf, are no different. If the user does not provide a valid login, an `Unauthorized` HTTP `401` status code is returned. 

azureml-model-management-sdk provides the client that supports several ways of authenticating against the Machine Learning Server. The arguments you use depends on the [type of authentication and deployment in your organization](../configure-authentication.md). Authentication of user identity is handled via Active Directory. Machine Learning Server never stores or manages any usernames and passwords.

By default, all azureml-model-management-sdk operations are available to authenticated users. Destructive tasks, such as deleting a web service, are available only to the user who initially created the service.  However, your administrator can also [assign role-based authorization](../configure-roles.md) to further control the permissions around web services. 

## On premises authentication

Use this approach if you are:
+ authenticating using Active Directory server on your network
+ using the [default administrator account](../configure-authentication.md#local) 

Be sure to pass credentials for AD or local as a tuple.

```Python 
# Import the DeployClient and MLServer classes from 
# the azureml-model-management-sdk package so you can 
# connect to Machine Learning Server (use=MLServer).

from azureml.deploy import DeployClient
from azureml.deploy.server import MLServer

# Define the location of the Machine Learning Server
HOST = '{{https://YOUR_HOST_ENDPOINT}}'
# And provide your username and password as a Python tuple
context = ('{{YOUR_USERNAME}}', '{{YOUR_PASSWORD}}')
client = DeployClient(HOST, use=MLServer, auth=context)
```

|Argument|Description|
|--- | --- |
|host endpoint|The Machine Learning Server HTTP/HTTPS endpoint, including the port number.  You can find this endpoint on the first screen when you [launch the administration utility](../configure-use-admin-utility.md#launch).|
|username|Enter your AD or [local Machine Learning Server](../configure-authentication.md#local) username. Username is required.|
|password|Enter the password. This value is required.|

>[!NOTE]
>If you do not know your connection settings, contact your administrator. This code calls the `/user/login` API and requires a username and password. 


## Cloud authentication (AAD)

Use this approach if you are authenticating using Azure Active Directory in the cloud.

Be sure to pass credentials for AAD as a dictionary {}.

```Python 
# First, import the DeployClient and MLServer classes from 
# the azureml-model-management-sdk package so you can 
# connect to Machine Learning Server (use=MLServer).

from azureml.deploy import DeployClient
from azureml.deploy.server import MLServer

# Define the endpoint of the host Machine Learning Server.
HOST = '{{YOUR_HOST_ENDPOINT}}'

# Pass in credentials for the AAD context as a dictionary. 
# Omit username & password to use ADAL to authenticate. 
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

If you do not know your tenant id, clientid, or other details, contact your administrator. Or, if you have access to the Azure portal for the relevant Azure subscription, you can find [these authentication details](../configure-authentication.md#azure-active-directory). This code calls the `/user/login` API and requires a username and password. 

|Argument|Description|
|--- | --- |
|host endpoint|The Machine Learning Server HTTP/HTTPS endpoint, including the port number.  This endpoint is the SIGN-ON URL value from the web application|
|authuri|The URI of the authentication service for Azure Active Directory.|
|tenant|The tenant ID of the Azure Active Directory account being used to authenticate is the domain of AAD account.|
|clientid|The numeric CLIENT ID of the AAD "native" application for the Azure Active Directory account.|
|resource|The numeric CLIENT ID from the AAD "Web" application for the Azure Active Directory account, also known by the `Audience` in the configuration file.|
|username|If NULL, user is prompted to enter username \<username>@\<AAD-account-domain>. |
|password|If NULL, user is prompted to enter password|

**Alternatives to putting the username and password in the script**

If you omit the username and password from the dictionary for the AAD context, then you can either:

+ Get a device code to complete authentication and token creation via Azure Active Directory Authentication Libraries (ADAL). Enter that code at https://aka.ms/devicelogin to complete the authentication. 

+ Programmatically authenticate using a call back. Use this code like this: 
  ```Python
  def callback_fn(code):
      print(code)

  context = {
      'authuri': 'https://login.windows.net',
      'tenant': '{{AAD_DOMAIN}}',
      'clientid': '{{NATIVE_APP_CLIENT_ID}}',
      'resource': '{{WEB_APP_CLIENT_ID}}',
      'user_code_callback': callback_fn 
  }
  ```

## Access tokens

Keep in mind that all APIs require authentication; therefore, all users must authenticate when making an API call using the `POST /login` API or through Active Directory. 

To simplify this process, bearer access tokens are issued so that users need not provide their credentials for every single call.  This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, Machine Learning Server's APIs. After authentication, the user does not need to provide credentials again as long as the token is still valid, and a header is submitted with every request.  The application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. [Learn more about managing these tokens.](../how-to-manage-access-tokens.md) 