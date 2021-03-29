--- 

# required metadata 
title: "selectFeatures function (MicrosoftML) " 
description: " The feature selection transform selects features from the specified variables using the specified mode. " 
keywords: "(MicrosoftML), selectFeatures, feature, selection, transform" 
author: "dphansen"
ms.author: "davidph" 
manager: "cgronlun" 
ms.date: 07/15/2019
ms.topic: "reference" 
ms.prod: "mlserver" 
ms.service: "" 
ms.assetid: "" 

# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
#ms.technology: "" 
ms.custom: "" 

--- 




 # selectFeatures: Machine Learning Feature Selection Transform 
 ## Description

The feature selection transform selects features from the specified
variables using the specified mode.


 ## Usage

```   
  selectFeatures(vars, mode, ...)

```

 ## Arguments



 ### `vars`
 A formula or a vector/list of strings specifying the name of variables upon which the feature selection is performed, if the mode is  minCount(). For example, `~ var1 + var2 + var3`. If mode is mutualInformation(), a formula or a named list of strings describing the dependent variable and the independent variables. For example, `label ~ ``var1 + var2 + var3`. 



 ### `mode`
 Specifies the mode of feature selection. This can be either  [minCount](minCount.md) or [mutualInformation](mutualInformation.md). 



 ### ` ...`
 Additional arguments to be passed directly to the Microsoft Compute Engine. 



 ## Details

The feature selection transform selects features from the specified
variables using one of the two modes: count or mutual information. For more
information, see [minCount](minCount.md) and [mutualInformation](mutualInformation.md).


 ## Value

A `maml` object defining the transform.

 ## See Also

[minCount](minCount.md) [mutualInformation](mutualInformation.md)

 ## Examples

 ```

  trainReviews <- data.frame(review = c( 
          "This is great",
          "I hate it",
          "Love it",
          "Do not like it",
          "Really like it",
          "I hate it",
          "I like it a lot",
          "I kind of hate it",
          "I do like it",
          "I really hate it",
          "It is very good",
          "I hate it a bunch",
          "I love it a bunch",
          "I hate it",
          "I like it very much",
          "I hate it very much.",
          "I really do love it",
          "I really do hate it",
          "Love it!",
          "Hate it!",
          "I love it",
          "I hate it",
          "I love it",
          "I hate it",
          "I love it"),
       like = c(TRUE, FALSE, TRUE, FALSE, TRUE,
          FALSE, TRUE, FALSE, TRUE, FALSE, TRUE, FALSE, TRUE,
          FALSE, TRUE, FALSE, TRUE, FALSE, TRUE, FALSE, TRUE, 
          FALSE, TRUE, FALSE, TRUE), stringsAsFactors = FALSE
      )

      testReviews <- data.frame(review = c(
          "This is great",
          "I hate it",
          "Love it",
          "Really like it",
          "I hate it",
          "I like it a lot",
          "I love it",
          "I do like it",
          "I really hate it",
          "I love it"), stringsAsFactors = FALSE)

  # Use a categorical hash transform which generated 128 features.
  outModel1 <- rxLogisticRegression(like~reviewCatHash, data = trainReviews, l1Weight = 0, 
      mlTransforms = list(categoricalHash(vars = c(reviewCatHash = "review"), hashBits = 7)))
  summary(outModel1)

  # Apply a categorical hash transform and a count feature selection transform
  # which selects only those hash slots that has value.
  outModel2 <- rxLogisticRegression(like~reviewCatHash, data = trainReviews, l1Weight = 0, 
      mlTransforms = list(
    categoricalHash(vars = c(reviewCatHash = "review"), hashBits = 7), 
    selectFeatures("reviewCatHash", mode = minCount())))
  summary(outModel2)

  # Apply a categorical hash transform and a mutual information feature selection transform
  # which selects only 10 features with largest mutual information with the label.
  outModel3 <- rxLogisticRegression(like~reviewCatHash, data = trainReviews, l1Weight = 0, 
      mlTransforms = list(
    categoricalHash(vars = c(reviewCatHash = "review"), hashBits = 7), 
    selectFeatures(like ~ reviewCatHash, mode = mutualInformation(numFeaturesToKeep = 10))))
  summary(outModel3)
```





