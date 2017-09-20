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

# List, get, and consume web services in Python

**Applies to:  Machine Learning Server**

This article is for data scientists who wants to learn how to find, examine, and consume the [analytic web services](../concept-what-are-web-services.md) hosted in Machine Learning Server using Python. Web services offer fast execution and scoring of arbitrary Python or R code and models. [Learn more about web services](../concept-what-are-web-services.md). This article assumes that you are proficient in Python.

After a web service has been published, any authenticated user can list, examine, and consume that web service. You can do so directly in Python using the functions in the [azureml-model-management-sdk package](../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) or in a [preferred language via Swagger](../how-to-build-api-clients-from-swagger-for-app-integration.md). The azureml-model-management-sdk package is installed with Machine Learning Server.  To list, examine, or consume the web service _outside_ of Python, use the [RESTful APIs](concept-api.md) that provide direct programmatic access to a service's lifecycle.

By default, web service operations are available to authenticated users. However, your administrator can also assign [roles](../configure-roles.md)  (RBAC) to further control the permissions around web services. 

<a name="auth"></a>

## Requirements

Before you can use the web service management functions in the [azureml-model-management-sdk](../../python-reference/azureml-model-management-sdk/azureml-model-management-sdk.md) Python package, you must:
+ Have access to a Python-enabled instance of Machine Learning Server that was  [properly configured](../../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) to host web services. 

+ Authenticate with Machine Learning Server in Python as described in "[Connecting to Machine Learning Server in Python](how-to-authenticate-in-python.md)."

<a name="list_services"></a>

## Find and list web services

Any authenticated user can retrieve a list of web services using the `list_services` function on the DeployClient object. You can use arguments to return a specific web service or all labeled versions of a given web service. 
 
```Python
## -- Return metadata for all services hosted on this server
client.list_services()

## -- Return metadata for all versions of service "myService" 
client.list_services('myService')

## -- Return metadata for a specific version "v1.0" of service "myService" 
client.list_services('myService', version='v1.0')
```


Once you find the service you want, use the [get_service](#get_service)  function to retrieve the service object for consumption.

<a name="get_service"></a>

## Retrieve and examine service objects

Authenticated users can retrieve the [web service object](../../python-reference/azureml-model-management-sdk/service.md) in order to get the client stub for consuming that service. Use the `get_service` function from the azureml-model-management-sdk package to retrieve the object. 

After the object is returned, you can use a help function to explore the published service, such as `print(help(myServiceObject))`. You can call the help function on any azureml-model-management-sdk functions, even those that are dynamically generated to learn more about them. 

You can also print the capabilities that define the service holdings to see what the service can do and how it should be consumed. Service holdings include the service name, version, descriptions, inputs, outputs, and the name of the function to be consumed. 

You can only see the code stored within a web service if you have published the web service or are assigned to the "Owner" role. To learn more [about roles](../configure-roles.md) in your organization, contact your Machine Learning Server administrator.

Example code:

```Python
# -- List all versions of the service 'myService'--
client.list_services('myService')

# -- Now get the service object for myService v2.0
svc = client.get_service('myService', version='v2.0')
# -- Learn more about that service.
print(help(svc))
# -- View the service capabilities/schema for this service
svc.capabilities()
```

####Interact with API clients

You can use the following supported public functions to interact with the DeployClient API instance.

| Function      | Description                                            |
| ------------- |--------------------------------------------------------|
| print       |	Print method that lists all members of the object      |
| capabilities | Report on the service features such as I/O schema, name, version	   |
| swagger     |	Displays the service's swagger specification, such as 'swagger(json=True)'         |
| batch |Define the data records to be batched. There are additional publish functions used to consume a service asynchronously via batch execution. For example, 'batch(records, parallel_count=10)'|


<a name="consume-service"></a>

## Consume web services 

Web services are published to facilitate the consumption and integration of the operationalized models and code.  Whenever the web service is published or updated, a Swagger-based JSON file is generated automatically that define the service.

When you publish a service, you can let people know that it is ready for consumption. Users can get the Swagger file they need to consume the service directly in R or via the API. To make it easy for others to find your service, provide them with the service name and version number (or they can use [the list_services function](#list_services)).

Users can consume the service directly using a single consumption call. This approach is referred to as a "Request Response" approach and is described in the following section. Another approach is the [asynchronous "Batch" consumption approach](how-to-consume-web-service-asynchronously-batch.md), where users send as a single request to Machine Learning Server, which then makes multiple asynchronous API calls on your behalf.
  
<a name="data-scientists-share"></a>

#### Collaborate with data scientists

Other data scientists may want to explore, test, and consume Web services directly in R using the functions in the mrsdeploy package. Quality engineers might want to bring the models in these web services into validation and monitoring cycles.

You can share the name and version of a web service with fellow data scientists so they can call that service in R using the functions in the mrsdeploy package.  After authenticating, data scientists can use the getService() function in R to call the service. Then, they can get details about the service and start consuming it.

You can also [build a client library directly in R using the httr package](https://blogs.msdn.microsoft.com/rserver/2017/07/20/using-r-to-generate-api-client-from-swagger/).

>[!NOTE]
> It is also possible to perform batch consumption as [described here](how-to-consume-web-service-asynchronously-batch.md).


In this example, replace the following remoteLogin() function with the correct login details for your configuration. Connecting to Machine Learning Server using the mrsdeploy package is covered [in this article](how-to-connect-log-in-with-mrsdeploy.md).

```R
##########################################################################
#      Perform Request-Response Consumption & Get Swagger Back in R      #
##########################################################################

# Use `remoteLogin` to authenticate with the server using the local admin 
# account. Use session = false so no remote R session started
remoteLogin("http://localhost:12800", 
            username = “admin”, 
            password = “{{YOUR_PASSWORD}}”,
            session = FALSE)
   
# Get service using getService() function from `mrsdeploy`
# Assign service to the variable `api`.
api <- getService("mtService", "v1.0.0")

# Print capabilities to see what service can do.
print(api$capabilities())

# Start interacting with the service, for example:
# Calling the function, `manualTransmission` contained in this service.
result <- api$manualTransmission(120, 2.8)

# Print response output named `answer`
print(result$output("answer")) # 0.6418125  

# Since you're authenticated now, get `swagger.json`.
swagger <- api$swagger()
cat(swagger, file = "swagger.json", append = FALSE)
```

<a name="swagger-app-dev"></a>

#### Collaborate with application developers

Application developers can call and integrate a web service into their applications using the service-specific Swagger-based JSON file and by providing any required inputs to that service. 

Using the Swagger-based JSON file, application developers can generate client libraries for integration. Read "[How to integrate web services and authentication into your application](how-to-build-api-clients-from-swagger-for-app-integration.md)" for more details.  
   
Application developers can get the Swagger-based JSON file in one of these ways:

+ A data scientist, probably the one who published the service, can send you the Swagger-based JSON file. This approach is often faster than the following one. They can get the file in R with the following code and send it to the application developer:
   ```R
   api <- getService("<name>", "<version>")
   swagger <- api$swagger()
   cat(swagger, file = "swagger.json", append = FALSE) 
   ```

+ Or, the application developer can request the file  **as an authenticated user with an [active bearer token](how-to-build-api-clients-from-swagger-for-app-integration.md#authentication) in the request header** (since all API calls must be authenticated). The URL is formed as follows:
  ```
  GET /api/<service-name>/<service-version>/swagger.json
  ```


## See also

+ [mrsdeploy function overview](../r-reference/mrsdeploy/mrsdeploy-package.md)
+ [How to publish and manage web services in R](how-to-deploy-web-service-publish-manage-in-r.md)
+ [Quickstart: Deploying an R model as a web service](quickstart-publish-r-web-service.md)
+ [Connecting to Machine Learning Server from mrsdeploy](how-to-connect-log-in-with-mrsdeploy.md).
+ [Get started guide for data scientists](concept-operationalize-deploy-consume.md)
+ [How to integrate web services and authentication into your application](how-to-build-api-clients-from-swagger-for-app-integration.md)
+ [Asynchronous batch execution of web services in R](how-to-consume-web-service-asynchronously-batch.md)
+ [Execute on a remote Machine Learning Server](../r/how-to-execute-code-remotely.md)










<!--
**Applies to:  SQL Server 2017 CTP 2.0 (Public Preview)**

This article is for data scientists who wants to learn how to publish Python code/models as web services hosted in R Server and how to consume them. This article assumes you are proficient in Python.

>[!IMPORTANT]
>Python web services are available only with Python-enabled installs of SQL Server 2017 CTP 2.0 (Public Preview). Expanded platform support in future releases.

## Publish-to-consume workflow

The workflow from publishing and consuming a Python web service is as follows:

1. Fulfill the [prerequisite](#prereq) of having a Python client library generated from the core API swagger document.
1. Add authentication and header logic to your Python script.
1. Create a Python session, prepare the environment, and create a snapshot to preserve the environment.
1. Publish the web service and embed this snapshot.
1. Try out the web service by consuming it in your session.
1. Manage these services.

![Swagger Workflow](./media/how-to-publish-consume-python-web-service/data-scientist-python-workflow.png)


## End-to-end example

This example assumes you have satisfied the [prerequisites](#prereq) to generate a Python client library from that Swagger file and that you've used Autorest.

A step-by-step walkthrough with more detailed descriptions is provided below this code block.

>[!IMPORTANT]
>This full example uses the local 'admin' account for authentication. Use the credentials and [authentication method](#python-auth) configured by your administrator. 

```python
##################################################
##       IMPORT GENERATED CLIENT LIBRARY        ##
##################################################

# Import the generated client library. 
import deployrclient

##################################################
##              AUTHENTICATION                  ##
##################################################

#Using client library generated from Autorest
#Create client instance and point it at an R Server. 
#In this case, R Server is local.
client = deployrclient.DeployRClient("http://localhost:12800")

#Define the login request and provide credentials 
#Update values with the connection parameters from your admin
login_request = deployrclient.models.LoginRequest("<<your-username>>","<<your-password>>")

#Make a call to the /login API. 
#Store the returned an access token in a variable.
token_response = client.login(login_request)

#Add the returned access token to the headers variable.
headers = {"Authorization": "Bearer {0}".format(token_response.access_token)}

#Verify that the server is running.
#Remember to include `headers` in every request!
status_response = client.status(headers) 
print(status_response.status_code)


##################################################
##        CREATE SESSION, MODEL, SNAPSHOT       ##
##################################################

#Since already logged in, create a Python session.
#Define session using name (`Session 1`) and `runtime_type="Python"`.
#Remember to specify the Python runtime type.
create_session_request = deployrclient.models.CreateSessionRequest("Session 1", runtime_type="Python")

#Make the call to start the session. 
#Remember to include headers in every method call to the server.
#Returns a session ID.
response = client.create_session(create_session_request, headers) 
   
#Store the session ID in a variable called `session_id` 
#to be able to identify it later at execution time.
session_id = response.session_id

#Create model - Import SVM and datasets from the SciKit-Learn library
execute_request = deployrclient.models.ExecuteRequest("from sklearn import svm\nfrom sklearn import datasets")
execute_response = client.execute_code(session_id,execute_request, headers)
#Report if it was a success
execute_response.success
   
#Define the untrained Support Vector Classifier (SVC) object
#and the dataset to be preloaded.
execute_request = deployrclient.models.ExecuteRequest("clf=svm.SVC()\niris=datasets.load_iris()\nnames={0:'I. setosa',1:'I. versicolor',2:'I. virginica'}")
#Now, go create the object and preload Iris Dataset in R Server
execute_response = client.execute_code(session_id,execute_request, headers)
#Report if it was a success
execute_response.success
   
#Define two rows from the Iris Dataset as a sample for scoring
workspace_object = deployrclient.models.WorkspaceObject("species_1",[7,3.2,4.7,1.4])
workspace_object_2 = deployrclient.models.WorkspaceObject("species_2",[3,2.6,3,2.5])

#Define how to train the classifier model; what to predict; what to return
execute_request = deployrclient.models.ExecuteRequest(
    "clf.fit(iris.data, iris.target)\n"+
    "result=clf.predict(species_1)\n"+
    "other_result=clf.predict(species_2)",
    [workspace_object,workspace_object_2], #Input
    ["result", "other_result"]) #Output

#Now, go train that model on the Iris Dataset in R Server
execute_response = client.execute_code(session_id,execute_request, headers)

#If successful, print name and result of each output parameter. Else, print error.
if(execute_response.success):
    for result in execute_response.output_parameters:
        print("{0}: {1}".format(result.name,result.value))
else:
    print (execute_response.error_message)

#Create a snapshot of the current session
response = client.create_snapshot(session_id, deployrclient.models.CreateSnapshotRequest("Iris Snapshot"), headers)
#Return the snapshot ID for reference when you publish later.
response.snapshot_id
#If you forget the ID, list every snapshot to get the ID again.
for snapshot in client.list_snapshots(headers):
    print(snapshot)

##################################################
##        PUBLISH AS A SERVICE IN PYTHON        ##
##################################################

#Define a web service that determines the iris species by scoring 
#a vector of sepal length and width, petal length and width

#Set `flower_data` for the sepal and petal length and width
flower_data = deployrclient.models.ParameterDefinition(name = "flower_data", type = "vector")
#Set `iris_species` for the species of iris
iris_species = deployrclient.models.ParameterDefinition(name = "iris_species", type = "vector")

#Define the publish request for the web service and its arguments.
#Specify the code, inputs, outputs, and snapshot.
#Don't forget to set runtime_type="Python"`.
publish_request = deployrclient.models.PublishWebServiceRequest(
       code = "iris_species = [names[x] for x in clf.predict(flower_data)]", 
       input_parameter_definitions = [flower_data], 
       output_parameter_definitions = [iris_species],
       runtime_type = "Python",
       snapshot_id = response.snapshot_id)

#Publish the service using the specified name (iris), version (V1.0)
client.publish_web_service_version("Iris", "V1.0", publish_request, headers)

##################################################
##           CONSUME SERVICE IN PYTHON          ##
##################################################

# Inspect holdings and metadata for service Iris V1.0.
for service in client.get_web_service_version("Iris","V1.0", headers):
    #print service metadata (description, who published,...)
    print(service)
    #Print input and output parameters as defined in service definition object
    print("Input Parameters: {0}".format([str(parameter) for parameter in service.input_parameter_definitions]))
    print("Output Parameters: {0}".format([str(parameter) for parameter in service.output_parameter_definitions]))

#Import the requests library to make requests on the server
import requests
#Import the JSON library to use for pretty printing of json responses
import json

#Create a requests `Session` object.
s = requests.Session()

#Record the R Server endpoint URL hosting the web services you created before
url = "http://localhost:12800"

#Give the request.Session object the authentication headers 
#so you don't have to repeat it with each request.
s.headers = headers

#Retrieve the service-specific swagger file using the requests library.
swagger_resp = s.get(url+"/api/Iris/V1.0/swagger.json")

#Either, download service-specific swagger file using the json library.
with open('iris_swagger.json','w') as f:
   (json.dump(client.get_web_service_swagger("Iris","V1.0",headers),f, indent = 1))

#Or, print just what you need from the Swagger file, 
#such as the routing paths for the endpoints to be consumed.
print(json.dumps(swagger_resp.json()["paths"], indent = 1, sort_keys = True))

#Or, print input and output parameters as defined in the Swagger.io format
print("Input")
print(json.dumps(swagger_resp.json()["definitions"]["InputParameters"], indent = 1, sort_keys = True))
print("Output")
print(json.dumps(swagger_resp.json()["definitions"]["WebServiceResult"], indent = 1, sort_keys = True))

#Make the request to consume the service using these flower_data inputs
resp = s.post(url+"/api/Iris/V1.0",json={"flower_data":[7,3.2,4.7,1.4]})
#Print the output
print(json.dumps(resp.json(), indent = 1, sort_keys = True))

#Use input from another Iris species.
resp = s.post(url+"/api/Iris/V1.0",json={"flower_data":[3,2.6,3,2.5]})
print(json.dumps(resp.json(), indent = 1, sort_keys = True))

#Use input from another Iris species.
resp = s.post(url+"/api/Iris/V1.0",json={"flower_data":[5.1,3.5,1.4,.2]})
print(json.dumps(resp.json(), indent = 1, sort_keys = True))

##################################################
##          MANAGE SERVICES IN PYTHON           ##
##################################################

#Define what needs to be updated. Here we add a description.
#Be sure to specify the runtime_type again.
update_request = deployrclient.models.PublishWebServiceRequest(
    description = "Determines iris species using length and width of sepal and petal", runtime_type = "Python")
#Now update it by specifying the service name and version number
client.patch_web_service_version("Iris", "V1.0", update_request, headers)

#Or, publish another version of the web service, but this time 
#the service returns the species as a string instead of list of strings.
flower_data = deployrclient.models.ParameterDefinition(name = "flower_data", type = "vector")
iris_species = deployrclient.models.ParameterDefinition(name = "iris_species", type = "string")
 
#Define the publish request for the service and its arguments.
#Specify the changed code, inputs, outputs, and snapshot.
#Don't forget to set runtime_type="Python"`.
publish_request = deployrclient.models.PublishWebServiceRequest(
    code = "iris_species = [names[x] for x in clf.predict(flower_data)][0]",
    description = "Determines the species of iris, based on Sepal Length, Sepal Width, Petal Length and Petal Width",
    input_parameter_definitions = [flower_data],
    output_parameter_definitions = [iris_species],
    runtime_type = "Python",
    snapshot_id = response.snapshot_id)
 
#Now, publish the service with version (V2.0)
client.publish_web_service_version("Iris", "V2.0", publish_request, headers)
 
#Make request to consume service using these flower_data inputs; print output
resp = s.post(url+"/api/Iris/V2.0",json={"flower_data":[5.1,3.5,1.4,.2]})
print(json.dumps(resp.json(), indent = 1, sort_keys = True))

#Return the list of all existing web services.
for service in client.get_all_web_services(headers):
    #print the service information
    print(service)
    #Print each input and output parameter
    print("Input Parameters: {0}".format([str(parameter) for parameter in service.input_parameter_definitions]))
    print("Output Parameters: {0}".format([str(parameter) for parameter in service.output_parameter_definitions]))

#Delete the second version we just published.
client.delete_web_service_version("Iris","V2.0",headers)
```

## Walkthrough

This section runs through the preceding example in more detail.

<a name="prereq"></a>

### Prerequisite: the client library

Before you can start authenticating with R Server and publishing your Python code and models, you must generate a client library using the Swagger document we provide. Here's how.

1. Install a Swagger code generator on your local machine and familiarize yourself with it. You use it to generate the API client libraries in Python. Popular tools include [Azure AutoRest](https://github.com/Azure/autorest) (requires Node.js) and [Swagger Codegen](https://github.com/swagger-api/swagger-codegen). 

1. Download the Swagger file containing the core APIs for your version of R Server. This file contains a Swagger template defining the list of REST resources and the operations that can be called on those resources. Find this file under `https://microsoft.github.io/deployr-api-docs/swagger/<version>/rserver-swagger-<version>.json`, where `<version>` is the 3-digit R Server version number. 

   For example, for R Server 9.1 you would download from:
   https://microsoft.github.io/deployr-api-docs/9.1.0/swagger/rserver-swagger-9.1.0.json

1. Generate the statically generated client library by passing the `rserver-swagger-<version>.json` file to the Swagger code generator and specifying the language you want. In this case, you should specify Python.  

   For example, if you use AutoRest to generate a Python client library, it might look like this, where the 3-digit number represents the R Server version number:
   ```
   AutoRest.exe -Input rserver-swagger-9.1.0.json -CodeGenerator Python  -OutputDirectory C:\Users\rserver-user\Documents\Python
   ```

   You can now provide some custom headers and make other changes before using the generated client library stub. See the <a href="https://github.com/Azure/autorest/blob/master/docs/user/cli.md" target="_blank">Command Line Interface</a> documentation for details regarding different configuration options and preferences, such as renaming the namespace.
   
1. Explore the core client library to see the various API calls you can make. 

   In our example, Autorest generated some directories and files for the Python client library on your local system. By default, the namespace (and directory) is `deployrclient` and might look like this:
   
   ![autorest output path](./media/how-to-publish-consume-python-web-service/data-scientist-python-autorest.png)

   For this default namespace, the client library itself is called `deploy_rclient.py`. If you open this file in your IDE such as Visual Studio, you see something like this:
   
   ![autorest output path](./media/how-to-publish-consume-python-web-service/data-scientist-python-client-library.png)

<br>

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

### 2. Prepare session and code

After authentication, you can start a Python session and create a model you'll publish later. You can include any Python code or models in a web service. Once you've set up your session environment, you can even save it as a snapshot so you can reload your session as you had it before. 

>[!IMPORTANT]
>Remember to include `headers` in every request.

1. Create a Python session on R Server. You must specify a name and the Python language (`runtime_type="Python"`). 

   If you don't set the runtime type to Python, it defaults to R.

   This is a continuation of the example using the client library generated by Autorest:

   ```python
   #Since already logged in, create a Python session.
   #Define session using name (`Session 1`) and `runtime_type="Python"`.
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

1. Create or call a model in Python that you publish as a web service. 

   In this example, we train a SciKit-Learn support vector machines (SVM) model on the Iris Dataset on a remote R Server.  This dataset is available here: http://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html 

   ```python
   #Import SVM and datasets from the SciKit-Learn library
   execute_request = deployrclient.models.ExecuteRequest("from sklearn import svm\nfrom sklearn import datasets")
   execute_response = client.execute_code(session_id,execute_request, headers)
   #Report if it was a success
   execute_response.success
   
   #Define the untrained Support Vector Classifier (SVC) object
   #and the dataset to be preloaded.
   execute_request = deployrclient.models.ExecuteRequest("clf=svm.SVC()\niris=datasets.load_iris()\nnames={0:'I. setosa',1:'I. versicolor',2:'I. virginica'}")
   #Now, go create the object and preload Iris Dataset in R Server
   execute_response = client.execute_code(session_id,execute_request, headers)
   #Report if it was a success
   execute_response.success
   
   #Define two rows from the Iris Dataset as a sample for scoring
   workspace_object = deployrclient.models.WorkspaceObject("species_1",[7,3.2,4.7,1.4])
   workspace_object_2 = deployrclient.models.WorkspaceObject("species_2",[3,2.6,3,2.5])

   #Define how to train the classifier model; what to predict; what to return
   execute_request = deployrclient.models.ExecuteRequest(
       "clf.fit(iris.data, iris.target)\n"+
       "result=clf.predict(species_1)\n"+
       "other_result=clf.predict(species_2)",
       [workspace_object,workspace_object_2], #Input
       ["result", "other_result"]) #Output

   #Now, go train that model on the Iris Dataset in R Server
   execute_response = client.execute_code(session_id,execute_request, headers)

   #If successful, print name and result of each output parameter. Else, print error.
   if(execute_response.success):
       for result in execute_response.output_parameters:
           print("{0}: {1}".format(result.name,result.value))
   else:
       print (execute_response.error_message)
   ```

1. Create a session snapshot of this Python session so this environment can be saved in the web service and reproduced at consume time. You can only use a session snapshot that you've created.

   Session snapshots are very useful when you need a prepared environment that includes certain libraries, objects, models, files, and artifacts. Snapshots save the whole workspace and working directory. 

   When publishing, you can only include a snapshot you've created. 

   > [!NOTE] 
   > While session snapshots can also be used when publishing a web service for environment dependencies, it may have an impact on the performance of the consumption time.  For optimal performance, consider the size of the snapshot carefully and ensure that you keep only those workspace objects you need and purge the rest. In a session, you can use the Python `del` function or [the `deleteWorkspaceObject` API request](https://microsoft.github.io/deployr-api-docs/#delete-workspace-object) to remove unnecessary objects. 

   ```python
   #Create a snapshot of the current session.
   response = client.create_snapshot(session_id, deployrclient.models.CreateSnapshotRequest("Iris Snapshot"), headers)

   #Return the snapshot ID for reference when you publish later.
   response.snapshot_id

   #If you forget the ID, list every snapshot to get the ID again.
   for snapshot in client.list_snapshots(headers):
       print(snapshot)
   ```

### 3. Publish the model 

After your client library has been generated and you've built the authentication logic into your application, you can interact with the core APIs to create a Python session, create a model, and then publish a web service using that model.

>[!NOTE]
>Remember that you must be authenticated before you make any API calls. Therefore, include `headers` in every request.

Publish this SVM model as a Python web service in R Server. This web service will score a vector that gets passed to it.

>[!IMPORTANT]
> To ensure that the web service is registered as a Python service, be sure to specify `runtime_type="Python"`. If you don't set the runtime type to Python, it defaults to R.

```python
#Define a web service that determines the iris species by scoring 
#a vector of sepal length and width, petal length and width

#Set `flower_data` for the sepal and petal length and width
flower_data = deployrclient.models.ParameterDefinition(name = "flower_data", type = "vector")
#Set `iris_species` for the species of iris
iris_species = deployrclient.models.ParameterDefinition(name = "iris_species", type = "vector")

#Define the publish request for the web service and its arguments.
#Specify the code, inputs, outputs, and snapshot.
#Don't forget to set runtime_type="Python"`.
publish_request = deployrclient.models.PublishWebServiceRequest(
       code = "iris_species = [names[x] for x in clf.predict(flower_data)]", 
       input_parameter_definitions = [flower_data], 
       output_parameter_definitions = [iris_species],
       runtime_type = "Python",
       snapshot_id = response.snapshot_id)

#Publish the service using the specified name (iris), version (V1.0)
client.publish_web_service_version("Iris", "V1.0", publish_request, headers)
```

### 4. Consume the web service

This section demonstrates how to consume the service in the same session where it was created.

1. In the same session, get service holdings and metadata for the service.

   ```python
   # Inspect holdings and metadata for service Iris V1.0.
   for service in client.get_web_service_version("Iris","V1.0", headers):
       #print service metadata (description, who published,...)
       print(service)
       #Print input and output parameters as defined in service definition object
       print("Input Parameters: {0}".format([str(parameter) for parameter in service.input_parameter_definitions]))
       print("Output Parameters: {0}".format([str(parameter) for parameter in service.output_parameter_definitions]))
   ```

1. Examine the service holdings and prepare to consume. 
   ```python
   #Import the requests library to make requests on the server
   import requests
   #Import the JSON library to use for pretty printing of json responses
   import json

   #Create a requests `Session` object.
   s = requests.Session()

   #Record the R Server endpoint URL hosting the web services you created
   url = "http://localhost:12800"

   #Give the request.Session object the authentication headers 
   #so you don't have to repeat it with each request.
   s.headers = headers

   #Retrieve the service-specific swagger file using the requests library.
   swagger_resp = s.get(url+"/api/Iris/V1.0/swagger.json")

   #Either, download service-specific swagger file using the json library.
   with open('iris_swagger.json','w') as f:
      (json.dump(client.get_web_service_swagger("Iris","V1.0",headers),f, indent = 1))

   #Or, print just what you need from the Swagger file, 
   #such as the routing paths for the endpoints to be consumed.
   print(json.dumps(swagger_resp.json()["paths"], indent = 1, sort_keys = True))

   #Or, print input and output parameters as defined in the Swagger.io format
   print("Input")
   print(json.dumps(swagger_resp.json()["definitions"]["InputParameters"], indent = 1, sort_keys = True))
   print("Output")
   print(json.dumps(swagger_resp.json()["definitions"]["WebServiceResult"], indent = 1, sort_keys = True))
   ```

1. Consume the service by supplying some input and getting the Iris species. 
   ```python
   #Make the request to consume the service using these flower_data inputs
   resp = s.post(url+"/api/Iris/V1.0",json={"flower_data":[7,3.2,4.7,1.4]})
   #Print the output
   print(json.dumps(resp.json(), indent = 1, sort_keys = True))

   ##Use input from another Iris species.
   resp = s.post(url+"/api/Iris/V1.0",json={"flower_data":[3,2.6,3,2.5]})
   print(json.dumps(resp.json(), indent = 1, sort_keys = True))

   ##Use input from another Iris species.
   resp = s.post(url+"/api/Iris/V1.0",json={"flower_data":[5.1,3.5,1.4,.2]})
   print(json.dumps(resp.json(), indent = 1, sort_keys = True))
   ```

### Manage the services

Now that we've created a web service, you can update, delete, or republish that service. You can also list every single web server hosted with R Server. Here's how.

#### Update a web service

You can update a web service to change the code, model, description, inputs, outputs, and more. In this example, we update the service to add a description useful to people who might consume this service. 

```python
#Define what needs to be updated. Here we add a description.
#Be sure to specify the runtime_type again.
update_request = deployrclient.models.PublishWebServiceRequest(
    description = "Determines iris species using length and width of sepal and petal", runtime_type = "Python")
#Now update it by specifying the service name and version number
client.patch_web_service_version("Iris", "V1.0", update_request, headers)
```

#### Publish another version

You can also publish another version of the web service. In this example, the service will return the Iris species as a string instead of as a list of strings.

```python
#Publish another version of the web service, but this time 
#the service returns the species as a string instead of list of strings.
flower_data = deployrclient.models.ParameterDefinition(name = "flower_data", type = "vector")
iris_species = deployrclient.models.ParameterDefinition(name = "iris_species", type = "string")
 
#Define the publish request for the service and its arguments.
#Specify the changed code, inputs, outputs, and snapshot.
#Don't forget to set runtime_type="Python"`.
publish_request = deployrclient.models.PublishWebServiceRequest(
    code = "iris_species = [names[x] for x in clf.predict(flower_data)][0]",
    description = "Determines the species of iris, based on Sepal Length, Sepal Width, Petal Length and Petal Width",
    input_parameter_definitions = [flower_data],
    output_parameter_definitions = [iris_species],
    runtime_type = "Python",
    snapshot_id = response.snapshot_id)
 
#Now, publish the service with version (V2.0)
client.publish_web_service_version("Iris", "V2.0", publish_request, headers)
 
#Make request to consume service using these flower_data inputs; print output
resp = s.post(url+"/api/Iris/V2.0",json={"flower_data":[5.1,3.5,1.4,.2]})
print(json.dumps(resp.json(), indent = 1, sort_keys = True))
```

#### List services

Get a list of all web services, including those created by other users or in different languages.

```python
#Return the list of all existing web services.
for service in client.get_all_web_services(headers):
    #print the service information
    print(service)
    #Print each input and output parameter
    print("Input Parameters: {0}".format([str(parameter) for parameter in service.input_parameter_definitions]))
    print("Output Parameters: {0}".format([str(parameter) for parameter in service.output_parameter_definitions]))
``` 

#### Delete services

You can delete services you've created. You can also delete the services of others if you are [assigned to a role](../configure-roles.md) with those permissions.

In this example, we delete the second web service version we just published.

```python
#Delete the second version we just published.
client.delete_web_service_version("Iris","V2.0",headers)
```
-->