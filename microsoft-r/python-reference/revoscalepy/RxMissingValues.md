--- 
 
# required metadata 
title: "RxMissingValues: Provides different missing (NA) value constants for various scalar types" 
description: "revoscalepy package uses Pandas dataframe as the abstraction to hold the data and manipulate it. Pandas dataframe in turn uses NumPy ndarray to hold homogeneous data efficiently and provides fast access and manipulation functions on the ndarray. Python and NumPy ndarray in particular only has numpy.NaN value for float types to indicate missing values. For other primitive datatypes, like int, there isn’t a missing value notation. If one uses Python’s None object to represent missing value in a series of data, say int, the whole ndarray’s dtype changes to object. This makes dealing with data with missing values quite inefficient during processing.This class provides missing values for various NumPy data types which one can use to mark missing values in a sequence of data in ndarray.It provides missing value for following types:int8() for numpy.int8 with value of numpy.iinfo(np.int8).min  uint8() for numpy.uint8 with value of numpy.iinfo(np.uint8).max  int16() for numpy.int16 with value of numpy.iinfo(np.int16).min  uint16() for numpy.uint16 with value of numpy.iinfo(np.uint16).max  int32() for numpy.int32 with value of numpy.iinfo(np.int32).min  uint32() for numpy.uint32 with value of numpy.iinfo(np.uint32).max  int64() for numpy.int64 with value of numpy.iinfo(np.int64).min  uint64() for numpy.uint64 with value of numpy.iinfo(np.uint64).max  float16() for numpy.float16 with value of numpy.NaN  float32() for numpy.float32 with value of numpy.NaN  float64() for numpy.float64 with value of numpy.NaN" 
keywords: "missing values, NA, NaN, nan, none, None" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "09/06/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# RxMissingValues


**Applies to: SQL Server 2017**


## Usage



```
revoscalepy.RxMissingValues
```





## Description

revoscalepy package uses Pandas dataframe as the abstraction to hold the
data and manipulate it. Pandas dataframe in turn uses NumPy ndarray to
hold homogeneous data efficiently and provides fast access and manipulation
functions on the ndarray. Python and NumPy ndarray in particular only has
numpy.NaN value for float types to indicate missing values. For other primitive
datatypes, like int, there isn’t a missing value notation. If one uses Python’s
None object to represent missing value in a series of data, say int, the whole
ndarray’s dtype changes to object. This makes dealing with data with
missing values quite inefficient during processing.

This class provides missing values for various NumPy data types which one
can use to mark missing values in a sequence of data in ndarray.

It provides missing value for following types:

    * int8() for numpy.int8 with value of numpy.iinfo(np.int8).min 

    * uint8() for numpy.uint8 with value of numpy.iinfo(np.uint8).max 

    * int16() for numpy.int16 with value of numpy.iinfo(np.int16).min 

    * uint16() for numpy.uint16 with value of numpy.iinfo(np.uint16).max 

    * int32() for numpy.int32 with value of numpy.iinfo(np.int32).min 

    * uint32() for numpy.uint32 with value of numpy.iinfo(np.uint32).max 

    * int64() for numpy.int64 with value of numpy.iinfo(np.int64).min 

    * uint64() for numpy.uint64 with value of numpy.iinfo(np.uint64).max 

    * float16() for numpy.float16 with value of numpy.NaN 

    * float32() for numpy.float32 with value of numpy.NaN 

    * float64() for numpy.float64 with value of numpy.NaN 


## Example



```
## Not run:
from revoscalepy import RxXdfData, RxSqlServerData, RxInSqlServer
from revoscalepy import RxOptions, rx_logit, RxMissingValues
from revoscalepy.functions.RxLogit import RxLogitResults
import os

def transform_late (data, context):
    import numpy

    #
    # create a new 'Late' column based on 'ArrDelay' int32 column
    #
    data['Late'] = data['ArrDelay'] > 15

    #
    # replace all 'Late' with NaN for 'NA' or missing values in ArrDelay
    #
    data.loc[data['ArrDelay'] == RxMissingValues.int32(), ('Late')] = numpy.NaN

    return data

transformVars = ["ArrDelay"]
sample_data_path = RxOptions.get_option("sampleDataDir")
xdf_file = os.path.join(sample_data_path, "AirlineDemoSmall.xdf")

data = RxXdfData(xdf_file)
model = rx_logit("Late~CRSDepTime + DayOfWeek", data=data, transform_function=transform_late, transform_variables=transformVars)

assert isinstance(model, RxLogitResults)
## End(Not run)
```

