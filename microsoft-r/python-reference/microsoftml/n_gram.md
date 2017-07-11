--- 
 
# required metadata 
title: "n_gram" 
description: "Extracts NGrams from text and converts them to a vector using dictionary." 
keywords: "N-Grams" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "07/11/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

## *n_gram*: Converts text into features using N-Grams


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.n_gram(ngram_length: numbers.Real = 1, skip_length: numbers.Real = 0, all_lengths: bool = True, max_num_terms: list = [10000000], weighting: str = ‘Tf’)
```




### Description

Extracts NGrams from text and converts them to a vector using dictionary.


### Arguments


##### ngram_length

Ngram length (settings).


##### skip_length

Maximum number of tokens to skip when constructing an ngram (settings).


##### all_lengths

Whether to include all ngram lengths up to ngramLength or only ngramLength (settings).


##### max_num_terms

Maximum number of ngrams to store in the dictionary (settings).


##### weighting

The weighting criteria (settings).


### See also

[n_gram_hash](microsoftml.modules.text_analytics.n_gram_hash.md),
[featurize_text](microsoftml.modules.text_analytics.featurize_text.md)
