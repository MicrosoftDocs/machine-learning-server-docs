---

# required metadata
title: "Publish and consume Python web services - Machine Learning Server | Microsoft Docs"
description: "Publish and consume Python web services with Microsoft R Server"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "6/21/2017"
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

# Publish and consume Python web services

**Applies to:  SQL Server 2017 CTP 2.0 (Public Preview)**

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
   ```python
   #Using client library generated from Autorest
   #Create client instance and point it at an R Server. 
   #In this case, R Server is local.
   client = deployrclient.DeployRClient("http://localhost:12800")

   #Define the login request and provide credentials. 
   #Update values with the connection parameters from your admin.
   login_request = deployrclient.models.LoginRequest("<<your-username>>","<<your-password>>")
   #Make a call to the /login API. 
   #Store the returned an access token in a variable.
   token_response = client.login(login_request)
   ```

   **Azure Active Directory (AAD) authentication**

   You must pass the AAD credentials, authority, and client ID. In turn, AAD issues [the `Bearer` access token](../how-to-manage-access-tokens.md). After authenticated, the user will not need to provide credentials again as long as the token is still valid, and a header is submitted with every request. If you do not know your connection settings, contact your administrator.
   ```python
   #Import the AAD authentication library
   import adal

   #Define the login request and provide credentials. 
   #Use the AAD connection parameters provided by your admin.
   url = "http://localhost:12800"
   authuri = https://login.windows.net,
   tenantid = "<<AAD_DOMAIN>>", 
   clientid = "<<NATIVE_APP_CLIENT_ID>>", 
   resource = "<<WEB_APP_CLIENT_ID>>", 

   #Acquire authentication token using AAD Device Code Login
   context = adal.AuthenticationContext(authuri+'/'+tenantid, api_version=None)
   code = context.acquire_user_code(resource, clientid)
   print(code['message'])
   token = context.acquire_token_with_device_code(resource, code, clientid)
   #The authentication code returned must be entered at https://aka.ms/devicelogin 
   ```     

1. Add the `Bearer` access token and check that R Server is currently running.  This token was returned during authentication and **must be included in every subsequent request header**. This example uses a client library generated by Autorest.

   >[!IMPORTANT]
   >Every API call must be authenticated. Therefore, remember to include headers with tokens to every single request.

   ```python
   #Add the returned access token to the headers variable.
   headers = {"Authorization": "Bearer {0}".format(token_response.access_token)}

   #Verify that the server is running.
   #Remember to include `headers` in every request!
   status_response = client.status(headers) 
   print(status_response.status_code)
   ```