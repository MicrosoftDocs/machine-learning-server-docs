---

# required metadata
title: "Tutorial: PySpark and revoscalepy interoperabilty in Machine Learning Server | Microsoft Docs "
description: "Learn how to use PySpark and revoscalepy Python functions in Spark applications in Hadoop clusters having Machine Learning Server."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "09/20/2017"
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

# Tutorial: PySpark and revoscalepy interoperability in Machine Learning Server

[PySpark](https://spark.apache.org/downloads.html) is Apache Spark's programmable interface for Python. The [revoscalepy](../python-reference/revoscalepy/revoscalepy-package.md) module is Machine Learning Server's Python library for predictive analytics at scale. In this tutorial, you learn how to create a logistic regression model using functions from both libraries.

> [!div class="checklist"]
> * Import packages
> * Connect to Spark using `revoscalepy.rx_spark_connect()`, specifying PySpark interop
> * Use PySpark for basic data manipulation
> * Use revoscalepy to build a logistic regression model

> [!Note]
> The [revoscalepy](../python-reference/revoscalepy/revoscalepy-package.md) module provides functions for data sources and data manipulation. We are using PySpark in this tutorial to illustrate a basic technique for passing data objects between the two programming contexts.

## Prerequisites

+ A Hadoop cluster with Spark 2.0-2.4 with [Machine Learning Server for Hadoop](../install/machine-learning-server-hadoop-install.md)
+ A Python IDE, such as Jupyter Notebooks, [Visual Studio for Python](https://www.visualstudio.com/vs/python/), or PyCharm. 
+ Sample data (AirlineSubsetCsv mentioned in the example) downloaded from our sample data web site to your Spark cluster.

> [!Note]
> Jupyter Notebook users, update your notebook to include the MMLSPy kernel. Select this kernel in your Jupyter notebook to use the interoperability feature.
>

## Import the relevant packages

The following commands import the required libraries into the current session.

```Python
from pyspark import SparkContext
from pyspark.sql import SparkSession
from revoscalepy import *
```

## Connect to Spark

Setting `interop = ‘pyspark’` indicates that you want interoperability.

```Python
    # with PySpark for this Spark session
    cc = rx_spark_connect(interop='pyspark', reset=True)
    
    # Get the PySpark context
    sc = rx_get_pyspark_connection(cc)
    spark = SparkSession(sc)
```

## Data acquisition and manipulation

The sample data used in this tutorial is airline arrival and departure data, which you can store in a local file path.

```Python
    # Read in the airline data into a data frame
    airlineDF = spark.read.csv('<data source location like "file:///some-file-path/airline.csv">')
    
    # Get a count on rows
    airlineDF.count()

    # Return the first 10 lines to get familiar with the data
    airlineDF.take(10)

    # Rename columns for readability
    airlineTransformed = airlineDF.selectExpr('ARR_DEL15 as ArrDel15', \
    'YEAR as Year', \
    'MONTH as Month', \
    'DAY_OF_MONTH as DayOfMonth', \
    'DAY_OF_WEEK as DayOfWeek', \
    'UNIQUE_CARRIER as Carrier', \
    'ORIGIN_AIRPORT_ID as OriginAirportID', \
    'DEST_AIRPORT_ID as DestAirportID', \
    'FLOOR(CRS_DEP_TIME / 100) as CRSDepTime', \
    'CRS_ARR_TIME as CRSArrTime')
    
    # Break up the data set into train and test. We use training data for  
    # all years before 2012 to predict flight delays for Jan 2012
    airlineTrainDF = airlineTransformed.filter('Year < 2012')
    airlineTestDF = airlineTransformed.filter('(Year == 2012) AND (Month == 1)')
    
    # Define column info for factors
    column_info = {
        'ArrDel15': { 'type': 'numeric' },
        #'CRSDepTime': { 'type': 'integer' },
        'CRSDepTime': {
            'type': 'factor',
            'levels': ['0', '2', '3', '4', '5', '6', '7', '8', '9', '10',
                      '11', '12', '13', '14', '15', '16', '17', '18', '19', '20',
                      '21', '22', '23']
        },
        'CRSArrTime': { 'type': 'integer' },
        'Month': {
            'type': 'factor',
            'levels': ['1', '2']
        },
        'DayOfMonth': {
            'type': 'factor',
            'levels': ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10',
                      '11', '12', '13', '14', '15', '16', '17', '18', '19', '20',
                      '21', '22', '23', '24', '25', '26', '27', '28', '29', '30',
                      '31']
        },
        'DayOfWeek': {
            'type': 'factor',
            'levels': ['1', '2', '3', '4', '5', '6', '7']
            #, 'newLevels': ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'] # ignored
        }#,
        #'Carrier': { 'type': 'factor' }
    }
    
    # Define a Spark data frame data source, required for passing to revoscalepy
    trainDS = RxSparkDataFrame(airlineTrainDF, column_info=column_info)
    testDS = RxSparkDataFrame(airlineTestDF, column_info=column_info)
 ```

## Create the model

A logistic regression model requires a symbolic formula, specifying the dependent and indepdendent variables, and a data set. You can output the results using the print function.

 ```Python
    # Create the formula
    formula = "ArrDel15 ~ DayOfMonth + DayOfWeek + CRSDepTime + CRSArrTime"
    
    # Run a logistic regression to predict arrival delay
    logitModel = rx_logit(formula, data = trainDS)
    
    # Print the model summary to look at the co-efficients
    print(logitModel.summary())
```

## Next steps

This tutorial provides an introduction to a basic workflow using PySpark for data preparation and revoscalepy functions for logistic regression. For further exploration, review our [Python samples](python-samples.md).

## See also

+ [microsoftml function reference](../python-reference/microsoftml/microsoftml-package.md)
+ [revoscalepy function reference](../python-reference/revoscalepy/revoscalepy-package.md)
