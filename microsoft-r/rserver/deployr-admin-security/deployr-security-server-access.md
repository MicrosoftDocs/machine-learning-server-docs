---

# required metadata
title: " Security in DeployR"
description: "Security in DeployR: Authentication, HTTPS, SSL, and access controls for server, Project file and Repository File, and more."
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "05/06/2016"
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


#Server Access Controls

## Working with IP Address Filters

While access to DeployR is typically controlled by the authentication mechanisms discussed in this document, DeployR also supports access controls based on IP address filters.

Under the [**Server Policies**](./deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties) tab in the DeployR Administration Console, you have a mechanism to configure your IP address filter policy. The `Operation Policies` for authenticated, asynchronous, and anonymous operations each support an [IP filter](./deployr-admin-console/deployr-admin-managing-server-policies.md#server-policy-properties) property. If you assign an IP filter to this property, then any attempt by a client application to connect from outside of the IP address range on that filter will be automatically rejected.

For example, you can make your DeployR server instance accessible only from IP addresses on the local LAN or VPN, such as `192.168.1.xxx` or `10.xxx.xxx.xxx`. Note that it is possible to achieve these same kinds of access controls with an appropriate configuration on your firewall and/or routers.

Refer to the [Administration Console Help](./deployr-admin-console/deployr-admin-console-about.md) for further details on IP filters and server policies.

## Cross-Origin Resource Sharing

Cross-Origin Resource Sharing (CORS) enables your client application to freely communicate and make cross-site HTTP requests for resources from a domain other than where the DeployR is hosted.

CORS can be enabled or disabled in the DeployR external configuration file, `deployr.groovy`. In DeployR Enterprise, support for CORS is disabled by default.

    /*
     * DeployR CORS Policy Configuration
     *
     * cors.headers = [ 'Access-Control-Allow-Origin': 'http://app.example.com']
     */
    cors.enabled = false

**To enable CORS support:**

1.  Update the relevant properties in `deployr.groovy` by setting `cors.enabled = true`.
2.  Stop and restart the DeployR server using [these instructions](deployr-common-administration-tasks.md#starting-and-stopping-deployr).

Optionally, to restrict cross-site HTTP requests to only those requests coming from a specific domain, specify a value for `Access-Control-Allow-Origin` on the `cors.headers` property.
