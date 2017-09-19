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

# Python tutorial: Create a linear regression model using revscalepy

This example demonstrates how you can create a logistic regression model in a local compute context in Machine Learning Server, using an algorithm from the revoscalepy package and built-in sample data. 

All operations are performed from a Python command line using Machine Learning Server in the default local compute context.

By default, operations are run locally, which means that if you don't specify a different compute context, the data will be fetched from the data source, and the model-fitting will run in your current Python environment.

The revoscalepy package for Python contains objects, transformations, and algorithms similar to those provided for the [RevoScaleR package](../r-reference/revoscaler/revoscaler.md) for the R language. With revoscalepy, you can write Python script that creates a compute context, moves data between compute contexts, transforms data, and trains predictive models using popular algorithms such as logistic and linear regression, decision trees, and more.

> [!Note]
> For the SQL Server version of this tutorial, see [Use Python with revoscalepy to create a model (SQL Server)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model).

## Start Python

+ On Windows, go to C:\Program Files\Microsoft\ML Server\PYTHON_SERVER, and double-click **Python.exe**.
+ On Linux, enter **mlserver-python** at the command line.

## Import libraries and functions

Paste in the following statements:

```
from revoscalepy import RxComputeContext, RxXdfData
from revoscalepy import rx_lin_mod, rx_predict, rx_summary
from revoscalepy import RxOptions, rx_import

from pandas import Categorical
import os
```

## Create a data source object

The data is retrieved locally from sample .xdf files included in Machine Learning Server. In this step, set the file path and then create a data source object to load the data. The sample data provides airline delays over a given time period, for multiple airlines.

    sample_data_path = revoscalepy.RxOptions.get_option("sampleDataDir")

    data_source = revoscalepy.RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))

## Create a linear regression models

You can model the relationship between dependent and independent variables. In this step, the duration of a delay is captured for each day of the week. If the variables are independent, we should see a straight line.

    linmod_local = revoscalepy.rx_lin_mod("ArrDelay ~ DayOfWeek", data = data_source)

## Predict delays

Using a prediction function, you can predict the likelihood of a delay for each day.

    predict = revoscalepy.rx_predict(linmod_local, data = revoscalepy.rx_import(input_data = data_source))

## Summarize data

In this last step, lets print a summary of the output to get statistical descriptions, 

    summary = revoscalepy.rx_summary("ArrDelay ~ DayOfWeek", data = data_source)

    print(summary)


## Next steps

The ability to switch compute context to a different machine or platform is a powerful capability. To see how this works, continue with the SQL Server version of this tutorial: [Use Python with revoscalepy to create a model (SQL Server)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/use-python-revoscalepy-to-create-model).

You can also review [linear modeling for RevoScaleR](https://docs.microsoft.com/r-server/r/how-to-revoscaler-linear-model). The revoscalepy implementation is very similar.


## See Also

+ [How to use revoscalepy in local and remote compute contexts](how-to-revoscalepy.md)
+ [Python Function Reference](../python-reference/introducing-python-package-reference.md)
+ [microsoftml function reference](../python-reference/microsoftml/microsoftml-package.md)
+ [revoscalepy function reference](../python-reference/revoscalepy/revoscalepy-package.md)
