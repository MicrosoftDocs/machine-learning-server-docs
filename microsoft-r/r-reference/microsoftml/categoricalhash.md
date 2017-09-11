--- 
 
# required metadata 
title: "Machine Learning Categorical HashData Transform" 
description: " Categorical hash transform that can be performed on data before  training a model. " 
keywords: "MicrosoftML, categoricalHash, transform" 
author: "bradsev"
ms.author: "bradsev" 
manager: "jhubbard" 
ms.date: "04/17/2017" 
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
 
 
 
 
 #categoricalHash: Machine Learning Categorical HashData Transform

 Applies to version 1.3.0 of package MicrosoftML.
 
 ##Description
 
Categorical hash transform that can be performed on data before 
training a model.
 
 
 ##Usage

```   
  categoricalHash(vars, hashBits = 16, seed = 314489979, ordered = TRUE,
    invertHash = 0, bag = TRUE, ...)
 
```
 
 ##Arguments

   
  
 ### vars
 A character vector or list of variable names to transform. If named, the names represent the names of new variables to be created. 
  
  
  
 ### hashBits
 An integer specifying the number of bits to hash into.  Must be between 1 and 30, inclusive. The default value is 16. 
  
  
  
 ### seed
 An integer specifying the hashing seed. The default value is 314489979. 
  
  
  
 ### ordered
 `TRUE` to include the position of each term in the  hash. Otherwise, `FALSE`. The default value is `TRUE`. 
  
  
  
 ### invertHash
 An integer specifying the limit on the number of keys  that can be used to generate the slot name. `0` means no invert  hashing; `-1` means no limit. While a zero value gives better  performance, a non-zero value is needed to get meaningful coefficent names. The default value is `0`. 
  
  
  
 ### bag
 `TRUE` to combine multiple indicator vectors into a single  bag vector instead of concatenating them. This is only relevant when the  input is a vector. The default value is `TRUE`. 
  
  
  
 ###  ...
 Additional arguments sent to the compute engine. 
  
 
 
 ##Details
 
`categoricalHash` converts a categorical value into an indicator
array by hashing the value and using the hash as an index in the bag.  If
the input column is a vector, a single indicator bag is returned for it.

`categoricalHash` does not currently support handling factor data.
 
 
 ##Value
 
a `maml` object defining the transform.
 

 


 
 
 ##See Also
 
[rxFastTrees](rxfasttrees.md), [rxFastForest](rxfastforest.md),
[rxNeuralNet](rxneuralnet.md), [rxOneClassSvm](rxoneclasssvm.md),
[rxLogisticRegression](rxlogisticregression.md).
   
 ##Examples

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
  
    
  # Use a categorical hash transform
  outModel2 <- rxLogisticRegression(like~reviewCatHash, data = trainReviews, 
      mlTransforms = list(categoricalHash(vars = c(reviewCatHash = "review"))))
  # Weights are similar to categorical
  summary(outModel2)
  
  # Use the model to score
  scoreOutDF2 <- rxPredict(outModel2, data = testReviews, 
      extraVarsToWrite = "review")
  scoreOutDF2
 
```
 
 
