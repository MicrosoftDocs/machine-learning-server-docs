---

# required metadata
title: "DeployR FAQs"
description: "Frequently asked questions about DeployR"
keywords: "FAQs, frequently asked questions, DeployR"
author: "j-martens"
manager: "jhubbard"
ms.date: "03/17/2016"
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
ms.technology: "deployr"
ms.custom: ""

---


# Frequently Asked Questions

## If I want to use the DeployR Web server for my Web application, where should I put my application files?

If I want to use the DeployR Web server for my Web application, where should I put my application files?
 + **Linux / OS X:** For product evaluation purposes, you can put your Web application files under the directory &lt;DEPLOYR-INSTALL-DIR&gt;/www/apps/&lt;YOUR-EVALUATION-APP&gt;, such as: `     /home/deployr-user/deployr/8.0.5/www/apps/my-demo-application/     `

 + **For Windows:** For product evaluation purposes, you can put your Web application files under the directory &lt;DEPLOYR-INSTALL-DIR&gt;\\www\\apps\\&lt;YOUR-EVALUATION-APP&gt;, such as: `     C:\Program Files\Microsoft\DeployR-8.0\www\apps\my-demo-application\     `

No other configuration changes are needed. Once you put your files there, your evaluation application becomes live. Using the previous example, you could see your application files at this Web address: `http://<DEPLOYR-SERVER-IP:8050>/apps/my-demo-application/one-of-my-files.html`  
 
>[!WARNING]
>**These instructions for handling Web application files under the DeployR Web server are for demonstration or evaluation purposes only!** 
>In practice, we strongly recommend that you place your Web application files and any other assets under a different Web server.

## I'm having issues after installing DeployR. Where do I go for answers?

1.  Check out our [troubleshooting topics](deployr-admin-diagnostics-troubleshooting.md#troubleshooting).
2.  Run the [diagnostics tests](deployr-admin-diagnostics-troubleshooting.md#running-the-diagnostic-check).
3.  Join the [DeployR Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=microsoftr) where you can post questions and find answers.

## Why can't I reach DeployR landing page anymore?

If you were able to reach the landing page after install, but cannot reach that page anymore, or if your login attempt stalls on a blank page in your browser, then the Server Web Context might not be set to the right IP address. [Find out how to correct this issue here](deployr-installing-configuring.md).

## How does DeployR compare to Shiny?

DeployR is an integration technology for application developers to deploy R analytics inside any Web, desktop, mobile, dashboard application or backend system. Shiny is a package from [RStudio](http://www.rstudio.com/) designed to help R users take their analysis and build them into interactive Web applications. However, Shiny is not designed to build applications other than interactive Web applications.

The following table contrasts the two solutions:

| Comparison          | DeployR                                                                                                                                                                                                                                    | Shiny                                        |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------|
| Supports            | R analysis integration inside any application                                                                                                                                                                                              | The building of interactive Web applications |
| Primary Audience    | Application developers                                                                                                                                                                                                                     | Data scientists                              |
| Required Skills     | Application programming skills                                                                                                                                                                                                             | R programming skills                         |
| Developer Interface | [API](deployr-api-reference.md), [Client Libraries](deployr-tools-and-samples.md), [RBroker Framework](deployr-tools-and-samples.md) | R package                                    |
| Optional Skills     | R programming skills                                                                                                                                                                                                                       | Web programming skills                       |
