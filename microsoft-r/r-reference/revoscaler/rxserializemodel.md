--- 
 
# required metadata 
title: " RevoScaleR Model Serialization and Unserialization " 
description: "   Serialize a **RevoScaleR**/**MicrosoftML** model in raw format to enable saving the model. This allows model to be loaded into SQL Server for real-time scoring. " 
keywords: "RevoScaleR, rxSerializeModel, rxUnserializeModel" 
author: "HeidiSteen"
ms.author: "heidist" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
#ROBOTS: "" 
#audience: "" 
#ms.devlang: "" 
#ms.reviewer: "" 
#ms.suite: "" 
#ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
#ms.custom: "" 
 
--- 
 
 
 
 #`rxSerializeModel`:  RevoScaleR Model Serialization and Unserialization 

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Serialize a **RevoScaleR**/**MicrosoftML** model in raw format to enable saving the model. This allows model to be loaded into SQL Server for real-time scoring.
 
 
 ##Usage

```   
  rxSerializeModel(model, metadata = NULL, realtimeScoringOnly = FALSE, ...)
  
  rxUnserializeModel(serializedModel, ...)
 
```
 
 
 ##Arguments

   
    
 ### `model`
 `RevoScaleR`/`MicrosoftML` model to be serialized 
  
    
 ### `metadata`
 Arbitrary metadata of `raw` type to be stored with the serialized model. Metadata will be returned when unserialized.  
  
    
 ### `realtimeScoringOnly`
 Drops fields not required for real-time scoring.  NOTE: Setting this flag could reduce the model size but `rxUnserializeModel` can no longer retrieve the RevoScaleR model 
  
    
 ### `serializedModel`
 Serialized model to be unserialized 
  
 
 
 
 ##Details
 
`rxSerializeModel` converts models into `raw` bytes to allow them to be saved and used for real-time scoring.

The following is the list of models that are currently supported in real-time scoring:


* 
 **RevoScaleR**


   * 
 rxLogit

   * 
 rxLinMod

   * 
 rxBTrees

   * 
 rxDTree

   * 
 rxDForest




* 
**MicrosoftML**


   * 
 rxFastTrees

   * 
 rxFastForest

   * 
 rxLogisticRegression

   * 
 rxOneClassSvm

   * 
 rxNeuralNet

   * 
 rxFastLinear






**RevoScaleR** models containing R transformations or transform based formula (e.g `"A ~ log(B)"`) are not supported in real-time scoring. Such transforms to input data may need to be handled before calling real-time scoring.

`rxUnserializeModel` method is used to retrieve the original R model object and metadata from the serialized raw model.
 
 
 ##Value
 
`rxSerializeModel` returns a serialized model.

`rxUnserializeModel` returns original R model object. If metadata is also present returns a list containing the original model object and metadata.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 
 ##See Also
 
   
 ##Examples

 ```
   
  myIris <- iris
  myIris[1:5,]
  form <- Sepal.Length ~ Sepal.Width + Petal.Length + Petal.Width + Species
  irisLinMod <- rxLinMod(form, data = myIris)
  
  # Serialize model for scoring
  serializedModel <- rxSerializeModel(irisLinMod)
  
  # Save model to file or SQL Server (use rxWriteObject)
  # serialized model can now be used for real-time scoring
  
  unserializedModel <- rxUnserializeModel(serializedModel)
 
```
 
