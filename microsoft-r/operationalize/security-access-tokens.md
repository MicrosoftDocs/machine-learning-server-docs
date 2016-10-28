---

# required metadata
title: "Managing Access Tokens for API Requests"
description: "Operationalization of R Analytics with Microsoft R Server"
keywords: ""
author: "j-martens"
manager: "jhubbard"
ms.date: "05/06/2016"
ms.topic: "get-started-article"
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

# Managing Tokens for API Requests

AUDIENCE: APP DEVELOPER

You can use tokens to identify and authenticate the user sending the API call. Once all of the APIs have been authenticated, you will only need to log in once.  HOW DO ALL APIS GET AUTHENTICATED????

Then, add that token in the header of any subsequent API call. 

## Getting Tokens

You can get the API tokens in one of two ways:
+ If you have Active Directory LDAP (or LDAP-S) enabled, then send a `/login` API request to retrieve the **access** token and the **refresh** token
+ If you have Azure Active Directory enabled, then get token from there.

## Using Access Tokens

Place the token in the header of an API.

When the API call is sent with an access token, DeployR validates that the user is valid and that the token itself is not expired.


## Renewing Tokens

Access tokens expire. Use the refresh token to renew the token. Refresh tokens do expire as well.

There is an API CALL to do this



## Revoking Tokens

There is an API CALL to do this