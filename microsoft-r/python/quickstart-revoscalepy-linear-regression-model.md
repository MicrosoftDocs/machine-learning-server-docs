---

# required metadata
title: "Python tutorial: Linear regression model using revoscalepy on Machine Learning Server "
description: "Learn how to build and deploy a model using revoscalepy Python functions. Predict outcomes. Summarize  data."
keywords: 
author: "dphansen"
ms.author: "davidph"
manager: "cgronlun"
ms.date: 02/16/2018
ms.topic: "quickstart"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
#ms.custom: ""
---

# Quickstart: Create a linear regression model using revoscalepy in Python

[!INCLUDE [retirement banner](~/includes/machine-learning-server-retirement.md)]

**Applies to: Machine Learning Server 9.x**

This Python quickstart demonstrates a linear regression model on a local Machine Learning Server, using functions from the [revoscalepy library](../python-reference/revoscalepy/revoscalepy-package.md) and built-in sample data. 

Steps are executed on a Python command line using Machine Learning Server in the default local compute context. In this context, all operations run locally: data is fetched from the data source, and the model-fitting runs in your current Python environment.

The revoscalepy library for Python contains objects, transformations, and algorithms similar to what's included in the [RevoScaleR package](../r-reference/revoscaler/revoscaler.md) for the R language. With revoscalepy, you can write a Python script that creates a compute context, moves data between compute contexts, transforms data, and trains predictive models using popular algorithms such as logistic and linear regression, decision trees, and more.

> [!Note]
> For the SQL Server version of this tutorial, see [Use Python with revoscalepy to create a model (SQL Server)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model).

## Start Python

+ On Windows, go to C:\Program Files\Microsoft\ML Server\PYTHON_SERVER, and double-click **Python.exe**.
+ On Linux, enter **mlserver-python** at the command line.

## Import libraries and functions

Paste in the following statements to import libraries and functions.

```python
import os
from revoscalepy import RxOptions, RxXdfData
from revoscalepy import rx_lin_mod, rx_predict, rx_summary, rx_import
```

## Create a data source object

The data is retrieved locally from a sample .xdf file included in your installation. In this step, set the file path and then create a data source object to load the data. The sample data provides airline delays over a given time period, for multiple airlines.

```python
### Set the location
sample_data_path = RxOptions.get_option("sampleDataDir")

### Create the data source object
data_source = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
```

## Create a linear regression model

In a linear regression, you model the relationship between dependent and independent variables. In this step, the duration of a delay is captured for each day of the week. 

```python
### Create a linear model
linmod_local = rx_lin_mod("ArrDelay ~ DayOfWeek", data = data_source)
```

## Predict delays

Using a prediction function, you can predict the likelihood of a delay for each day.

```python
### Predict airport delays.

predict = rx_predict(linmod_local, data = rx_import(input_data = data_source))

### Print the output. For large data, you get the first and last instances.
### An ArrDelay_Pred column is created based on input with _Pred appended.
print(predict)
```

## Summarize data

In this last step, extract summary statistics from the computed prediction to understand the range and shape of prediction. Print the output to the console. The rx_summary function returns mean, standard deviation, and min-max values.

```python
### Create an object to store summary data
summary = rx_summary("ArrDelay_Pred", predict)

### Send the output to the console
print(summary)
```
**Results**

```text
Summary Statistics Results for: ArrDelay_Pred

Number of valid observations: 600000.0
Number of missing observations: 0.0

            Name       Mean    StdDev       Min        Max  ValidObs  \
0  ArrDelay_Pred  11.323407  1.760001  8.658007  14.804335  600000.0

   MissingObs
0         0.0
```

## Next steps

The ability to switch compute context to a different machine or platform is a powerful capability. To see how this works, continue with the SQL Server version of this tutorial: [Use Python with revoscalepy to create a model (SQL Server)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model).

You can also review [linear modeling for RevoScaleR](../r/how-to-revoscaler-linear-model.md). For linear models, the Python implementation in revoscalepy is similar to the R implementation in RevoScaleR.


## See Also

+ [How to use revoscalepy in local and remote compute contexts](how-to-revoscalepy.md)
+ [Python Function Reference](../python-reference/introducing-python-package-reference.md)
+ [microsoftml function reference](/sql/machine-learning/python/ref-py-microsoftml)
+ [revoscalepy function reference](../python-reference/revoscalepy/revoscalepy-package.md)
