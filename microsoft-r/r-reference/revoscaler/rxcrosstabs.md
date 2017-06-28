--- 
 
# required metadata 
title: "Cross Tabulation" 
description: " Use `rxCrossTabs` to create contingency tables from cross- classifying factors using a formula interface. It performs equivalent computations to the [rxCube](rxcube.md) function, but returns its results in a different way. " 
keywords: "RevoScaleR, rxCrossTabs, print.rxCrossTabs, summary.rxCrossTabs, mean.rxCrossTabs, as.list.rxCrossTabs, category, models" 
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
 
 
 
 
 
 
 #`rxCrossTabs`: Cross Tabulation

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Use `rxCrossTabs` to create contingency tables from cross-
classifying
factors using a formula interface. It performs equivalent computations to the
[rxCube](rxcube.md) function, but returns its results in a different way.
 
 
 ##Usage

```   
  rxCrossTabs(formula, data, pweights = NULL, fweights = NULL, means = FALSE,
              marginals = FALSE, cube = FALSE, rowSelection = NULL,
              transforms = NULL, transformObjects = NULL,
              transformFunc = NULL, transformVars = NULL, 
              transformPackages = NULL, transformEnvir = NULL, 
              useSparseCube = rxGetOption("useSparseCube"),            
              removeZeroCounts = useSparseCube, returnXtabs = FALSE, na.rm = FALSE,
              blocksPerRead = rxGetOption("blocksPerRead"),
              reportProgress = rxGetOption("reportProgress"), verbose = 0,
              computeContext = rxGetOption("computeContext"), ...)
  
 ## S3 method for class `rxCrossTabs':
print  (x, output, header = TRUE, marginals = FALSE,
        na.rm = FALSE, ...)
  
 ## S3 method for class `rxCrossTabs':
summary  (object, output, type = "%", na.rm = FALSE, ...)
  
 ## S3 method for class `rxCrossTabs':
as.list  (x, output, marginals = FALSE, na.rm = FALSE, ...)
  
 ## S3 method for class `rxCrossTabs':
mean  (x, marginals = TRUE, na.rm = FALSE, ...)
 
```
 
 
 ##Arguments

   
  
    
 ### `formula`
 formula as described in [rxFormula](rxformula.md) with the categorical cross-classifying variables (separated by `:`) on the right hand side. 
  
  
    
 ### `data`
 either a data source object, a character string specifying a .xdf file, or a data frame object containing the cross-classifying variables. 
  
  
    
 ### `pweights`
 character string specifying the variable to use as probability weights for the observations. 
  
  
    
 ### `fweights`
 character string specifying the variable to use as frequency weights for the observations. 
  
  
    
 ### `means`
 logical value. If `TRUE`, the mean values of the contingency table are also stored in the output object along with the sums and counts. By default, if the mean values are stored, the `print` and `summary` methods display them. However, the `output` argument in those methods can be used to override this behavior by setting `output` equal to `"sums"` or `"counts"`. 
  
  
    
 ### `marginals`
 logical value. If `TRUE`, a list of marginal table values is stored as an attribute named `"marginals"` for each of the contingency tables. Each marginals list contains entries for the row, column and grand totals or means, depending on the type of data table. To access them directly, use the [rxMarginals](rxmarginals.md) function. 
  
  
    
 ### `cube`
 logical value. If `TRUE`, the C++ cube functionality is called. 
  
  
    
 ### `rowSelection`
 name of a logical variable in the data set (in quotes) or a logical expression using variables in the data set to specify row selection.  For example, `rowSelection = "old"` will use only observations in which the value of the variable `old` is `TRUE`.  `rowSelection = (age > 20) & (age < 65) & (log(income) > 10)` will use only observations in which the value of the `age` variable is between 20 and 65 and the value of the `log` of the `income` variable is greater than 10.  The row selection is performed after processing any data transformations  (see the arguments `transforms` or `transformFunc`). As with all expressions, `rowSelection` can be defined outside of the function  call using the expression function. 
  
  
    
 ### `transforms`
 an expression of the form `list(name = expression, ...)` representing the first round of variable transformations. As with all expressions, `transforms` (or `rowSelection`)  can be defined outside of the function call using the expression function. 
  
  
    
 ### `transformObjects`
 a named list containing objects that can be referenced by `transforms`, `transformsFunc`, and `rowSelection`. 
  
  
    
 ### `transformFunc`
 variable transformation function. The variables used in the  transformation function must be specified in `transformVars` if they are not variables used in the model. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### `transformVars`
 character vector of input data set variables needed for the transformation function. See [rxTransform](rxtransform.md) for details. 
  
  
    
 ### `transformPackages`
 character vector defining additional R packages (outside of those specified in `rxGetOption("transformPackages")`) to be made available and  preloaded for use in variable transformation functions, e.g., those explicitly defined in **RevoScaleR** functions via their `transforms` and `transformFunc` arguments or those  defined implicitly via their `formula` or `rowSelection` arguments.  The `transformPackages` argument may also be `NULL`,  indicating that no packages outside `rxGetOption("transformPackages")` will be preloaded. 
  
  
    
 ### `transformEnvir`
 user-defined environment to serve as a parent to  all environments developed internally and used for variable data transformation. If `transformEnvir = NULL`, a new "hash" environment with parent `baseenv()` is used instead. 
  
  
    
 ### `useSparseCube`
 logical value. If `TRUE`, sparse cube is used. For large crosstab computation, R may run out of memory due to the resulting expanded contingency tables  even if the internal C++  computation succeeds. In which cases, try to use `rxCube` instead. 
  
  
    
 ### `removeZeroCounts`
 logical flag. If `TRUE`, rows with no observations will be removed from the contingency tables. By default, it has the same value as `useSparseCube`.   Please note this affects only those zeroed counts in the final contingency table for which there are no observations in the input data. However, if the input data contains a row with *frequency* zero it will be reported in the final contingency table.  This should be set to `TRUE` if the total number of combinations of factor values on the right-hand side of the `formula` is significant and as a result R might run out of memory when handling the resulting large contingency table. 
  
  
    
 ### `returnXtabs`
 logical flag. If `TRUE`, an object of class xtabs is returned. Note that the only difference between the structures of an equivalent `xtabs` call output and the output of `rxCrossTabs(..., returnXtabs = TRUE)` is that they will contain different `"call"` attributes.  Note also that `xtabs` expects the cross-classifying variables in the `formula` to be separated by plus (+) symbols whereas `rxCrossTabs` expects them to be separated by a colon (:) symbols.  
  
  
    
 ### `na.rm`
 logical value. If `TRUE`, `NA` values are removed when calculating the marginal means of the contingency tables. 
  
  
    
 ### `blocksPerRead`
 number of blocks to read for each chunk of data read from the data source. 
  
  
    
 ### `reportProgress`
 integer value:  Options are:  
*   `0`: no progress is reported. 
*   `1`: the number of processed rows is printed and updated. 
*   `2`: rows processed and timings are reported. 
*   `3`: rows processed and all timings are reported. 
 
  
  
    
 ### `verbose`
 integer value. If `0`, no additional output is printed.  If `1`, additional summary information is printed. 
  
  
     
 ### `computeContext`
 a valid [RxComputeContext](rxcomputecontext.md).  The `RxSpark`,  `RxHadoopMR`, and `RxInTeradata` compute  contexts distribute the computation among the nodes specified by the  compute context; for other compute contexts, the  computation is distributed if possible on the local computer. 
  
  
    
 ### ` ...`
 for `rxCrossTabs`, additional arguments to be passed directly to the base computational function. 
  
  
    
 ### `x, object`
 objects of class rxCrossTabs. 
  
  
    
 ### `output`
 character string used to specify the type of output to display. Choices are `"sums"`, `"counts"` and `"means"`. 
  
  
    
 ### `header`
 logical value. If `TRUE`, header information is printed. 
  
  
    
 ### `type`
 character string used to specify the summary to create. Choices are `"%"` or `"percentages"` and `"chisquare"` to summarize the cross-tabulation results with percentages or performs a chi-squared test for independence of factors, respectively. 
  
 
 
 
 ##Details
 
The output is returned in a list and the `print` and `summary`
methods can be used to display and summarize the contingency table(s)
contained in each element of the output list. The `print` method produces
an output similar to that of the xtabs function. The
`summary` method produces a summary table for each output contingency
table and displays the column, row, and total table percentages as well as the
counts. 
 
 
 ##Value
 
an object of class rxCrossTabs that contains a list of elements described as
follows:


###`sums`
list of contingency tables whose values are cross-tabulation sums. *This object is NULL if there are no dependent variables specified in the formula*. The names of the list objects are built using the dependent variables specified in the formula (if they exist) along with the independent variable factor levels corresponding to each contingency table. For example, `z <- rxCrossTabs(ncontrols ~ agegp + alcgp + tobgp, esoph); names(z$sums)` will return the character vector with elements `"ncontrols, tobgp = 0-9g/day"`, `"ncontrols, tobgp = 10-19"`, `"ncontrols, tobgp = 20-29"`, `"ncontrols, tobgp = 30+"`.  Typically, the user should rely on the `print` or `summary` methods to display the cross tabulation results but you can also directly access an individual contingency table using its name in R's standard list data access methods. For example, to access the "ncontrols, tobgp = 10-19" table containing cross tabulation summations you would use `z$sums[["ncontrols, tobgp = 10-19"]]` or equivalently `z$sums[[2]]`. To print the entire list of cross-tabulation summations one would issue `print(z, output="sums")`. 



###`counts`
list of contingency tables whose values are cross-tabulation counts. The names of the list objects are equivalent to those of the 'sums' output list.



###`means`
list of contingency tables containing cross tabulation mean values. *This object is NULL if there are no dependent variables specified in the formula*. The 'means' list is returned only if the user has specified `means=TRUE` in the call to rxCrossTabs. If `means=FALSE` in the call, mean values still may be calculated and returned using the `print` and `summary` methods with an `output="means"` argument. In this case, the mean values are calculated dynamically. If you wish to have quick access to the means, use `means=TRUE` in the call to rxCrossTabs. The names of the list objects are equivalent to those of the 'sums' output list. 



###`call`
original call to the underlying `rxCrossTabs.formula` method.



###`chisquare`
list of chi-square tests, one for each cross-tabulation table. Each entry contains the results of a chi-squared test for independence of factors as used in the summary method for the xtabs function. The names of the list objects are equivalent to those of the 'sums' output list. 



###`formula`
formula used in the `rxCrossTabs` call.



###`depvars`
character vector of dependent variable names as extracted from the formula.

 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
xtabs,
[rxMarginals](rxmarginals.md),
[rxCube](rxcube.md),
[as.xtabs](as-xtabs.md),
[rxChiSquaredTest](rxchisquaredtest.md),
[rxFisherTest](rxchisquaredtest.md),
[rxKendallCor](rxchisquaredtest.md),
[rxPairwiseCrossTab](rxpairwisecrosstab.md),
[rxRiskRatio](rxriskratio.md),
[rxOddsRatio](rxriskratio.md),
[rxTransform](rxtransform.md).
   
 ##Examples

 ```
   
  # Basic data.frame source example
  admissions <- as.data.frame(UCBAdmissions)
  admissCTabs <- rxCrossTabs(Freq ~ Gender : Admit, data = admissions)
  
  # print different outputs and summarize different types
  print(admissCTabs) # same as print(admissCTabs, output = "sums")
  print(admissCTabs, output = "counts")
  print(admissCTabs, output = "means")
  summary(admissCTabs) # same as summary(admissCTabs, type = "%")
  summary(admissCTabs, output="means", type = "%")
  summary(admissCTabs, type = "chisquare")
  
  # Example using multiple dependent variables in formula 
  rxCrossTabs(ncontrols ~ agegp : alcgp : tobgp, data = esoph)
  rxCrossTabs(ncases ~ agegp : alcgp : tobgp, data = esoph)
  esophCTabs <-
      rxCrossTabs(cbind(ncases, ncontrols) ~ agegp : alcgp : tobgp, esoph)
  esophCTabs
  
  # Obtaining the mean values
  esophMeans <- mean(esophCTabs, marginals = FALSE)
  esophMeans
  esophMeans <- mean(esophCTabs, marginals = TRUE)
  esophMeans
  
  # XDF example: small subset of census data
  censusWorkers <- file.path(rxGetOption("sampleDataDir"), "CensusWorkers.xdf")
  censusCTabs <- rxCrossTabs(wkswork1 ~ sex : F(age), data = censusWorkers,
  	pweights = "perwt", blocksPerRead = 3)
  censusCTabs
  barplot(censusCTabs$sums$wkswork1/1e6, xlab = "Age (years)",
  	ylab = "Population (millions)", beside = TRUE,
  	legend.text = c("Male", "Female"))
  
  # perform a census crosstab, limiting the analysis to ages
  # on the interval [20, 65]. Verify the age range from the output.
  censusXtabAge.20.65 <- rxCrossTabs(wkswork1 ~ sex : F(age), data = censusWorkers,
      rowSelection = age >= 20 & age <= 65)
  ageRange <- range(as.numeric(colnames(censusXtabAge.20.65$sums$wkswork1)))
  (ageRange[1] >= 20 & ageRange[2] <=65)
  
  # Create a data frame 
  myDF <- data.frame(sex = c("Male", "Male", "Female", "Male"),
  	age = c(20, 20, 12, 15), score = 1.1:4.1, sport=c(1:3,2))
  
  # Use the 'transforms' argument to dynamically transform the
  # variables of the data source. Here, we form a named list of 
  # transformation expressions. To avoid evaluation when assigning
  # to a local variable, we wrap the transformation list with expression().
  transforms <- expression(list(
  	ageHalved = age/2,
  	sport = factor(sport, labels=c("tennis", "golf", "football"))))
  rxCrossTabs(score ~ sport:sex, data = myDF, transforms = transforms)
  rxCrossTabs(~ sport : F(ageHalved, low = 7, high = 10), data = myDF, 
  	transforms = transforms)
  
  # Arithmetic formula expression only (no transformFunc specification).
  rxCrossTabs(log(score) ~ F(age) : sex, data = myDF)
  
  # No transformFunc or formula arithmetic expressions.
  rxCrossTabs(score ~ F(age) : sex, data = myDF)
  
  # Transform a categorical variable to a continuous one and use it
  # as a response variable in the formula for cross-tabulation.
  # The transformation is equivalent to doing the following, which
  # is reflected in the cross-tabulation results.
  #
  #   > as.numeric(as.factor(c(20,20,12,15))) - 1
  #   [1] 2 2 0 1
  # 
  # Note that the effect of N() is to return the factor codes
  myDF <- data.frame(sex = c("Male", "Male", "Female", "Male"),
  	age = factor(c(20, 20, 12, 15)), score = factor(1.1:4.1))
  rxCrossTabs(N(age) ~ sex : score, data = myDF)
  
  # To transform a categorical variable (like age) that has numeric levels
  # (as opposed to codes), use the following construction:
  myDF <- data.frame(sex = c("Male", "Male", "Female", "Male"),
                     age = factor(c(20, 20, 12, 15)), score = factor(1.1:4.1))
  rxCrossTabs(as.numeric(levels(age))[age] ~ sex : score, data = myDF)
  
  # this should break because 'age' is a categorical variable
  ## Not run:
 
try(rxCrossTabs(age ~ sex + score, data = myDF))
 ## End(Not run) 
  
  
  # frequency weighting
  fwts <- 1:4
  sex <- c("Male", "Male", "Female", "Male")
  age <- c(20, 20, 12, 15)
  score <- 1.1:4.1
  
  myDF1 <- data.frame(sex = sex, age = age, score = factor(score), fwts = fwts)
  myDF2 <- data.frame(sex = rep(sex, fwts), age = rep(age, fwts),
  	score = factor(rep(score, fwts)))
  
  mySums1 <- rxCrossTabs(age ~ sex : score, data = myDF1,
  	fweights = "fwts")$sums$age[c("Male", "Female"),]
  mySums2 <- rxCrossTabs(age ~ sex : score,
  	data = myDF2)$sums$age[c("Male", "Female"),]
  all.equal(mySums1, mySums2)
  
  # Compare xtabs and rxCrossTabs(..., returnXtabs = TRUE)  
  # results for 3-way interaction, one dependent variable
  set.seed(100)
  divs <- letters[1:5]
  glads <- c("spartacus", "crixus")
  romeDF <- data.frame( division = rep(divs, 5L), 
                        score = runif(25, min = 0, max = 10), 
                        rank = runif(25, min = 1, max = 100), 
                        gladiator = c(rep(glads[1L], 12L), rep(glads[2L], 13L)),
                        arena = sample(c("colosseum", "ludus", "market"), 25L, replace = TRUE))
  
  z1 <- rxCrossTabs(score ~ division : gladiator : arena, data = romeDF, returnXtabs = TRUE)
  z2 <- xtabs(score ~ division + gladiator + arena, romeDF)
  all.equal(z1, z2, check.attributes = FALSE) # all the same except "call" attribute
  
  # Compare xtabs and rxCrossTabs(..., returnXtabs = TRUE) 
  # results for 3-way interaction, multiple dependent variable
  z1 <- rxCrossTabs(cbind(score, rank) ~ division : gladiator : arena, data = romeDF, returnXtabs = TRUE, means = TRUE)
  z2 <- xtabs(cbind(score, rank) ~ division + gladiator + arena, romeDF)
  all.equal(z1, z2, check.attributes = FALSE) # all the same except "call" attribute
  
  # Compare xtabs and rxCrossTabs(..., returnXtabs = TRUE) 
  # results for 3-way interaction, no dependent variable
  z1 <- rxCrossTabs( ~ division : gladiator : arena, data = romeDF, returnXtabs = TRUE, means = TRUE)
  z2 <- xtabs(~ division + gladiator + arena, romeDF)
  all.equal(z1, z2, check.attributes = FALSE) # all the same except "call" attribute
  
  # removeZeroCounts
  admissions <- as.data.frame(UCBAdmissions)
  admissions[admissions$Dept == "F", "Freq"] <- 0
  
  # removeZeroCounts does not make a difference for the zero values observed from input data
  crossTab1 <- rxCrossTabs(Freq ~ Dept : Gender, data = admissions, removeZeroCounts = TRUE)
  crossTab2 <- rxCrossTabs(Freq ~ Dept : Gender, data = admissions)
  all.equal(as.data.frame(crossTab1$sums$Freq), as.data.frame(crossTab2$sums$Freq))
  
  # removeZeroCounts removes the missing values that are not observed from input data
  admissions_NoZero <- admissions[admissions$Dept != "F",]
  crossTab1 <- rxCrossTabs(Freq ~ Dept : Gender, data = admissions,  removeZeroCounts = TRUE, rowSelection = (Freq != 0))
  crossTab2 <- rxCrossTabs(Freq ~ Dept : Gender, data = admissions_NoZero, removeZeroCounts = TRUE)
  all.equal(as.data.frame(crossTab1$sums$Freq), as.data.frame(crossTab2$sums$Freq))
  
 
```
 
 
 
