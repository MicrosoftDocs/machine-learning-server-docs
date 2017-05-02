---

# required metadata
title: "Quick Start for Microsoft R Client"
description: "Microsoft R Client"
keywords: "R Client, Microsoft R Client, Introduction, Get Started with R Client"
author: "j-martens"
manager: "jhubbard"
ms.date: "11/30/2016"
ms.topic: "get-started-article"
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
ms.technology: "r-client"
ms.custom: ""

---
# Quick Start: Use R Client to predict delayed flights

## Objective

This quick start shows you how to predict flight delays in R locally using R Client. The example in this article uses historical on-time performance and weather data to predict whether the arrival of a scheduled passenger flight will be delayed by more than 15 minutes.  We approach this problem as a classification problem, predicting two classes -- whether the flight will be delayed, or whether it will be on time.

Broadly speaking, in machine learning and statistics, classification is the task of identifying the class or category to which a new observation belongs on the basis of a training set of data containing observations with known categories. Classification is generally a supervised learning problem. Since this is a binary classification task, there are only two classes.

In this example, you train a model using a large number of examples from historic flight data, along with an outcome measure that indicates the appropriate category or class for each example. The two classes are `0` for ontime flights and `1` for flights delayed longer than 15 minutes.

## Time estimate

If you have completed the prerequisites, this task will take approximately *5* minutes to complete.

## Prerequisites

This quick start assumes that you have installed Microsoft R Client and launch R on the commandline or in an R integrated development environment (IDE). Read the article [Get Started with Microsoft R Client](r-client-get-started.md) for more information.

## Example code

Here is the whole script you can walk through step by step below.

```r
       #############################################
       ##           ENTIRE SAMPLE SCRIPT          ##
       #############################################

#Step 1: Prep and Import Data
#Initialize some variables to specify the data sets.
   github <- "https://github.com/Microsoft/RTVS-docs/tree/master/examples/MRS_and_Machine_Learning"
   inputFileFlightURL <- paste0(github, "Flight_Delays_Sample.csv")
   inputFileWeatherURL <- paste0(github, "Weather_Sample.csv")

#Create a temporary directory to store the intermediate XDF files. 
   td <- tempdir()
   outFileFlight <- paste0(td, "/flight.xdf")
   outFileWeather <- paste0(td, "/weather.xdf")
   outFileOrigin <- paste0(td, "/originData.xdf")
   outFileDest <- paste0(td, "/destData.xdf")
   outFileFinal <- paste0(td, "/finalData.xdf")

#Import the flight data.
    flight_mrs <- rxImport(
      inData = inputFileFlightURL, outFile = outFileFlight,
      missingValueString = "M", stringsAsFactors = FALSE,
      # Remove columns that are possible target leakers from the flight data.
      varsToDrop = c("DepDelay", "DepDel15", "ArrDelay", "Cancelled", "Year"),
      # Define "Carrier" as categorical.
      colInfo = list(Carrier = list(type = "factor")),
      # Round down scheduled departure time to full hour.
      transforms = list(CRSDepTime = floor(CRSDepTime/100)),  
      overwrite = TRUE
    )

#Review the first 6 rows of flight data.
    head(flight_mrs)

#Summarize the flight data.
    rxSummary(~., data = flight_mrs, blocksPerRead = 2)

#Import the weather data.
    xform <- function(dataList) {
      # Create a function to normalize some numerical features.
      featureNames <- c(
        "Visibility", 
        "DryBulbCelsius", 
        "DewPointCelsius", 
        "RelativeHumidity", 
        "WindSpeed", 
        "Altimeter"
      )
      dataList[featureNames] <- lapply(dataList[featureNames], scale)
      return(dataList)
    }

    weather_mrs <- rxImport(
      inData = inputFileWeatherURL, outFile = outFileWeather,
      missingValueString = "M", stringsAsFactors = FALSE,
      # Eliminate some features due to redundance.
      varsToDrop = c("Year", "Timezone", 
                     "DryBulbFarenheit", "DewPointFarenheit"),
      # Create a new column "DestAirportID" in weather data.
      transforms = list(DestAirportID = AirportID),
      # Apply the normalization function.
      transformFunc = xform,  
      transformVars = c(
        "Visibility", 
        "DryBulbCelsius", 
        "DewPointCelsius", 
        "RelativeHumidity", 
        "WindSpeed", 
        "Altimeter"
      ),
      overwrite = TRUE
    )

#Review the variable information for the weather data.
    rxGetVarInfo(weather_mrs)


#Step 2: Pre-process Data
#Rename some column names in the weather data to prepare it for merging.
    newVarInfo <- list(
      AdjustedMonth = list(newName = "Month"),
      AdjustedDay = list(newName = "DayofMonth"),
      AirportID = list(newName = "OriginAirportID"),
      AdjustedHour = list(newName = "CRSDepTime")
    )
    rxSetVarInfo(varInfo = newVarInfo, data = weather_mrs)

#Concatenate/Merge flight records and weather data.
##Join flight records and weather data at origin of the flight `OriginAirportID`.
      originData_mrs <- rxMerge(
        inData1 = flight_mrs, inData2 = weather_mrs, outFile = outFileOrigin,
        type = "inner", autoSort = TRUE, 
        matchVars = c("Month", "DayofMonth", "OriginAirportID", "CRSDepTime"),
        varsToDrop2 = "DestAirportID",
        overwrite = TRUE
      )

##Join flight records and weather data using the destination of the flight `DestAirportID`.
      destData_mrs <- rxMerge(
        inData1 = originData_mrs, inData2 = weather_mrs, outFile = outFileDest,
        type = "inner", autoSort = TRUE, 
        matchVars = c("Month", "DayofMonth", "DestAirportID", "CRSDepTime"),
        varsToDrop2 = c("OriginAirportID"),
        duplicateVarExt = c("Origin", "Destination"),
        overwrite = TRUE
      )

##Call `rxFactors()` function to convert `OriginAirportID` and `DestAirportID` as categorical.
      rxFactors(inData = destData_mrs, outFile = outFileFinal, sortLevels = TRUE,
                factorInfo = c("OriginAirportID", "DestAirportID"),
                overwrite = TRUE)


#Step 3: Prepare Training and Test Datasets
#Randomly split data (80% for training, 20% for testing).
   rxSplit(inData = outFileFinal,
           outFilesBase = paste0(td, "/modelData"),
           outFileSuffixes = c("Train", "Test"),
           splitByFactor = "splitVar",
           overwrite = TRUE,
           transforms = list(
             splitVar = factor(sample(c("Train", "Test"),
                                       size = .rxNumRows,
                                       replace = TRUE,
                                       prob = c(.80, .20)),
                                levels = c("Train", "Test"))),
            rngSeed = 17,
            consoleOutput = TRUE)

#Point to the XDF files for each set.
   train <- RxXdfData(paste0(td, "/modelData.splitVar.Train.xdf"))
   test <- RxXdfData(paste0(td, "/modelData.splitVar.Test.xdf"))


#Step 4: Predict using Logistic Regression
#Choose and apply the Logistic Regression learning algorithm.

   #Build the formula.
   modelFormula <- formula(train, depVars = "ArrDel15",
                           varsToDrop = c("RowNum", "splitVar"))

   #Fit a Logistic Regression model.
   logitModel_mrs <- rxLogit(modelFormula, data = train)

   #Review the model results.
   summary(logitModel_mrs)

#Predict using new data.
    #Predict the probability on the test dataset.
    rxPredict(logitModel_mrs, data = test,
              type = "response",
              predVarNames = "ArrDel15_Pred_Logit",
              overwrite = TRUE)

    #Calculate Area Under the Curve (AUC).
    paste0("AUC of Logistic Regression Model:",
           rxAuc(rxRoc("ArrDel15", "ArrDel15_Pred_Logit", test)))

    #Plot the ROC curve.
    rxRocCurve("ArrDel15", "ArrDel15_Pred_Logit", data = test,
               title = "ROC curve - Logistic regression")


#Step 5: Predict using Decision Tree
#Choose and apply the Decision Tree learning algorithm.
    #Build a decision tree model.
    dTree1_mrs <- rxDTree(modelFormula, data = train, reportProgress = 1)

    #Find the Best Value of cp for Pruning rxDTree Object.
    treeCp_mrs <- rxDTreeBestCp(dTree1_mrs)

    #Prune a decision tree created by rxDTree and return the smaller tree.
    dTree2_mrs <- prune.rxDTree(dTree1_mrs, cp = treeCp_mrs)

#Predict using new data.
    #Predict the probability on the test dataset.
    rxPredict(dTree2_mrs, data = test,
              predVarNames = "ArrDel15_Pred_Tree",
              overwrite = TRUE)

    #Calculate Area Under the Curve (AUC).
    paste0("AUC of Decision Tree Model:",
           rxAuc(rxRoc(" ArrDel15 ", " ArrDel15_Pred_Tree ", test)))

    #Plot the ROC curve.
    rxRocCurve("ArrDel15",
               predVarNames = c("ArrDel15_Pred_Tree", "ArrDel15_Pred_Logit"),
               data = test,
               title = "ROC curve - Logistic regression")
```

## Step 1. Prepare and import data

1. Initialize some variables to specify the data sets.
   ```
   github <- "https://github.com/Microsoft/RTVS-docs/tree/master/examples/MRS_and_Machine_Learning"
   inputFileFlightURL <- paste0(github, "Flight_Delays_Sample.csv")
   inputFileWeatherURL <- paste0(github, "Weather_Sample.csv")
   ```

1. Create a temporary directory to store the intermediate XDF files. The External Data Frame (XDF) file format is a high-performance, binary file format for storing big data sets for use with RevoScaleR. This file format has an R interface and optimizes rows and columns for faster processing and analysis.  [Learn more](scaler-user-guide-introduction.md)
   ```
   td <- tempdir()
   outFileFlight <- paste0(td, "/flight.xdf")
   outFileWeather <- paste0(td, "/weather.xdf")
   outFileOrigin <- paste0(td, "/originData.xdf")
   outFileDest <- paste0(td, "/destData.xdf")
   outFileFinal <- paste0(td, "/finalData.xdf")
   ```

1. Import the flight data.
    ```
    flight_mrs <- rxImport(
      inData = inputFileFlightURL, outFile = outFileFlight,
      missingValueString = "M", stringsAsFactors = FALSE,
      # Remove columns that are possible target leakers from the flight data.
      varsToDrop = c("DepDelay", "DepDel15", "ArrDelay", "Cancelled", "Year"),
      # Define "Carrier" as categorical.
      colInfo = list(Carrier = list(type = "factor")),
      # Round down scheduled departure time to full hour.
      transforms = list(CRSDepTime = floor(CRSDepTime/100)),  
      overwrite = TRUE
    )
    ```

1. Review the first 6 rows of flight data.
    ```
    head(flight_mrs)
    ```

1. Summarize the flight data.
    ```
    rxSummary(~., data = flight_mrs, blocksPerRead = 2)
    ```

1. Import the weather data.
    ```
    xform <- function(dataList) {
      # Create a function to normalize some numerical features.
      featureNames <- c(
        "Visibility", 
        "DryBulbCelsius", 
        "DewPointCelsius", 
        "RelativeHumidity", 
        "WindSpeed", 
        "Altimeter"
      )
      dataList[featureNames] <- lapply(dataList[featureNames], scale)
      return(dataList)
    }

    weather_mrs <- rxImport(
      inData = inputFileWeatherURL, outFile = outFileWeather,
      missingValueString = "M", stringsAsFactors = FALSE,
      # Eliminate some features due to redundance.
      varsToDrop = c("Year", "Timezone", 
                     "DryBulbFarenheit", "DewPointFarenheit"),
      # Create a new column "DestAirportID" in weather data.
      transforms = list(DestAirportID = AirportID),
      # Apply the normalization function.
      transformFunc = xform,  
      transformVars = c(
        "Visibility", 
        "DryBulbCelsius", 
        "DewPointCelsius", 
        "RelativeHumidity", 
        "WindSpeed", 
        "Altimeter"
      ),
      overwrite = TRUE
    )
    ```
1. Review the variable information for the weather data.
    ```
    rxGetVarInfo(weather_mrs)
    ```

## Step 2. Pre-process data

1. Rename some column names in the weather data to prepare it for merging.
    ```
    newVarInfo <- list(
      AdjustedMonth = list(newName = "Month"),
      AdjustedDay = list(newName = "DayofMonth"),
      AirportID = list(newName = "OriginAirportID"),
      AdjustedHour = list(newName = "CRSDepTime")
    )
    rxSetVarInfo(varInfo = newVarInfo, data = weather_mrs)
    ```

1. Concatenate/Merge flight records and weather data.
   1. Join flight records and weather data at origin of the flight `OriginAirportID`.
      ```
      originData_mrs <- rxMerge(
        inData1 = flight_mrs, inData2 = weather_mrs, outFile = outFileOrigin,
        type = "inner", autoSort = TRUE, 
        matchVars = c("Month", "DayofMonth", "OriginAirportID", "CRSDepTime"),
        varsToDrop2 = "DestAirportID",
        overwrite = TRUE
      )
      ```                

   1. Join flight records and weather data using the destination of the flight `DestAirportID`.
      ```
      destData_mrs <- rxMerge(
        inData1 = originData_mrs, inData2 = weather_mrs, outFile = outFileDest,
        type = "inner", autoSort = TRUE, 
        matchVars = c("Month", "DayofMonth", "DestAirportID", "CRSDepTime"),
        varsToDrop2 = c("OriginAirportID"),
        duplicateVarExt = c("Origin", "Destination"),
        overwrite = TRUE
      )
      ```

   1. Call `rxFactors()` function to convert `OriginAirportID` and `DestAirportID` as categorical.
      ```
      rxFactors(inData = destData_mrs, outFile = outFileFinal, sortLevels = TRUE,
                factorInfo = c("OriginAirportID", "DestAirportID"),
                overwrite = TRUE)
      ```

## Step 3. Prepare training and test datasets

1. Randomly split data (80% for training, 20% for testing).
   ```
   rxSplit(inData = outFileFinal,
           outFilesBase = paste0(td, "/modelData"),
           outFileSuffixes = c("Train", "Test"),
           splitByFactor = "splitVar",
           overwrite = TRUE,
           transforms = list(
             splitVar = factor(sample(c("Train", "Test"),
                                       size = .rxNumRows,
                                       replace = TRUE,
                                       prob = c(.80, .20)),
                                levels = c("Train", "Test"))),
            rngSeed = 17,
            consoleOutput = TRUE)
   ```

1. Point to the XDF files for each set.
   ```
   train <- RxXdfData(paste0(td, "/modelData.splitVar.Train.xdf"))
   test <- RxXdfData(paste0(td, "/modelData.splitVar.Test.xdf"))
   ```

## Step 4. Predict using logistic regression

1. Choose and apply the Logistic Regression learning algorithm.
   ```
   # Build the formula.
   modelFormula <- formula(train, depVars = "ArrDel15",
                           varsToDrop = c("RowNum", "splitVar"))

   # Fit a Logistic Regression model.
   logitModel_mrs <- rxLogit(modelFormula, data = train)

   # Review the model results.
   summary(logitModel_mrs)
   ```

1. Predict using new data.
    ```
    # Predict the probability on the test dataset.
    rxPredict(logitModel_mrs, data = test,
              type = "response",
              predVarNames = "ArrDel15_Pred_Logit",
              overwrite = TRUE)

    # Calculate Area Under the Curve (AUC).
    paste0("AUC of Logistic Regression Model:",
           rxAuc(rxRoc("ArrDel15", "ArrDel15_Pred_Logit", test)))

    # Plot the ROC curve.
    rxRocCurve("ArrDel15", "ArrDel15_Pred_Logit", data = test,
               title = "ROC curve - Logistic regression")
    ```

## Step 5. Predict using decision tree
1. Choose and apply the Decision Tree learning algorithm.
    ```
    # Build a decision tree model.
    dTree1_mrs <- rxDTree(modelFormula, data = train, reportProgress = 1)

    # Find the Best Value of cp for Pruning rxDTree Object.
    treeCp_mrs <- rxDTreeBestCp(dTree1_mrs)

    # Prune a decision tree created by rxDTree and return the smaller tree.
    dTree2_mrs <- prune.rxDTree(dTree1_mrs, cp = treeCp_mrs)
    ```

1. Predict using new data.
    ```
    # Predict the probability on the test dataset.
    rxPredict(dTree2_mrs, data = test,
              predVarNames = "ArrDel15_Pred_Tree",
              overwrite = TRUE)

    # Calculate Area Under the Curve (AUC).
    paste0("AUC of Decision Tree Model:",
           rxAuc(rxRoc(" ArrDel15 ", " ArrDel15_Pred_Tree ", test)))

    # Plot the ROC curve.
    rxRocCurve("ArrDel15",
               predVarNames = c("ArrDel15_Pred_Tree", "ArrDel15_Pred_Logit"),
               data = test,
               title = "ROC curve - Logistic regression")
    ```


## Next steps

Now that you've tried this example, you can start developing your own solutions using the [`RevoScaleR` R package functions](scaler/scaler.md), [`MicrosoftML` R package functions](microsoftml/microsoftml.md), and APIs. When ready, you can run that R code using R Client or even send those R commands to a [remote R Server](operationalize/remote-execution.md) for execution if Microsoft R Server is also installed in your organization. 
