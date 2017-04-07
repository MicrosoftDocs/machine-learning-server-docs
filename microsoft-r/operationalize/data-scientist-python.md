---

# required metadata
title: "Publish and consume Python web services | Microsoft R Server Docs"
description: "Publish and consume Python web services with Microsoft R Server"
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


##  Workflow: Publish Python web service

1. Download the core API swagger file. 
1. Generate a statically generated Python client library. you can learn about all of the API calls you can use to publish, manage, and consume Python web services.
1. Import the library into your Python editor.
1. Add authentication logic.
1. Create a Python session.
1. Prepare the environment (load libraries, load data, create models, train models, generate objects, ...)
1. Create a snapshot to preserve the environment.
1. Publish the web service and embed this snapshot.

![Swagger Workflow](../media/o16n/api-swagger-workflow.png)


## Example

This example assumes you have the following (all of which are covered in Part 1):
+ You have Autorest as your Swagger code generator installed and you are familiar with it.
+ You've already downloaded the Swagger file containing the core APIs for your version of R Server. 
+ You have already generated a Python client library from that Swagger file.

```python

@@ SAMPLE BEING DEVELOPED

```

### Part 1. Generate core client library in Python

1. Install a Swagger code generator on your local machine and familiarize yourself with it. You'll be using it to generate the API client libraries in Python. Popular Swagger code generation tools include [Azure AutoRest](https://github.com/Azure/autorest) (requires node.js) and [Swagger Codegen](https://github.com/swagger-api/swagger-codegen). 

1. Download the Swagger file containing the core APIs for your version of R Server from `https://microsoft.github.io/deployr-api-docs/swagger/<version>/rserver-swagger-<version>.json`, where `<version>` is the 3-digit R Server version number. To simplify the integration, R Server provides several Swagger templates each defining the list of resources that are available in the REST API and the operations that can be called on those resources.  
   
   For example, for R Server 9.1 you would download from:
   ```
   https://microsoft.github.io/deployr-api-docs/9.1.0/swagger/rserver-swagger-9.1.0.json
   ```

1. Generate the statically-generated client library by passing the `rserver-swagger-<version>.json` file to the Swagger code generator and specifying the language you want. In this case, you should specify Python.  

   For example, if you use AutoRest to generate a Python client library, it might look like this, where `<version>` is the 3-digit R Server version number:
   ```
   AutoRest.exe -Input rserver-swagger-<version>.json -CodeGenerator Python  -OutputDirectory C:\Users\rserver-user\Documents\Python
   ```

   You can now provide some custom headers and make other changes before using the generated client library stub. See the <a href="https://github.com/Azure/autorest/blob/master/docs/user/cli.md" target="_blank">Command Line Interface</a> documentation for details regarding different configuration options and preferences, such as renaming the namespace.
   
1. Explore the core client library to see the various API calls you can make. 

   In our example, Autorest generated some directories and files for the Python client library on your local system. By default, the namespace (and directory) is `deployrclient` and might look like this:
   
   ![autorest output path](../media/o16n/data-scientist-python-autorest.png)

   For this default namespace, the client library itself is called `deploy_rclient.py`. If you open this file in your IDE such as Visual Studio, you will see something like this:
   
   ![autorest output path](../media/o16n/data-scientist-python-client-library.png)

1. Import the library to make it accessible in the Python code editor of your choice such as Jupyter, Visual Studio, VS Code, or iPython for example.

   Specify the parent directory of the client library. In our example, the Autorest generated client library is under `C:\Users\rserver-user\Documents\Python\deployrclient`:

   ```python
   # Import the generated client library. 
   import deployrclient
   ```   

### Part 2. Add authentication and header logic to your script

Keep in mind that all APIs require authentication; therefore, all users must authenticate when making an API call using the `POST /login` API or through Azure Active Directory (AAD). 

To simplify this process, bearer access tokens are issued so that users need not provide their credentials for every since call.  This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's APIs. After a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties. [Learn more about managing these tokens.](security-access-tokens.md) 

Before you interact with the core APIs, first authenticate, get the bearer access token using [the authentication method](security-authentication.md) configured by your administrator, and then include it in each header for each subsequent request:

1. Add the authentication logic to the script to define a connection from your local machine to R Server, provide credentials, capture the access token, add that token to the header, and use that header for all subsequent requests.  Use the authentication method defined by your R Server administrator: basic admin account, Active Directory/LDAP (AD/LDAP), or Azure Active Directory (AAD).

   + **For AD/LDAP or the `admin` account authentication**, you must call the `POST /login` API in order to authenticate. You'll need to pass in the  `username` and `password` for the local administrator, or if Active Directory is enabled, pass the LDAP account information. In turn, R Server will issue you a [bearer/access token](security-access-tokens.md). After authenticated, the user will not need to provide credentials again as long as the token is still valid and a header is submitted with every request.

     ```python
     #Using client library generated from Autorest
     #Create client instance and point it at an R Server. In this case, R Server is local.
     client = deployrclient.DeployRClient("http://localhost:12800")

     #Define the login request and provide credentials 
     login_request = deployrclient.models.LoginRequest("<<your-username>>","<<your-password>>")
     #Make a call to the /login API. Store the returned an access token in a variable
     token_response = client.login(login_request)
     ```

   + **For Azure Active Directory (AAD)**, you must pass the AAD credentials, authority, and client ID. In turn, AAD will issue [the `Bearer` access token](security-access-tokens.md). After authenticated, the user will not need to provide credentials again as long as the token is still valid and a header is submitted with every request. 
   
     If you do not know your connection settings, please contact your administrator.

     ```python
     #Import the AAD authentication library
     import adal

     #Provide the AAD connection parameters your admin gave you
     url = "https://deployr-dogfood.mrs.microsoft-tst.com"
     authuri = https://login.windows.net,
     tenantid = "<AAD_DOMAIN>", 
     clientid = "<NATIVE_APP_CLIENT_ID>", 
     resource = "<WEB_APP_CLIENT_ID>", 

     #Acquire authentication token using AAD Device Code Login
     context = adal.AuthenticationContext(authuri+'/'+tenantid, api_version=None)
     code = context.acquire_user_code(resource, clientid)
     print(code['message'])
     token = context.acquire_token_with_device_code(resource, code, clientid)
     #The authentication code returned must be entered at https://aka.ms/devicelogin 
     ```     

     @@@ Where do we set session true or false???

1. Add the `Bearer` access token and check that R Server is currently running.  This token was returned during authentication and **must be included in every subsequent request header**. This example uses a client library generated by Autorest.

     >[!IMPORTANT]
     >Every API call must be authenticated. Therefore, remember to include headers with tokens to every single request.

     ```python
     #Add the returned access token to the headers variable
     headers = {"Authorization": "Bearer {0}".format(token_response.access_token)}

     #Verify that the server is running
     #Remember to include `headers` in every request!
     status_response = client.status(headers) 
     print(status_response.status_code)
     ```

    @@You USED THE FOLLOWING IN YOUR AAD EXAMPLE: `headers = {"Authorization":"Bearer "+token["accessToken"]}`  Which is better?????



### Part 3. Create a session, the model, and a snapshot

After authentication, you can start a Python session and create a model you'll publish later. You can include any Python code or models in a web service. Once you've set up your session environment, you can even save it as a snapshot so you can reload your session as you had it before. 

>[!IMPORTANT]
>Remember to include `headers` in every request.

1. Create a Python session on R Server. You must specify a name and the Python language (`runtime_type="Python"`). 

   If you don't set the runtime type to Python, it defaults to R.

   This is a continuation of the example using the client library generated by Autorest:

   ```python
   #Since already logged into R Server, create a Python session.
   #Define session using name (`Session 1`) and type `runtime_type="Python"`.
   #Remember to specify the Python runtime type.
   create_session_request = deployrclient.models.CreateSessionRequest("Session 1", runtime_type="Python")

   #Make the call to start the session. 
   #Remember to include headers in every method call to the server.
   #Returns a session ID.
   response = client.create_session(create_session_request, headers) 
   
   #Store the session ID in a variable called `session_id` 
   #to be able to identify it later at execution time.
   session_id = response.session_id
   ```

1. Create or call a model in Python that you'll publish as a web service. 

   In this example, we train a SciKit-Learn support vector machines (SVM) model on the Iris Dataset on a remote R Server.  This dataset is available here: http://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html 

   ```python
   #Import SVM and datasets from the SciKit-Learn library
   execute_request = deployrclient.models.ExecuteRequest("from sklearn import svm\nfrom sklearn import datasets")
   execute_response = client.execute_code(session_id,execute_request, headers)
   #Report if it was a success
   execute_response.success
   
   #Define the untrained Support Vector Classifier (SVC) object and the dataset to be preloaded
   execute_request = deployrclient.models.ExecuteRequest("clf=svm.SVC()\niris=datasets.load_iris()")
   #Now, go create the object and preload Iris Dataset in R Server
   execute_response = client.execute_code(session_id,execute_request, headers)
   #Report if it was a success
   execute_response.success
   
   #Define two rows from the Iris Dataset as a sample for scoring
   workspace_object = deployrclient.models.WorkspaceObject("variety_1",[7,3.2,4.7,1.4])
   workspace_object_2 = deployrclient.models.WorkspaceObject("variety_2",[3,2.6,3,2.5])

   #Define how to train the classifier model; what to predict; what to return
   execute_request = deployrclient.models.ExecuteRequest("clf.fit(iris.data, iris.target)\n"+
                                                      "result=clf.predict(variety_1)\n"+
                                                      "other_result=clf.predict(variety_2)"
                                                      ,[workspace_object,workspace_object_2], #Input Parameters
                                                      ["result", "other_result"]) #Output parameter names
   #Now, go train that model on the Iris Dataset in R Server
   execute_response = client.execute_code(session_id,execute_request, headers)

   #If successful, print name and result of each output parameter. Else, print error.
   if(execute_response.success):
       for result in execute_response.output_parameters:
           print("{0}: {1}".format(result.name,result.value))
   else:
       print (execute_response.error_message)
   ```

1. Create a snapshot of this Python session so this environment can be saved in the web service and reproduce at consume time. Note that 
   you can only include a snapshot that you've created when publishing.

   Snapshots are very useful when you need a prepared environment that includes certain libraries, objects, models, files and artifacts. Snapshots save the whole workspace and working directory. 

   When publishing, you can only include a snapshot you've created. 

   > [!NOTE] 
   > While snapshots can also be used when publishing a web service for environment dependencies, it may have an impact on the performance of the consumption time.  For optimal performance, consider the size of the snapshot carefully and ensure that you keep only those workspace objects you need and purge the rest. In a session, you can use the Python `del` function or [the `deleteWorkspaceObject` API request](https://microsoft.github.io/deployr-api-docs/#delete-workspace-object) to remove unnecessary objects. 

   ```python
   #Create a snapshot of the current session
   client.create_snapshot(session_id, deployrclient.models.CreateSnapshotRequest("Iris Snapshot"), headers)

   #Return the snapshot ID so you can reference it in the publish service function later.
   response.snapshot_id

   #If you forget the ID later, you can always list every snapshot to get the ID again.
   for snapshot in client.list_snapshots(headers):
       print(snapshot)
   ```

### Part 4. Publish as a web service in Python

After your client library has been generated and you've built the authentication logic into your application, you can interact with the core APIs to create a Python session, create a model, and then publish a web service using that model.

>[!NOTE]
>Remember that you must be authenticated before you make any API calls. Therefore, include `headers` in every request.

1. Publish this SVM model as a Python web service in R Server. This web service will score a vector that gets passed to it.

   >[!IMPORTANT]
   > To ensure that the web service is registered as a Python service, be sure to specify `runtime_type="Python"`. If you don't set the runtime type to Python, it defaults to R.

   ```python
   #Define a web service that determines the iris variety by scoring 
   #a vector of sepal length and width, petal length and width

   #Set `flower_data` for the sepal and petal length and width
   flower_data = deployrclient.models.ParameterDefinition(name = "flower_data", type = "vector")
   #Set `iris_type` for the variety of iris
   iris_type = deployrclient.models.ParameterDefinition(name = "iris_type", type = "vector")

   #Define the publish request for the web service and its arguments.
   #Specify the code, inputs, outputs, snapshot, and runtime_type="Python"`.
   publish_request = deployrclient.models.PublishWebServiceRequest(
       code = "iris_type = [names[x] for x in clf.predict(flower_data)]", 
       input_parameter_definitions = [flower_data], 
       output_parameter_definitions = [iris_type],
       runtime_type = "Python",
       snapshot_id = response.snapshot_id)

   #Publish the service using the specified name (iris), version (V1.0)
   client.publish_web_service_version("Iris", "V1.0", publish_request, headers)
   ```

1. Get and examine the service holdings and metadata for the service.

   @@ DOES THIS GET ALL SERVICES OR JUST Iris V1.0?

   ```python
   # Inspect holdings and metadata for service Iris V1.0.
   for service in client.get_all_web_services(headers):
       #print the service information
       print(service)
       #Print each input and output parameter
       print("Input Parameters: {0}".format([str(parameter) for parameter in service.input_parameter_definitions]))
       print("Output Parameters: {0}".format([str(parameter) for parameter in service.output_parameter_definitions]))
   ```


<br>
<br>
<br>
```python
###This can all be replaced by generating a client library with autorest
#Import the requests library to make requests on the server
import requests
#Import the JSON library to use for pretty printing of json responses
import json

#Create a requests `Session` object.
s = requests.Session()

#Record the URL  @@@OF WHAT AND FOR WHAT???
url = "http://localhost:12800"

#Give it the authentication headers
s.headers = headers
## @@WHAT IS THIS?????
resp = s.get(url+"/api/Iris/V1.0/swagger.json")
#Print out the valid paths  @@PATHS OF WHAT???
# @@DOES THIS PRINT THE WHOLE SWAGGER TO SCREEN
print(json.dumps(resp.json()["paths"], indent = 1, sort_keys = True))
```

@@ WHICH LINE OR LINES PRINTS OUT THE SERVICE SPECIFIC SWAGGER FILE CONTENTS??


```python
# print out the input and output parameters
print("Input")
print(json.dumps(resp.json()["definitions"]["InputParameters"], indent = 1, sort_keys = True))
print("Output")
print(json.dumps(resp.json()["definitions"]["WebServiceResult"], indent = 1, sort_keys = True))
```

 <br>

```python
## @@ WHAT IS THIS DOING
resp = s.post(url+"/api/Iris/V1.0",json={"flower_data":[7,3.2,4.7,1.4]})
## @@ WHAT IS THIS DOING... WHY DON'T YOU HAVE PATHS LIKE ABOVE??  DOES IT ONLY PRINT A SMALL PIECE OF THE SWAGGER TO THE SCREEN
print(json.dumps(resp.json(), indent = 1, sort_keys = True))
```
<br>

```python
## @@ WHAT IS THIS DOING THAT THE PREVIOUS SECTION DID NOT
resp = s.post(url+"/api/Iris/V1.0",json={"flower_data":[3,2.6,3,2.5]})
print(json.dumps(resp.json(), indent = 1, sort_keys = True))
```

<br>

```python
resp = s.post(url+"/api/Iris/V1.0",json={"flower_data":[5.1,3.5,1.4,.2]})
print(json.dumps(resp.json(), indent = 1, sort_keys = True))
```
<br>
@@HOW DO I DOWNLOAD THE SWAGGER FILE TO MY MACHINE

<br>
1. Update the web service to add a description useful to people who might consume this service. You can update the description, code, inputs, outputs, models, and even the snapshot. 

   ```python
   #Define what needs to be updated. Here we add a description.
   update_request = deployrclient.models.UpdateWebServiceRequest(
       description = "Determines iris type using length and width of sepal and petal")

   #Update the service using its name and version number
   client.update_web_service_version("Iris", "V1.0", update_request, headers)
   ```
<br>
@@ EASY EXAMPLE OF SOMETHING WE COULD CHANGE TO PUBLISH A V2.0 OF THIS. I'LL THEN DELETE 2.0 LATER
<br>
@@CODE EXAMPLE FOR LIST SERVICES

<br>
```python
#Deletes the second version of the service
s.delete(url+"/services/Iris/V2.0")
```













##  Workflow: Consume Python web service

Download the core API swagger file. 
Generate the python client library. you can learn about all of the API calls you can use to publish, manage, and consume Python web services.

![Swagger Workflow](../media/o16n/api-swagger-workflow.png)


### Example

This example assumes you have the following (all of which are covered in Part 1):
+ You have Autorest as your Swagger code generator installed and you are familiar with it.
+ You've already downloaded the Swagger file containing the core APIs for your version of R Server. 
+ You have already generated a Python client library from that Swagger file.

```python

@@ SAMPLE BEING DEVELOPED


## THE FOLLOWING DOC IS TO BE IGNORED> NOT FOR PYTHON

## IGNORE THE REST
<a name="clientlib-core"></a>

## Example: Core Client Library from Swagger (in CSharp)

This example shows how you can use the `rserver-swagger-9.1.0.json` swagger file to build a client library to interact with the core APIs from your application.  

1. In Visual Studio, add the following `NuGet` package dependencies to your VS project. 
   + `Microsoft.Rest.ClientRuntime`
   + `Microsoft.IdentityModel.Clients.ActiveDirectory`

   Open the Package Manager Console for NuGet and add them with this command:

   ```
   PM> Install-Package Microsoft.Rest.ClientRuntime
   PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

1. Use the statically-generated client library files to call the APIs. In your application code, import the required namespace types and create an API client to manage the API calls:

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

1. Begin consuming the core APIs.
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

1. Use the statically-generated client library files to call the APIs. In your application code, import the required namespace types and create an API client to manage the API calls:

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
   // You can now begin to interact with the APIs
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
