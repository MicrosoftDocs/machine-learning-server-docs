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

You can use tokens to identify and authenticate the user who is sending the API call within your application. When an un-authenticated user makes an API call, they will be required to provide their credentials. R Server will then issue a bearer token to your application for this user. 

This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's core operationalization APIs. Once a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties.

The bearer token's request output properties consist in an `access_token` / `refresh_token` pair and expiration dates. The `access_token` is active for 1 hour (3600 seconds). and expires after 90 days. The `refresh_token`, on the other hand, is active for 336 hours (14 days).  For example:

```
{
  "token_type":"Bearer",
  "access_token":"eyJhbGci....",
  "expires_in":3600,
  "expires_on":1479937454,
  "refresh_token":"0/LTo...."
}
```

<br>


## Token Lifecycle Details


||"access_token" Property|"refresh_token" Property|
|---|----|-----|
|**Creation**|When bearer token is created|When bearer token is created, or<br><br>When the `access_token` is used|
|**Expiration**|After 1 hour (3660 seconds) of inactivity|After 336 hours (14 days) of inactivity|
|**Invalidity**|If bearer token older than 90 days, or<br><br>If `refresh_token` is expired, or<br><br>If user's password changed, or<br><br>If bearer token was revoked|If not used for 336 hours (14 days), or<br><br>When `access_token` expires, or<br><br>After `access_token` is used (replaced by another)|

<br>

## Tokens Creation

API bearer tokens can be generated in one of two ways:
+ If you have Active Directory LDAP enabled, then send a `POST /login HTTP/1.1` API request to retrieve the bearer token.

+ If Azure Active Directory (AAD) is enabled, then the token will come from AAD. [Learn more](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios). 


This bearer token is valid for up to 90 days with an active `access_token` and `refresh_token`. After 90 days, the user will be forced to authenticate again and a new bearer token will be issued.  Note that the bearer token's `access_token` does expires after 1 hour of inactivity. It can easily be renewed using the `refresh_token`. This `refresh_token` also expires after inactivity (336 hrs/14 days). 


### Example: Token creation request 

Example Request|Example Response
---------------|--------------
``POST /login HTTP/1.1``<br>`{`<br>`"username": "my-user-name",`<br>`"password": "$ecRet1"`<br>`}`|`{"token_type":"Bearer",`<br>`"access_token":"eyJhbGci....",`<br>`"expires_in":3600,`<br>`"expires_on":1479937454,`<br>`"refresh_token":"0/LTo...."}`


<br>



## Tokens Usage

As defined by HTTP/1.1 [RFC2617], the application should send the `access_token` directly in the Authorization request header. Do so by including the bearer token's `access_token` value in the HTTP request body as `Authorization: Bearer {access_token_value}`. When the API call is sent with the token, R Server will validate that the user is successfully authenticated and that the token itself is not expired.

### Example: HTTP header for session creation
```
 POST /sessions HTTP/1.1
     Host: mrs.contoso.com
     Authorization: Bearer eyJhbGci....
     ...
```

### Example: HTTP header for publishing web service
```
 POST /api/{service}/{version} HTTP/1.1
     Host: mrs.contoso.com
     Authorization: Bearer eyJhbGci....
     ...
```

<br>



## Refresh Tokens

A valid bearer token keeps the user's authentication alive without requiring him or her to enter their credentials all the time.  Each time an active bearer token is used, the `refresh_token` is replaced and the new one gets another 336 hours (14 days) before it expires. 
 
However, after a one-hour period of inactivity, the `access_token` will automatically expire. Fortunately, since each `access_token` has a `refresh_token`, you can renew the `access_token` on behalf of the user **if** the `refresh_token` has not yet expired. 

### Example: Refresh access_token

Refresh Request |Example Response
---------------|--------------
`POST /login/refreshToken HTTP/1.1`<br>`Connection: Keep-Alive`<br>`Content-Type: application/json;`<br> &nbsp;&nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;`charset=utf-8`<br>`Accept-Encoding: gzip, deflate`<br>`Content-Length: 370`<br>`Host: mrs.contoso.com`<br>`{"refreshToken": "0/LTo...."}`|`{"token_type":"Bearer",`<br>`"access_token":"eyJhbGci....",`<br>`"expires_in":3600,`<br>`"expires_on":1479937454,`<br>`"refresh_token":"0/LTo...."}`<br> <br> <br> <br> 


If the `refresh_token` itself has also expired, then the user's bearer token becomes invalid and they will need to authenticate again during their next API call. 

<br>

## Security Concerns and the Revocation of Tokens

>![Important!]
>Though a party must first authenticate first to receive the bearer token, if the required steps are not taken to secure the token in transmission and storage, it can be intercepted and used by an unintended party. While some security tokens have a built-in mechanism for preventing unauthorized parties from using them, bearer tokens do not have this mechanism and must be [transported in a secure channel such as transport layer security (HTTPS)](security-https.md). 
>
>If a bearer token is transmitted in the clear, a man-in the middle attack can be used by a malicious party to acquire the token and use it for an unauthorized access to a protected resource. The same security principles apply when storing or caching bearer tokens for later use. Always ensure that your application transmits and stores bearer tokens in a secure manner. 

You can revoke a token if a user is no longer permitted to make requests on the API or if the token has been compromised.

Use the API call `DELETE /login/refreshToken/{refreshToken} HTTP/1.1` to revoke a token.

### Example: Revoke token

Example Request|Example Response
---------------|--------------
`DELETE https://mrs.contoso.com/login/refreshToken?refreshToken=OeAiJl HTTP/1.1`<br>`Connection: Keep-Alive`<br>`Accept-Encoding: gzip, deflate`<br>`Host: mrs.contoso.com`| Error code: `200`
