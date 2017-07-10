--- 
 
# required metadata 
title: "Return the Microsoft R Home Directory" 
description: "Return the Microsoft R home directory. " 
keywords: ", Revo.home, utilities, documentation" 
author: "richcalaway"
ms.author: "richcala" 
manager: "jhubbard" 
ms.date: "03/27/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
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
 
 
 
 #Revo.home: Return the MicrosoftR Home Directory 
 ##Description
 Return the MicrosoftR installation directory.
 
 
 ##Usage

```   
  Revo.home(component = "home")
 
```
 
 
 ##Arguments

   
    
 ### component
 One of the strings `"home"` or `"licenses"`.  
  
 
 
 ##Details
 
`Revo.home` returns the path to the Microsoft R package installation directory, or its licenses subdirectory. 
 
 
 ##Value
 
a character string containing the path to the
MicrosoftR installation directory. 
 
 
 
 
 
