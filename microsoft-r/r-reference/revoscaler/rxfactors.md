--- 
 
# required metadata 
title: "rxFactors function (RevoScaleR) " 
description: " Recodes a factor variable by mapping one set of factor levels and indices to a new set. Can also be used to convert non-factor variable into a factor. " 
keywords: "(RevoScaleR), rxFactors, manip, category" 
author: "heidisteen" 
manager: "jhubbard" 
ms.date: "09/07/2017" 
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
 
 
 #rxFactors:  Factor Variable Recoding  
 ##Description
 
Recodes a factor variable by mapping one set of factor levels and indices to a new set.
Can also be used to convert non-factor variable into a factor.
 
 
 
 ##Usage

```   
  rxFactors(inData, factorInfo, sortLevels = FALSE, otherLevel = NULL,
            outFile = NULL, varsToKeep = NULL, varsToDrop = NULL,
            overwrite = FALSE, maxRowsByCols = NULL, 
            blocksPerRead = rxGetOption("blocksPerRead"),
            reportProgress = rxGetOption("reportProgress"), verbose = 0,
            xdfCompressionLevel = rxGetOption("xdfCompressionLevel"), ...)
 
```
 
 
 ##Arguments

   
    
 ### `inData`
 either an RxXdfData object, a character string specifying the .xdf file, or a data frame. 
  
  
    
 ### `factorInfo`
 character vector of variable names,a list of named variable information lists, or empty or `NULL`.  If `sortLevels` is set to `TRUE`, the levels of the variables named in the character vector will  all be sorted; if `sortLevels` is `TRUE` and `factorInfo` is empty or `NULL`, all   factors will be sorted.    If a `factorInfo` list is provided, each variable information list contains one or more of the   named elements given below.  
*   Currently available properties for a column information list are:   
* `levels` - optional vector, containing values to match in converting non-factor data to factor levels. If `levels = NULL`, all of the unique values in the data are converted to levels. in the order encountered. However, the user can  override this behavior and sort the resulting levels alphabetically by setting `sortLevels = TRUE`.   The user may also specify a subset of the data to convert to levels. In this case, if `otherLevel = NULL`,  all data values *not* found in the `levels` subset will be converted to missing (`NA`) values. For example, if a variable `x` is comprised of integer data `1`, `2`, `3`, `4`, `5`, then  
* `` - `factorInfo = list(x = list(levels = 2:4, otherLevel = NULL))`  
 will convert `x` into a factor with data `NA`, `"2"`, `"3"`, `"4"`, `NA` with levels `"2"`,`"3"`,`"4"`.  Alternatively, the user may wish to place all of those unspecified values into a single category, say `"other"`. In that case, use `otherLevel = "other"` along with the subset `levels` specification.  Note that the `levels` vector may be any type, e.g., 'integer', 'numeric', 'character'. However, behind the scenes,  it is always converted to type 'character', as are the data values being converted. The resulting strings are matched  with those of the data to populate the categories.     
* `otherLevel` - character string defining the level to assign to all factor values that  are not listed in the `newLevels` field, if `newLevels` is specified. If `otherLevel = NULL`,  the default, the factor levels that are not listed in `newLevels` will be left unchanged and in their original  order. If specified, the value set here overrides the default argument of the same in the primary argument list.   
* `sortLevels` - logical scalar. If `TRUE`, the resulting levels will be sorted alphabetically. If the input variable is not a factor and levels are not specified, this will be ignored and levels will be in the order in which they are encountred.   
* `varName` - character string defining the name of an existing data variable to recode. If this field is left unspecified, then the name of the corresponding list element in `factorInfo` will be used. For example, all of the following are acceptable and equivalent `factorInfo` specifications for alphabetically  sorting the levels of an existing factor variable named `"myFacVar"`:  
* `` - `factorInfo = list( myFacVar = list( sortLevels = TRUE ) )`  
* `` - `factorInfo = list( list( sortLevels = TRUE, varName = "myFacVar" ) )`  
* `` - `factorInfo = list( myFacVar = list( sortLevels = TRUE, varName = "myFacVar" ) )`  
  However, if you wish to rename a variable after conversion (keeping the old variable in tact),  there is only one acceptable format: the variable to be recoded must appear in the `varName` field while the new variable name for the converted data must appear as the name of the corresponding list element. For example, to sort the levels of an existing factor variable `"myFacVar"` and store the result in a new variable `"myNewVar"`, you would issue:  
* `` - `factorInfo = list( myNewFacVar = list( sortLevels = TRUE, varName = "myFacVar" ) )`  
    
* `newLevels` - a character vector or list, possibly with named elements, used to rename the levels of a factor. While `levels` provides a means of filtering the data the user wishes to import and convert to factor levels, `newLevels` is used to alter converted or existing levels by renaming, collapsing, or sorting them. See the Examples section below for typical use cases of `newLevels`.  
* `description` - character string defining a description for the recoded variable.  
 
  
  
  
    
 ### `sortLevels`
 the default value to use for the `sortLevels` field in the `factorInfo` list. 
  
  
    
 ### `otherLevel`
 the default value to use for the `otherLevel` field in the `factorInfo` list. 
  
  
    
 ### `outFile`
 either an RxXdfData object, a character string specifying the .xdf file, or `NULL`. If `outFile = NULL`, a data frame is returned. When writing to HDFS, `outFile` must be an `RxXdfData` object representing a new composite XDF. 
  
  
    
 ### `varsToKeep`
 character vector of variable names to include in the data file. If `NULL`, argument is ignored. Cannot be used with `varsToDrop`. 
  
  
    
 ### `varsToDrop`
 character vector of variable names to not include in the data file. If `NULL`, argument is ignored. Cannot be used with `varsToKeep`. 
  
  
    
 ### `overwrite`
 logical value. If `TRUE`, an existing `outFile` will be overwritten. Ignored if a dataframe is returned. 
  
  
    
 ### `maxRowsByCols`
 this argument is used only when  `inData` is referring to an .xdf file  (character string defining a path to an existing .xdf file or an RxXdfData object) and we wish to return the output as a data frame (`outFile = NULL`).   In this case, and behind the scenes, the output is written to a temporary .xdf file and `rxDataStep` is subsequently called to convert the output into a data frame. The `maxRowsByCols` argument is passed directly in the `rxDataStep` call, giving the user some control over the conversion. See [rxDataStep](rxDataStep.md) for more details on the `maxRowsByCols` argument. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. If the `data` and `outFile` are the same file, blocksPerRead must be 1. 
  
  
    
 ### `reportProgress`
 integer value with options:  
   *   `0`: no progress is reported. 
   *   `1`: the number of processed rows is printed and updated. 
   *   `2`: rows processed and timings are reported. 
   *   `3`: rows processed and all timings are reported. 
  
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 
  
  
    
 ### `xdfCompressionLevel`
 integer in the range of -1 to 9.  The higher the value, the greater the  amount of compression - resulting in smaller files but a longer time to create them. If  `xdfCompressionLevel` is set to 0, there will be no compression and files will be compatible  with the 6.0 release of Revolution R Enterprise.  If set to -1, a default level of compression  will be used. 
   
  
    
 ### ` ...`
 additional arguments to be passed directly to the Microsoft R Services Compute Engine. 
  
 
 
 
 
 ##Details
 
Factors are variables that represent categories. An example is a variable named `"state"` 
whose values are the levels `"Alabama"`, `"Alaska"`, ..., `"Wyoming"`. 
There are two parts to a factor variable: 


1 
 a vector of N (number of observations) integer indexes
with values in the range of `1:K`, where K is the number of categories.

1 
 a vector of K strings
(characters) that are used when the vector is displayed and in some other situations.


For instance, when state levels are alphabetical, all observations for which `state == "Alabama"` 
will have the index `1`,  `state == "Washington"` values correspond to index `47`,
and so on.

Recoding a factor means changing from one set of indices to another. For instance, if the levels
for `"state"` are currently arranged in the order in which they were encountered when importing a .csv
file, and it is desired to put them in alphabetical order, then it is necessary to change the index for every
observation.

If numeric data is converted to a factor, a maximum precision of 6 is used. So, for example,
the values 7.123456 and 7.12346 would be placed in the same category.

To recode a categorical or factor variable into a continuous variable within a 
formula use `N()`. To recode continuous variable to a categorical or factor 
variable within a formula use `F()`. See [rxFormula](rxFormula.md). 

To rename the levels of a factor variable in an .xdf file (without change the levels
themselves), use [rxSetVarInfoXdf](rxSetVarInfoXdf.md).

 
 
 
 ##Value
 
if `outFile` is `NULL`, then a data frame is returned. Otherwise, the results
are written to the specified `outFile` file and an RxXdfData object is returned 
*invisibly* corresponding to the output file.
 
 

 
 
 
 
 ##See Also
 
[rxFormula](rxFormula.md),
[rxSetVarInfoXdf](rxSetVarInfoXdf.md),
[rxImport](rxImport.md),
[rxDataStep](rxDataStep.md).
   
 
 ##Examples

 ```
   
  ###
  # Example 1: Recoding levels in alphabetical order
  ###
  
  # Use the 'warpbreaks' data frame found in the 'datasets' package
  # Note that the 'tension' factor variable has levels that are not
  # alphabetically ordered.
  rxGetVarInfo( warpbreaks )
  
  # Reorder all factor levels that are not in alphabetical order
  recodedDF1 <- rxFactors(inData = warpbreaks, sortLevels = TRUE)
  rxGetVarInfo( recodedDF1 )
  
  # Specify that only 'tension' levels should be reordered alphabetically
  recodedDF2 <- rxFactors(inData = warpbreaks, sortLevels = TRUE,
  	factorInfo = c("tension"))
  rxGetVarInfo( recodedDF2 )
  
  # Specify that only 'tension' levels should be reordered alphabetically using a list
  recodedDF3 <- rxFactors(inData = warpbreaks, 
                         factorInfo = list(tension = list(sortLevels = TRUE)))
  rxGetVarInfo( recodedDF3 )
                          
  # write data frame to .xdf file and perform similar recoding
  # but write the recoded factor to a new variable. Compare the
  # original with the recoded factor.
  inXDF <- file.path(tempdir(), "warpbreaks.xdf")
  outXDF <- file.path(tempdir(), "warpbreaksRecoded.xdf")
  rxDataStep(warpbreaks, outFile = inXDF, overwrite = TRUE)
  outDS <- rxFactors(inData = inXDF, outFile = outXDF, overwrite = TRUE,
            factorInfo = list(recodedTension = list(sortLevels = TRUE, 
                                                    varName = "tension")))
  DF <- rxDataStep(outDS)
  rxGetVarInfo( DF )
  
  # clean up
  if (file.exists(inXDF)) unlink(inXDF)
  if (file.exists(outXDF)) unlink(outXDF)
  
  ###
  # Example 2: Recoding levels and indexes, saving recoding to a new factor variable
  ###
  
  # Create an .xdf file with a factor variable named 'sex' with levels 'M and 'F'
  set.seed(100)
  sex <- factor(sample(c("M","F"), size = 10, replace = TRUE), levels = c("M", "F"))
  DF <- data.frame(sex = sex, score = rnorm(10))
  DF[["sex"]]
  XDF <- file.path(tempdir(), "sex.xdf")
  XDF2 <- file.path(tempdir(), "newSex.xdf")
  rxDataStep(DF, outFile = XDF, overwrite = TRUE)
  
  # Assume that we change our minds and now wish to 
  # rename the levels to "Female" and "Male"
  # Let us do the recoding and store the result into a new
  # variable named "Gender" keeping the old variable in place.
  outDS <- rxFactors(inData = XDF, outFile = XDF2, overwrite = TRUE,
            factorInfo = list(Gender = list(newLevels = c(Female = "F", Male = "M"), 
                                            varName = "sex")))
  newDF <- rxDataStep(outDS)
  print(newDF)
  
  # clean up
  if (file.exists(XDF)) unlink(XDF)
  if (file.exists(XDF)) unlink(XDF2)
  
  ###
  # Example 3: Combining subsets of factor levels into single levels
  ###
  
  # Create a data set that contains a factor variable 'Month'
  # Note that the levels are not in alphabetical order.
  set.seed(100)
  DF <- data.frame(Month = factor(sample(month.name, size = 20, replace = TRUE),
                                  levels = rev(month.name)))
  
  # Recode the months into quarters and store result into new variable named "Quarter"
  recodedDF <- rxFactors(inData = DF, 
      factorInfo = list(Quarter = list(newLevels = list(Q1 = month.name[1:3], 
                                                        Q2 = month.name[4:6],
                                                        Q3 = month.name[7:9], 
                                                        Q4 = month.name[10:12]),
                                       varName = "Month")))
  
  head(recodedDF)
  recodedDF$Quarter
  
  ###
  # Example 4: Coding and recoding combinations using a single factorInfo list
  ###
  
  set.seed(100)
  size <- 10
  months <- factor(sample(month.name, size = size, replace = TRUE), levels = rev(month.name))
  states <- factor(sample(state.name, size = size, replace = TRUE), levels = state.name)
  animalFarm <- c("cow","horse","pig","goat","chicken", "dog", "cat")
  animals <- factor(sample(animalFarm, size = size, replace = TRUE), levels = animalFarm)
  values <- sample.int(100, size = size, replace = TRUE)
  dblValues <- c(1, 2.1, 3.12, 4.123, 5.1234, 6.12345, 7.123456, 7.12346, 81234.56789, 91234567.8)
  DF <- data.frame(Month = months, State = states, Animal = animals, VarInt = values,
                   VarDbl = dblValues, NotUsed1 = seq(size), NotUsed2 = rev(seq(size)))
  
  factorInfo <- list(
      
      # Convert months to quarters
      Quarter = list(newLevels = list(Q1 = month.name[1:3], Q2 = month.name[4:6],
                                      Q3 = month.name[7:9], Q4 = month.name[10:12]),
                     varName = "Month"),
      
      # Sort animal levels
      Animal = list(sortLevels = TRUE),
      
      # Convert integer data to factor and do not sort levels
      VarIntFac = list(varName = "VarInt", sortLevels = FALSE),
      
      # Convert double data to factor; it will use a precision up to 6                        
      VarDblFac = list(varName = "VarDbl"),
      
      # In-place arbitrary grouping of state names using indexMap
      StateSide = list(newLevels = c(LeftState = "1", RightState = "2"), 
                        indexMap = c(rep(1, 25), rep(2, 25)),
                        varName = "State")
      )
  
  rxFactors(DF, factorInfo)
  
  ###
  # Example 5: Using 'newLevels' to rename, reorder, or collapse existing factor levels.
  #            All of these examples make use of the iris data set, which contains levels
  #            "setosa", "versicolor", and "virginica", in that order.
  ###
  
  # Renaming factor levels:
  #
  #   "setosa" to "Seto"
  #   "versicolor" to "Vers"
  #   "virginica" to "Virg"
  newLevels <- list(Seto = "setosa", Vers = "versicolor", Virg = "virginica")
  rxFactors(iris, factorInfo = list(Species = list(newLevels = newLevels)))$Species
  
  # Reordering:
  newLevels <- c("versicolor", "setosa", "virginica")
  rxFactors(iris, factorInfo = list(Species = list(newLevels = newLevels)))$Species
  
  # Collapsing: order does matter here, so the resulting order of the levels will 
  # be "V" then "S". The 'sortLevels' argument is a quick means of alphabetically
  # sorting the resultant level names.
  newLevels <- list(V = "setosa", S = c("versicolor", "virginica"))
  rxFactors(iris, factorInfo = list(Species = list(newLevels = newLevels)))$Species
  rxFactors(iris, factorInfo = list(Species = list(newLevels = newLevels, sortLevels = TRUE)))$Species
  
  # Subset collapsing with renaming: accomplish with the use of 'otherLevel'
  newLevels <- list(S = "setosa")
  rxFactors(iris, factorInfo = list(Species = list(newLevels = newLevels, otherLevel = "otherSpecies")))$Species
  
  # Superset specification: adding new species for a future study
  newLevels <- c("setosa", "versicolor", "virginica", "pumila", "narbuti", "camillae")
  rxFactors(iris, factorInfo = list(Species = list(newLevels = newLevels)))$Species
 
```
 
 
 
 
