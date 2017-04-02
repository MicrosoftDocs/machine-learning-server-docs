--- 
 
# required metadata 
title: " Compress .xdf files " 
description: " Compress one or more .xdf files " 
keywords: "RevoScaleR, rxCompressXdf, manip, file" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 #`rxCompressXdf`:  Compress .xdf files 

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
Compress one or more .xdf files
 
 
 ##Usage

```   
  rxCompressXdf(inFile, outFile = NULL, xdfCompressionLevel = 1, overwrite = FALSE, reportProgress = rxGetOption("reportProgress"))
 
```
 
 ##Arguments

   
    
 ### `inFile`
  An .xdf file name, an RxXdfData data source, a directory containing .xdf files, or a vector of .xdf file names or RxXdfData data sources to compress  
  
    
 ### `outFile`
  An .xdf file name, an RxXdfData data source, a directory, or a vector of .xdf file names or RxXdfData data sources to contain the compressed files.  
  
    
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used.   
  
    
 ### `overwrite`
  If `outFile` is specified and is different from `inFile`, `overwrite` must be set to `TRUE` in order to have `outFile` overwritten.  
  
    
 ### `reportProgress`
  integer value with options:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
  
  
 
 
 ##Details
 
`rxCompressXdf` uses ZLIB to compress `.xdf` files in blocks.  The auto compression level
of -1 is equivalent to approximately 6.  Typically setting the `xdfCompressionLevel` to 1 
will provide an adequate amount of compression at the fastest speed.
 
 
 ##Value
 
A vector of [RxXdfData](RxXdfData.md) data sources
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxImport](rxImport.md),
[rxDataStep](rxDataStep.md),
[RxXdfData](RxXdfData.md),
   
 ##Examples

 ```
   
  
  # Get location of sample uncompressed .xdf file
  sampleXdf <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmallUC.xdf")
  	
  # Create name for a temporary file
  compressXdf <- tempfile(pattern = ".rxCompress", fileext = ".xdf")	
  
  # Create a new compressed .xdf file from the sample file
  newDS <- rxCompressXdf(inFile = sampleXdf, outFile = compressXdf, xdfCompressionLevel = 1)
  	
  # Get information about files and compare sizes
  sampleFileInfo <- file.info(sampleXdf)
  compressFileInfo <- file.info(compressXdf)
  sampleFileInfo$size
  compressFileInfo$size
  
  # Clean-up
  file.remove(compressXdf)
  
 
```
 
 
 
