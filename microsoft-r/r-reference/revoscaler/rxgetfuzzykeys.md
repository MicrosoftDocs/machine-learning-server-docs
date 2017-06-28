--- 
 
# required metadata 
title: "Fuzzy keys for a character vector" 
description: " EXPERIMENTAL: Get fuzzy keys for a character vector " 
keywords: "RevoScaleR, rxGetFuzzyKeys, manip" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "04/18/2017" 
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
 
 
 #`rxGetFuzzyKeys`: Fuzzy keys for a character vector

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
EXPERIMENTAL: Get fuzzy keys for a character vector
 
 
 ##Usage

```   
  rxGetFuzzyKeys(stringsIn, data = NULL, outFile = NULL, varsToKeep = NULL,
          matchMethod = c("lv", "j", "jw", "bag", "exact","none"),
          keyType = c( "all", "alphanum", "alpha", "mphone3", "soundex", "soundexNum", 
              "soundexAll", "soundexAm", "mphone3Vowels", "mphone3Exact"),
          ignoreCase = FALSE, ignoreSpaces = NULL, ignoreNumbers = TRUE, ignoreWords = NULL,
          minDistance = 0.7, dictionary = NULL, keyVarName = NULL,
          costs = c(insert = 1, delete = 1, subst = 1),
          hasMatchVarName = NULL, matchDistVarName = NULL, numMatchVarName = NULL, noMatchNA = FALSE,
          overwrite = FALSE
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
 `NULL` or character vector of variables from the input 'data' to keep in the output data set. If `NULL`, all variables are kept. Ignored if data is `NULL`. 
  
    
 ### `matchMethod`
 Method for matching to dictionary: 'none' for no matching, 'lv' for Levenshtein; 'j' for Jaro, 'jw' for JaroWinkler,  'bag' for bag of words, 'exact' for exact matching. The default matchMethod is 'lv'. 
  
    
 ### `keyType`
 Transformation type in creating keys: 'all' to retain all characters, 'alphanum' for alphanumeric characters only,  'alpha' for letters only", 'mphone3' for Metaphone3 phonetic transformation, 'soundex' for Soundex phonetic transformation,  'mphone3Vowels' for Metaphone3 encoding more than initial vowels, 'mphone3Exact' for Metaphone3 with more exact consonants, 'soundexNum' for Soundex with numbers only, 'soundexAll' for Soundex not truncated at 4 characters, and 'soundexAm' for the American variant of Soundex. 
  
    
 ### `ignoreCase`
 A logical specifying whether or not to ignore case when comparing strings to the dictionary 
  
    
 ### `ignoreSpaces`
 A logical specifying whether or not to ignore spaces.  If `FALSE`, for phonetic conversions each word in the string is processed separately and then concatenated together. 
  
    
 ### `ignoreNumbers`
 A logical. If `FALSE`, numbers are converted to words before phonetic processing. If `TRUE`, numbers are ignored or removed. 
  
    
 ### `ignoreWords`
 An optional character vector containing words to ignore when matching. 
  
    
 ### `noMatchNA`
 A logical. If `TRUE`, if a match is not found an empty string is returned.  Only applies when dictionary is provided. 
  
    
 ### `minDistance`
 Minimum distance required for a match; applies only to distance metric algorithms (Levenshtein, Jaro, JaroWinkler).   One is a perfect match. Zero is no similarity. 
  
    
 ### `dictionary`
 Character vector containing the dictionary for string comparisons.  Used for distance metric algorithms. from strings before processing. 
  
    
 ### `keyVarName`
 `NULL` or a character string specifying the name to use for the newly created key variable. If `NULL`, the new variable name will be constructed using the `stringsIn` variable name and `.key`. Ignored if `data` is `NULL`. 
  
    
 ### `costs`
 A named numeric vector with names `insert`, `delete`, and  `subst` giving the respective costs for computing the Levenshtein distance. The default uses unit cost for all three. Ignored if Levenshtein distance not being used. 
  
    
 ### `hasMatchVarName`
 `NULL` or a character string specifying the name to use for a logical variable indicating whether word was matched to dictionary or not. If `NULL`, no variable will be created. 
  
    
 ### `matchDistVarName`
 `NULL` or a character string specifying the name to use for a numeric variable containing the distance of the match. If `NULL`, no variable will be created. 
  
    
 ### `numMatchVarName`
 `NULL` or a character string specifying the name to use for an integer variable containing the number of alternative matches were found that satisfied  `minDistance` criterion . If `NULL`, no variable will be created. 
  
    
 ### `overwrite`
 A logical. If `TRUE` and the specified `outFile` exists, it will be overwritten. 
  
 
 
 ##Details
 
Two basic algorithms are provided, Soundex and Metaphone 3, with variations on both of them.  
The `rxGetFuzzyKeys` function can process a character vector of data, a character column 
in a data frame, or a string variable in a RevoScaleR data source.

The simplified Soundex algorithm creates a 4-letter code:  the first letter of the
word as is, followed by 3 numbers representing consonants in the word, or zeros if necessary to fill 
out the four characters. Non-alphabetical characters are ignored. 
The Soundex method is used widely, and is available, for example, on the RootsWeb 
web site [`http://searches.rootsweb.ancestry.com/cgi-bin/Genea/soundex.sh`](http://searches.rootsweb.ancestry.com/cgi-bin/Genea/soundex.sh)
.

American Soundex differs slightly from standard simplified Soundex in its treatment of 'H' and 'W', 
which are treated differently when present between two other consonants.
This is described by the U.S. National Archives [`http://www.archives.gov/research/census/soundex.html`](http://www.archives.gov/research/census/soundex.html)
. 

Another variant of Soundex is to code the first letter in the same way that all letters are coded, 
giving the first letter less importance.  
Using `soundexNums` as the `keyType` provides this variant.

Another popular variant of Soundex is to continue coding all letters rather than stopping at 4.  
Extra zeros are not added to fill out the code, so the result may be less than 4 characters.  For this
variant, use `soundexAll` as the `keyType`.  
Note that in this case, spaces are treated as vowels in separating consonants.

The Metaphone 3 algorithm was developed by Lawrence Philips, who designed and developed the 
original Metaphone algorithm in 1990 and the Double Metaphone algorithm in 2000. Metaphone 3 is reported 
to increase the accuracy of phonetic encoding from the 89
against a database of the most common English words, and names and non-English words 
familiar in North America.

In Metaphone3, all vowels are encoded to the same value - 'A'. If the `mphone3`
is used as the `keyType`, only initial vowels will be encoded at all. If `mphone3Vowels`
is used as the `keyType`, 'A' will be encoded at all places in the word that any vowels are normally
pronounced. 'W' as well as 'Y' are treated as vowels.

The `mphone3Exact` keyType is a variant of Metaphone3 that allows you to encode consonants as exactly as possible.
This does not include 'S' vs. 'Z', since Americans will pronounce 'S' at the
at the end of many words as 'Z', nor does it include 'CH' vs. 'SH'. It does cause
a distinction to be made between 'B' and 'P', 'D' and 'T', 'G' and 'K', and 'V' and 'F'.

In phonetic matching, all non-alphabetic characters are ignored, so that any strings
containing numbers may give misleading results.  This is typically handled by removing
numbers from strings and handling them separately, but occasionally it is convenient
to have the numbers converted to their phonetic equivalent.  This is accomplished
by setting the `ignoreNumbers` parameter to FALSE.

Phonetic matching also typically treats the entire string as a single word. Setting
the `ignoreSpaces` argument to `TRUE` results in each word being processed separately,
with the result concatenated into a single (longer) code.

For information on similarity distance measures, see [rxGetFuzzyDist](rxgetfuzzydist.md).
 
 
 
 ##Value
 
A character vector containing the fuzzy keys or a data source containing the fuzzy keys in a new variable.
 
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 
 ##See Also
 
[rxGetFuzzyDist](rxgetfuzzydist.md),
[rxDataStep](rxdatastep.md)
   
 ##Examples

 ```
   
  cityDictionary <- c("Aberdeen", "Anacortes", "Arlington", "Auburn",
   "Bellevue", "Bellingham", "Bothell", "Bremerton", "Bothell", "Camas",
   "Des Moines", "Edmonds", "Everett", "Federal Way", "Kennewick",
   "Marysville", "Olympia", "Pasco", "Pullman", "Redmond", "Renton", "Sammamish",
   "Seattle", "Shoreline", "Spokane", "Tacoma", "Vancouver", "Yakima")
  
  cityNames <- c("Redmond", "Redmonde", "Edmondse", "REDMND", "EDMOnts")
  
  # The string distance matchMethods require a dictionary
  rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "lv", dictionary = cityDictionary)
  rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "jw", dictionary = cityDictionary)
  
  rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "lv", dictionary = cityDictionary, ignoreCase = TRUE)
  rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "jw", dictionary = cityDictionary, ignoreCase = TRUE)
  
  # Use mphone3 converstion before matching
  rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "lv", keyType = "mphone3",
      dictionary = cityDictionary)
  
  # Compare soundex and American soundex    
  origStr <- c("Ashcroft", "FLASHCARD")
  soundexCode <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "soundex")
  soundexAmCode <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "soundexAm")
  # Show the original string, Soundex, and American Soundex codes
  data.frame(origStr, soundexCode, soundexAmCode)
  
  # Compare Metaphone3 with Metaphone3 with internal vowels
  origStr <- c("Phorensics", "forensics", "Nicholas", "Nicolas", "Nikolas",
       "Knight", "Night", "Stephen", "Steven", "Matthew", "Matt", "Shuan", "Shawn",
       "McDonald", "MacDonald", "Schwarzenegger", "Shwardseneger", "Ellen", "Elena", "Allen")
  
  mphone3Code <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "mphone3")
  mphone3VowelsCode <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "mphone3Vowels")
  data.frame(origStr, mphone3Code, mphone3VowelsCode)
  
  # Compare Metaphone 3 with Metaphone3 with more exact consonants
  origStr <- c("Phorensics", "forensics", "Nicholas", "Nicolas", "Nikolas",
       "Knight", "Night", "Stephen", "Steven", "Matthew", "Matt", "Shuan", "Shawn",
       "McDonald", "MacDonald", "Schwarzenegger", "Shwardseneger", "Ellen", "Elena", "Allen")
  
  mphone3Code <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "mphone3")
  mphone3ExactCode <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "mphone3Exact")
  data.frame(origStr, mphone3Code, mphone3ExactCode)
  
  # Including numbers and spaces
  origStr <- c("10 Main Apt 410", "20 Main Apt 300", "20 Main Apt 302")
  mphone3Code <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "mphone3")
  mphone3NumCode <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "mphone3", ignoreNumbers = FALSE)
  mphone3NumSpCode <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "mphone3", ignoreNumbers = FALSE, ignoreSpaces = FALSE)
  data.frame(origStr, mphone3Code, mphone3NumCode, mphone3NumSpCode)
  
  # Non-phonetic key types
  origStr <- c("10 Main St", "Main St. ", "15 Second  Ave.", "Second 15 Ave.")
  alphaCode <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "alpha")
  alphaNumCode <- rxGetFuzzyKeys(stringsIn = origStr, keyType = "alphaNum")
  data.frame(origStr, alphaCode, alphaNumCode)
  
  # Use minDistance to correct data
  # Create a small city dictionary
  cityDictionary <- c("Seattle", "Olympia", "Spokane")
  
  # Create a vector of city names to correct
  cityNames <- c("Seattle", "Seattlee", "Olympa", "SPOKANE", "OLYmpia")
  
  # Large minimum distance should result in no change
  cityKey1 <- rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "lv", dictionary = cityDictionary, minDistance = .9)
  # Print results to console
  cityKey1
  
  # Smaller minimum distance should fix matched cases, but not mixed case issues
  cityKey2 <- rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "lv", dictionary = cityDictionary, minDistance = .8)
  # Print results to console
  cityKey2
  
  # Small minimum distance should fix everything, even mixed case issues
  cityKey3 <- rxGetFuzzyKeys(stringsIn = cityNames, matchMethod = "lv", dictionary = cityDictionary, minDistance = .1)
  # Print results to console
  cityKey3
  
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
      
  outDataSource <- rxGetFuzzyKeys(stringsIn = "institution",
      data = tempInFile, outFile = tempOutFile,
      dictionary = uDictionary,
      ignoreWords = c("University", "Univ", 
  		"of", "U"),
      ignoreCase = TRUE,
      matchMethod = "bag",  
      ignoreSpaces = FALSE,  
      minDistance = .4, 
      keyType = "mphone3", overwrite = TRUE)     
   rxGetVarInfo(outDataSource) 
   # Read the new data set back into memory
   newData <- rxDataStep(outDataSource)
   
   # See the 'cleaned-up' institution names in the new institution.key variable
   # Note that one input, Seattle Inst Tech, was not cleaned
   newData 
   
   # Clean-up
   file.remove(tempInFile)
   file.remove(tempOutFile) 
 
```
 
 
 
 
