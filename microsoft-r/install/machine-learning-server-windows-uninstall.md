---
# required metadata
title: "Uninstall Machine Learning Server for Windows"
description: "How to uninstall Machine Learning Server for Windows."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "02/16/2018"
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

# Uninstall Machine Learning Server for Windows

You can re-run the Windows installer to remove Machine Learning Server for Windows. Clearing the checkbox for an option in the Configuration page removes that component from your computer. Redistributable components, such as the Analysis Services OLE DB provider, are not removed because doing so might break other applications using those components.

If you have ServerSetup.exe in the Downloads folder, double-click the file to start it. Clear the options. Click **Install**.

You can also use **Add and Remove programs** in Windows to uninstall the software.

## Manual uninstall

If Setup fails to install all of the components, you can run a VB Script that forces uninstall and cleans up residual components. 

1. Run a script that produces a list of all installed products and saves it in a log.
2. Review log output for Microsoft Machine Learning Server, Microsoft R Server, or Microsoft R Client.
3. For either product, find the value for `LocalPackage`. For example: `C:\windows\Installer\222d19.msi`
4. Run the package at an elevated command prompt with this sytnax: `Msiexec /x {LocalPackage.msi} /L*v uninstall.log`, where local package is the value from the previous step.
5. Review the uninstall log to confirm the software was removed. You can repeat steps 1 and 2 for further verification.

If the script contains no evidence of the program, but the program folder still exists, you can safely delete it. Program files are located at `\Program Files\Microsoft`.

## VBScript listing installers for all installed products

Copy the following script and save it as **msi_lister.vbs**.

```
Dim objInstaller : Set objInstaller = Wscript.CreateObject("WindowsInstaller.Installer")
Set objProdSet = objInstaller.Products

On Error Resume Next

For Each objProd In objProdSet

  Dim s
  s = vbNewLine & vbNewLine & "===========================================" & vbNewLine
  s = s & "InstalledProductCode: " & vbTab & objInstaller.ProductInfo(objProd, "InstalledProductCode") & vbNewLine
  s = s & "InstalledProductName: " & vbTab & objInstaller.ProductInfo(objProd, "InstalledProductName") & vbNewLine
  s = s & "===========================================" & vbNewLine

  s = s & "ProductInfo for: "   & vbTab & objProd & vbNewLine
  s = s & "VersionString: "     & vbTab & objInstaller.ProductInfo(objProd, "VersionString") & vbNewLine
  s = s & "HelpLink:      "     & vbTab & objInstaller.ProductInfo(objProd, "HelpLink") & vbNewLine
  s = s & "HelpTelephone: "     & vbTab & objInstaller.ProductInfo(objProd, "HelpTelephone") & vbNewLine
  s = s & "InstallLocation: "   & vbTab & objInstaller.ProductInfo(objProd, "InstallLocation") & vbNewLine
  s = s & "InstallSource: "     & vbTab & objInstaller.ProductInfo(objProd, "InstallSource") & vbNewLine
  s = s & "InstallDate: "           & vbTab & objInstaller.ProductInfo(objProd, "InstallDate") & vbNewLine
  s = s & "Publisher: "             & vbTab & objInstaller.ProductInfo(objProd, "Publisher") & vbNewLine
  s = s & "LocalPackage: "          & vbTab & objInstaller.ProductInfo(objProd, "LocalPackage") & vbNewLine
  s = s & "URLInfoAbout: "          & vbTab & objInstaller.ProductInfo(objProd, "URLInfoAbout") & vbNewLine
  s = s & "URLUpdateInfo: "     & vbTab & objInstaller.ProductInfo(objProd, "URLUpdateInfo") & vbNewLine
  s = s & "VersionMinor: "      & vbTab & objInstaller.ProductInfo(objProd, "VersionMinor") & vbNewLine
  s = s & "VersionMajor: "      & vbTab & objInstaller.ProductInfo(objProd, "VersionMajor") & vbNewLine
  s = s & vbNewLine
  s = s & "Transforms: "        & vbTab & objInstaller.ProductInfo(objProd, "Transforms") & vbNewLine
  s = s & "Language: "          & vbTab & objInstaller.ProductInfo(objProd, "Language") & vbNewLine
  s = s & "ProductName: "       & vbTab & objInstaller.ProductInfo(objProd, "ProductName") & vbNewLine
  s = s & "AssignmentType: "    & vbTab & objInstaller.ProductInfo(objProd, "AssignmentType") & vbNewLine
  s = s & "PackageCode: "       & vbTab & objInstaller.ProductInfo(objProd, "PackageCode") & vbNewLine
  s = s & "Version:        "    & vbTab & objInstaller.ProductInfo(objProd, "Version") & vbNewLine
  s = s & "ProductIcon: "       & vbTab & objInstaller.ProductInfo(objProd, "ProductIcon") & vbNewLine
 
  Wscript.StdOut.WriteLine(s)

Next

Wscript.Quit 0 
```

## See also

+ [Install Machine Learning Server](r-server-install.md)
+ [What's new in Machine Learning Server](../whats-new-in-machine-learning-server.md)
+ [Supported platforms](r-server-install-supported-platforms.md)  
+ [Known Issues](../resources-known-issues.md)  
+ [Configure Machine Learning Server to operationalize your analytics](../what-is-operationalization.md)