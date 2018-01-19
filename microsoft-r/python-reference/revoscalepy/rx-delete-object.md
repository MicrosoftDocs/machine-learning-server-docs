--- 
 
# required metadata 
title: "rx_delete_object: Manage objects in ODBC Data Sources" 
description: "Deletes an object from an ODBC data source. The APIs are modelled after a simple key value store." 
keywords: "delete, object" 
author: "heidist" 
manager: "cgronlun" 
ms.date: "01/19/2018" 
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

# rx_delete_object


 


## Usage



```
revoscalepy.rx_delete_object(src: revoscalepy.datasource.RxOdbcData.RxOdbcData,
    key: str = None, version: str = None,
    key_name: str = 'id', version_name: str = 'version',
    all: bool = False)
```





## Description

Deletes an object from an ODBC data source. The APIs are modelled
after a simple key value store.


## Details

Deletes an object from the ODBC data source. If there
are multiple objects identified by the key/version combination, all are
deleted.

The key and the version column should be of some SQL character type
(CHAR, VARCHAR, NVARCHAR, etc) supported by the data source. The value
column should be a binary type (VARBINARY for instance). Some
conversions to other types might work, however, they are dependant on
the ODBC driver and on the underlying package functions.


## Arguments


### src

The object being stored into the data source.


### key

A character string identifying the object. The intended use is
for the key+version to be unique.


### version

None or a character string which carries the version of the
object. Combined with key identifies the object.


### key_name

Character string specifying the column name for the key in
the underlying table.


### version_name

Character string specifying the column name for the
version in the underlying table.


### all

Bool value. True to remove all objects from the data source.
If True, the ‘key’ parameter is ignored.


## Returns

rx_read_object returns an object. rx_write_object and rx_delete_object
return bool, True on success. rx_list_keys returns a single column
data frame containing strings.


## Example



```
from pandas import DataFrame
from numpy import random
from revoscalepy import RxOdbcData, rx_write_object, rx_read_object, rx_list_keys, rx_delete_object

connection_string = 'Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;'
dest = RxOdbcData(connection_string, table = "dataframe")

df = DataFrame(random.randn(10, 5))

status = rx_write_object(dest, key = "myDf", value = df)

read_obj = rx_read_object(dest, key = "myDf")

keys = rx_list_keys(dest)

rx_delete_object(dest, key = "myDf")
```

