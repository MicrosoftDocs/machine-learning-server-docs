--- 
 
# required metadata 
title: " Find File on a Given Data Path " 
description: " Obtain the normalized absolute path to the first occurrence of the specified input file  in the specified set of paths. " 
keywords: "RevoScaleR, rxLocateFile, IO" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
ms.topic: "reference" 
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
ms.technology: "r-server" 
ms.custom: "" 
 
--- 
 
 
 #`rxLocateFile`:  Find File on a Given Data Path 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Obtain the normalized absolute path to the first occurrence of the specified input file 
in the specified set of paths.
 
 
 
 ##Usage

```   
  rxLocateFile(file, pathsToSearch = NULL, fileSystem = NULL, 
     isOutFile = FALSE, defaultExt = ".xdf", verbose = 0)
 
```
 
 
 ##Arguments

   
  
 ### `file`
 Character string defining path to a file or a data source containing a file name. 
  
  
 ### `pathsToSearch`
 character vector of search paths. If `NULL`, the search path  determined by `isOutFile` is used. Only the paths listed are searched;  the list is not recursive. 
  
     
 ### `fileSystem`
 `NULL`, character string or [RxFileSystem](RxFileSystem.md) object indicating type of file system;  `"native"`or `RxNativeFileSystem` object can be used for the local operating system, or an `RxHdfsFileSystem` object for the Hadoop file system. If `NULL`, the active compute context will be checked for a valid file system.  If not available, `rxGetOption("fileSystem")` will be used to obtain the file system. If `file` is a data source containing a file system, that file system will take precedent.  
  
  
  
 ### `isOutFile`
 logical value. If `TRUE`, search using `outDataPath` instead of `dataPath` in [rxOptions](rxOptions.md) or in the local compute context (e.g.,[RxLocalSeq](RxLocalSeq.md)). 
  
  
  
 ### `defaultExt`
 character string containing default extension to use if extension is missing from `file`. 
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, information on the file to be located is printed. 
  
 
 
 
 ##Details
 
The `file` path may be relative, for example, `"one/two/myfile.xdf"` or 
absolute, for example, `"C:/one/two/myfile.xdf"`.
If `file` is an absolute path, the existence of the file is verified and, if found,
the absolute path is returned in normalized form. If `file` is a relative path, 
the current working directory and then any paths specified in `pathsToSearch` are 
prepended to `file` and a search for the file along the concatenated 
paths is performed. The first path where the file is found is returned in normalized
form. If the file does not exist an error is thrown.
 
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##Examples

 ```
   
  ## Not run:
 
rxLocateFile( "myFile.xdf" )
 ## End(Not run) 
  
 
```
 
 
