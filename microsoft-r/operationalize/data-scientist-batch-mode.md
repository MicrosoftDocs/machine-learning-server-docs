---

# required metadata
title: "Batch Scoring & Deployment"
description: "mrsdeploy Functions"
keywords: "mrsdeploy package reference"
author: "j-martens"
manager: "jhubbard"
ms.date: "2/14/2017"
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

# Batch consumption of web services

**Applies to:  Microsoft R Server 9.1**

This article described how to use batch processing with analytic web services.  

Batch processing refers to the execution and consumption of code in the form of a web service without manual intervention, whereas interactive "Request Response" processing for consumption requires human intervention on the command line. A batch is a group of one or more  asynchronous API calls on a specific web service sent as a single request to R Server. Then, R Server will immediately execute those operations for you N times. 

While web services can still be consumed using the "Request Response" mode [described here](data-scientist-manage-services.md#consume-service), there are scenarios and circumstances that make batch processing a more interesting approach. 

An asynchronous batch call to a web service involves the following:
+ Call the service by name/version
+ Define a batch operation by name 
+ Start the batch operation
+ Get the results of the batch processing and any associated files
+ When necessary, cancel the operation

A [series of public API functions](data-scientist-manage-services.md#api-client) can be used to programmatically accomplish the task. 





Other assumptions:

O16N now supports two new inputs: rdata.frame and csv.  Not batch specific. Accepted input to any web service produced by O16N via mrsdeploy or API.  Is that true? What about for realtime scoring?






Asynchronous consume via batch processing
Request response consume via direct interaction

	• Batch scoring/deployment (Sean)  - 2 new inputs  rdata.frame and csv.
		○ 5 new functions() in mrsdeploy (register, start, cancel job, get results, get associated files)
		○ Consume via Request-response interaction or batch interaction (like Azure)


What is “Request-response interactions” and “batch interactions”. It will describe the 5 new public API functions in mrsdeploy package.  

We’ll need an example users can actually try out of the box on their own.

Existing mrsdeploy docs and this new article will reference support of new inputs (rdata.frame and csv).






## Workflow overview

```R

# Produce a prediction function
echo <- function(mpg, cyl, disp, hp, drat, wt, qsec, vs, am, gear, carb) {
   hist(mpg)
   hist(cyl)
   data.frame(mpg, cyl, disp, hp, drat, wt, qsec, vs, am, gear, carb)
}


# Publish as service using `publishService()` function from 
# `mrsdeploy` package. Name service "mtService" and provide
# unique version number. Assign service to the variable `api`

###WHAT ABOUT THIS SERVICE NAME CHANGE
api <- publishService(
  "mtService", 
  code = echo,
  inputs = list(
             mpg  = "numeric",
             cyl  = "numeric",
             disp = "numeric",
             hp   = "numeric",
             drat = "numeric",
             wt   = "numeric",
             qsec = "numeric",
             vs   = "numeric",
             am   = "numeric",
             gear = "numeric",
             carb = "numeric"
           ),
  outputs = list(output = "data.frame")
  v = 1.0
)

#
# Register batch by `data.frame` 
#
# mtcars is a data.frame of 11 cols with names (mpg, cyl, ..., carb)
# and 32 rows of numerics 
#
record <- mtcars
batch <- api$batch(records, parallelCount = 5) 

# Kick the batch processing off
batch$start()
batch$id()

#
# Poll for results every 3 second
#
res <- NULL
while(TRUE) {
   res <- batch$results()

   if (res$status == batch$STATUS$failed) { stop("Batch execution failed") } 
   if (res$status == batch$STATUS$complete) { break }
      
   Sys.sleep(3)
}
      
#
# From the completed Batch results `res`, get execution records by index. For
# every record:
#   - List all generated files
#   - Get file content by filename
#   - Download file by filename
#   - Donwload all files
#   - Get execution results (e.g. service output)
#
for(i in seq(res$totalItemCount)) {
   # Demonstrate: List all generated files found in this execution index
   files <- batch$listFiles(i)
    
   # Demonstrate: Get file content by filename
   for (fileName in files) {
     content <- batch$file(i, fileName)
     if (is.null(content)) { stop("Unable to get file") }
   }
   
   # Demonstrate: Download helper function
   for (fileName in files) {
     batch$download(i, fileName, dest = "~swells/dev/null") # default dest = getwd()
   }
   
   #  Demonstrate: Download all files
   batch$download(i)
   
   # Demonstrate: Get execution results (e.g. service output)
   exe <- res$execution(i)
   singleRowDataFrame <- exc$outputParameters$output
}
``` 

## Batch by `data.frame`

```R
# Get service to batch
service <- getService("mtcars")

#
# Register batch by `data.frame` 
#
# mtcars is a data.frame of 11 cols with names (mpg, cyl, ..., carb)
# and 32 rows of numerics 
#
records <- mtcars

batch <- service$batch(records, parallelCount = 5)$start()
```

## Batch by CSV, TSV, using `base::read.csv()` ect... 
```R
# Get service to batch
service <- getService("mtcars")

#
# Register batch by Comma-separated data (.csv)
#
# mtcars is a data.frame of 11 cols with names (mpg, cyl, ..., carb)
# and 32 rows of numerics 
#

records <- read.csv("mtcars.csv")
batch <- service$batch(records)$start()

#
# Register batch by Tab-separated data (.tsv) 
#
# mtcars is a data.frame of 11 cols with names (mpg, cyl, ..., carb)
# and 32 rows of numerics 
#

records <- read.csv("mtcars.tsv", sep = "\t")
batch <- service$batch(records)$start()

```

# New Batch Functions

Batching is accessible from the service object:

```R
# Get service to batch
service <- getService("mtcars")

# Register service to be batched
batch <- service$batch(inputs, parallelCount = 10)

# Get batch by execution identifier
batch <- service$getBatch(id)

# List the Batch execution identifiers
ids <- service$getBatchExecutions()
```

### Get service to batch
```R
batch(inputs, parallelCount = 10)
```

**Arguments**

- inputs: `data.frame`, or a `list`
- parallelCount: default is `10`

**Return**

The `Batch` object

### Get batch by execution identifier

```R
getBatch(id)
```

**Arguments**

- id: The batch execution identifiers

**Return**

The `Batch` object


### List the Batch execution identifiers

```R
 getBatchExecutions()
```

**Return**

List of batch execution identifiers

## `Batch` object

Batching is accessible from the service object. 

```R
# Get service to batch
service <- getService("mtcars")

# Register service to be batched
batch <- service$batch(inputs, parallelCount = 10)

# Start batch execution
batch$start()

# Cancel batch execution
batch$cancel()

# Get execution identifier for the started batch process
id <- batch$id()

# Poll for batch execution results
batchResult <- batch$results(partialResults = TRUE)

# Get file content by filename
content <- batch$file(index, fileName)
   
# Download helper function (default dest = getwd())
batch$download(index, fileName, dest = "~admin/dev/null") 

# Download all files
batch$download(index)
```

### Start batch execution

```R
start()
```

**Return**

The `Batch` object

### Cancel batch execution

```R
cancel()
```
**Return**

The `Batch` object

### Get execution identifier

```R
id()
```

**Return**

The execution identifier

### Get batch execution results

```R
batch$results(partialResult = TRUE)
```

**Arguments**

- partialResults: Returns the already processed results of the batch execution even if it has not been fully completed.

**Return**

The `BatchResult` object

### Get file content by filename

```R
file(index, fileName)
```

**Arguments**

- index: Batch execution record index
- filename: Name of file artifact created during batch execution

**Return**

### Download helper function (default dest = getwd())

```R
download(index, fileName, dest = getwd())
```

**Arguments**

- index: Batch execution record index
- filename: Name of file artifact created during batch execution
- dest: The download desition. The default is working directory.

**Return**

Filepath to download location

### Download all files

```R
download(index)
```
**Arguments**

- index: Batch execution record index

**Return**

List of filepaths to download location

## `BatchResult` object

### Get batch execution results

```R
execution(index)
```

**Arguments**

- index: Batch execution record index

**Return**

List of lists of batch execution results



## See also

+ [mrsdeploy function overview](../mrsdeploy/mrsdeploy.md)
+ [Data scientist get started guide](data-scientist-get-started.md)
+ [Working with web services in R](../operationalize/data-scientist-manage-services.md)
+ [Execute on a remote Microsoft R Server](../operationalize/remote-execution.md)
+ [Application developer get started guide](../operationalize/app-developer-get-started.md)
