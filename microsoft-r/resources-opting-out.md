---

# required metadata
title: "Opting out of usage data collection (Microsoft R)"
description: "Learn how to turn off telemetry data collection on Microsoft R Server and R Client using the `rxPrivacyControl` function in RevoScaleR."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "03/24/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: "r-server"
#ms.custom: ""

---
# Opting out of usage data collection (Micorosft R)

**Applies to:** version 9.0.1 and later

By default, telemetry data is collected during your usage of Microsoft R Server and R Client for the purpose of improving products and services. Anonymous usage data includes device information, operating system version, regional and language settings, and errors reports. Please see the [Microsoft privacy statement](https://privacy.microsoft.com/privacystatement) for a detailed explanation.

To turn data collection off, use the RevoScaleR package function `rxPrivacyControl(FALSE)`. To turn it back on, change the setting to `TRUE`.

## Version Requirements

To verify server version is 9.0.1 or later (or 3.3.2 for R Client), open an R IDE, such the R Console (RGui.exe). The console app reports server version information for Microsoft R Open, R Client, and R Server. From the console, you can use the `print` command to return verbose version information:

    > print(Revo.version)

## Permission Requirements

Opting out of telemetry requires administrator rights. The instructions below explain how to run RGui.exe as an administrator.

As an administrator, running the command `rxPrivacyControl(TRUE)` will permanently change the setting to TRUE, and `rxPrivacyControl(FALSE)` will permanently change the setting to FALSE. There is no user-facing way to change the setting for a single session.

Without a parameter, running `rxPrivacyControl()` returns the current setting. Similarly, if you are not an administrator, `rxPrivacyControl()` returns just the current setting.

## How to opt out

The RevoScaleR package providing `rxPrivacyControl` is installed and loaded in both R Client and R Server. To turn off telemetry data collection, you can use any R console application. The following steps include Rgui.exe which is available in all installations of R Server and R Client.

1. Log in to the computer as an administrator.
2. Go to C:\Program Files\Microsoft\R Server\R_SERVER\bin\x64.
3. Right-click Rgui.exe and select **Run as administrator**.
4. At the command line, type search() to view a list of objects already loaded. You should see the RevoScaleR package in the list.
5. Type `rxPrivacyControl` to return the current value.
6. Type `rxPrivacyControl(FALSE)`to turn off telemetry data collection.

The  `rxPrivacyControl` command sets the state to be opted-in or out for anonymous usage collection.

**Command syntax**
~~~~
     rxPrivacyControl(optIn)
~~~~

This command takes one parameter, `optIn`, set to either ‘TRUE’ or ‘FALSE’ to opt out. Left unspecified, the current setting is returned.

## How to Contact Us

Please contact Microsoft if you have any questions or concerns.

+ For general privacy question or a question for the Chief Privacy Officer of Microsoft or want to request access to your personal information, please contact us by using our [Web form](http://go.microsoft.com/fwlink/?LinkId=321116).

+ For technical or general support question, please visit [http://support.microsoft.com/](http://support.microsoft.com/) to learn more about Microsoft Support offerings.

+ If you have a Microsoft account password question, please visit [Microsoft account support](http://go.microsoft.com/FWLink/p/?LinkID=320207).

+ By mail: Microsoft Privacy, Microsoft Corporation, One Microsoft Way, Redmond, Washington 98052 USA

+ By Phone: 425-882-8080
