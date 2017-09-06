--- 
 
# required metadata 
title: "n_gram_hash: n_gram_hash" 
description: "Extracts NGrams from text and convert them to vector using hashing trick." 
keywords: "N-Grams, hash" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "09/05/2017" 
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

# *microsoftml.n_gram_hash*: Converts text into features using hashed n-grams


**Applies to: SQL Server 2017 RC2**


## Usage



```
microsoftml.n_gram_hash(hash_bits: numbers.Real = 16,
    ngram_length: numbers.Real = 1, skip_length: numbers.Real = 0,
    all_lengths: bool = True, seed: numbers.Real = 314489979,
    ordered: bool = True, invert_hash: numbers.Real = 0)
```





## Description

Extracts NGrams from text and convert them to vector using hashing trick.


## Arguments


### hash_bits

Number of bits to hash into. Must be between 1 and 30, inclusive. (settings).


### ngram_length

Ngram length (settings).


### skip_length

Maximum number of tokens to skip when constructing an ngram (settings).


### all_lengths

Whether to include all ngram lengths up to ngramLength or only ngramLength (settings).


### seed

Hashing seed (settings).


### ordered

Whether the position of each source column should be included in the hash (when there are multiple source columns). (settings).


### invert_hash

Limit the number of keys used to generate the slot name to this many. 0 means no invert hashing, -1 means no limit. (settings).


## See also

[n_gram](microsoftml.modules.text-analytics.n-gram.md),
[featurize_text](microsoftml.modules.text-analytics.featurize-text.md)
