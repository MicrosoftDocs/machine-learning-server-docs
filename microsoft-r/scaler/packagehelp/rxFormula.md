--- 
 
# required metadata 
title: "Formula Syntax for RevoScaleR Analysis Functions" 
description: " Highlights of the similarities and differences in formulas between **RevoScaleR** and standard R functions. " 
keywords: "RevoScaleR, rxFormula, models" 
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
 
 
 #`rxFormula`: Formula Syntax for RevoScaleR Analysis Functions

 Applies to version 9.1.0 of package RevoScaleR.
 
 ##Description
 
Highlights of the similarities and differences in formulas between
**RevoScaleR** and standard R functions.
 
 
 ##Details
 
The formula syntax used by the **RevoScaleR** analysis functions is similar,
but not identical, to regular R formula syntax. The most important differences
are:


* 
 With the exception of [rxSummary](rxSummary.md), dot (`.`)
explanatory variable expansion is not supported.

* 
 Multiple column producing in-line variable transformations, e.g.
poly and bs, are not supported.

* 
 The original order of the explanatory variables are maintained, i.e.
the main effects are not forced to precede the interaction terms. (See
`keep.order = TRUE` setting in terms.formula for more
information.)



A formula typically consists of a *response*, which in most
**RevoScaleR** functions can be a single variable or multiple variables 
combined using cbind, the `"~"` operator, and one or 
more *predictors*,typically separated by the `"+"` operator.
The [rxSummary](rxSummary.md) function typically requires a formula with no 
response.

Interactions are indicated using the `":"` operator. The interaction of
two categorical variables results in a categorical variable containing the
full set of combinations of the two categories and adds a coefficient to the
model for each category. The interaction of two continuous variables is the
same as the multiplication of the two variables. The interaction of a
continuous and a categorical variable adds a coefficient for the continuous
variable for each level of the categorical variable. The asterisk operator
`"*"` between categorical variables adds all subsets of interactions of
the variables to the model.

In **RevoScaleR**, predictors must be single-column variables. 

**RevoScaleR** formulas support two formula functions for managing
categorical variables:


* `F(x, low, high, exclude)`creates a categorical variable out of continuous variable `x`. Additional arguments `low`, `high`, and `exclude` can be included to specify the value of the lowest category, the highest category, and how to handle values outside the specified range. For each integer in the range from `low` to `high` inclusive, **RevoScaleR** creates a level and assigns values greater than or equal to an integer n, but less than n+1, to n's level. If `x` is already a factor, `F(x, low, high, exclude)` can be used to limit the range of levels used; in this case `low` and `high` represent the indexes of the factor levels, and must be integers  in the range from 1 to the number of levels.


* `N(x)`creates a continuous variable from categorical variable `x`. Note, however, that the value of this  function is equivalent to the factor codes, and has no relation to any  numeric values within the levels of the function. For this, use the construction `as.numeric(levels(x))[x]`.



  
 
 ##Author(s)
 Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)
 
 
 ##See Also
 
[rxTransform](rxTransform.md),
[rxCrossTabs](../../r-reference/revoscaler/rxcrosstabs.md),
[rxCube](rxCube.md),
[rxLinMod](rxLinMod.md),
[rxLogit](rxLogit.md),
[rxSummary](rxSummary.md).
   
 ##Examples

 ```
   
  
  # These two lines are set up for the examples
  sampleDataDir <- rxGetOption("sampleDataDir")
  censusWorkers <- file.path(sampleDataDir, "CensusWorkers.xdf")
  
  rxSummary(~ F(age) + sex, data = censusWorkers)
  rxSummary(~ F(age, low = 30, high = 45, exclude = FALSE), data = censusWorkers)
  
  rxCube(incwage ~ F(age) : sex, data = censusWorkers)
 
```
 
 
