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


### Example: publish Python web service

This example assumes you have completed Part 1:
+ You have Autorest as your Swagger code generator installed and you are familiar with it.
+ You've already downloaded the Swagger file containing the core operationalization APIs for your version of R Server. 
+ You have already generated a Python client library from that Swagger file.

```python

@@ SAMPLE BEING DEVELOPED

```

## Part 1. Generate client library in Python

1. Install a Swagger code generator on your local machine and familiarize yourself with it. You'll be using it to generate the API client libraries in Python. Popular Swagger code generation tools include [Azure AutoRest](https://github.com/Azure/autorest) (requires node.js) and [Swagger Codegen](https://github.com/swagger-api/swagger-codegen). 

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

1. In the editor, import the generated client library to make it accessible in Python. Specify the parent directory of the client library. In our example, the Autorest generated client library is under `C:\Users\rserver-user\Documents\Python\deployrclient`:

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
     #Using client library generated from Autorest
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

1. Add the `Bearer` access token so that it can be included in every subsequent request header. Then, check that R Server is currently running.  In this example, using a client library generated by Autorest.

     ```python
     #Add the returned access token to the headers variable
     headers = {"Authorization": "Bearer {0}".format(token_response.access_token)}

     #Verify that the server is running
     #Remember to include `headers` in every request!
     status_response = client.status(headers) 
     print(status_response.status_code)
     ```

## 3. Interact with APIs to publish a service

After your client library has been generated and you've build the authentication logic into your application, you can begin to interact with the core operationalization APIs to create a Python session, create a model, and then publish a web service using that model.

1. Create a Python session on R Server by specifying a session name and indicating that you want a Python session (`runtime_type="Python"`). 

   If you don't set the runtime type to Python, it defaults to R.

   This is a continuation of the example using the client library generated by Autorest:

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

1. Create or call a Python model you'll use when you publish the web service.

   ```python
   #Import SVM and Datasets from the SciKitLearn library
   execute_request = deployrclient.models.ExecuteRequest("from sklearn import svm\nfrom sklearn import datasets")
   execute_response = client.execute_code(id,execute_request, headers)
   execute_response.success
   
   #create an untrained Support Vector Classifier object and load the Iris Dataset
   execute_request = deployrclient.models.ExecuteRequest("clf=svm.SVC()\niris=datasets.load_iris()")
   execute_response = client.execute_code(id,execute_request, headers)
   execute_response.success
   
   #Define two rows of sample data
   workspace_object = deployrclient.models.WorkspaceObject("plant_1",[7,3.2,4.7,1.4])
   workspace_object_2 = deployrclient.models.WorkspaceObject("plant_2",[3,2.6,3,2.5])
   
   #Train the model and test it against simple data
   execute_request = deployrclient.models.ExecuteRequest("clf.fit(iris.data, iris.target)\n"+
                                                      "result=clf.predict(plant_1)\n"+
                                                      "other_result=clf.predict(plant_2)"
                                                      ,[workspace_object,workspace_object_2], #Input Parameters
                                                      ["result", "other_result"]) #Output parameter names
   execute_response = client.execute_code(id,execute_request, headers)
   if(execute_response.success):
       for result in execute_response.output_parameters:
           print("{0}: {1}".format(result.name,result.value))
   else:
       print (execute_response.error_message)
   ```



##THE FOLLOWING DOC IS TO BE IGNORED> NOT FOR PYTHON

<a name="clientlib-core"></a>

## Example: Core Client Library from Swagger (in CSharp)

This example shows how you can use the `rserver-swagger-9.1.0.json` swagger file to build a client library to interact with the core operationalization APIs from your application.  

Build and use a core R Server 9.1.0 client library from swagger in CSharp and Azure Active Directory authentication:

1. Download `rserver-swagger-9.1.0.json` from https://microsoft.github.io/deployr-api-docs/swagger/rserver-swagger-9.1.0.json.

1. Build the statically generated client library files for CSharp from the `rserver-swagger-9.1.0.json` swagger. 
   Notice the language is `CSharp` and the namespace is `IO.Swagger.Client`.

   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input rserver-swagger-9.1.0.json -Namespace IO.Swagger.Client
   ```

1. In Visual Studio, add the following `NuGet` package dependencies to your VS project. 
   + `Microsoft.Rest.ClientRuntime`
   + `Microsoft.IdentityModel.Clients.ActiveDirectory`

   Open the Package Manager Console for NuGet and add them with this command:

   ```
   PM> Install-Package Microsoft.Rest.ClientRuntime
   PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

1. Use the statically-generated client library files to call the operationalization APIs. In your application code, import the required namespace types and create an API client to manage the API calls:

   ```
   // --- IMPORT NAMESPACE TYPES -------------------------------------------------------
   // Use the namespace provided with `AutoRest.exe -Namespace IO.Swagger.Client`
   using IO.Swagger.Client;
   using IO.Swagger.Client.Models;
       
   using Microsoft.IdentityModel.Clients.ActiveDirectory;
   using Microsoft.Rest

   // --- CREATE API CLIENT -------------------------------------------------------------
   SwaggerClientTitle client = new SwaggerClientTitle(new Uri("https://rserver.contoso.com:12800"));
   ```

1. Add the authentication workflow to your application.  In this example, the organization has Azure Active Directory.

   Since all APIs require authentication, we first need to obtain our `Bearer` access token such that it can be included in every request header like this:
   ```
   GET /resource HTTP/1.1
   Host: rserver.contoso.com
   Authorization: Bearer mFfl_978_.G5p-4.94gM-
   ```

   In your application code, insert the following:

   ```
   // --- AUTHENTICATE WITH AAD ------------------------------------------------------
   // Note - Update these with your appropriate values
   // Once authenticated, user won't provide credentials again until token is invalid. 
   // You can now begin to interact with the core Op APIs
   // --------------------------------------------------------------------------------

   //
   // ADDRESS OF AUTHORITY ISSUING TOKEN
   //
   const string tenantId = "microsoft.com";
   const string authority = "https://login.windows.net/" + tenantId;
   
   //
   // ID OF CLIENT REQUESTING TOKEN
   //
   const string clientId = "00000000-0000-0000-0000-000000000000";

   //
   // SECRET OF CLIENT REQUESTING TOKEN 
   //
   const string clientKey = "00000000-0000-0000-0000-00000000000";

   var authenticationContext = new AuthenticationContext(authority);
   var authenticationResult = await authenticationContext.AcquireTokenAsync(
          clientId, new ClientCredential(clientId, clientKey));

   //
   // SET AUTHORIZATION HEADER WITH BEARER ACCESS TOKEN FOR FUTURE CALLS
   //
   var headers = client.HttpClient.DefaultRequestHeaders;
   var accessToken = authenticationResult.AccessToken;

   headers.Remove("Authorization");
   headers.Add("Authorization", $"Bearer {accessToken}");
   ```

1. Begin consuming the core operationalization APIs.
   ```
   // --- INVOKE API -----------------------------------------------------------------

   // Try creating an R Session `POST /sessions`
   var createSessionResponse = client.CreateSession(
         new CreateSessionRequest("Session One"));
   
   Console.WriteLine("Session ID: " + createSessionResponse.SessionId);
   ```

<a name="clientlib-service"></a>

## Example: Service Consumption Client Library from Swagger (in CSharp)

This example shows how you can use the `swagger.json` swagger file for version 1.0.0 of a service named `transmission` to build a client library to interact with published service from your application.  

Build and use a service consumption client library from swagger in CSharp and Active Directory LDAP authentication:

1. Get the `swagger.json` for the service you want to consume named `transmission`:
   ```
   GET /api/transmission/1.0.0/swagger.json
   ```

1. Build the statically generated client library files for CSharp from the `swagger.json` swagger. 
   Notice the language is `CSharp` and the namespace is `Transmission`.

   ```
   AutoRest.exe -CodeGenerator CSharp -Modeler Swagger -Input swagger.json -Namespace Transmission
   ```

1. In Visual Studio, add the following `NuGet` package dependencies to your VS project. 
   + `Microsoft.Rest.ClientRuntime`
   + `Microsoft.IdentityModel.Clients.ActiveDirectory`

   Open the Package Manager Console for NuGet and add them with this command:

   ```
   PM> Install-Package Microsoft.Rest.ClientRuntime
   PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

1. Use the statically-generated client library files to call the operationalization APIs. In your application code, import the required namespace types and create an API client to manage the API calls:

   ```
   // --- IMPORT NAMESPACE TYPES -------------------------------------------------------
   // Use the namespace provided with `AutoRest.exe -Namespace Transmission`
   using System;
   
   using Transmission;
   using Transmission.Models;
       
   using Microsoft.IdentityModel.Clients.ActiveDirectory;
   using Microsoft.Rest

   // --- CREATE API CLIENT -------------------------------------------------------------
   Transmission client = new Transmission(new Uri("https://rserver.contoso.com:12800”));
   ```

1. Add the authentication workflow to your application.  In this example, the organization has Active Directory/LDAP.

   Since all APIs require authentication, we first need to obtain our `Bearer` access token such that it can be included in every request header like this:
   ```
   GET /resource HTTP/1.1
   Host: rserver.contoso.com
   Authorization: Bearer mFfl_978_.G5p-4.94gM-
   ```

   In your application code, insert the following:

   ```
   // --- AUTHENTICATE WITH ACTIVE DIRECTORY -----------------------------------------
   // Note - Update these with your appropriate values
   // Once authenticated, user won't provide credentials again until token is invalid. 
   // You can now begin to interact with the operationalization APIs
   // --------------------------------------------------------------------------------
   var loginRequest = new LoginRequest("LDAP_USERNAME", "LDAP_PASSWORD");
   var loginResponse = client.Login(loginRequest);

   //
   // SET AUTHORIZATION HEADER WITH BEARER ACCESS TOKEN FOR FUTURE CALLS
   //
   var headers = client.HttpClient.DefaultRequestHeaders;
   var accessToken = loginResponse.AccessToken;
   headers.Remove("Authorization");
   headers.Add("Authorization", $"Bearer {accessToken}");
   ```

1. Begin consuming the service consumption APIs.
   ```
   // --- INVOKE API -----------------------------------------------------------------
   InputParameters inputs = new InputParameters() { hp = 120, wt = 2.8 };
   var serviceResult = api.Manual.Transmission(inputs).Result;
    
   Console.Out.WriteLine(serviceResult.OutputParameters);
   ```
