---

# required metadata
title: "Enterprise-Grade Security: Authentication | Microsoft R Server Docs"
description: "Enterprise-Grade Security: Authentication for Operationalization with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "03/25/2017"
ms.topic: "article"
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

# Publish and consume Python web services

**Applies to:  Microsoft R Server 9.1**

This article is for data scientists who wants to learn how to publish Python code/models as web services hosted in R Server and how to consume them. This article assumes you are proficient in Python.

## Supported platforms

Python web services are supported on Windows platforms on which Python was enabled during installation. Expanded platform support in future releases.


## Swagger workflow

Download the core API swagger file. 
Generate the python client library. you can learn about all of the API calls you can use to publish, manage, and consume Python web services.

![Swagger Workflow](../media/o16n/api-swagger-workflow.png)

## 1. Generate client library in Python

1. Install a Swagger code generator on your local machine and familiarize yourself with it. You'll be using it to generate the API client libraries in Python. Popular Swagger code generation tools include [Azure AutoRest](https://github.com/Azure/autorest) (requires Visual Studio or chocolatey) and [Swagger Codegen](https://github.com/swagger-api/swagger-codegen). 

1. Download the Swagger file containing the core operationalization APIs for your version of R Server from `https://microsoft.github.io/deployr-api-docs/swagger/rserver-swagger-<version>.json`, where `<version>` is the 3-digit R Server version number. To simplify the integration, R Server provides several Swagger templates each defining the list of resources that are available in the REST API and the operations that can be called on those resources.  
   
   For example, for R Server 9.1.0 you would download from:
   ```
   https://microsoft.github.io/deployr-api-docs/swagger/rserver-swagger-9.1.0.json
   ```

1. Generate the client library by passing the `rserver-swagger-<version>.json` file to the Swagger code generator and specifying the language you want. In this case, you should specify Python.  

   For example, if you use AutoRest to generate a Python client library, it might look like this, where `<version>` is the 3-digit R Server version number:
   ```
   AutoRest.exe -Input rserver-swagger-<version>.json -CodeGenerator Python  -OutputDirectory C:\Users\rserver-user\Documents\Python
   ```

   You can now provide some custom headers and make other changes before using the generated client library stub. See the <a href="https://github.com/Azure/autorest/blob/master/docs/user/cli.md" target="_blank">Command Line Interface</a> documentation for details regarding different configuration options and preferences, such as renaming the namespace.
   
1. Explore the client library to see the various API calls you can make. 

   In our example, Autorest generated some directories and files for the Python client library on your local system. By default, the namespace (and directory) is `deployrclient` and might look like this:
   
   ![autorest output path](../media/o16n/data-scientist-python-autorest.png)

   For this default namespace, the client library itself is called `deploy_rclient.py`. If you open this file in your IDE such as Visual Studio, you will see something like this:
   
   ![autorest output path](../media/o16n/data-scientist-python-client-library.png)

## 2. Add authentication and header logic to your script

Keep in mind that all APIs require authentication; therefore, all users must authenticate when making an API call using the `POST /login` API or through Azure Active Directory (AAD). 

To simplify this process, bearer access tokens are issued so that users need not provide their credentials for every since call.  This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's operationalization APIs. After a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. [Learn more about managing these tokens.](security-access-tokens.md) 

Before you interact with the core APIs, first authenticate, get the bearer access token using [the authentication method](security-authentication.md) your administrator configured for operationalization, and then include it in each header for each subsequent request:


1. Open the Python code editor of your choice such as Jupyter, Visual Studio, VS Code, or iPython for example.

1. In the editor, import the generated client library to make it accessible in Python. Specify the parent directory of the client library. In our example, the client library is under `C:\Users\rserver-user\Documents\Python\deployrclient`:

@@# WHY NOT A COMPLETE PATH. HOW DOES IT KNOW WHERE DEPLOYRCLIENT IS??

```python
# Import the generated client library. 
import deployrclient
```

1. Add the authentication logic to the script to define a connection from your local machine to R Server, provide credentials, capture the access token, add that token to the header, and use that header for all subsequent requests.  Use the authentication method defined by your R Server administrator: basic admin account, Active Directory/LDAP (AD/LDAP), or Azure Active Directory (AAD).

   + For AD/LDAP or the `admin` account authentication:

     For these authentication methods, you must call the `POST /login` API in order to authenticate. You'll need to pass in the  `username` and `password` for the local administrator, or if Active Directory is enabled, pass the LDAP account information. In turn, R Server will issue you a [bearer/access token](security-access-tokens.md). After authenticated, the user will not need to provide credentials again as long as the token is still valid and a header is submitted with every request.

     >[!IMPORTANT]
     >Every call must be authenticated. Therefore, remember to include headers with tokens to every single request.

     ```python
     #Create client instance and point it at an R Server. In this case, R Server is local.
     client = deployrclient.DeployRClient("http://localhost:12800")

     #Define the login request and provide credentials 
     login_request = deployrclient.models.LoginRequest("rserver-user","1@2@3@4@5@6@7")
     #Make a call to the /login API. Store the returned an access token in a variable
     token_response = client.login(login_request)
     ```

   + For Azure Active Directory (AAD): 

     For AAD, you must pass the AAD credentials, authority, and client ID. In turn, AAD will issue [the `Bearer` access token](security-access-tokens.md). After authenticated, the user will not need to provide credentials again as long as the token is still valid and a header is submitted with every request.

     >[!IMPORTANT]
     >Every call must be authenticated. Therefore, remember to include headers with tokens to every single request.

     ```python
     ????@@
     ```     

1. Add the `Bearer` access token so that it can be included in every subsequent request header. Then, check that R Server is currently running.

     ```python
     #Add the returned access token to the headers variable
     headers = {"Authorization": "Bearer {0}".format(token_response.access_token)}

     #Verify that the server is running
     #Remember to include `headers` in every request!
     status_response = client.status(headers) 
     print(status_response.status_code)
     ```

## 3. 
After your client library has been generated and you've build the authentication logic into your application, you can begin to interact with the core operationalization APIs to create a Python session, create a model, and then publish a web service using that model.

1. Create a Python session on R Server by specifying a session name and indicating that you want a Python session (`runtime_type="Python"`). If you don't set the runtime type to Python, it defaults to R.

```python
#Since already logged into R Server, create a Python session.
#Define session using name (`Session 1`) and type `runtime_type="Python"`.
create_session_request = deployrclient.models.CreateSessionRequest("Session 1", runtime_type="Python") # Don't forget the runtime type
#Start the session. 
#Remember to include headers in every method call to the server
#Returns a session ID.
response = client.create_session(create_session_request, headers) 

#Store the session ID in a variable called `id`
id = response.session_id
```
