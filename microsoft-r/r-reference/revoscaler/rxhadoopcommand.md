--- 
 
# required metadata 
title: " Execute Hadoop Commands " 
description: " Execute arbitrary Hadoop commands and perform standard file operations in Hadoop. " 
keywords: "RevoScaleR, rxHadoopCommand, rxHadoopCopy, rxHadoopCopyFromLocal, rxHadoopCopyFromClient, rxHadoopCopyToLocal, rxHadoopFileExists, rxHadoopListFiles, rxHadoopMakeDir, rxHadoopMove, rxHadoopRemove, rxHadoopRemoveDir, rxHadoopVersion, file" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 
 
 
 
 
 
 
 
 
 
 
 #`rxHadoopCommand`:  Execute Hadoop Commands 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Execute arbitrary Hadoop commands and perform standard file operations in Hadoop.
 
 
 ##Usage

```   
  rxHadoopCommand(cmd, computeContext, sshUsername=NULL, 
                  sshHostname=NULL, 
                  sshSwitches=NULL,
                  sshProfileScript=NULL, intern=FALSE)
  
  rxHadoopCopyFromLocal(source, dest, ...) 
  
  rxHadoopCopyFromClient(source, nativeTarget="/tmp", hdfsDest, 
                         computeContext, sshUsername=NULL, 
                         sshHostname=NULL, sshSwitches=NULL, sshProfileScript=NULL)
  
  rxHadoopCopyToLocal(source, dest, ...) 
  
  rxHadoopFileExists(path)
  
  rxHadoopListFiles(path="", recursive=FALSE, print, computeContext = rxGetComputeContext(), ...)
  
  rxHadoopMakeDir(path, ...)
  
  rxHadoopMove(source, dest,  ...)
  
  rxHadoopCopy(source, dest,  ...)
  
  rxHadoopRemove(path,  skipTrash=FALSE, ...)
  
  rxHadoopRemoveDir(path,  skipTrash=FALSE, ...)
  
  rxHadoopVersion()
  
 
```
 
 
 ##Arguments

   
    
 ### `cmd`
 A character string containing a valid Hadoop command, that is, the `cmd` portion of `hadoop cmd`. Embedded quotes are not permitted. 
  
  
    
 ### `computeContext`
 Run against this compute context. Default to the current compute context as returned by [rxGetComputeContext](rxsetcomputecontext.md) 
  .
  
    
 ### `sshUsername`
 character string specifying the username for making an ssh connection to the Hadoop cluster. 
  
  
    
 ### `sshHostname`
 character string specifying the hostname or IP address of the Hadoop cluster node or edge node that the client will log into for launching Hadoop commands. 
  
  
    
 ### `sshSwitches`
 character string specifying any switches needed for making an ssh connection to the Hadoop cluster. 
  
  
    
 ### `sshProfileScript`
 Optional character string specifying the absolute path to a profile script that will exist on the `sshHostname` host. This is used when the target ssh host does not automatically read in a `.bash_profile`, `.profile` or other shell environment configuration file for the definition of requisite variables. 
  
  
    
 ### `intern`
 logical (not `NA`) specifying whether to capture the output of a Hadoop command as an R character vector in a local compute context. (When using the `RxHadoopMR` compute context, any output is always returned as an R character vector.) 
  
  
    
 ### `source`
 character vector specifying file(s) to be copied or moved. 
  
  
    
 ### `dest`
 character string specifying the destination of a copy or move. If `source` includes more than one file, `dest` must be a directory. 
  
  
    
 ### `nativeTarget`
 character string specifying a directory in the Hadoop cluster's native file system, to be used as an intermediate location for file(s) copied from a client machine. 
  
  
    
 ### `hdfsDest`
 character string specifying a directory in the Hadoop Distributed File System. 
  
  
    
 ### `path`
 character vector specifying location of one or more files or directories. 
  
  
    
 ### `print`
 Deprecation Warning: the `print` argument in `rxHadoopListFiles` is now deprecated and is going to be removed in the next release. If `FALSE`, `rxHadoopListFiles` will return a `character` vector of paths; by default it prints paths to the console. 
  
  
    
 ### `recursive`
 logical flag. If `TRUE`, directory listings are recursive. 
  
  
    
 ### `skipTrash`
 logical flag. If `TRUE`, removal via `rxHadoopRemove` and `rxHadoopRemoveDir` bypasses the trash folder, if one has been set up. 
  
  
    
 ### ` ...`
 additional arguments to be passed directly to the `rxHadoopCommand` function. 
  
  
 
 
 ##Details
 
`rxHadoopCommand` allows you to run basic Hadoop commands. `rxCopyFromClient`
allows a file to be copied from a remote client to the Hadoop Distributed File System on the
Hadoop cluster. `rxHadoopVersion` calls the Hadoop `version` command and extracts
and returns the version number only. The remaining functions
are wrappers for various Hadoop file system commands:


* 
`rxHadoopCopyFromLocal` wraps the Hadoop `fs -copyFromLocal` command.

* 
`rxHadoopCopyToLocal` wraps the Hadoop `fs -copyToLocal` command.

* 
`rxHadoopListFiles` wraps the Hadoop `fs -ls` or `fs -lsr` command.

* 
`rxHadoopRemove` wraps the Hadoop `fs -rm` command.

* 
`rxHadoopCopy` wraps the Hadoop `fs -cp` command.

* 
`rxHadoopMove` wraps the Hadoop `fs -mv` command.

* 
`rxHadoopMakeDir` wraps the Hadoop `fs -mkdir` command.

* 
`rxHadoopRemoveDir` wraps the Hadoop `fs -rm -r` command.


 
 
 ##Value
 
These functions are executed for their side effects and typically return `NULL`
invisibly.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
 ##See Also
 
[RxHadoopMR](rxhadoopmr.md).
   
 ##Examples

 ```
   
  ## Not run:
 
rxHadoopCommand("version") # should return version information
rxHadoopMakeDir("/user/RevoShare/newUser")
rxHadoopCopyFromLocal("/tmp/foo.txt", "/user/RevoShare/newUser")
rxHadoopRemoveDir("/user/RevoShare/newUser")

 ## End(Not run) 
  
  
 
```
 
 
 
