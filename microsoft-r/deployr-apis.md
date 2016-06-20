# DeployR API - Reference Documentation


<a name="overview"></a>
## Overview
The DeployR API exposes the R platform as a service allowing the integration of R statistics, analytics, and visualizations inside Web, desktop and mobile applications. This API is exposed by the DeployR server, a standards-based server technology capable of scaling to meet the needs of enterprise-grade deployments.


### Version information
*Version* : 8.0.5


### Contact information
*Contact Email* : deployr@microsoft.com


### License information
*License* : Apache 2.0  
*License URL* : http://www.apache.org/licenses/LICENSE-2.0.html  
*Terms of service* : http://deployr.micorsoft.com/terms/


### URI scheme
*Host* : localhost:10010  
*BasePath* : /deployr  
*Schemes* : HTTP, HTTPS


### Consumes

* `application/x-www-form-urlencoded`
* `multipart/form-data`


### Produces

* `application/json`
* `application/xml`
* `application/zip`



<a name="paths"></a>
## Paths

<a name="jobcancel"></a>
### POST /r/job/cancel

#### Description
This call cancels the specified job. <br/><br/>
Only jobs in an open-state can be cancelled. The set of job open-states are shown here:

 - *Scheduled*: job is scheduled but not yet queued for running.
 - *Queued*: job is queued for running.
 - *Running*: job is running.

**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/job/cancel",
      "success": true,
      "job": {
        "job": "JOB-91baaca8-574d-4703-b225-97e64ec63e70",
        "name": "Scheduled Analysis",
        "descr": "Demonstrates job scheduling."
        "schedstart": 1316751060000,
        "schedrepeat": 2,
        "schedinterval": 86400000,
        "onrepeat": 0,
        "priority": "low",
        "tag": null,
        "timeStart": 0,
        "timeCode": 0,
        "timeTotal": 0,
        "status": "Cancelled",
        "statusMsg": null,
        "project": null
      }
      "uid": "5CB911405DC6EB0F8E283990F7969E63"
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**job**  <br>*required*|specifies a comma-separated list of job identifiers.|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[JobCancelResponse](#jobcancelresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="jobdelete"></a>
### POST /r/job/delete

#### Description
This call deletes the specified job. <br/><br/>
Only jobs in one of the success, cancelled or  incompleted states can be deleted. The set of  states are shown here:

  - Completed: job execution has run to successful completion.
  - Interrupted : job execution has been interrupted.
  - Cancelled : job has been cancelled.
  - Aborted : job execution has been aborted.
  - Failed : job execution has resulted in failure.
  
<br/> Jobs that are in an open-state can not be deleted. <br/><br/> **Important!** Deleting jobs will not delete the projects that resulted from those jobs. <br/><br/> **Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/job/delete",
      "success": true
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**job**  <br>*required*|specifies a comma-separated list of job identifiers.|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[JobDeleteResponse](#jobdeleteresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="joblist"></a>
### GET /r/job/list

#### Description
This call lists jobs for the currently _authenticated_ user. <br/><br/>
The openonly parameter allows the caller to see only thosejobs in an open state.  The set of job open states are shown here: <br/>

  - Scheduled : job is scheduled but not yet queued for running.
  - Queued : job is queued for running.
  - Running : job is running.

<br/> **Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/job/list",
      "success": true,
      "jobs": [
       {
            "job": "JOB-e202f4f3-e24f-4d5d-b0cf-fda8912fbaca",
            "name": "Scheduled Analysis",
            "descr": "Demonstrates job scheduling."
            "schedstart": 1378334640000,
            "schedrepeat": 0,
            "schedinterval": 60000,
            "onrepeat": 0,
            "priority": "medium",
            "tag": null,
            "timeStart": 0,
            "timeCode": 0,
            "timeTotal": 0,
            "status": "Scheduled",
            "statusMsg": null,
            "project": null
        },
        {
            "job": "JOB-91baaca8-574d-4703-b225-97e64ec63e70",
            "name": "Submitted Analysis",
            "descr": "Demonstrates job submission."
            "schedstart": 0,
            "schedrepeat": 0,
            "schedinterval": 0,
            "priority": "low",
            "onrepeat": 0,
            "tag": null,
            "timeStart": 1378327215176,
            "timeCode": 102,
            "timeTotal": 537,
            "status": "Completed",
            "statusMsg": "Job successfully executed to completion.",
            "project": "PROJECT-91baaca8-574d-4703-b225-97e64ec63e70"
        }                
      ],
      "uid": "5CB911405DC6EB0F8E283990F7969E63"
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**extended**  <br>*optional*|if `true`, additional data properties describing each job are listed in the response markup.|boolean||
|**Query**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**Query**|**openonly**  <br>*optional*|if `true`, only jobs in an open-state are listed in the response markup.|boolean||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[JobListResponse](#joblistresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="jobquery"></a>
### GET /r/job/query

#### Description
This call queries the job status. The status property will indicate one  of the following values:

  - Scheduled
  - Queued
  - Running
  - Completed
  - Cancelling
  - Cancelled
  - Interrupted
  - Aborted
  - Failed

**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/job/query",
      "success": true,
      "job": {
        "job": "JOB-91baaca8-574d-4703-b225-97e64ec63e70",
        "name": "Scheduled Analysis",
        "descr": "Demonstrates job scheduling."
        "schedstart": 1316751060000,
        "schedrepeat": 2,
        "schedinterval": 86400000,
        "onrepeat": 0,
        "priority": "low",
        "tag": null,
        "timeStart": 0,
        "timeCode": 0,
        "timeTotal": 0,
        "status": "Scheduled",
        "statusMsg": null,
        "project": null
      }
      "uid": "5CB911405DC6EB0F8E283990F7969E63"
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**extended**  <br>*optional*|if `true`, additional data properties describing each job are listed in the response markup.|boolean||
|**Query**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**Query**|**job**  <br>*required*|specifies a comma-separated list of job identifiers.|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[JobQueryResponse](#jobqueryresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="jobschedule"></a>
### POST /r/job/schedule

#### Description
This call schedules a job for background execution on behalf of the  user. <br/><br/>
The *schedstart* parameter identifies the start time for the job. This parameter value is specified as UTC in milliseconds. The *schedrepeat*  parameter indicates the number of times the job is to be repeated, and if omitted the job is executed just once. The *schedinterval* parameter indicates the interval, measured in milliseconds, between repeat  executions. <br/><br/>
To schedule the execution of an arbitrary block of R code the caller  must provide a value on the code parameter. <br/><br/>
To schedule the execution of a single repository-managed script the  caller must provide parameter values for *rscriptname , rscriptauthor  and optionally rscriptdirectory and rscriptversion*. To schedule the  execution of a chain of repository-managed scripts the caller must  provide a comma-separated list of values on the *rscriptname,  rscriptauthor and optionally rscriptdirectory and rscriptversion*  parameters. <br/><br/>
To schedule the execution of a single external script the caller must  provide a valid URL or file path using the *externalsource* parameter.  To schedule the execution of a chain of external scripts the caller  must provide a comma-separated list of values on the *externalsource* parameter. Note, to schedule the execution of an external script the  caller must have been granted the POWER_USER role. <br/><br/>
Note: A chained execution executes each of the scripts identified on  the call in a sequential fashion on the R session for the job, with  execution occurring in the order specified on the parameter list. <br/><br/>
Please see the **Standard Execution Model** section in this guide for  further details about the parameters on this call. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/job/schedule",
      "success": true,
      "job": {
        "job": "JOB-91baaca8-574d-4703-b225-97e64ec63e70",
        "name": "Scheduled Analysis",
        "descr": "Demonstrates job scheduling."
        "schedstart": 1316751060000,
        "schedrepeat": 2,
        "schedinterval": 86400000,
        "onrepeat": 0,
        "priority": "low",
        "tag": null,
        "timeStart": 0,
        "timeCode": 0,
        "timeTotal": 0,
        "status": "Scheduled",
        "statusMsg": null,
        "project": null
      },
      "uid": "5CB911405DC6EB0F8E283990F7969E63"
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**adoptdirectory**  <br>*optional*|identifies project whose directory is to be adopted|string||
|**FormData**|**adoptpackages**  <br>*optional*|identifies project whose package dependencies are to be adopted|string||
|**FormData**|**adoptworkspace**  <br>*optional*|identifies project whose workspace is to be adopted|string||
|**FormData**|**artifactsoff**  <br>*optional*|when enabled, artfiacts generated in the working directory are neither cached to the database nor reported in the response markup|boolean||
|**FormData**|**cluster**  <br>*optional*|identifies a grid node cluster for targeted execution|string||
|**FormData**|**code**  <br>*optional*|R code to execute on job|string||
|**FormData**|**consoleoff**  <br>*optional*|if `true` console output is not saved on the project execution history for the job|boolean||
|**FormData**|**csvinputs**  <br>*optional*|comma-separated list of primitive name/value inputs|string||
|**FormData**|**descr**  <br>*optional*|job description|string||
|**FormData**|**echooff**  <br>*optional*|if `true` R commands will not appear in the console output saved on the project execution history for the job|boolean||
|**FormData**|**enableConsoleEvents**  <br>*optional*|if `true` console events are delivered on the event stream when the job executes|boolean||
|**FormData**|**externalsource**  <br>*optional*|comma-separated list of authors, one author per rscriptname|string||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**graphics**  <br>*optional*|specifies preferred R graphics device for execution (png or svg)|string||
|**FormData**|**graphicsheight**  <br>*optional*|specifies preferred height for R graphics device images|integer||
|**FormData**|**graphicswidth**  <br>*optional*|specifies preferred width for R graphics device images|integer||
|**FormData**|**inputs**  <br>*optional*|DeployR-encoded inputs|string||
|**FormData**|**name**  <br>*required*|job name|string||
|**FormData**|**preloadbydirectory**  <br>*optional*|comma-separated list of repository directory names|string||
|**FormData**|**preloadfileauthor**  <br>*optional*|comma-separated list of authors, one author per preloadfilename|string||
|**FormData**|**preloadfiledirectory**  <br>*optional*|comma-separated list of directories, one directory per preloadfilename|string||
|**FormData**|**preloadfilename**  <br>*optional*|comma-separated list of repository filenames|string||
|**FormData**|**preloadfileversion**  <br>*optional*|comma-separated list of versions, one version per preloadfilename|string||
|**FormData**|**preloadobjectauthor**  <br>*optional*|comma-separated list of authors, one author per preloadobjectname|string||
|**FormData**|**preloadobjectdirectory**  <br>*optional*|comma-separated list of directories, one directory per preloadobjectname|string||
|**FormData**|**preloadobjectname**  <br>*optional*|comma-separated list of repository object (.rData) filenames|string||
|**FormData**|**preloadobjectversion**  <br>*optional*|comma-separated list of versions, one version per preloadobjectname|string||
|**FormData**|**priority**  <br>*optional*|specifies the scheduling priority for the job low (default), medium or high|string||
|**FormData**|**rscriptauthor**  <br>*optional*|comma-separated list of authors, one author per rscriptname|string||
|**FormData**|**rscriptdirectory**  <br>*optional*|comma-separated list of repository-managed directories for scripts, defaults to root|string||
|**FormData**|**rscriptname**  <br>*optional*|comma-separated list of repository-managed script filenames to execute on job|string||
|**FormData**|**rscriptversion**  <br>*optional*|comma-separated list of versions, one version per rscriptname|string||
|**FormData**|**storedirectory**  <br>*optional*|repository directory for stored files and objects after the execution completes, defaults to root|string||
|**FormData**|**storefile**  <br>*optional*|comma-separated list of working directory filenames|string||
|**FormData**|**storenewversion**  <br>*optional*|if `true`, ensures each file stored in repository results in new version being created if needed|boolean||
|**FormData**|**storenoproject**  <br>*optional*|if `true`, no project persistence following job execution|boolean||
|**FormData**|**storeobject**  <br>*optional*|comma-separated list of workspace object names|string||
|**FormData**|**storepublic**  <br>*optional*|if `true`, publishes each file stored in the repository|boolean||
|**FormData**|**storeworkspace**  <br>*optional*|filename (.rData) where workspace contents will be saved in the repository|string||
|**FormData**|**tag**  <br>*optional*|specifies a tag that labels the execution|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[JobScheduleResponse](#jobscheduleresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="jobsubmit"></a>
### POST /r/job/submit

#### Description
This call submits a job for background execution on behalf of the user. To submit the execution of an arbitrary block of R code the caller must provide a value on the code parameter. <br/><br/>
To submit the execution of a single repository-managed script the caller must provide parameter values for *rscriptname, rscriptauthor and optionally rscriptdirectory and rscriptversion*. To submit the execution  of a chain of repository-managed scripts the caller must provide a  comma-separated list of values on the *rscriptname , rscriptauthor and  optionally rscriptdirectory and rscriptversion* parameters.<br/><br/>
To submit the execution of a single external script the caller must provide a valid URL or file path using the *externalsource* parameter. To submit the  execution of a chain of external scripts the caller must provide a comma -separated list of values on the *externalsource* parameter. Note, to submit  the execution of an external script the caller must have been granted the  POWER_USER role.<br/><br/>
Note: A chained execution executes each of the scripts identified on the call in a sequential fashion on the R session for the job, with execution  occurring in the order specified on the parameter list.<br/><br/>
Please see the **Standard Execution Model** section in this guide for further  details about the parameters on this call.<br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/job/submit",
      "success": true,
      "job": {
        "job": "JOB-e202f4f3-e24f-4d5d-b0cf-fda8912fbaca",
        "name": "Scheduled Analysis",
        "descr": "Demonstrates job scheduling."
        "schedstart": 1378334640000,
        "schedrepeat": 0,
        "schedinterval": 60000,
        "onrepeat": 0,
        "priority": "medium",
        "tag": null,
        "timeStart": 0,
        "timeCode": 0,
        "timeTotal": 0,
        "status": "Scheduled",
        "statusMsg": null,
        "project": null
      },
      "uid": "5CB911405DC6EB0F8E283990F7969E63"
    }
  }
}'
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**adoptdirectory**  <br>*optional*|identifies project whose directory is to be adopted|string||
|**FormData**|**adoptpackages**  <br>*optional*|identifies project whose package dependencies are to be adopted|string||
|**FormData**|**adoptworkspace**  <br>*optional*|identifies project whose workspace is to be adopted|string||
|**FormData**|**artifactsoff**  <br>*optional*|when enabled, artfiacts generated in the working directory are neither cached to the database nor reported in the response markup|boolean||
|**FormData**|**cluster**  <br>*optional*|identifies a grid node cluster for targeted execution|string||
|**FormData**|**code**  <br>*optional*|R code to execute on job|string||
|**FormData**|**consoleoff**  <br>*optional*|if `true` console output is not saved on the project execution history for the job|boolean||
|**FormData**|**csvinputs**  <br>*optional*|comma-separated list of primitive name/value inputs|string||
|**FormData**|**descr**  <br>*optional*|job description|string||
|**FormData**|**echooff**  <br>*optional*|if `true` R commands will not appear in the console output saved on the project execution history for the job|boolean||
|**FormData**|**enableConsoleEvents**  <br>*optional*|if `true` console events are delivered on the event stream when the job executes|boolean||
|**FormData**|**externalsource**  <br>*optional*|comma-separated list of authors, one author per rscriptname|string||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**graphics**  <br>*optional*|specifies preferred R graphics device for execution (png or svg)|string||
|**FormData**|**graphicsheight**  <br>*optional*|specifies preferred height for R graphics device images|integer||
|**FormData**|**graphicswidth**  <br>*optional*|specifies preferred width for R graphics device images|integer||
|**FormData**|**inputs**  <br>*optional*|DeployR-encoded inputs|string||
|**FormData**|**name**  <br>*required*|job name|string||
|**FormData**|**preloadbydirectory**  <br>*optional*|comma-separated list of repository directory names|string||
|**FormData**|**preloadfileauthor**  <br>*optional*|comma-separated list of authors, one author per preloadfilename|string||
|**FormData**|**preloadfiledirectory**  <br>*optional*|comma-separated list of directories, one directory per preloadfilename|string||
|**FormData**|**preloadfilename**  <br>*optional*|comma-separated list of repository filenames|string||
|**FormData**|**preloadfileversion**  <br>*optional*|comma-separated list of versions, one version per preloadfilename|string||
|**FormData**|**preloadobjectauthor**  <br>*optional*|comma-separated list of authors, one author per preloadobjectname|string||
|**FormData**|**preloadobjectdirectory**  <br>*optional*|comma-separated list of directories, one directory per preloadobjectname|string||
|**FormData**|**preloadobjectname**  <br>*optional*|comma-separated list of repository object (.rData) filenames|string||
|**FormData**|**preloadobjectversion**  <br>*optional*|comma-separated list of versions, one version per preloadobjectname|string||
|**FormData**|**priority**  <br>*optional*|specifies the scheduling priority for the job low (default), medium or high|string||
|**FormData**|**rscriptauthor**  <br>*optional*|comma-separated list of authors, one author per rscriptname|string||
|**FormData**|**rscriptdirectory**  <br>*optional*|comma-separated list of repository-managed directories for scripts, defaults to root|string||
|**FormData**|**rscriptname**  <br>*optional*|comma-separated list of repository-managed script filenames to execute on job|string||
|**FormData**|**rscriptversion**  <br>*optional*|comma-separated list of versions, one version per rscriptname|string||
|**FormData**|**storedirectory**  <br>*optional*|repository directory for stored files and objects after the execution completes, defaults to root|string||
|**FormData**|**storefile**  <br>*optional*|comma-separated list of working directory filenames|string||
|**FormData**|**storenewversion**  <br>*optional*|if `true`, ensures each file stored in repository results in new version being created if needed|boolean||
|**FormData**|**storenoproject**  <br>*optional*|if `true`, no project persistence following job execution|boolean||
|**FormData**|**storeobject**  <br>*optional*|comma-separated list of workspace object names|string||
|**FormData**|**storepublic**  <br>*optional*|if `true`, publishes each file stored in the repository|boolean||
|**FormData**|**storeworkspace**  <br>*optional*|filename (.rData) where workspace contents will be saved in the repository|string||
|**FormData**|**tag**  <br>*optional*|specifies a tag that labels the execution|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[JobSubmitResponse](#jobsubmitresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectabout"></a>
### GET /r/project/about

#### Description
This call retrieves a set of properties that describe the specified project. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "success": true,
      "call": "/r/project/about",
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis",
        "descr": "Predicting retail sales."
        "longdescr": null,
        "author": "george",
        "authors": [
          "george"
        ],
        "shared": false,
        "lastmodified": 1378307713478,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectaboutupdate"></a>
### POST /r/project/about/update

#### Description
This call updates a set of properties that describe the specified project. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "success": true,
      "call": "/r/project/about/update",
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis",
        "descr": "Predicting retail sales, updated."
        "longdescr": null,
        "author": "george",
        "authors": [
          "george"
        ],
        "shared": false,
        "lastmodified": 1378307713478,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectclose"></a>
### POST /r/project/close

#### Description
This call closes the specified project. <br/><br/>
Closing a live project releases all resources associated with the project on  the **DeployR grid**. If the specified project is a persistent project then  the default autosave semantics will cause the project to be saved  automatically. The caller can override that default behavior using the  *disableautosave* parameter. <br/><br/>
The set of *drop* parameters allow the caller to selectively drop aspects,  such as workspace, working directory, or execution history, of the project  state when closing. The *flushhistory* parameter allows the caller to preserve the project execution history itself while destroying all generated console  output and results associated with that history. <br/><br/>
 
 **Example Response (json):**
 
 ```
 
 {
   "deployr": {
     "response": {
       "call": "/r/project/close",
       "success": true,
       "uid": "UID-1e518408953d7920e1d7ae261694091c"
     }
   }
 }

 ```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[Response](#response)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectcreate"></a>
### POST /r/project/create

#### Description
This call creates a new project for the currently *authenticated* user. If the *projectname* parameter is specified on the call, then the newly  created project will be a *persistent project*. If the projectname parameter is omitted on the call, then the newly created project will be  a *temporary project*. The *projectdescr* parameter is ignored if the *projectname* parameter is omitted. <br/><br/>
The *blackbox* parameter ensures that calls on the temporary project are limited to the User Blackbox API Controls.  <br/><br/>
Using the *inputs, preloadfile, preloadobject* and *adopt* parameters the  project can be pre-initialized with data in the workspace and/or working  directory. <br/><br/>
The *inputs* parameter allows the caller to pass **DeployR-encoded** R  object values as inputs. These inputs are turned into R objects in the  workspace of the new R session before the call returns. <br/><br/>
The *csvinputs* parameter allows the caller to pass R object primitive values as comma-separated name/value pairs. These inputs are turned into R objects in the workspace before the execution begins. <br/><br/>
The *preloadbydirectory* parameter allows the caller to load all files within one or more repository-managed directories into the working directory before the call returns. <br/><br/>
The set of *preloadfile* parameters allow the caller to load one or more files from the repository into the working directory of the new R session before the call returns. <br/><br/>
The set of *preloadobject* parameters allow the caller to load one or more binary R objects (.rData) from the repository into the workspace of the new R session before the call returns. <br/><br/>
The set of *adopt* parameters allow the caller to load a pre-existing project workspace, project working directory and/or project package dependencies into the new R session before the call returns. <br/><br/>
**Example Response (json):**
```
{ 
  "deployr": { 
    "response": { 
      "success": true,
      "call: "/r/project/create",
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": { 
        "project": "PROJECT-2dfb6321-dfa4-431c-88c2-7f7d144865b4",
        "name": null,
        "descr": null,
        "longdescr": null,
        "live": true,
        "shared": false,
        "cookie": null,
        "origin": "Project original.",
        "author": "testuser",
        "authors: [ "testuser" ],
        "lastmodified": 1466396262531 
      } 
    } 
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectdelete"></a>
### POST /r/project/delete

#### Description
This call deletes the specified project. <br/><br/>
Deleting a project is a permanent operation that cannot be undone or  recovered. <br/><br/>
Refer to the section **Introducing Projects** for a detailed description  of project collaboration and the workflows facilitated by grants. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/delete",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis",
        "descr": "Predicting retail sales, updated."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectexecutecode"></a>
### POST /r/project/execute/code

#### Description
This call executes a block of R code on the R session identified by the  *project* parameter. <br/><br/>
Please see the **Standard Execution Model** section in this guide for  further details about the parameters on this call. <br/><br/>
Some key data indicated in the response markup on this call:

  - *code* - indicates the code that has been executed
  - *console* - indicates the R console output resulting from the execution
  - *results* - indicates the list of files generated by the R graphics device
  - *artifacts* - indicates the list of files generated or modified in the working directory
  - *objects* - indicates the list of R objects returned from the workspace
  - *files* - indicates the list of files and objects stored in the repository after the execution completes
  - *interrupted* - indicates the interrupted status of execution
  - *error* - on failure, indicates the reason for failure
  - *errorCode* - on failure, indicates the **error code** for failure


**Example Response (json):**
``` {
    "deployr": {
        "response": {
            "call": "/r/project/execute/code",
            "success": true,
            "uid": "UID-1e518408953d7920e1d7ae261694091c",
            "interrupted": false,
            "project": {
                "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
                "name": "Demo Analysis",
                "descr": "Predicting retail sales, updated."
                "longdescr": null,
                "author": "george",
                "authors": [
                    "george",
                    "testuser"
                ],
                "shared": false,
                "lastmodified": 1378308723041,
                "live": true,
                "cookie": null,
                "origin": "Project original."
            },
            "execution": {
                "code": "x <- c(1:50)rplot(x)rpng("namedPlot.png")rplot(x)",
                "execution": "EXEC-4d60922e-9ff8-44ec-9ac9-b358f6356c38",
                "tag": null,
                "timeStart": 1378315243221,
                "timeCode": 102,
                "timeTotal": 820,
                "actor": "george",
                "console": "n> x <- c(1:50)nn> plot(x)nn>
                png("namedPlot.png")nn> plot(x)n",
                "resultsAvailable": 1,
                "resultsGenerated": 1,
                "results": [
                    {
                        "execution": "EXEC-4d60922e-9ff8-44ec-9ac9-b358f6356c38",
                        "filename": "unnamedplot001.png",
                        "length": 6642,
                        "type": "image/png",
                        "md5": "67ec8f704613be81c47af1bfc1c6bc5b",
                        "url": "http://184.106.178.37:7000/deployr/r/project/
                        execute/result/download/
                        PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca/
                        EXEC-4d60922e-9ff8-44ec-9ac9-b358f6356c38/unnamedplot001.png"
                    }
                ],
                "artifacts": [
                    {
                        "filename": "namedPlot.png",
                        "descr": null,
                        "length": 6642,
                        "type": "image/png",
                        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
                        "category": "plot",
                        "lastmodified": 1378315300000
                        "url": "http://184.106.178.37:7000/deployr/r/project/
                        directory/download/
                        PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca/namedPlot.png",
                    }
                ],
                "warnings": [],
                "info": [],
                "help": null,
                "interrupted": false
            },
            "workspace": {
                "objects": []
            },
            "repository": {
                "files": []
            }
        }
    }
}

```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**adoptdirectory**  <br>*optional*|identifies project whose directory is to be adopted|string||
|**FormData**|**adoptpackages**  <br>*optional*|identifies project whose package dependencies are to be adopted|string||
|**FormData**|**adoptworkspace**  <br>*optional*|identifies project whose workspace is to be adopted|string||
|**FormData**|**artifactsoff**  <br>*optional*|when enabled, artfiacts generated in the working directory are neither cached to the database nor reported in the response markup|boolean||
|**FormData**|**code**  <br>*optional*|specifies the block of R code|string||
|**FormData**|**consoleoff**  <br>*optional*|if `true` console output is not saved on the project execution history for the job|boolean||
|**FormData**|**csvinputs**  <br>*optional*|comma-separated list of primitive name/value inputs|string||
|**FormData**|**echooff**  <br>*optional*|if `true` R commands will not appear in the console output saved on the project execution history for the job|boolean||
|**FormData**|**enableConsoleEvents**  <br>*optional*|if `true` console events are delivered on the event stream when the job executes|boolean||
|**FormData**|**encodeDataFramePrimitiveAsVector**  <br>*optional*|if `true`, data.frame primitives are encoded vectors in R object data returned on call|boolean||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**graphics**  <br>*optional*|specifies preferred R graphics device for execution (png or svg)|string||
|**FormData**|**graphicsheight**  <br>*optional*|specifies preferred height for R graphics device images|integer||
|**FormData**|**graphicswidth**  <br>*optional*|specifies preferred width for R graphics device images|integer||
|**FormData**|**infinity**  <br>*optional*|specifies custom value for Infinity appearing in R object data returned on call, otherwise Infinity is represented by 0x7ff0000000000000L|string||
|**FormData**|**inputs**  <br>*optional*|DeployR-encoded inputs|string||
|**FormData**|**nan**  <br>*optional*|specifies custom value for `NaN` appearing in R object data returned on call, otherwise `NaN` is represented by `null`|string||
|**FormData**|**phantom**  <br>*optional*|if `true` the execution is treated as a **phantom execution**|boolean||
|**FormData**|**preloadbydirectory**  <br>*optional*|comma-separated list of repository directory names|string||
|**FormData**|**preloadfileauthor**  <br>*optional*|comma-separated list of authors, one author per preloadfilename|string||
|**FormData**|**preloadfiledirectory**  <br>*optional*|comma-separated list of directories, one directory per preloadfilename|string||
|**FormData**|**preloadfilename**  <br>*optional*|comma-separated list of repository filenames|string||
|**FormData**|**preloadfileversion**  <br>*optional*|comma-separated list of versions, one version per preloadfilename|string||
|**FormData**|**preloadobjectauthor**  <br>*optional*|comma-separated list of authors, one author per preloadobjectname|string||
|**FormData**|**preloadobjectdirectory**  <br>*optional*|comma-separated list of directories, one directory per preloadobjectname|string||
|**FormData**|**preloadobjectname**  <br>*optional*|comma-separated list of repository object (.rData) filenames|string||
|**FormData**|**preloadobjectversion**  <br>*optional*|comma-separated list of versions, one version per preloadobjectname|string||
|**FormData**|**project**  <br>*required*|specifies the project identifier|string||
|**FormData**|**storedirectory**  <br>*optional*|repository directory for stored files and objects after the execution completes, defaults to root|string||
|**FormData**|**storefile**  <br>*optional*|comma-separated list of working directory filenames|string||
|**FormData**|**storenewversion**  <br>*optional*|if `true`, ensures each file stored in repository results in new version being created if needed|boolean||
|**FormData**|**storeobject**  <br>*optional*|comma-separated list of workspace object names|string||
|**FormData**|**storepublic**  <br>*optional*|if `true`, publishes each file stored in the repository|boolean||
|**FormData**|**storeworkspace**  <br>*optional*|filename (.rData) where workspace contents will be saved in the repository|string||
|**FormData**|**tag**  <br>*optional*|specifies a tag that labels the execution|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectExecuteResponse](#projectexecuteresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectexecuteconsole"></a>
### GET /r/project/execute/console

#### Description
This call retrieves the R console output for the latest execution on  specified project. <br/><br/>
**Example Response (json):**
``` {
    "deployr": {
        "response": {
            "call": "/r/project/execute/console",
            "success": true,
            "uid": "UID-1e518408953d7920e1d7ae261694091c",
            "project": {
                "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
                "name": "Demo Analysis",
                "descr": "Predicting retail sales, updated.",
                "longdescr": null,
                "author": "george",
                "authors": [
                    "george",
                    "testuser"
                ],
                "shared": false,
                "lastmodified": 1378308723041,
                "live": true,
                "cookie": null,
                "origin": "Project original."
            },
            "execution": {
                "console": "n> x <- rnorm(5)n"
            }
        }
    }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**Query**|**project**  <br>*required*|specifies the project identifier|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectExecuteConsoleResponse](#projectexecuteconsoleresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectexecuteflush"></a>
### POST /r/project/execute/flush

#### Description
This call flushes executions in the history on the specified project. <br/><br/>
Flushing an execution deletes both the R console output and the generated results  associated with that execution but does not remove the execution itself from the  history. By omitting the *execution* parameter, the caller can flush every  execution in the history on the specified project. <br/><br/>
This flushing facility is provided to help users manage the levels of  resource usage associated with their *persistent* projects. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/execute/flush",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis",
        "descr": "Predicting retail sales."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**execution**  <br>*optional*|comma-separated list of execution identifiers|string||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**project**  <br>*required*|specifies the project identifier|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectexecutehistory"></a>
### GET /r/project/execute/history

#### Description
This call retrieves the execution history for the specified project. <br/><br/>
Some of the key data indicated for each history item in the response markup  on this call include:

  - *code* - indicates the code that has been executed
  - *console* - indicates the R console output resulting from the code execution
  - *resultsGenerated* - indicates the number of generated results on the execution
  - *resultsAvailable* - indicates the number of generated results still stored on the execution
  - *resourceUsage* - indicates the current storage byte count for results on the execution
  - *execution* - indicates the execution identifier, which can then be used on **/r/project/execution/result/download** call to retrieve results
  - *interrupted* - indicates the interrupted status of execution
  - *error* - on failure, indicates the reason for failure
  - *errorCode* - on failure, indicates the **error code** for failure

<br/><br/>
**Example Response (json):**
```
        
{
    "deployr": {
        "response": {
            "call": "/r/project/execute/history",
            "success": true,
            "uid": "UID-1e518408953d7920e1d7ae261694091c",
            "project": {
                "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
                "name": "Demo Analysis",
                "descr": "Predicting retail sales.",
                "longdescr": null,
                "author": "george",
                "authors": [
                    "george"
                ],
                "lastmodified": 1378319703711,
                "live": true,
                "shared": false,
                "cookie": null,
                "origin": "Project original.",
            },
            "execution": {
                "history": [
                    {
                        "code": "x <- rnorm(5)",
                        "actor": "george",
                        "tag": null,
                        "timeStart": 1378319697120,
                        "timeCode": 35,
                        "timeTotal": 106,
                        "resultsGenerated": 0,
                        "resultsAvailable": 0,
                        "resourceUsage": 17,
                        "console": "n> x <- rnorm(5)n",
                        "execution": "EXEC-82d99c82-8c26-4a71-9576-23d40d51b388",
                        "onDate": 1378319697231,
                        "success": true,
                        "interrupted": false,
                        "warnings": null,
                        "error": null,
                        "errorCode": null
                    },
                    {
                        "code": "plot(x)",
                        "actor": "george",
                        "tag": null,
                        "timeStart": 1378319702391,
                        "timeCode": 61,
                        "timeTotal": 266,
                        "resultsGenerated": 1,
                        "resultsAvailable": 1,
                        "resourceUsage": 3234,
                        "console": "n> plot(x)n",
                        "execution": "EXEC-0808900b-8dfb-43ac-a2ef-f697715b3c8f",
                        "onDate": 1378319702662,
                        "success": true,
                        "interrupted": false,
                        "warnings": null,
                        "error": null,
                        "errorCode": null
                    }
                ]
            }
        }
    }
} ```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**execution**  <br>*optional*|specifies the comma-separated list of execution identifiers on which to filter history|string||
|**Query**|**filterdepth**  <br>*optional*|specifies the max number of executions to be returned in the history|integer||
|**Query**|**filtertag**  <br>*optional*|specifies the execution tag on which to filter history|string||
|**Query**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**Query**|**project**  <br>*required*|specifies the project identifier|string||
|**Query**|**reversed**  <br>*optional*|if true, the execution history is returned in a reverse-chronological order|boolean||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectExecuteHistoryResponse](#projectexecutehistoryresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectexecuteinterrupt"></a>
### POST /r/project/execute/interrupt

#### Description
This call interrupts the current execution on specified project. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/execute/interrupt",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis",
        "descr": "Predicting retail sales, updated."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**project**  <br>*required*|specifies the project identifier|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectexecuteresultdelete"></a>
### POST /r/project/execute/result/delete

#### Description
This call deletes the execution results for the specified project. <br/><br/>
By specifying a value for the execution parameter the caller can delete  only those results on the specified executions. By specifying a value for  the *filename* parameter the caller can delete a specific result on the  specified executions.
<br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/execute/result/delete",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis",
        "descr": "Predicting retail sales."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**execution**  <br>*optional*|comma-separated list of execution identifiers|string||
|**FormData**|**filename**  <br>*optional*|specifies a result file name|string||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**project**  <br>*required*|specifies the project identifier|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectexecuteresultdownload"></a>
### GET /r/project/execute/result/download

#### Description
This call downloads the execution results for the specified project. <br/><br/>
By specifying a value for the *execution* parameter the caller can  download only results on the specified executions. By specifying a value  for the *filename* parameter the caller can download a specific result on  the specified execution. <br/><br/>


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**drophistory**  <br>*optional*|if true, the Content-Disposition response header indicating attachment is omitted|boolean||
|**Query**|**execution**  <br>*optional*|specifies a comma-separated list of execution identifiers|string||
|**Query**|**filename**  <br>*optional*|specifies a result file name|string||
|**Query**|**project**  <br>*required*|specifies project identifier|string||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|file|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


#### Produces

* `application/zip`


<a name="projectexecuteresultlist"></a>
### GET /r/project/execute/result/list

#### Description
This call lists the execution results for the specified project. <br/><br/>
By specifying a value for the *execution* parameter the caller can limit  the response to those results found on a specific execution or set of  executions. <br/><br/>
**Important!** The URLs returned in the response markup on this call  remain valid for as long as the results remain part of the project. <br/><br/>
 
 **Example Response (json):**
 
 ```
 
 {
     "deployr": {
         "response": {
             "call": "/r/project/execute/result/list",
             "success": true,
             "uid": "UID-1e518408953d7920e1d7ae261694091c",
             "project": {
                 "name": "Demo Analysis",
                 "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
                 "descr": "Predicting retail sales."
                 "longdescr": null,
                 "author": "george",
                 "authors": [
                     "george"
                 ],
                 "shared": false,
                 "lastmodified": 1378320574509,
                 "live": true,
                 "cookie": null,
                 "origin": "Project original.",
             },
             "execution": {
                 "results": [
                     {
                         "filename": "unnamedplot001.png",
                         "length": 3223,
                         "md5": "b9e84e5690bcba3cbee03b73b0c9a9f7",
                         "execution": "EXEC-76b503a3-3c2f-4114-b4b2-e3b1b6f88657",
                         "url": "http://dhost:dport/deployr/r/project/execute
                         /result/download/
                         PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca/
                         EXEC-76b503a3-3c2f-4114-b4b2-e3b1b6f88657/unnamedplot001.png",
                         "type": "image/png"
                     }
                 ]
             }
         }
     }
 }

 ```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**execution**  <br>*optional*|comma-separated list of execution identifiers|string||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**project**  <br>*required*|specifies the project identifier|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectExecuteResultListResponse](#projectexecuteresultlistresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectexecutescript"></a>
### POST /r/project/execute/script

#### Description
This call executes repository-managed scripts or external scripts on the  R session identified by the *project* parameter. <br/><br/>
To execute a single repository-managed script the caller must provide  parameter values for *filename, author* and optionally *directory* and  *version*. To execute a chain of repository-managed scripts the caller  must provide a comma-separated list of values on the *filename, author*  and optionally *directory* and *version* parameters. <br/><br/>
To execute a single external script the caller must provide a valid URL  or file path using the *externalsource* parameter. To execute a chain of  external scripts the caller must provide a comma-separated list of  values on the *externalsource* parameter. Note, that in order to execute  an external script, the caller must have been granted the POWER_USER  role. <br/><br/>
Note: A chained execution executes each of the scripts identified on the  call in a sequential fashion on the R session, with execution occurring  in the order specified on the parameter list. <br/><br/>
Please see the **Standard Execution Model** section in this guide for  further details about the parameters on this call. <br/><br/>
Some key data indicated in the response markup on this call:

  - console - indicates the R console output resulting from the execution
  - results - indicates the list of files generated by the R graphics device
  - artifacts - indicates the list of files generated or modified in the working directory
  - objects - indicates the list of R objects returned from the workspace
  - files - indicates the list of files and objects stored in the repository after the execution completes
  - interrupted - indicates the interrupted status of execution
  - error - on failure, indicates the reason for failure
  - errorCode - on failure, indicates the **error code** for failure

<br/>
**Example Response (json):**
```
{
    "deployr": {
        "response": {
            "call": "/r/project/execute/script",
            "success": true
            "interrupted": false,
            "uid": "UID-1e518408953d7920e1d7ae261694091c",
            "project": {
                "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
                "name": "Demo Analysis",
                "descr": "Predicting retail sales, updated."
                "longdescr": null,
                "author": "george",
                "authors": [
                    "george",
                    "testuser"
                ],
                "shared": false,
                "lastmodified": 1378308723041,
                "live": true,
                "cookie": null,
                "origin": "Project original.",
            },
            "execution": {
                "code": "# Create a vector of auto sales for each category
of vechicles. rn# The value contains the number of units sold each weekday:rn# (i.e. 1 car sold on Monday, 3 cars sold on Tuesday...)rn cars <- c(1, 3, 6, 4, 9)rntrucks <- c(2, 5, 4, 5, 12)rnsuvs <- c(4,4,6,6,16)rn rn# Create a table from the vectors.rnautos_data <- cbind(cars, trucks, suvs)rn rn# Show the the table.rn print(autos_data)rn rn# Send the plot to a file called histogram.png. rnpng("histogram.png")rn rn# Create a histogram of auto sales. rnbarplot(as.matrix(autos_data), main="Autos", ylab= "Total", beside=TRUE, col=rainbow(5))rn rn# Place the legend at the top-left corner with no frame, using rainbow colors.rnlegend("topleft", c("Mon","Tue","Wed","Thu","Fri"), cex=0.6, bty="n", fill=rainbow(5))rn# Restore the plot device.rndev.off()rn",
                "resultsAvailable": 0,
                "actor": "george",
                "tag": null,
                "interrupted": false,
                "timeStart": 1378316295608,
                "timeCode": 74
                "timeTotal": 373,
                "resultsGenerated": 0,
                "resultsAvailable": 0,
                "execution": "EXEC-38bc9824-9dc8-4d42-90d3-9cb5d9d366b1",
                "console": "n> # Create a vector of auto sales for each
category of vechicles. n> # The value contains the number of units sold each weekday:n> # (i.e. 1 car sold on Monday, 3 cars sold on Tuesday...)n> cars <- c(1, 3, 6, 4, 9)nn> trucks <- c(2, 5, 4, 5, 12)nn> suvs <- c(4,4,6,6,16)nn> # Create a table from the vectors.n> autos_data <- cbind(cars, trucks, suvs)nn> # Show the the table.n> print(autos_data)n     cars trucks suvsn[1,]    1      2    4n[2,]
    3      5    4n[3,]    6      4    6n[4,]    4      5    6n
[5,]    9     12   16nn> # Send the plot to a file called histogram.png. n> png("histogram.png")nn> # Create a histogram of auto sales.n> barplot(as.matrix(autos_data), main="Autos", ylab= "Total",  beside=TRUE, col=rainbow(5))nn> # Place the legend at the top-left corner with no frame, using rainbow colors.n> legend("topleft", c("Mon","Tue","Wed", "Thu","Fri"), cex=0.6, bty="n", fill=rainbow(5))nn> # Restore the plot device.n> dev.off()npng n  2 n",
                "results": [],
                "artifacts": [
                    {
                        "filename": "histogram.png",
                        "descr": null,
                        "length": 7285,
                        "type": "image/png",
                        "category": "plot",
                        "url": "http://184.106.178.37:7000/deployr/r/
                        project/directory/download/
                        PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca/histogram.png",
                        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
                        "lastmodified": 1378316352000
                    }
                ],
                "warnings": [
                ]
            },
            "repository": {
                "files": []
            },
            "workspace": {
                "objects": []
            }
        }
    }
}        
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**adoptdirectory**  <br>*optional*|identifies project whose directory is to be adopted|string||
|**FormData**|**adoptpackages**  <br>*optional*|identifies project whose package dependencies are to be adopted|string||
|**FormData**|**adoptworkspace**  <br>*optional*|identifies project whose workspace is to be adopted|string||
|**FormData**|**artifactsoff**  <br>*optional*|when enabled, artfiacts generated in the working directory are neither cached to the database nor reported in the response markup|boolean||
|**FormData**|**author**  <br>*optional*|comma-separated list of authors, one author per filename|string||
|**FormData**|**consoleoff**  <br>*optional*|if `true` console output is not saved on the project execution history for the job|boolean||
|**FormData**|**csvinputs**  <br>*optional*|comma-separated list of primitive name/value inputs|string||
|**FormData**|**directory**  <br>*optional*|comma-separated list of repository-managed directories for scripts, defaults to root|string||
|**FormData**|**echooff**  <br>*optional*|if `true` R commands will not appear in the console output saved on the project execution history for the job|boolean||
|**FormData**|**enableConsoleEvents**  <br>*optional*|if `true` console events are delivered on the event stream when the job executes|boolean||
|**FormData**|**encodeDataFramePrimitiveAsVector**  <br>*optional*|if `true`, data.frame primitives are encoded vectors in R object data returned on call|boolean||
|**FormData**|**externalsource**  <br>*optional*|comma-separated list of URLs or file paths to external scripts|string||
|**FormData**|**filename**  <br>*optional*|comma-separated list of repository-managed script filenames|string||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**graphics**  <br>*optional*|specifies preferred R graphics device for execution (png or svg)|string||
|**FormData**|**graphicsheight**  <br>*optional*|specifies preferred height for R graphics device images|integer||
|**FormData**|**graphicswidth**  <br>*optional*|specifies preferred width for R graphics device images|integer||
|**FormData**|**infinity**  <br>*optional*|specifies custom value for Infinity appearing in R object data returned on call, otherwise Infinity is represented by 0x7ff0000000000000L|string||
|**FormData**|**inputs**  <br>*optional*|DeployR-encoded inputs|string||
|**FormData**|**nan**  <br>*optional*|specifies custom value for `NaN` appearing in R object data returned on call, otherwise `NaN` is represented by `null`|string||
|**FormData**|**phantom**  <br>*optional*|if `true` the execution is treated as a **phantom execution**|boolean||
|**FormData**|**preloadbydirectory**  <br>*optional*|comma-separated list of repository directory names|string||
|**FormData**|**preloadfileauthor**  <br>*optional*|comma-separated list of authors, one author per preloadfilename|string||
|**FormData**|**preloadfiledirectory**  <br>*optional*|comma-separated list of directories, one directory per preloadfilename|string||
|**FormData**|**preloadfilename**  <br>*optional*|comma-separated list of repository filenames|string||
|**FormData**|**preloadfileversion**  <br>*optional*|comma-separated list of versions, one version per preloadfilename|string||
|**FormData**|**preloadobjectauthor**  <br>*optional*|comma-separated list of authors, one author per preloadobjectname|string||
|**FormData**|**preloadobjectdirectory**  <br>*optional*|comma-separated list of directories, one directory per preloadobjectname|string||
|**FormData**|**preloadobjectname**  <br>*optional*|comma-separated list of repository object (.rData) filenames|string||
|**FormData**|**preloadobjectversion**  <br>*optional*|comma-separated list of versions, one version per preloadobjectname|string||
|**FormData**|**project**  <br>*required*|specifies the project identifier|string||
|**FormData**|**storedirectory**  <br>*optional*|repository directory for stored files and objects after the execution completes, defaults to root|string||
|**FormData**|**storefile**  <br>*optional*|comma-separated list of working directory filenames|string||
|**FormData**|**storenewversion**  <br>*optional*|if `true`, ensures each file stored in repository results in new version being created if needed|boolean||
|**FormData**|**storeobject**  <br>*optional*|comma-separated list of workspace object names|string||
|**FormData**|**storepublic**  <br>*optional*|if `true`, publishes each file stored in the repository|boolean||
|**FormData**|**storeworkspace**  <br>*optional*|filename (.rData) where workspace contents will be saved in the repository|string||
|**FormData**|**tag**  <br>*optional*|specifies a tag that labels the execution|string||
|**FormData**|**version**  <br>*optional*|comma-separated list of versions, one version per filename|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectExecuteResponse](#projectexecuteresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectexport"></a>
### POST /r/project/export

#### Description
This call exports a compressed archive file for the specified project. <br/><br/>
The set of *drop* parameters allow the caller to selectively drop aspects,  such as workspace, working directory, or execution history of the project  state when generating the archive. The *flushhistory* parameter allows the  caller to preserve the project execution history itself while excluding all  generated console output and results associated with that history. <br/><br/>


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**dropdirectory**  <br>*optional*|if true, the content of the project's working directory is dropped on export.|string||
|**FormData**|**drophistory**  <br>*optional*|if true, the project's execution history is dropped on export.|string||
|**FormData**|**dropworkspace**  <br>*optional*|if true, the content of the project's workspace is dropped on export.|string||
|**FormData**|**flushhistory**  <br>*optional*|if true, the project's execution history is flushed on export.|string||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**project**  <br>*required*|specifies project identifier|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|file|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


#### Produces

* `application/zip`


<a name="projectgrant"></a>
### POST /r/project/grant

#### Description
This call grants authorship of the specified project to other users. <br/><br/>
Refer to the section **Introducing Projects** for a detailed description  of project collaboration and the workflows facilitated by grants. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/grant",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis",
        "descr": "Predicting retail sales, updated."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectimport"></a>
### POST /r/project/import

#### Description
This call imports the specified project archive as a new persistent project. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/import",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Sample Sales Projects (Import)",
        "descr": "Import of latests sales projections."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project generated on import of project named,  Sample Sales Projects."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**descr**  <br>*optional*|specifies a description for the newly imported project|string||
|**FormData**|**file**  <br>*required*|specifies the file|file||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**name**  <br>*required*|specifies the name of the project archive file|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `multipart/form-data`


<a name="projectlist"></a>
### GET /r/project/list

#### Description
This call lists all projects owned by the currently *authenticated*  user and/or all projects shared by other users. <br/><br/>
Shared projects are available as read-only projects to the caller. The  shared or private nature of a project can be controlled using the  **/r/project/about/update** call. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/list",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "projects": [ 
        {
            "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
            "name": "Demo Analysis",
            "descr": "Predicting retail sales.",
            "longdescr": null,
            "author": "george",
            "authors": [
                "george"
            ],
            "shared": false,
            "lastmodified": 1378307321841,
            "live": false,
            "cookie": null,
            "origin": "Project original."
        },
        {
            "project": "PROJECT-8aaa65ab-db33-4a28-b351-579705f5ead9",
            "name": "Demo Graphics",
            "descr": null,
            "longdescr": null,
            "author": "george",
            "authors": [
                "george"
            ],
            "shared": false,
            "lastmodified": 1378307321842,
            "live": false,
            "cookie": null,
            "origin": "Project original."
        }
      ]
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectListResponse](#projectlistresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectping"></a>
### GET /r/project/ping

#### Description
This call pings the specified project to determine if the project is  live on the DeployR grid. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/ping",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis",
        "descr": "Predicting retail sales."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectpool"></a>
### POST /r/project/pool

#### Description
This call creates a pool of temporary projects for the currently  *authenticated* user. <br/><br/>
The *blackbox* parameter ensures that calls on each project in the pool  are limited to the *User Blackbox API Controls*. <br/><br/>
Using the *inputs, preloadfile, preloadobject* and *adopt* parameters each project in the pool can be pre-initialized with data in the  workspace and/or working directory. <br/><br/>
The *inputs* parameter allows the caller to pass **DeployR-encoded** R  object values as inputs. These inputs are turned into R objects in the  workspace of the new R session before the call returns. <br/><br/>
The *csvinputs* parameter allows the caller to pass R object primitive values as comma-separated name/value pairs. These inputs are turned into R objects in the workspace before the execution begins. <br/><br/>
The *preloadbydirectory* parameter allows the caller to load all files within one or more repository-managed directories into the working directory before the call returns. <br/><br/>
The set of *preloadfile* parameters allow the caller to load one or more files from the repository into the working directory of the new R session before the call returns. <br/><br/>
The set of *preloadobject* parameters allow the caller to load one or more binary R objects (.rData) from the repository into the workspace of the new R session before the call returns. <br/><br/>
The set of *adopt* parameters allow the caller to load a pre-existing project workspace, project working directory and/or project package dependencies into the new R session before the call returns. <br/><br/>
**Example Response (json):**
```
{  
   "deployr":{  
      "response":{  
         "call":"/r/project/pool",
         "success":true,
         "projects":[  
            {  
               "project":"PROJECT-cc5434fc-3c8c-426d-a2a3-f2d9d8c978d5",
               "name":null,
               "descr":null,
               "longdescr":null,
               "author":"george",
               "authors":[  
                  "george"
               ],
               "shared":false,
               "lastmodified":1378586882974,
               "live":true,
               "cookie":null,
               "origin":"Project original."
            },
            {  
               "project":"PROJECT-5f4034f4-35d1-4020-8873-41186d71caa8",
               "name":null,
               "descr":null,
               "longdescr":null,
               "author":"george",
               "authors":[  
                  "george"
               ],
               "shared":false,
               "lastmodified":1378586883040,
               "live":true,
               "cookie":null,
               "origin":"Project original."
            },
            {  
               "project":"PROJECT-89004460-a4c8-40f1-98a9-d4cf4d423095",
               "name":null,
               "descr":null,
               "longdescr":null,
               "author":"george",
               "authors":[  
                  "george"
               ],
               "shared":false,
               "lastmodified":1378586883109,
               "live":true,
               "cookie":null,
               "origin":"Project original."
            }
         ],
         "httpcookie":"0873555DFCB6B083C61B6C4FECF2C3BA"
      }
   }
}

```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectListResponse](#projectlistresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectrecycle"></a>
### POST /r/project/recycle

#### Description
This call recycles the R session associated with the project by  deleting all R objects from the workspace and all files from the  working directory. <br/><br/>
Recycling a project is a convenient and efficient alternative to  starting over by closing an existing project and then creating a new  project. <br/><br/>
**Example Response (json):**
```
{ 
  "deployr": { 
    "response": { 
      "success": true,
      "call: "/r/project/recyle",
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": { 
        "project": "PROJECT-2dfb6321-dfa4-431c-88c2-7f7d144865b4",
        "name": null,
        "descr": null,
        "longdescr": null,
        "live": true,
        "shared": false,
        "cookie": null,
        "origin": "Project original.",
        "author": "testuser",
        "authors: [ "testuser" ],
        "lastmodified": 1466396262531 
      } 
    } 
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectsave"></a>
### POST /r/project/save

#### Description
This call saves the persistent state of the specified project. <br/><br/>
The set of *drop* parameters allows the caller to selectively drop aspects, such as workspace, working directory, or execution history of the project  state when saving. The *flushhistory* parameter allows the caller to  preserve the project execution history itself while destroying all  generated console output and results associated with that history. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/save",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis",
        "descr": "Predicting retail sales, saved."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectsaveas"></a>
### POST /r/project/saveas

#### Description
This call saves the persistent state of the specified project to a new  persistent project. <br/><br/>
The set of *drop* parameters allows the caller to selectively drop aspects, such as workspace, working directory, or execution history of the project  state when saving. The *flushhistory* parameter allows the caller to  preserve the project execution history itself while destroying all  generated console output and results associated with that history. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/save",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Demo Analysis Snapshot",
        "descr": "Predicting retail sales, snapshot September 4, 2013."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project copied from project named,  Demo Analysis."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="projectworkspaceget"></a>
### GET /r/project/workspace/get

#### Description
This call retrieves DeployR-encoded objects from the workspace for the specified project. <br/><br/>
**Example Response (json):**
```
{
    "deployr": {
        "response": {
            "call": "/r/project/workspace/get",
            "success": true,
            "uid": "5CB911405DC6EB0F8E283990F7969E63",
            "project": {
                "name": "Demo Analysis",
                "project": "PROJECT-4f9fe8bf-9425-4f2c-aa15-5d94818988f9",
                "descr": "Predicting retail sales."
                "longdescr": null,
                "author": "george",
                "authors": [
                    "george"
                ],
                "shared": false,
                "lastmodified": 1378321946753,
                "live": true,
                "cookie": null,
                "origin": "Project original.",
            },
            "workspace": {
                "objects": [ {
                    "name": "x_matrix",
                    "type": "matrix",
                    "rclass": "matrix",
                    "value": [
                        [
                            1,
                            3,
                            12
                        ],
                        [
                            2,
                            11,
                            13
                        ]
                    ]
                } ]
            }
        }
    }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**encodeDataFramePrimitiveAsVector**  <br>*optional*|if `true`, `data.frame` primitives are encoded vectors in R object data returned on call.|integer||
|**Query**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**Query**|**infinity**  <br>*optional*|specifies a custom value for Infinity appearing in R object data returned on the call, otherwise Infinity is represented by 0x7ff0000000000000L.|boolean||
|**Query**|**length**  <br>*optional*|specifies the segment of object data to retrieve.|integer||
|**Query**|**name**  <br>*required*|specifies a comma-separated list of object names.|string||
|**Query**|**nan**  <br>*optional*|specifies custom value for `NaN` appearing in R object data returned on the call, otherwise `NaN` is represented by `null`.|integer||
|**Query**|**project**  <br>*required*|specifies the project identifier.|string||
|**Query**|**root**  <br>*optional*|specifies the object graph root.|string||
|**Query**|**start**  <br>*optional*|specifies the offset into object data.|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectWorkspaceResponse](#projectworkspaceresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectworkspacelist"></a>
### GET /r/project/workspace/list

#### Description
This call lists the objects in the workspace for the specified project. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/workspace/list",
      "success": true,
      "uid": "5CB911405DC6EB0F8E283990F7969E63",
      "project": {
        "name": "Demo Analysis",
        "project": "PROJECT-4f9fe8bf-9425-4f2c-aa15-5d94818988f9",
        "descr": "Predicting retail sales.",
        "longdescr": null,
        "author": "george",
        "authors": [
          "george"
        ],
        "shared": false,
        "lastmodified": 1378321615345,
        "live": true,
        "cookie": null,
        "origin": "Project original."
      },
      "workspace": {
        "objects": [
          {
            "branch": false,
            "rclass": "character",
             "name": "x_character",
             "type": "primitive",
             "xdf": null
            },
            {
              "branch": true,
              "rclass": "data.frame",
              "name": "x_dataframe",
              "type": "dataframe",
              "xdf": null
            },
            {
              "branch": false,
              "rclass": "matrix",
              "name": "x_matrix",
              "type": "matrix",
              "xdf": null
            }
        ]
      }
    }
  }
} ```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**clazz**  <br>*optional*|specifies the R class based filter.|string||
|**Query**|**filter**  <br>*optional*|specifies the R object name based filter.|string||
|**Query**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**Query**|**pageoffset**  <br>*optional*|specifies the page offset for paging results in response markup.|integer||
|**Query**|**pagesize**  <br>*optional*|specifies the page size for paging results in response markup.|integer||
|**Query**|**project**  <br>*required*|specifies the project identifier.|string||
|**Query**|**restrict**  <br>*optional*|if `true`, limits returned objects to object types with supported DeployR-encoding|boolean||
|**Query**|**root**  <br>*optional*|specifies the object graph root.|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectWorkspaceResponse](#projectworkspaceresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `application/x-www-form-urlencoded`


<a name="projectworkspaceupload"></a>
### POST /r/project/workspace/upload

#### Description
This call uploads a binary object from file into the workspace for the  specified *project*. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/project/workspace/upload",
      "success": true,
      "uid": "UID-1e518408953d7920e1d7ae261694091c",
      "project": {
        "project": "PROJECT-e2edd15f-dd67-42f6-8d82-90a2a5a669ca",
        "name": "Sample Sales Projects (Import)",
        "descr": "Upload of latests sales projections."
        "longdescr": null,
        "author": "george",
        "authors": [
            "george"
        ],
        "shared": false,
        "lastmodified": 1378307626647,
        "live": false,
        "cookie": null,
        "origin": "Project original."
      }
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**file**  <br>*required*|specifies the file|file||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**name**  <br>*required*|specifies the name of the object file|string||
|**FormData**|**project**  <br>*required*|specifies the project identifier|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[ProjectResponse](#projectresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


#### Consumes

* `multipart/form-data`


<a name="userabout"></a>
### POST /r/user/about

#### Description
This call retrieves details about the currently authenticated user. The details  returned in the response markup on this call are exactly the same details as  those returned in the response markup on the /r/user/login call. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/user/about",
      "success": true,
      "user": {
        "username": "george",
        "displayname": "George Best",
        "permissions": {
            "scriptManager": true,
            "powerUser": true,
            "packageManager": true,
            "administrator": false,
            "basicUser": true
        },
        "cookie": null
      },
      "limits": {
        "maxIdleLiveProjectTimeout": 1800,
        "maxConcurrentLiveProjectCount": 170,
        "maxFileUploadSize": 268435456
      },
      "uid": "UID-5CB911405DC6EB0F8E283990F7969E63",
      "X-XSRF-TOKEN": "53708abe-c5c2-4091-9ced-6314d49de0a3"  
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[UserAboutResponse](#useraboutresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="userautosave"></a>
### POST /r/user/autosave

#### Description
This call enables or disables the autosave semantics on persistent projects for the duration of the current users HTTP session. By  default, all live persistent projects are autosaved under the following conditions: <br/>

  - When a user closes a project using the **/r/project/close** call.

  - When a user signs-out using the **/r/user/logout** call.

  - When a user is automatically signed-out by the system after a prolonged period of inactivity.

  - When the autosave feature is disabled a user must make an explicit call on **/r/project/save** in order to save a project.

<br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/user/autosave",
      "success": true,
      "user": {
        "username": "george",
        "displayname": "George Best",
        "permissions": {
            "scriptManager": true,
            "powerUser": true,
            "packageManager": true,
            "administrator": false,
            "basicUser": true
        },
        "cookie": null
      },
      "limits": {
        "maxIdleLiveProjectTimeout": 1800,
        "maxConcurrentLiveProjectCount": 170,
        "maxFileUploadSize": 268435456
      },
      "uid": "UID-5CB911405DC6EB0F8E283990F7969E63"
    }
  }
} ```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**enable**  <br>*optional*|toggles autosave semantics for persistent projects.|boolean||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[UserAutoSaveResponse](#userautosaveresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="userlogin"></a>
### POST /r/user/login

#### Description
This call signs the user in by authenticating the credentials with the  DeployR server.</br></br>
If the login fails for any reason the _errorCode_ property in the response markup will indicate the underlying cause. See the **API Response Codes** section of this document for details.
<br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/user/login",
      "success": true,
      "user": {
        "username": "george",
        "displayname": "George Best",
        "permissions": {
            "scriptManager": true,
            "powerUser": true,
            "packageManager": true,
            "administrator": false,
            "basicUser": true
        },
        "cookie": null
      },
      "limits": {
        "maxIdleLiveProjectTimeout": 1800,
        "maxConcurrentLiveProjectCount": 170,
        "maxFileUploadSize": 268435456
      },
      "uid": "UID-5CB911405DC6EB0F8E283990F7969E63",
      "X-XSRF-TOKEN": "53708abe-c5c2-4091-9ced-6314d49de0a3"
    }
  }
} ```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**disableautosave**  <br>*optional*|disables autosave semantics for persistent projects|boolean||
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**password**  <br>*required*|specifies the password|string||
|**FormData**|**username**  <br>*required*|specifies the username|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[UserLoginResponse](#userloginresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**940**|Credentials provided could not be authenticated.|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="userlogout"></a>
### POST /r/user/logout

#### Description
This call signs out the currently authenticated user.
<br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/user/logout",
      "success": true,
      "uid": "UID-5CB911405DC6EB0F8E283990F7969E63"
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||
|**FormData**|**usercookie**  <br>*optional*|when specified, value sets application-specific persistent user cookie, which is retrievable on response to **/r/user/login** call.|string||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[UserLogoutResponse](#userlogoutresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|


<a name="userrelease"></a>
### POST /r/user/release

#### Description
This call releases server-wide grid resources held by the currently  authenticated user.<br/><br/>
Typically a call to **/r/project/close** is required to shutdown a  specific live R session. Closing the project releases all grid resources associated with the project. However, that call requires  the user to be connected on the same HTTP session where the project  was originally created. In the event of a network connection failure, for example, in many cases the client will no longer be able to  establish a connection on the original HTTP session. As such, the only way those grid resources will be released is when the idle  timeout for the project is finally reached, at which point, the  resources will be automatically reclaimed by the server.<br/><br/>
As the idle timeout period can be arbitrarily long (per Admin Console Server Policies) this call is provided so client applications can take  back control and actively release server-wide grid resources on demand.  This is particularly useful when a user acquires a large amount of grid  resources, for example, when using the **/r/project/pool** feature. <br/><br/>
**Example Response (json):**
```
{
  "deployr": {
    "response": {
      "call": "/r/user/release",
      "success": true,
      "whoami": "george",
      "uid": "UID-5CB911405DC6EB0F8E283990F7969E63"
    }
  }
}
```


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**FormData**|**format**  <br>*required*|specifies markup encoding on call (json or xml).|enum (json, xml)||


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Success|[UserReleaseResponse](#userreleaseresponse)|
|**400**|Bad Request: invalid data on call|No Content|
|**401**|Unauthorized Access: caller has insufficient privileges|No Content|
|**403**|Forbidden Access: caller is unauthorized|No Content|
|**405**|HTTP Method Disallowed: disallowed HTTP method on call|No Content|
|**503**|Service Temporarily Unavailable: HTTP session temporarily invalidated|No Content|
|**default**|Error|[ErrorResponse](#errorresponse)|





<a name="definitions"></a>
## Definitions

<a name="basejobresponse"></a>
### BaseJobResponse
Defines the base /r/job/* response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#basejobresponse-deployr)|

<a name="basejobresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#basejobresponse-response)|

<a name="basejobresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**job**  <br>*required*||[JobModel](#jobmodel)|


<a name="baseresponse"></a>
### BaseResponse
Defines the base abstract representation of a DeployR response.


|Name|Description|Schema|
|---|---|---|
|**call**  <br>*required*||string|
|**success**  <br>*required*||boolean|
|**uid**  <br>*required*||string|


<a name="baseuserresponse"></a>
### BaseUserResponse
The Base /r/user/* response model

*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**call**  <br>*required*||string|
|**success**  <br>*required*||boolean|
|**uid**  <br>*required*||string|


<a name="errorcodes"></a>
### ErrorCodes
When an API call returns an HTTP 200 status code the application should still test the success property in the response markup on each call to determine whether the DeployR server was able to successfully execute the service on behalf of the caller.<br><br> If the success property in the response markup indicates failure (with a value of false), then the application can inspect the error and errorCode properties in the response markup to determine the underlying cause of that failure.<br><br> The error property provides a plain text message describing the underlying failure. The errorCode property indicates the nature of the underlying error. Possible values for the errorCode property are shown here

*Type* : enum (, , , , , , , , , , , , , , , , , , , , )


<a name="errorresponse"></a>
### ErrorResponse

|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#errorresponse-deployr)|

<a name="errorresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*optional*||[response](#errorresponse-response)|

<a name="errorresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**error**  <br>*required*||string|
|**errorCode**  <br>*required*||[ErrorCodes](#errorcodes)|


<a name="file"></a>
### File
Defines the abstract repository-managed file (script, .RData, ect...).


|Name|Description|Schema|
|---|---|---|
|**access**  <br>*required*||string|
|**author**  <br>*required*||string|
|**authors**  <br>*required*||< string > array|
|**category**  <br>*required*||string|
|**createdDate**  <br>*required*||string|
|**directory**  <br>*required*||string|
|**filename**  <br>*required*||string|
|**id**  <br>*required*||string|
|**lastModified**  <br>*required*||integer|
|**latestby**  <br>*required*||string|
|**length**  <br>*required*||integer|
|**published**  <br>*required*||boolean|
|**sha256**  <br>*required*||string|
|**shared**  <br>*required*||boolean|
|**type**  <br>*required*||string|
|**url**  <br>*required*||string|
|**versioned**  <br>*required*||boolean|


<a name="history"></a>
### History
Defines the history model.


|Name|Description|Schema|
|---|---|---|
|**actor**  <br>*required*||string|
|**code**  <br>*required*||string|
|**console**  <br>*required*||string|
|**execution**  <br>*required*||string|
|**interrupted**  <br>*required*||boolean|
|**onDate**  <br>*required*||integer|
|**resourceUsage**  <br>*required*||integer|
|**resultsAvailable**  <br>*required*||integer|
|**resultsGenerated**  <br>*required*||integer|
|**success**  <br>*required*||boolean|
|**timeCode**  <br>*required*||integer|
|**timeStart**  <br>*required*||integer|
|**timeTotal**  <br>*required*||integer|


<a name="jobcancelresponse"></a>
### JobCancelResponse
Defines the/r/job/cancel response model

*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#jobcancelresponse-deployr)|

<a name="jobcancelresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#jobcancelresponse-response)|

<a name="jobcancelresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**job**  <br>*required*||[JobModel](#jobmodel)|


<a name="jobdeleteresponse"></a>
### JobDeleteResponse
Defines the/r/job/delete response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#jobdeleteresponse-deployr)|

<a name="jobdeleteresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||object|


<a name="joblistresponse"></a>
### JobListResponse
Defines the/r/job/list response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#joblistresponse-deployr)|

<a name="joblistresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#joblistresponse-response)|

<a name="joblistresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**jobs**  <br>*optional*||< [JobModel](#jobmodel) > array|


<a name="jobmodel"></a>
### JobModel
The Job APIs facilitate working with DeployR managed jobs. Jobs support the execution of commands in the background on behalf of users.


|Name|Description|Schema|
|---|---|---|
|**consoleoff**  <br>*optional*||boolean|
|**echooff**  <br>*optional*||boolean|
|**enableConsoleEvents**  <br>*optional*||boolean|
|**graphics**  <br>*optional*||string|
|**inputsformat**  <br>*optional*||string|
|**job**  <br>*required*||string|
|**name**  <br>*required*||string|
|**onrepeat**  <br>*optional*||integer|
|**priority**  <br>*optional*||string|
|**rscriptdirectory**  <br>*optional*||string|
|**schedinterval**  <br>*optional*||integer|
|**schedrepeat**  <br>*optional*||integer|
|**schedstart**  <br>*optional*||integer|
|**status**  <br>*required*||[JobStates](#jobstates)|
|**storedirectory**  <br>*optional*||string|
|**storenewversion**  <br>*optional*||boolean|
|**storenoproject**  <br>*optional*||boolean|
|**storepublic**  <br>*optional*||boolean|
|**timeCode**  <br>*optional*||integer|
|**timeStart**  <br>*optional*||integer|
|**timeTotal**  <br>*optional*||integer|


<a name="jobqueryresponse"></a>
### JobQueryResponse
Defines the/r/job/query response model

*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#jobqueryresponse-deployr)|

<a name="jobqueryresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#jobqueryresponse-response)|

<a name="jobqueryresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**job**  <br>*required*||[JobModel](#jobmodel)|


<a name="jobscheduleresponse"></a>
### JobScheduleResponse
Defines the/r/job/schedule response model

*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#jobscheduleresponse-deployr)|

<a name="jobscheduleresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#jobscheduleresponse-response)|

<a name="jobscheduleresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**job**  <br>*required*||[JobModel](#jobmodel)|


<a name="jobstates"></a>
### JobStates
The different job states:
 - Scheduled: job is scheduled but not yet queued for running.
 - Queued: job is queued for running.
 - Running: job is running.
 - Completed: job is completed.

*Type* : enum (Scheduled, Queued, Running, Completed)


<a name="jobsubmitresponse"></a>
### JobSubmitResponse
Defines the/r/job/submit response model

*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#jobsubmitresponse-deployr)|

<a name="jobsubmitresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#jobsubmitresponse-response)|

<a name="jobsubmitresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**job**  <br>*required*||[JobModel](#jobmodel)|


<a name="limitsmodel"></a>
### LimitsModel

|Name|Description|Schema|
|---|---|---|
|**maxConcurrentLiveProjectCount**  <br>*required*||integer|
|**maxFileUploadSize**  <br>*required*||integer|
|**maxIdleLiveProjectTimeout**  <br>*required*||integer|


<a name="project"></a>
### Project
Defines the project model.


|Name|Description|Schema|
|---|---|---|
|**author**  <br>*required*||string|
|**authors**  <br>*required*||< string > array|
|**lastmodified**  <br>*required*||integer|
|**live**  <br>*required*||boolean|
|**project**  <br>*required*||string|
|**shared**  <br>*required*||boolean|


<a name="projectexecuteconsoleresponse"></a>
### ProjectExecuteConsoleResponse
Defines the /r/project/execute/console response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#projectexecuteconsoleresponse-deployr)|

<a name="projectexecuteconsoleresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#projectexecuteconsoleresponse-response)|

<a name="projectexecuteconsoleresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**execution**  <br>*required*||[execution](#projectexecuteconsoleresponse-response-execution)|
|**project**  <br>*required*||[Project](#project)|

<a name="projectexecuteconsoleresponse-response-execution"></a>
**execution**

|Name|Description|Schema|
|---|---|---|
|**console**  <br>*required*||string|


<a name="projectexecutehistoryresponse"></a>
### ProjectExecuteHistoryResponse
Defines the /r/project/execute/history response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#projectexecutehistoryresponse-deployr)|

<a name="projectexecutehistoryresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#projectexecutehistoryresponse-response)|

<a name="projectexecutehistoryresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**execution**  <br>*required*||[execution](#projectexecutehistoryresponse-response-execution)|
|**project**  <br>*required*||[Project](#project)|

<a name="projectexecutehistoryresponse-response-execution"></a>
**execution**

|Name|Description|Schema|
|---|---|---|
|**history**  <br>*required*||< [History](#history) > array|


<a name="projectexecuteresponse"></a>
### ProjectExecuteResponse
Defines the base /r/project/execute/* response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#projectexecuteresponse-deployr)|

<a name="projectexecuteresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#projectexecuteresponse-response)|

<a name="projectexecuteresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**execution**  <br>*required*||[execution](#projectexecuteresponse-response-execution)|
|**interrupted**  <br>*required*||boolean|
|**project**  <br>*required*||[Project](#project)|
|**repository**  <br>*required*||[repository](#projectexecuteresponse-response-repository)|
|**workspace**  <br>*required*||[workspace](#projectexecuteresponse-response-workspace)|

<a name="projectexecuteresponse-response-execution"></a>
**execution**

|Name|Description|Schema|
|---|---|---|
|**actor**  <br>*required*||string|
|**artifacts**  <br>*optional*||< [artifacts](#projectexecuteresponse-response-execution-artifacts) > array|
|**code**  <br>*required*||string|
|**console**  <br>*required*||string|
|**execution**  <br>*required*||string|
|**info**  <br>*optional*||< string > array|
|**results**  <br>*required*||< [results](#projectexecuteresponse-response-execution-results) > array|
|**resultsAvailable**  <br>*required*||integer|
|**resultsGenerated**  <br>*required*||integer|
|**timeCode**  <br>*required*||integer|
|**timeStart**  <br>*required*||integer|
|**timeTotal**  <br>*required*||integer|
|**warning**  <br>*optional*||< string > array|

<a name="projectexecuteresponse-response-execution-artifacts"></a>
**artifacts**

|Name|Description|Schema|
|---|---|---|
|**category**  <br>*required*||string|
|**filename**  <br>*required*||string|
|**lastmodified**  <br>*required*||integer|
|**length**  <br>*required*||integer|
|**project**  <br>*required*||string|
|**type**  <br>*required*||string|
|**url**  <br>*required*||string|

<a name="projectexecuteresponse-response-execution-results"></a>
**results**

|Name|Description|Schema|
|---|---|---|
|**execution**  <br>*required*||string|
|**filename**  <br>*required*||string|
|**length**  <br>*required*||integer|
|**sha256**  <br>*required*||string|
|**type**  <br>*required*||string|
|**url**  <br>*required*||string|

<a name="projectexecuteresponse-response-repository"></a>
**repository**

|Name|Description|Schema|
|---|---|---|
|**files**  <br>*required*||< [File](#file) > array|

<a name="projectexecuteresponse-response-workspace"></a>
**workspace**

|Name|Description|Schema|
|---|---|---|
|**objects**  <br>*required*||< [RObject](#robject) > array|


<a name="projectexecuteresultlistresponse"></a>
### ProjectExecuteResultListResponse
Defines the /r/project/execute/result/list response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#projectexecuteresultlistresponse-deployr)|

<a name="projectexecuteresultlistresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#projectexecuteresultlistresponse-response)|

<a name="projectexecuteresultlistresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**execution**  <br>*required*||[execution](#projectexecuteresultlistresponse-response-execution)|
|**project**  <br>*required*||[Project](#project)|

<a name="projectexecuteresultlistresponse-response-execution"></a>
**execution**

|Name|Description|Schema|
|---|---|---|
|**results**  <br>*required*||< [results](#projectexecuteresultlistresponse-response-execution-results) > array|

<a name="projectexecuteresultlistresponse-response-execution-results"></a>
**results**

|Name|Description|Schema|
|---|---|---|
|**execution**  <br>*required*||string|
|**length**  <br>*required*||integer|
|**sha256**  <br>*required*||string|
|**type**  <br>*required*||string|


<a name="projectlistresponse"></a>
### ProjectListResponse
Defines the base /r/project/list and /r/project/poold response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#projectlistresponse-deployr)|

<a name="projectlistresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#projectlistresponse-response)|

<a name="projectlistresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**projects**  <br>*required*||< [Project](#project) > array|


<a name="projectresponse"></a>
### ProjectResponse
Defines the base /r/project/* response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#projectresponse-deployr)|

<a name="projectresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#projectresponse-response)|

<a name="projectresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**project**  <br>*required*||[Project](#project)|


<a name="projectworkspaceresponse"></a>
### ProjectWorkspaceResponse
Defines the /r/project/workspace/list and /r/project/workspace/list response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#projectworkspaceresponse-deployr)|

<a name="projectworkspaceresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#projectworkspaceresponse-response)|

<a name="projectworkspaceresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**execution**  <br>*required*||[execution](#projectworkspaceresponse-response-execution)|
|**project**  <br>*required*||[Project](#project)|

<a name="projectworkspaceresponse-response-execution"></a>
**execution**

|Name|Description|Schema|
|---|---|---|
|**workspace**  <br>*required*||< [Workspace](#workspace) > array|


<a name="robject"></a>
### RObject
Defines the R Object model in the workspace.


|Name|Description|Schema|
|---|---|---|
|**format**  <br>*optional*||string|
|**labels**  <br>*optional*||< string > array|
|**name**  <br>*required*||string|
|**orderd**  <br>*optional*||boolean|
|**rclass**  <br>*required*||enum (character, integer, numeric, logical, date, posixct, POSIXct, matrix, ordered, factor, list, data.frame)|
|**type**  <br>*required*||enum (primitive, date, matrix, factor, list, dataframe, vector)|


<a name="repositorydirectoryresponse"></a>
### RepositoryDirectoryResponse
Defines the base /r/repository/directory/* response model.


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#repositorydirectoryresponse-deployr)|

<a name="repositorydirectoryresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#repositorydirectoryresponse-response)|

<a name="repositorydirectoryresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**repository**  <br>*required*||[repository](#repositorydirectoryresponse-response-repository)|

<a name="repositorydirectoryresponse-response-repository"></a>
**repository**

|Name|Description|Schema|
|---|---|---|
|**directory**  <br>*required*||[directory](#repositorydirectoryresponse-response-repository-directory)|

<a name="repositorydirectoryresponse-response-repository-directory"></a>
**directory**

|Name|Description|Schema|
|---|---|---|
|**directory**  <br>*required*||string|
|**owner**  <br>*required*||string|


<a name="repositorydirectoryistresponse"></a>
### RepositoryDirectoryistResponse
Defines the/r/repository/directory/list response model.


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#repositorydirectoryistresponse-deployr)|

<a name="repositorydirectoryistresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#repositorydirectoryistresponse-response)|

<a name="repositorydirectoryistresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**repository**  <br>*required*||[repository](#repositorydirectoryistresponse-response-repository)|

<a name="repositorydirectoryistresponse-response-repository"></a>
**repository**

|Name|Description|Schema|
|---|---|---|
|**metadata**  <br>*required*||[metadata](#repositorydirectoryistresponse-response-repository-metadata)|
|**system**  <br>*required*||< [system](#repositorydirectoryistresponse-response-repository-system) > array|
|**user**  <br>*required*||< [user](#repositorydirectoryistresponse-response-repository-user) > array|

<a name="repositorydirectoryistresponse-response-repository-metadata"></a>
**metadata**

|Name|Description|Schema|
|---|---|---|
|**archived**  <br>*required*||< string > array|
|**custom**  <br>*required*||< string > array|

<a name="repositorydirectoryistresponse-response-repository-system"></a>
**system**

|Name|Description|Schema|
|---|---|---|
|**directory**  <br>*required*||string|
|**files**  <br>*required*||< [File](#file) > array|

<a name="repositorydirectoryistresponse-response-repository-user"></a>
**user**

|Name|Description|Schema|
|---|---|---|
|**directory**  <br>*required*||string|
|**files**  <br>*required*||< [File](#file) > array|


<a name="repositoryfilelistresponse"></a>
### RepositoryFileListResponse
Defines the/r/repository/file/list response model.


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#repositoryfilelistresponse-deployr)|

<a name="repositoryfilelistresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#repositoryfilelistresponse-response)|

<a name="repositoryfilelistresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**repository**  <br>*required*||[repository](#repositoryfilelistresponse-response-repository)|

<a name="repositoryfilelistresponse-response-repository"></a>
**repository**

|Name|Description|Schema|
|---|---|---|
|**files**  <br>*required*||< [File](#file) > array|


<a name="repositoryfileresponse"></a>
### RepositoryFileResponse
Defines the base /r/repository/file/* response model.


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#repositoryfileresponse-deployr)|

<a name="repositoryfileresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#repositoryfileresponse-response)|

<a name="repositoryfileresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**repository**  <br>*required*||[repository](#repositoryfileresponse-response-repository)|

<a name="repositoryfileresponse-response-repository"></a>
**repository**

|Name|Description|Schema|
|---|---|---|
|**file**  <br>*required*||object|


<a name="repositoryscriptlistresponse"></a>
### RepositoryScriptListResponse
Defines the/r/repository/script/list response model.


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#repositoryscriptlistresponse-deployr)|

<a name="repositoryscriptlistresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||[response](#repositoryscriptlistresponse-response)|

<a name="repositoryscriptlistresponse-response"></a>
**response**

|Name|Description|Schema|
|---|---|---|
|**repository**  <br>*required*||[repository](#repositoryscriptlistresponse-response-repository)|

<a name="repositoryscriptlistresponse-response-repository"></a>
**repository**

|Name|Description|Schema|
|---|---|---|
|**scripts**  <br>*required*||< [File](#file) > array|


<a name="response"></a>
### Response
Defines the basic /r/**/* response model.


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#response-deployr)|

<a name="response-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||object|


<a name="useraboutresponse"></a>
### UserAboutResponse
Defines the/r/user/about response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#useraboutresponse-deployr)|

<a name="useraboutresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||object|


<a name="userautosaveresponse"></a>
### UserAutoSaveResponse
Defines the/r/user/autosave response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#userautosaveresponse-deployr)|

<a name="userautosaveresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||object|


<a name="userloginresponse"></a>
### UserLoginResponse
Defines the /r/user/login response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#userloginresponse-deployr)|

<a name="userloginresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||object|


<a name="userlogoutresponse"></a>
### UserLogoutResponse
Defines the/r/user/logout response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#userlogoutresponse-deployr)|

<a name="userlogoutresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||object|


<a name="usermodel"></a>
### UserModel
The User APIs exist principally to facilitate user authentication with the DeployR server.


|Name|Description|Schema|
|---|---|---|
|**displayname**  <br>*required*||string|
|**permissions**  <br>*required*||[permissions](#usermodel-permissions)|
|**username**  <br>*required*||string|

<a name="usermodel-permissions"></a>
**permissions**

|Name|Description|Schema|
|---|---|---|
|**ADMINISTRATOR**  <br>*required*||boolean|
|**BASIC_USER**  <br>*required*||boolean|
|**PACKAGE_MANAGER**  <br>*required*||boolean|
|**POWER_USER**  <br>*required*||boolean|


<a name="userreleaseresponse"></a>
### UserReleaseResponse
Defines /r/user/release response model


|Name|Description|Schema|
|---|---|---|
|**deployr**  <br>*required*||[deployr](#userreleaseresponse-deployr)|

<a name="userreleaseresponse-deployr"></a>
**deployr**

|Name|Description|Schema|
|---|---|---|
|**response**  <br>*required*||object|


<a name="workspace"></a>
### Workspace

|Name|Description|Schema|
|---|---|---|
|**workspace**  <br>*required*||[workspace](#workspace-workspace)|

<a name="workspace-workspace"></a>
**workspace**

|Name|Description|Schema|
|---|---|---|
|**objects**  <br>*required*||< [RObject](#robject) > array|


<a name="package"></a>
### package
Defines the package model.


|Name|Description|Schema|
|---|---|---|
|**attached**  <br>*optional*||boolean|
|**name**  <br>*required*||string|
|**version**  <br>*optional*||string|



