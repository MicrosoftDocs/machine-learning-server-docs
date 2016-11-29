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

# Security: Token Management for API Requests

Microsoft R Server uses tokens to identify and authenticate the user who is sending the API call within your application. Users must authenticate when making an API call. They can do so with the `POST /login HTTP/1.1` API call, after which R Server will then issue a bearer token to your application for this user. Alternately, if the organization is using Azure Active Directory (AAD), users will receive a bearer token from AAD when they authenticate.

This bearer token is a lightweight security token that grants the “bearer” access to a protected resource, in this case, R Server's core operationalization APIs. Once a user has been authenticated, the application must validate the user’s bearer token to ensure that authentication was successful for the intended parties.

<br>

## Security Concerns 

Though a party must authenticate first to receive the bearer token, if the required steps are not taken to secure the token in transmission and storage, it can be intercepted and used by an unintended party. While some security tokens have a built-in mechanism for preventing unauthorized parties from using them, bearer tokens do not have this mechanism and must be [transported in a secure channel such as transport layer security (HTTPS)](security-https.md). 

If a bearer token is transmitted in the clear, a man-in the middle attack can be used by a malicious party to acquire the token and use it for an unauthorized access to a protected resource. The same security principles apply when storing or caching bearer tokens for later use. Always ensure that your application transmits and stores bearer tokens in a secure manner. 

> You can [revoke a token](#revoke) if a user is no longer permitted to make requests on the API or if the token has been compromised.

<br>

## Token Creation


API bearer tokens can be generated in one of two ways:
+ If Active Directory LDAP or a local administrator account is enabled, then send a `POST /login HTTP/1.1` API request to retrieve the bearer token.

+ If Azure Active Directory (AAD) is enabled, then [the token will come from AAD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios). 

[Learn more about authentication methods for operationalization...](security-authentication.md)

The bearer token's properties consist in an `access_token` / `refresh_token` pair and expiration dates. Learn more about its lifecycle and expiration dates below.

#### Example: Token creation request 

+ **Request**
  ```
  POST /login HTTP/1.1
  {
    "username": "my-user-name",
    "password": "$ecRet1"
  }
  ```

+ **Response**
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

## Bearer Token Lifecycle

The bearer token is made of an `access_token` property and  a `refresh_token` property.

|Properties|The "access_token" Lifecycle|The "refresh_token" Lifecycle|
|---|----|-----|
|**Gets<br>Created**|Whenever the bearer token is created|Whenever the bearer token is created, or<br><br>After an `access_token` is used|
|**Expires**|After 1 hour (3660 seconds) of inactivity|After 336 hours (14 days) of inactivity|
|**Becomes<br>Invalid**|If the bearer token is older than 90 days, or<br><br>If `refresh_token` expired, or<br><br>If user's password changed, or<br><br>If the bearer token was revoked|If not used for 336 hours (14 days), or<br><br>When it expires, or<br><br>After `access_token` is used (replaced by another)<br> <br> <br>|

<br>

## Token Usage

As defined by HTTP/1.1 [RFC2617], the application should send the `access_token` directly in the Authorization request header. 

You can do so by including the bearer token's `access_token` value in the HTTP request body as `Authorization: Bearer {access_token_value}`. 

When the API call is sent with the token, R Server will attempt to validate that the user is successfully authenticated and that the token itself is not expired.
+  If the user is successfully authenticated but the bearer token's `access_token` or `refresh_token` is expired, a `401 - Unauthorized (invalid or expired refresh token)` error is returned.

+ If the user is not successfully authenticated, a `401 - Unauthorized (invalid credentials)` error is returned.



#### Examples

**HTTP header for session creation:**
```
 POST /sessions HTTP/1.1
     Host: mrs.contoso.com
     Authorization: Bearer eyJhbGci....
     ...
```

**HTTP header for publishing web service:**
```
 POST /api/{service}/{version} HTTP/1.1
     Host: mrs.contoso.com
     Authorization: Bearer eyJhbGci....
     ...
```

<br>

## Token Renewal

A valid bearer token (with valid `access_token` or `refresh_token` properties) keeps the user's authentication alive without requiring him or her to re-enter their credentials frequently.  Each time an active bearer token is used, the `refresh_token` is replaced and the new one gets another 336 hours (14 days) before it expires. 
 
However, after a one-hour period of inactivity, the `access_token` will automatically expire. Fortunately, since each `access_token` has a `refresh_token`, you can renew the `access_token` on behalf of the user **if** the `refresh_token` has not yet expired. 

#### Example: Refresh access_token

+ **Request**
  ```
  POST /login/refreshToken HTTP/1.1
    Connection: Keep-Alive
    Content-Type: application/json; charset=utf-8
    Accept-Encoding: gzip, deflate
    Content-Length: 370
    Host: mrs.contoso.com
 
    {
      "refreshToken": "0/LTo...."
    }
  ```

+ **Response**
  ```
  {
    "token_type":"Bearer",
    "access_token":"eyJhbGci....",
    "expires_in":3600,
    "expires_on":1479937523,
    "refresh_token":"ScW2t...."
  }
  ```

If the `refresh_token` itself has also expired, then the user's bearer token becomes invalid and they will need to authenticate again before their next API call. 

<br>

<a name="revoke"></a>

## Token Revocation

You can revoke a token if a user is no longer permitted to make requests on the API or if the token has been compromised.

Use the API call `DELETE /login/refreshToken/{refreshToken} HTTP/1.1` to revoke a token.

#### Example: Revoke token

+ **Request**
  ```
  DELETE https://mrs.contoso.com/login/refreshToken?refreshToken=ScW2t HTTP/1.1
    Connection: Keep-Alive
    Accept-Encoding: gzip, deflate
    Host: mrs.contoso.com
  ```

+ **Response**

  HTTP Error: `200`