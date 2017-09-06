--- 
 
# required metadata 
title: "rx_set_var_info: Set Variable Information for .xdf File or Data Frame" 
description: "Set the variable information for an “.xdf” file, including variable names, descriptions, and value labels, or set attributes for variables in a data frame" 
keywords: "variables" 
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

# `rx_set_var_info`


**Applies to: SQL Server 2017 RC2**


## Usage



```
revoscalepy.rx_set_var_info(var_info, data)
```





## Description

Set the variable information for an “.xdf” file, including variable
names, descriptions, and value labels, or set attributes for variables in
a data frame


## Arguments


### var_info

list containing lists of variable information for variables
in the XDF data source or data frame.


### data

an RxDataSource object, a character string specifying the “.xdf”
file, or a data frame.


## Returns

If the input data is a data frame, a data frame is returned
containing variables with the new attributes. If the input data represents an
“.xdf” file or composite file, an RxXdfData object representing the modified
file is returned.


## See also

`rx_get_info`,
`rx_get_var_info`.


## Example



```
import os
import tempfile
from shutil import copy2
from revoscalepy import RxOptions, RxXdfData, rx_import, rx_get_var_info, rx_set_var_info

sample_data_path = RxOptions.get_option("sampleDataDir")
src_claims_file = os.path.join(sample_data_path, "claims.xdf")
dest_claims_file = os.path.join(tempfile.gettempdir(), "claims_new.xdf")
copy2(src_claims_file, dest_claims_file)
var_info = rx_get_var_info(src_claims_file)
print(var_info)

claims_labels = ['Type1', 'Type2', 'Type3', 'Type4']
var_info['type']['levels'] = claims_labels
var_info['cost']['low'] = 10
var_info['cost']['high'] = 1000
rx_set_var_info(var_info, dest_claims_file)

new_var_info = rx_get_var_info(dest_claims_file)
print(new_var_info)
```


Output:



```
Var 1: RowNum, Type: integer, Storage: int32, Low/High: (1.0000, 128.0000)
Var 2: age
	8 factor levels: ['17-20', '21-24', '25-29', '30-34', '35-39', '40-49', '50-59', '60+']
Var 3: car.age
	4 factor levels: ['0-3', '4-7', '8-9', '10+']
Var 4: type
	4 factor levels: ['A', 'B', 'C', 'D']
Var 5: cost, Type: numeric, Storage: float32, Low/High: (11.0000, 850.0000)
Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)

Var 1: RowNum, Type: integer, Storage: int32, Low/High: (1.0000, 128.0000)
Var 2: age
	8 factor levels: ['17-20', '21-24', '25-29', '30-34', '35-39', '40-49', '50-59', '60+']
Var 3: car.age
	4 factor levels: ['0-3', '4-7', '8-9', '10+']
Var 4: type
	4 factor levels: ['Type1', 'Type2', 'Type3', 'Type4']
Var 5: cost, Type: numeric, Storage: float32, Low/High: (10.0000, 1000.0000)
Var 6: number, Type: numeric, Storage: float32, Low/High: (0.0000, 434.0000)
```

