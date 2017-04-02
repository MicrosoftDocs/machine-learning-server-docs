--- 
 
# required metadata 
title: "Fuzzy distances for a character vector" 
description: " EXPERIMENTAL: Get fuzzy distances for a character vector " 
keywords: "RevoScaleR, rxGetFuzzyDist, manip" 
author: "richcalaway" 
manager: "jhubbard" 
ms.date: "03/23/2017" 
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
 
 
 #`rxGetFuzzyDist`: Fuzzy distances for a character vector

 Applies to version {PRODUCT_VERSION} of package RevoScaleR.
 
 ##Description
 
EXPERIMENTAL: Get fuzzy distances for a character vector
 
 
 ##Usage

```   
  rxGetFuzzyDist(stringsIn, data = NULL, outFile = NULL, varsToKeep = NULL,
          matchMethod = c("lv", "j", "jw", "exact","none"), 
          keyType = c( "all", "alphanum", "alpha", "mphone3", "soundex", "soundexNum", 
              "soundexAll", "soundexAm", "mphone3Vowels", "mphone3Exact"),
          ignoreCase = FALSE, ignoreSpaces = NULL, ignoreNumbers = TRUE, ignoreWords = NULL, 
          minDistance = 0.7, dictionary = NULL, noMatchNA = FALSE,
          returnString = TRUE, returnIndex = NULL, overwrite = FALSE
          )
 
```
 
 ##Arguments

   
    
 ### `stringsIn`
 Character vector of strings to process or name of character variable. 
  
    
 ### `data`
 `NULL` or data frame or RevoScaleR data source containing the variable to process. 
  
    
 ### `outFile`
 `NULL` or RevoScaleR data source in which to put output. 
  
    
 ### `varsToKeep`
 `NULL` or character vector of variables from the input 'data' to keep in the output data set. They will be replicated for multiple matches. 
  
    
 ### `matchMethod`
 Method for matching to dictionary: 'none' for no matching, 'lv' for Levenshtein; 'j' for Jaro, 'jw' for JaroWinkler,  'exact' for exact matching 
  
  
 ### `keyType`
 Transformation type in creating keys to be matched: 'all' to retain all characters, 'alphanum' for alphanumeric characters only,  'alpha' for letters only", 'mphone3' for Metaphone3 phonetic transformation, 'soundex' for Soundex phonetic transformation,  'mphone3Vowels' for Metaphone3 encoding more than intial vowels, 'mphone3Exact' for Metaphone3 with more exact consonants, 'soundexNum' for Soundex with numbers only, 'soundexAll' for Soundex not truncated at 4 characters, and 'soundexAm' for the American variant of Soundex. 
  
    
 ### `ignoreCase`
 A logical specifying whether or not to ignore case when comparing strings to the dictionary 
  
    
 ### `ignoreSpaces`
 A logical specifying whether or not to ignore spaces.  If `FALSE`, for phonetic conversions each word in the string is processed separately and then concatenated together. 
  
    
 ### `ignoreNumbers`
 A logical. If `FALSE`, numbers are converted to words before phonetic processing.  If `TRUE`, numbers are ignored or removed 
  
    
 ### `ignoreWords`
 An optional character vector containing words to ignore when matching. 
  
    
 ### `noMatchNA`
 A logical. If `TRUE`, if a match is not found an empty string is returned.   Only applies when dictionary is provided. 
  
    
 ### `minDistance`
 Minimum distance required for a match; applies only to distance metric algorithms (Levenshtein, Jaro, JaroWinkler).  One is a perfect match. Zero is no similarity. 
  
    
 ### `dictionary`
 Character vector containing the dictionary for string comparisons.   from strings before processing. 
  
    
 ### `returnString`
 A logical specifying whether to return the string and dictionary values 
  
    
 ### `returnIndex`
 `NULL` or logical specifying whether to return the string and dictionary indexes. If `NULL`, it is set to `!returnString` 
  .
    
 ### `overwrite`
 A logical. If `TRUE` and the specified `outFile` exists, it will be overwritten. 
  
 
 
 ##Details
 
Four similarity distance measures are provided: Levenshtein, Jaro, Jaro-Winkler, and Bag of Words.

The Levenshtein edit-distance algorithm computes the least number of edit operations 
(number of insertions, deletions, and substitutions) that are 
necessary to modify one string to obtain another string.  

The Jaro distance measure considers
the number of matching characters and transpositions. Jaro-Winkler adds a prefix-weighting,
giving higher match values to strings where prefixes match. 

The Bag of Words measure looks at the number of matching words in a phrase, independent of order. 
In the implementation used in `rxGetFuzzyDist`, all measures are 
normalized to the same scale, where 0 is no match and 1 is a perfect match.  

For information on phonetic conversions, see [rxGetFuzzyKeys](rxGetFuzzyKeys.md).
 
 
 
 ##Value
 
A data frame or data source containing the distances and either string and dictionary values or indexes. 
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxGetFuzzyKeys](rxGetFuzzyKeys.md),
[rxDataStep](rxDataStep.md)
   
 ##Examples

 ```
   
  # Basic examples
  myWords <- c("Allen", "Ellen", "Elena")
  myDictionary <- c("Allen", "Helen", "Harry")
  rxGetFuzzyDist(stringsIn = myWords, dictionary = myDictionary, minDistance = .5, returnString = FALSE)
  
  rxGetFuzzyDist(stringsIn = c("Univ of Ontario"), 
    dictionary = c("Univ of Michgan", "Univ of Ontario", "Univ Michigan", 
      "Michigan State", "Ontario Univ of"),
    matchMethod = "bag", ignoreWords = c("of"), minDistance = 0)
  
  rxGetFuzzyDist(stringsIn = c("Univ of Ontario"), 
    dictionary = c("Univ of Michgan", "Univ of Ontario", "Univ Michigan", 
      "Michigan State", "Ontario Univ of"),
    matchMethod = "bag",  keyType = "soundex", ignoreSpaces = FALSE,  minDistance = 0)
    
  ### The Jaro Measure
  # To use the Jaro edit-disance, set the `matchType` to `j`.
  # Create a small city dictionary
  cityDictionary <- c("Portland", "Eugene", "Corvalis")
  
  # Create a vector of city names to correct
  cityNames <- c("Portland", "Portlande", "Eugen", "CORVALIS", "COrval")
  
  # Large minimum distance should result in little change: final e's are dropped
  cityKey1 <- rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "j", dictionary = cityDictionary, minDistance = .9)
  # Print results to console
  cityKey1
  
  # Smaller minimum distance should fixes more
  cityKey2 <- rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "j", dictionary = cityDictionary, minDistance = .8)
  # Print results to console
  cityKey2
  
  # Small minimum distance should fix everything
  cityKey3 <- rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "j", dictionary = cityDictionary, minDistance = .1)
  # Print results to console
  cityKey3
  
  # Examine all of the distances greater than .1
  distOut1 <- rxGetFuzzyDist(stringsIn = cityNames, matchMethod = "j", dictionary = cityDictionary, minDistance = .1)
  distOut1
  
  # Use the ignoreCase argument: CORVALIS and Corvalis will now be a perfect match
  distOut2 <- rxGetFuzzyDist(stringsIn = cityNames, matchMethod = "j", dictionary = cityDictionary, 
      ignoreCase = TRUE, minDistance = .1)
  distOut2
  
  ### The Jaro-Winkler matchMethod
  # Create a small city dictionary
  cityDictionary <- c("Portland", "Eugene", "Corvalis")
  # Create a vector of city names to correct
  cityNames <- c("Portland", "Portlandia", "Eugenie", "Eugener", "Corvaliser", "Corving")
  # Examine all of the distances greater than .5 for Jaro-Winkler
  distOut1 <- rxGetFuzzyDist(stringsIn = cityNames, matchMethod = "jw", dictionary = cityDictionary, minDistance = .5)
  distOut1
  
  # Compare with Jaro, which does not emphasize the matching prefix
  distOut2 <- rxGetFuzzyDist(stringsIn = cityNames, matchMethod = "j", dictionary = cityDictionary, minDistance = .5)
  distOut2
  
  ### The Bag-of-Words matchMethod
  
  institutionDictionary = c("University of Maryland", "Michigan State University", "University of Michigan", 
      "University of Washington", "Washington State University" )
      
  origCol1 <- c("Univ of Maryland", "Michigan State", "Michigan State University", "Michigan University of",
      "WASHINGTON UNIVERSITY OF", "Washington State Univ")
        
  distOut <- rxGetFuzzyDist(stringsIn = origCol1, 
      dictionary = institutionDictionary, matchMethod = "bag", minDistance = .2, 
      ignoreCase = TRUE )
  distOut
  
  distOut <- rxGetFuzzyDist(stringsIn = origCol1, 
      dictionary = institutionDictionary, matchMethod = "bag", minDistance = .2, 
      ignoreWords = c("University", "Univ"), ignoreCase = TRUE )
  distOut
  
  ### Using Distance Measures with Phonetic Transformations
  # Distance measures can also be used with phonetic transformations of strings.  For example,
  # we can first convert the strings to Soundex, then perform the distance measure.  In both
  # cases shown, we will see a perfect match between `Univ of Ontario` and `Unive of Ontaro`
  # when we convert all of the words to Soundex.
  
  uDictionary = c("Univ of Michgan", "Univ of Ontario", "Univ of Oklahoma", "Michigan State")
  
  # Use a bag of words distance measure with a soundex conversion
  outDist1 <- rxGetFuzzyDist(stringsIn = c("Unive of Ontaro"), 
        dictionary = uDictionary,
        matchMethod = "bag",  keyType = "soundex", ignoreSpaces = FALSE,  minDistance = 0) 
  outDist1
        
  # Now use a Levenshtein distance measure with a soundex conversion      
  outDist2 <- rxGetFuzzyDist(stringsIn = c("Unive of Ontaro"), 
        dictionary = uDictionary,
        matchMethod = "lv",  keyType = "soundex", ignoreSpaces = FALSE,  minDistance = 0) 
  outDist2  
  
  ## Use with xdf file
  # Create temporary xdf file
  inData <- data.frame(institution = c("Seattle Pacific U", "SEATTLE UNIV", "Seattle Central",
  	"U Washingtn", "UNIV WASH BOTHELL", "Puget Sound Univ",
  	"ANTIOC U SEATTLE", "North Seattle Univ", "North Seatle U",
  	"Seattle Inst Tech", "SeATLE College Nrth", "UNIV waSHINGTON",
  	"University Seattle", "Seattle Antioch U",
  	"Technology Institute Seattle", "Pgt Sound U",
  	"Seattle Central Univ", "Univ North Seattle",
  	"Bothell - Univ of Wash", "ANTIOCH SEATTLE"), stringsAsFactors = FALSE)
  
  tempInFile <- tempfile(pattern = "fuzzyDistIn", fileext = ".xdf")
  rxDataStep(inData = inData, outFile = tempInFile, rowsPerRead = 10)
  
  uDictionary <- c("Seattle Pacific University", "Seattle University",
  	"University of Washington", "Seattle Central College",
  	"University of Washington, Bothell", "Puget Sound University",
  	"Antioch University, Seattle", "North Seattle College",
  	"Seattle Institute of Technology")
  
  tempOutFile <- tempfile(pattern = "fuzzyDistOut", fileext = ".xdf")
      
  outDataSource <- rxGetFuzzyDist(stringsIn = "institution",
      data = tempInFile, outFile = tempOutFile,
      dictionary = uDictionary,
      ignoreWords = c("University", "Univ", "U","of"),
      ignoreCase = TRUE,
      matchMethod = "bag",  
      ignoreSpaces = FALSE,  
      minDistance = .5, 
      keyType = "mphone3", overwrite = TRUE)     
   rxGetVarInfo(outDataSource) 
   # Read the new data set back into memory
   newData <- rxDataStep(outDataSource)
   oldWidth <- options(width = 120)
   newData
   options(oldWidth) 
   # Clean-up
   file.remove(tempInFile)
   file.remove(tempOutFile)         
 
```
 
 
 
 
