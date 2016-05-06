---

# required metadata
title: "DeployR Administration Console Help"
description: "Managing Server Policies in the DeployR Administration Console"
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "03/17/2016"
ms.topic: "article"
ms.prod: "deployr"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

# Managing Server Policies

The **Server Policies** tab contains the policies governing the DeployR server, including:

- Global settings such as name, default server boundary, and console timeout
- Project persistence policies governing resource usage (sizes and autosave)
- Runtime policies governing authenticated, asynchronous, and anonymous operations
- Runtime policies governing concurrent operation limits, file upload limits, and event stream access

_Figure: Server Policies tab_

![](media/deployr-admin-managing-server-policies/03000022_624x429.png)  

## Server Policy Properties

There are a number of settings that can be defined for each server. They are grouped into the following categories:

-  [Basic settings](#basic-settings)
-  [Authenticated Operation Policies](#authenticated-operation-policies)
-  [Asynchronous Operation Policies](#asynchronous-operation-policies)
-  [Anonymous Operation Policies](#anonymous-operation-policies)
-  [Project Persistence Policies](#project-persistence-policies)
-  [Concurrent Operation Policies](#concurrent-operation-policies)
-  [Event Stream Access Policies](#event-stream-access-policies)
-  [Server Runtime Policies](#server-runtime-policies)

<br/>
### Basic Settings

|Properties|Description|
|---|---|
|`Server name`|Descriptive name for the server that is used in the R script developer summary page to identify this server instance.|
|`Server web context`|This is the base URL representing the IP address and Servlet Web application context for the live DeployR server. The base URL for the Servlet Web application context defaults to `/deployr`.<br /><br />By default, the server tries to automatically detect the machine’s IP address each time the server machine is rebooted. This IP address is then used to construct the URL displayed in this field. You can disable this function using the setting below this one on this page.<br /><br />If the administrator wants to change the Web application context for the server, then they must update the Web application context component of this URL accordingly. Use a valid IP address and port number, and follow the format:<br /><br /> `http://xx.xx.xx.xx:<HTTP_PORT>/deployr`, where the port is `8000` or `8500` depending on your version<br /> or ([if HTTPS is enabled](../deployr-admin-security.md#enable-server-ssl-https))<br />`https://xx.xx.xx.xx:<HTTPS_PORT>/deployr`, where the port is `8001` or `8501` depending on your version<br /> <br /> In some cases, the auto-detection process may return an IP address that is not the one that the administrator intended for DeployR. For example, if the server host machine is configured to support multiple IP addresses, then there is a chance that the wrong address is auto-detected. In another case, when DeployR is installed on an [Azure or AWS EC2](../deployr-admin-install-in-cloud.md) instance, then the internal IP might be detected rather that the external IP. And, in another example, the IP address that once worked may no longer be the right number.  There may also be times when the lease on a dynamically-allocated IP address expires, thereby causing the host machine to acquire a new IP address while the server is live.<br /> <br /> To remedy these cases, the administrator can override the IP that was auto-detected by replacing the IP number in the URL listed in this field with the desired IP address. This fix will work until the next time the server machine is restarted. After booting, the IP auto-detection process is run again and the IP number in theServer Web context URL is refreshed.<br /><br /> **Important:** There may be times where you cannot reach the Administration Console due to this issue. [In that case, you must update this value and disable autodetection](../deployr-admin-diagnostics-troubleshooting.md#landing-page-cannot-be-reached).|
|`Disable IP auto-detection` |To impose an IP address, enter the correct IP address in the **Server Web context** field and then set **Disable IP auto-detection** to `True`.<br /><br />If you set this option to True by selecting the checkbox, you will turn off the auto-detection process and the IP number listed in the **Server Web context** URL will persist across server reboots.<br /><br />If set to False (no check), then auto-detection is enabled and the server machine will try to automatically detect and refresh the machine’s IP address each time the server machine is rebooted. This IP address is then used to create the URL displayed in **Server Web context** field.|
|`Default R boundary`| Name of an R boundary to be applied as the system-wide default. Grid node boundaries takes precedence over boundaries associated with users. Likewise, grid node R boundaries and user R boundaries take precedence over this default R boundary. If no other boundary is specified, then the default system-wide boundary is applied.|
|`Console idle timeout`|The length of idle time in seconds before a session in the Administration Console expires.

<br/>
### Authenticated Operation Policies

These policies govern how [authenticated operations](deployr-admin-managing-the-grid.md#node-operation-types) are handled. Authenticated operations refer to operations on projects requested by authenticated users. These operations are executed on grid nodes designated for either authenticated or mixed operation modes.

|Properties|Description|
|---|---|
|`HTTPS encrypted`|<small>(Applies to older versions of DeployR only)</small><br>By default this option is unchecked, which sets it to False, where incoming calls on the API can be made over a plain HTTP connection. If set to True, the server only accepts incoming API calls over an encrypted channel (HTTPS). For more information, see the [Security for DeployR](../deployr-admin-security.md#enable-server-ssl-https) guide for your version.<br /><br /> **Note:** If you enable HTTPS for one or more operations types, you must also provide a valid HTTPS URL in the Server web context property on this page.|
|`IP filter`|The name of the filter to be applied to all authenticated operations. If defined, then only authenticated users who connect from a qualified IP address (as defined by the filter) can make calls on the API.<br /><br />The [IP filtering](deployr-admin-managing-access-with-ip-filters.md) restrictions specified on a server context determines the full publicly accessible exposure of that server context.|
|`API timeout`|The length of time in seconds that an authenticated user can remain idle while connected to the server before the HTTP session is automatically timed-out and disconnected. 

<br/>
### Asynchronous Operation Policies

These policies govern how [asynchronous operations](deployr-admin-managing-the-grid.md#node-operation-types) are handled. Asynchronous operations are scheduled jobs executing in the background on behalf of authenticated users. These jobs are executed on grid nodes designated for either asynchronous or mixedoperation modes.

|Properties|Description|
|---|---|
|`HTTPS encrypted`|<small>(Applies to older versions of DeployR only)</small><br>By default this option is unchecked, which sets it to False, where calls on the API can be made over a plain HTTP connection. If set to true, the server will only accept API calls over an encrypted channel (HTTPS). For more information, see the [Security for DeployR](../deployr-admin-security.md#enable-server-ssl-https) guide.<br /><br /> **Note:** If you enable HTTPS for one or more operations types, you must also provide a valid HTTPS URL in the Server web context property on this page.|
|`IP filter`|The filter to be applied to all authenticated operations. If defined, then only jobs from authenticated users who connect from a qualified IP address can make calls on the API. The [IP filtering](deployr-admin-managing-access-with-ip-filters.md) restrictions specified on a server context determines the full publicly accessible exposure of that server context.|

<br/>
### Anonymous Operation Policies

These policies govern how [anonymous operations](deployr-admin-managing-the-grid.md#node-operation-types) are handled. Anonymous operations are executed on grid nodes designated for either anonymous or mixed operation modes. Anonymous operations refer to operations from users executing scripts anonymously.

|Properties|Description|
|---|---|
|`HTTPS encrypted`|<small>(Applies to older versions of DeployR only)</small><br>By default this option is unchecked, which sets it to False, where scripts can be executed over a plain HTTP connection. If set to true, the server only accepts script execution requests over an encrypted channel (HTTPS). For more information, see the [Security for DeployR](../deployr-admin-security.md#enable-server-ssl-https) guide.<br /><br />**Note:** If you enable HTTPS for one or more operations types, you must also provide a valid HTTPS URL in the Server web context property on this page.|
|`IP filter`|The name of the filter to be applied to all anonymous operations. If selected, then only anonymous users who connect from a qualified IP address (as defined by the filter) can execute R scripts on the API.<br /><br />The [IP filtering](deployr-admin-managing-access-with-ip-filters.md) restrictions specified on a server context determines the full publicly accessible exposure of that server context.|
|`API timeout`|The length of time in seconds an anonymous operation remains live on the grid before it is automatically terminated. The automatic termination will release all resources associated with that operation.|

<br/>
### Project Persistence Policies

These policies govern certain project behaviors and limits.

|Properties|Description|
|---|---|
|`History storage limit`|The maximum storage size in bytes for snapshots of PNG and SVG plots generated by the R graphics device associated with a project over its lifespan.<br /><br /> **Note:** This setting does not impact the size of a project’s working directory.|
|`History depth limit`|The maximum depth of the execution command history retained for each project. When the limit is reached, each new execution will flush the oldest one from the history. When an execution is flushed from the history, all associated plots and R console output are permanently deleted.|
|`Autosave temporary projects`|By default, this is unchecked, which sets the value to `False`, where temporary projects are not auto-saved when a project is closed or when a user logs out or times out on their connection. When `True`, temporary projects become candidates for auto-saving. Whether projects are auto-saved on close, logout, or timeout depends on the value of the `autosave` flag set when a user logs in on the API. For more information on autosaving, see the [API Reference Guide](../deployr-api-reference.md).

<br/>
### Concurrent Operation Policies

These policies govern the runtime grid usage limits on the number of concurrent operations that can be run simultaneously in the system by a given user.

Since these policies are disabled by default, no limits are enforced. This default behavior means that each user can take as many slots as desired on the grid framework until there are no more available slots. We typically recommend that these settings remain disabled if the DeployR server is being used:

-  By a single individual
-  To service a specific client application, which is common, for example, when an RBroker pool is in use

While leaving them disabled is typically recommended, enabling the policies can be quite useful in controlling slot usage on a per-user (operation type) basis.

|Properties|Description|
|---|---|
|`Enabled`|When enabled/checked, the policies are in effect and enforced. If disabled/unchecked, the limits are ignored.|
|`Per-user authenticated limit`|This value limits the number of live projects that a given authenticated user can run simultaneously on the grid. Whenever the number of live projects reaches this limit, all further requests to work on new projects are rejected on the API until one or more existing live projects are closed.|
|`Per-user asynchronous limit`|This value limits the number of scheduled jobs that a given authenticated user can run simultaneously on the grid. Whenever the number of scheduled jobs awaiting execution exceeds this limit for a given user, every pending job for that user remains queued until one or more of the currently running jobs complete.|
|`Per HTTP anonymous limit`|The value specified here limits the number of concurrent R scripts that an anonymous user at a given HTTP address can run simultaneously on the grid. Where the number of executing R scripts reaches this limit all further requests by the user to execute R scripts will be rejected on the API until one or more of the existing R script executions complete.|

<br/>
### Event Stream Access Policies

These settings limit access to the event streams. There are three distinct event streams pushed by DeployR.

-  The authenticated event stream allows authenticated users to see their own server-wide or HTTP-session specific execution and job lifecycle events. By default, this stream is accessible to all authenticated users. Access to this stream can be restricted using a role-based access control defined here.
-  The anonymous event stream allows anonymous users to see execution events within their current HTTP session. Access to this event stream can be disabled in the **Server Policies** tab.
-  The management event stream allows authenticated users to see server runtime events, including grid activity and security access events. By default this stream is accessible to `admin` only. Access to this stream can be restricted using a role-based access control specified here.

|Properties|Description|
|---|---|
|`Authenticated event stream restriction`|Only those authenticated users assigned to the role defined for this property have permission to view their event stream. By default, all authenticated users can access the authenticated event stream. For example, the administrator might restrict access only to those users assigned the `POWER_USER` role.|
|`Disable anonymous event stream`|When selected, the anonymous event stream is off.  When this checkbox is unselected, the anonymous stream pushes events to anonymous users.|
|`Management event stream restriction`|Only those authenticated users assigned to the role defined here can access the management event stream. By default, only `admin` can access this stream.|

<br/>
### Server Runtime Policies

|Properties|Description|
|---|---|
|`Max upload size`|This setting specifies the largest file size that can be uploaded to the server. This includes file uploads to both projects and the repository.<br /><br /> **Tip:** Use the DeployR external data directories to store larger files to avoid the limitations set here.|
|`Generate relative URLs`|When enabled/true, the URLs returned in the API response markup are relative URLs. By default or when disabled, the URLs returned in the response markup are absolute URLs.|
|`Log level`|Specifies the server logging level. Options are: **WARN**, **INFO**, and **DEBUG**. If you change this setting, the new level will take effect immediately. Any changes you make here persist until the server is restarted. After the server is restarted, the log level reverts to the default log level specified in the external configuration file, `deployr.groovy`.|

<br/>
## Viewing and Editing Server Policies

**To view and edit server-wide settings:**

1. From the main menu, click **Server Policies**.

1. From the **Server Policies** page, review the settings currently in place.

1. To edit one or more defaults:
	1. Click **Edit**. The **Edit Server Policies** page appears.

	1. Make any changes.

	1. Click **Update** to save the changes.
