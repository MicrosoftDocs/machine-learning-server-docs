---

# required metadata
title: " Security - DeployR 8.x "
description: "Security in DeployR: Authentication, HTTPS, SSL, and access controls for server, Project file and Repository File, and more."
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "cgronlun"
ms.date: "11/10/2017"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Account Locking Policies

To protect against brute force techniques that can be used to try and "guess" passwords on *basic authentication* and *PAM authenticated* user accounts, DeployR enforces automatic account locking when repeated authentication attempts fail within a given period of time for a given user account:

    /*
     * DeployR Authentication Failure Lockout Policy Configuration
     */
    deployr.security.tally.login.failures.enabled = true
    deployr.security.tally.login.failures.limit = 10
    deployr.security.tally.login.lock.timeout = 1800

By default, automatic account locking is enabled and activates following 10 failed attempts at authentication on a given user account. To disable this feature, set the following configuration property to `false`:

    deployr.security.tally.login.failures.enabled = false

When enabled, a count of failed authentication attempts is maintained by DeployR for each user account. The count is compared to the value specified on the following configuration property:

    deployr.security.tally.login.failures.limit = 10

If the count exceeds the `failures.limit` value, then the user account is locked and any further attempts to authentication on that account will be rejected with an error indicating that the account has been temporarily locked.

To manage locked user accounts, the administrator has two choices. The chosen behavior is determined by the value assigned to the following configuration property:

    deployr.security.tally.login.lock.timeout = 1800

If the `lock.timeout` value is set to 0, then locked user accounts must be manually unlocked by the administrator in the [Users tab](deployr-admin-console-user-accounts.md#viewing-and-editing-user-accounts) of the Administration Console.

If the `lock.timeout` value (measured in seconds), is set to any non-zero, positive value then a locked user account will be automatically *unlocked* by DeployR once the `lock.timeout` period of time has elapsed without further activity on the account.

By default, automatic account *unlocking* is enabled and occurs 30 minutes after the last failed attempt at authentication on an account.
