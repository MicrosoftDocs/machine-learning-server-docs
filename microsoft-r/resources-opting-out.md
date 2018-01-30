---

# required metadata
title: "Opting out of usage data collection - Machine Learning Server "
description: "Learn how to turn off telemetry data collection on Machine Learning Server, Microsoft R Server and R Client using the rxPrivacyControl function in RevoScaleR or rx-privacy-control function in revoscalepy."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "01/30/2018"
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
# Opting out of usage data collection

**Applies to:** Machine Learning Server, Microsoft R Server 9.x

By default, telemetry data is collected during your usage of Machine Learning Server, Microsoft R Server, and R Client for the purpose of improving products and services. Anonymous usage data includes device information, operating system version, regional and language settings, and errors reports. Please see the [Microsoft privacy statement](https://privacy.microsoft.com/privacystatement) for a detailed explanation.

To turn data collection on or off, use [rxPrivacyControl](r-reference/revoscaler/rxprivacycontrol.md) from [RevoScaleR](r-reference/revoscaler/revoscaler.md) on any platform, or [rx_privacy_control](python-reference/revoscalepy/rx-privacy-control.md) from [revoscalepy](python-reference/revoscalepy/revoscalepy-package.md).

## Version Requirements

Privacy controls are in newer versions of the server. To verify server version is 9.x or later (or 3.4.x for R Client), open an R IDE, such the R Console (RGui.exe). The console app reports server version information for Microsoft R Open, R Client, and R Server. From the console, you can use the `print` command to return verbose version information:

    > print(Revo.version)

## Permission Requirements

Opting out of telemetry requires administrator rights. The instructions below explain how to run console applications as an administrator.

As an administrator, running the R command `rxPrivacyControl(TRUE)` will permanently change the setting to TRUE, and `rxPrivacyControl(FALSE)` will permanently change the setting to FALSE. There is no user-facing way to change the setting for a single session.

Without a parameter, running `rxPrivacyControl()` returns the current setting. Similarly, if you are not an administrator, `rxPrivacyControl()` returns just the current setting.

## How to opt out (Python)

The revoscalepy package providing `rx-privacy-control` is installed when you add Python support to Machine Learning Server:

1. Log on as root: `sudo su`
2. Start a Python session: `mlserver-python`
3. Load the library:`import revoscalepy`
4. Return the current value: `revoscalepy.rx_privacy_control()` 
5. Change the current value:`revoscalepy.rx_privacy_control(TRUE)` to turn on telemetry data collection. Otherwise, set it to FALSE.

## How to opt out (R)

The RevoScaleR package provides`rxPrivacyControl` is installed and loaded in both R Client and R Server. To turn off telemetry data collection, you can use the built-in R console application, which loads RevoScaleR automatically.

**On Linux**

1. Log on as root: `sudo su`
2. Start an R session: `Revo64`
3. Return the current value: `rxPrivacyControl` 
4. Change the current value:`rxPrivacyControl(TRUE)` to turn on telemetry data collection. Otherwise, set it to FALSE.


**On Windows**

1. Log in to the computer as an administrator.
2. Go to C:\Program Files\Microsoft\ML Server\R_SERVER\bin\x64.
3. Right-click Rgui.exe and select **Run as administrator**.
4. Type `rxPrivacyControl` to return the current value.
5. Type `rxPrivacyControl(FALSE)`to turn off telemetry data collection.

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
