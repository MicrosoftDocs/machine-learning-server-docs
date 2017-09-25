---

# required metadata
title: "Python tutorial: Linear regression model using revoscalepy on Machine Learning Server | Microsoft Docs"
description: "Learn how to build and deploy a model using revoscalepy Python functions. Predict outcomes. Summarize  data."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/19/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: ""
#ms.custom: ""

---

# Create a linear regression model using revoscalepy in Python

This Python quickstart demonstrates a linear regression model on a local Machine Learning Server, using functions from the [revoscalepy library](../python-reference/revoscalepy/revoscalepy-package.md) and built-in sample data. 

Steps in this quicktart are executed on a Python command line, using Machine Learning Server in the default local compute context. In this context, all operations run locally: data is fetched from the data source, and the model-fitting runs in your current Python environment.

The revoscalepy library for Python contains objects, transformations, and algorithms similar to those provided for the [RevoScaleR package](../r-reference/revoscaler/revoscaler.md) for the R language. With revoscalepy, you can write a Python script that creates a compute context, moves data between compute contexts, transforms data, and trains predictive models using popular algorithms such as logistic and linear regression, decision trees, and more.

> [!Note]
> For the SQL Server version of this tutorial, see [Use Python with revoscalepy to create a model (SQL Server)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model).

## Start Python

+ On Windows, go to C:\Program Files\Microsoft\ML Server\PYTHON_SERVER, and double-click **Python.exe**.
+ On Linux, enter **mlserver-python** at the command line.

## Import libraries and functions

Paste in the following statements to import libraries and functions.

```python
import revoscalepy
import os
import pandas

from revoscalepy import RxComputeContext, RxXdfData
from revoscalepy import rx_lin_mod, rx_predict, rx_summary
from revoscalepy import RxOptions, rx_import

from pandas import Categorical

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
linmod_local = revoscalepy.rx_lin_mod("ArrDelay ~ DayOfWeek", data = data_source)
```

## Predict delays

Using a prediction function, you can predict the likelihood of a delay for each day.

```python
predict = revoscalepy.rx_predict(linmod_local, data = revoscalepy.rx_import(input_data = data_source))
```

## Summarize data

In this last step, extract summary statistics from the sample dataset and then print the output to the console. The rx_summary function returns mean, standard deviation, and min-max values.

```python
### Create an object to store summary data
summary = revoscalepy.rx_summary("ArrDelay ~ DayOfWeek", data = data_source)

### Send the output to the console
print(summary)
```

## Next steps

The ability to switch compute context to a different machine or platform is a powerful capability. To see how this works, continue with the SQL Server version of this tutorial: [Use Python with revoscalepy to create a model (SQL Server)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model).

You can also review [linear modeling for RevoScaleR](../r/how-to-revoscaler-linear-model.md). For linear models, the Python iplementation in revoscalepy is very similar to the R implementation in RevoScaleR.


## See Also

+ [How to use revoscalepy in local and remote compute contexts](how-to-revoscalepy.md)
+ [Python Function Reference](../python-reference/introducing-python-package-reference.md)
+ [microsoftml function reference](../python-reference/microsoftml/microsoftml-package.md)
+ [revoscalepy function reference](../python-reference/revoscalepy/revoscalepy-package.md)
