--- 
 
# required metadata 
title: "Machine Learning Feature Extractors" 
description: " Feature Extractors that can be used with mtText. " 
keywords: ", ngram, ngramCount, ngramHash, transform" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "03/13/2017" 
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
 
 
 
 
 
 
 #`ngram`: Machine Learning Feature Extractors 
 ##Description
 
Feature Extractors that can be used with mtText.
 
 
 ##Usage

```   
  ngramCount(maxNumTerms = 1e+07, weighting = "tf")
  
  ngramHash(hashBits = 16, seed = 314489979, ordered = TRUE,
    invertHash = 0)
 
```
 
 ##Arguments

   
  
 ### `maxNumTerms`
 An integer that specifies the maximum number of categories  to include in the dictionary. The default value is 10000000. 
  
  
  
 ### `weighting`
 A character string that specifies the weighting criteria:  
*   `"tf"`: to use term frequency.    
*   `"idf"`: to use inverse document frequency.   
*   `"tfidf"`: to use both term frequency and inverse document   frequency.   
 
  
  
  
 ### `hashBits`
 integer value. Number of bits to hash into. Must be between 1 and 30, inclusive. 
  
  
  
 ### `seed`
 integer value. Hashing seed. 
  
  
  
 ### `ordered`
 `TRUE` to include the position of each term in the  hash. Otherwise, `FALSE`. The default value is `TRUE`. 
  
  
  
 ### `invertHash`
 An integer specifying the limit on the number of keys  that can be used to generate the slot name. `0` means no invert  hashing; `-1` means no limit. While a zero value gives better  performance, a non-zero value is needed to get meaningful coefficent names. 
  
 
 
 ##Details
 
`ngramCount` allows defining arguments for count-based feature
extraction. It accepts two options: `maxNumTerms` and `weighting`.

`ngramHash` allows defining arguments for hashing-based feature
extraction.  It accepts the following options: `hashBits`, `seed`,
`ordered` and `invertHash`.
 
 
 ##Value
 
A character string defining the transform.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##See Also
 
[featurizeText](featurizeText.md).
   
 ##Examples

 ```
   
   myData <- data.frame(opinion = c(
      "I love it!",
      "I love it!",
      "Love it!",
      "I love it a lot!",
      "Really love it!",
      "I hate it",
      "I hate it",
      "I hate it.",
      "Hate it",
      "Hate"),
      like = rep(c(TRUE, FALSE), each = 5),
      stringsAsFactors = FALSE)
      
  outModel1 <- rxLogisticRegression(like~opinionCount, data = myData, 
      mlTransforms = list(featurizeText(vars = c(opinionCount = "opinion"), 
          featureExtractor = ngramHash(invertHash = -1, hashBits = 3)))) 
  summary(outModel1)   
         
  outModel2 <- rxLogisticRegression(like~opinionCount, data = myData, 
      mlTransforms = list(featurizeText(vars = c(opinionCount = "opinion"), 
          featureExtractor = ngramCount(maxNumTerms = 5, weighting = "tf"))))         
  summary(outModel2)
 
```
 
 
 
