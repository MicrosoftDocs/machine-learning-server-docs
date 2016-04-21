---

# required metadata
title: "Upgrading or Reinstalling Microsoft R Server, Revolution R Open, or R"
description: "Reinstalling R on a machine with DeployR"
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

# Upgrading or Reinstalling Microsoft R Server, Revolution R Open, or R

## DeployR Enterprise

Carefully follow the order presented whenever you reinstall Microsoft R Server on a machine hosting DeployR Enterprise 8.0.0:

### On Linux:

On the main DeployR server machine:

1.  [Stop DeployR](deployr-common-administration-tasks.md#starting-and-stopping-deployr).
2.  Uninstall Microsoft R Server using the instructions provided with your existing version of Microsoft R Server.
3.  Install Microsoft R Server and all its prerequisites as described in the instructions provided with that version of Microsoft R Server.
4.  [Start DeployR](deployr-common-administration-tasks.md#starting-and-stopping-deployr).

On each DeployR grid node machine:

1.  [Uninstall the DeployR grid node](deployr-installing-configuring.md#uninstalling-deployr).
2.  Uninstall Microsoft R Server using the instructions provided with your existing version of Microsoft R Server.
3.  Install Microsoft R Server and all its prerequisites as described in the instructions provided with that version of Microsoft R Server.
4.  [Install the DeployR grid node](deployr-installing-configuring.md#grid-node-install).

### For Windows:

On the main DeployR server machine:

1.  [Backup the DeployR database](deployr-common-administration-tasks.md#backing-up-database-data).
2.  [Uninstall the DeployR main server](deployr-installing-configuring.md#uninstalling-deployr).
3.  Uninstall Microsoft R Server using the instructions provided with your existing version of Microsoft R Server.
4.  Install Microsoft R Server and all its prerequisites as described in the instructions provided with that version of Microsoft R Server.
5.  [Reinstall the DeployR main server](deployr-installing-configuring.md#basic-deployr-install).
6.  [Restore the DeployR database](deployr-common-administration-tasks.md#backing-up-and-restoring-data).

On each DeployR grid node machine:

1.  [Uninstall the DeployR grid node](deployr-installing-configuring.md#uninstalling-deployr).
2.  Uninstall Microsoft R Server using the instructions provided with your existing version of Microsoft R Server.
3.  Install Microsoft R Server and all its prerequisites as described in the instructions provided with that version of Microsoft R Server.
4.  [Install the DeployR grid node](deployr-installing-configuring.md#grid-node-install).

---------

>[!IMPORTANT]
>For each DeployR Enterprise instance, the server and grid nodes should all run the same version of Microsoft R Server, Revolution R Open, or R.

## DeployR Open

Carefully follow the order presented whenever you reinstall Revolution R Open or CRAN R on a machine hosting DeployR Open 8.0.0:

### For Linux / Mac OS X:

On the main DeployR server machine:

1.  [Stop DeployR](deployr-common-administration-tasks.md#starting-and-stopping-deployr).
2.  Uninstall Revolution R Open or CRAN R.
3.  Install Revolution R Open or CRAN R along with all of its prerequisites as described in the Revolution R Open or CRAN R installation instructions.
4.  [Start DeployR](deployr-common-administration-tasks.md#starting-and-stopping-deployr).

### For Windows:

On the main DeployR server machine:

1.  [Backup the DeployR database](deployr-common-administration-tasks.md#backing-up-database-data).
2.  [Uninstall the DeployR main server](deployr-installing-configuring.md#uninstalling-deployr).
3.  Uninstall Revolution R Open or CRAN R.
4.  Install Revolution R Open or CRAN R along with all of its prerequisites as described in the Revolution R Open or CRAN R installation instructions.
5.  [Reinstall the DeployR main server](deployr-installing-configuring.md#basic-deployr-install).
6.  [Restore the DeployR database](deployr-common-administration-tasks.md#restoring-database-data).

## Troubleshooting

Carefully follow these steps to troubleshoot a problem after Microsoft R Server, Revolution R Open, or CRAN R was reinstalled or upgraded.

### For DeployR Enterprise

#### For Linux:

If the issue is that Rserve is not running on a machine, then you'll need to restart it.

Try the following:

1.  [Run the diagnostics](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing) to determine whether the issue is on the main server or a grid node.
2.  Make sure there is [no activity on the DeployR grid.](deployr-admin-console/deployr-admin-managing-the-grid.md#viewing-or-stopping-slot-activity)
3.  [Stop DeployR on the machine in question.](deployr-common-administration-tasks.md#starting-and-stopping-deployr)
4.  Attempt to correct all issues reported in the [diagnostics log.](deployr-admin-diagnostics-troubleshooting.md#inspecting-diagnostic-log-files)
5.  [Start DeployR on the machine in question.](deployr-common-administration-tasks.md#starting-and-stopping-deployr)
6.  Run the diagnostics again.

In the event that you need additional support, send the diagnostics tar/zip file to the Microsoft Corporation technical support team.

#### For Windows:

If Microsoft R Server, Revolution R Open, or R is not reinstalled as described at the beginning of this document, then Rserve may be missing.

Try the following:

1.  [Run the diagnostics](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing) to determine whether the issue is on the main server or a grid node.
2.  If RServe is not running and you cannot find it in the `\bin\x64` subdirectory of the Microsoft R Server install directory, then you must reinstall as [described here](deployr-admin-configure-reinstall-r.md#deployr-enterprise).
3.  If there are other issues reported in the [diagnostics log](deployr-admin-diagnostics-troubleshooting.md#inspecting-diagnostic-log-files), attempt to fix them now.
4.  Run the diagnostics again.

-----

In the event that you need additional support, send the diagnostics tar/zip file to the Microsoft Corporation technical support team.

>[!IMPORTANT]
>For each DeployR Enterprise instance, the server and grid nodes should all run the same version of Microsoft R Server, Revolution R Open, or R.

### For DeployR Open

#### Linux / Mac OS X:

If the issue is that Rserve is not running on a machine, then you'll need to restart it.

Try the following:

1.  [Run the diagnostics](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing) to confirm Rserve is not running.
2.  Make sure there is [no activity on the DeployR grid.](deployr-admin-console/deployr-admin-managing-the-grid.md#viewing-or-stopping-slot-activity)
3.  [Stop DeployR.](deployr-common-administration-tasks.md#starting-and-stopping-deployr)
4.  Attempt to correct all issues reported in the [diagnostics log](deployr-admin-diagnostics-troubleshooting.md#inspecting-diagnostic-log-files).
5.  [Start DeployR.](deployr-common-administration-tasks.md#starting-and-stopping-deployr)
6.  Run the diagnostics again.

In the event that you need additional support and have purchased a support agreement, send the diagnostics tar/zip file to the Microsoft Corporation technical support team.

#### For Windows:

If Revolution R Open or CRAN R is not reinstalled as described at the beginning of this document, then Rserve may be missing.

Try the following:

1.  [Run the diagnostics](deployr-admin-diagnostics-troubleshooting.md#diagnostic-testing) to confirm RServe is missing.
2.  If RServe is not running and you cannot find it in the `\bin\x64` subdirectory of the Revolution R Open or R install directory, then you must reinstall as [described here](deployr-admin-configure-reinstall-r.md#deployr-open).
3.  If RServe is present and running, and there are other issues reported in the [diagnostics log](deployr-admin-diagnostics-troubleshooting.md#inspecting-diagnostic-log-files), attempt to fix them now.
4.  Run the diagnostics again.

---------

In the event that you need additional support and have purchased a support agreement, send the diagnostics tar/zip file to the Microsoft Corporation technical support team.


