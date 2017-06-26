--- 
 
# required metadata 
title: "Data Transformation for RevoScaleR data sources" 
description: "Transforms data from an input data set to an output data set." 
keywords: "manip" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/26/2017" 
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

# rx_predict



```
microsoftml.modules.predict.rx_predict(model, data, out_data=None, write_model_vars=False, extra_vars_to_write=None, suffix=None, overwrite=False, data_threads=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>, **kargs)
```




## Description

Transforms data from an input data set to an output data set.


## Parameters


### data

A  data source object, a data frame, or the path
to a ``.xdf`` file.


### out_data

Output text or xdf file name or an ``RxDataSource`` with
write capabilities in which to store transformed data. If *None*, a data
frame is returned. The default value is *None*.


### overwrite

If ``TRUE``, an existing ``outData`` is overwritten;
if ``FALSE`` an existing ``outData`` is not overwritten. The default
value is /code{FALSE}.


### data_threads

An integer specifying the desired degree of parallelism in
the data pipeline. If *None*, the number of threads used is determined
internally. The default value is *None*.


### random_seed

Specifies the random seed. The default value is *None*.


### max_slots

Max slots to return for vector valued columns (<=0 to return all).


### ml_transforms

Specifies a list of MicrosoftML transforms to be
performed on the data before training or *None* if no transforms are
to be performed. See ``featurize_text()``, ``categorical()``,
and ``categorical_hash()``, for transformations that are supported.
These transformations are performed after any specified R transformations.
The default value is *None*.


### ml_transform_vars

Specifies a character vector of variable names
to be used in ``mlTransforms`` or *None* if none are to be used.
The default value is *None*.


### row_selection

Specifies the rows (observations) from the data set that
are to be used by the model with the name of a logical variable from the
data set (in quotes) or with a logical expression using variables in the
data set. For example, ``row_selection = "old"`` will only use
observations in which the value of the variable ``old`` is ``TRUE``.
``row_selection = (age > 20) & (age < 65) & (log(income) > 10)`` only uses
observations in which the value of the ``age`` variable is between
20 and 65 and the value of the ``log`` of the ``income`` variable is
greater than 10. The row selection is performed after processing any data
transformations (see the arguments ``transforms`` or
``transform_func``). As with all expressions, ``row_selection`` can be
defined outside of the function call using the ``expression()``
function.


### transforms

An expression of the form that represents
the first round of variable transformations. As with
all expressions, ``transforms`` (or ``row_selection``) can be defined
outside of the function call using the ``expression()`` function.
The default value is *None*.


### transform_objects

A named list that contains objects that can be
referenced by ``transforms``, ``transformsFunc``, and
``row_selection``. The default value is *None*.


### transform_func

The variable transformation function.
The default value is *None*.


### transform_vars

A character vector of input data set variables needed for
the transformation function.
The default value is *None*.


### transform_envir

A user-defined environment to serve as a parent to all
environments developed internally and used for variable data transformation.
If ``transformEnvir = NULL``, a new “hash” environment with parent
``baseenv()`` is used instead The default value is *None*.


### blocks_per_read

Specifies the number of blocks to read for each chunk
of data read from the data source.


### report_progress

An integer value that specifies the level of reporting
on the row processing progress:

* ``0``: no progress is reported. 

* ``1``: the number of processed rows is printed and updated. 

* ``2``: rows processed and timings are reported. 

* ``3``: rows processed and all timings are reported. 

The default value is ``1``.


### verbose

An integer value that specifies the amount of output wanted.
If ``0``, no verbose output is printed during calculations. Integer
values from ``1`` to ``4`` provide increasing amounts of information.
The default value is ``1``.


### compute_context

Sets the context in which computations are executed,
specified with a valid ``revo_scale_r``.
Currently local and ``revo_scale_r`` compute contexts
are supported.


## Returns

A data frame or an ``revo_scale_r`` object
representing the created output data.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


## See also

``rx_data_step``,
``rx_import_datasource``.


## Example



```
# rxFeaturize basically allows you to access data from the MicrosoftML transforms
# In this example we'll look at getting the output of the categorical transform

# Create the data
categoricalData <- data.frame(
  placesVisited = c(
    "London",
    "Brunei",
    "London",
    "Paris",
    "Seria"
  ),
  stringsAsFactors = FALSE
)

# Invoke the categorical transform
categorized <- rxFeaturize(
  data = categoricalData,
  mlTransforms = list(categorical(vars = c(xDataCat = "placesVisited")))
)

# Now let's look at the data
categorized
```

