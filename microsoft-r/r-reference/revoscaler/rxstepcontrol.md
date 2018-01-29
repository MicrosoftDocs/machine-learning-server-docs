--- 
 
# required metadata 
title: "rxStepControl function (revoAnalytics) | Microsoft Docs" 
description: "     Various parameters that control aspects of stepwise regression. " 
keywords: "(revoAnalytics), rxStepControl, models, classification, regression" 
author: "heidisteen" 
manager: "cgronlun" 
ms.date: "01/24/2018" 
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
 
 
 #rxStepControl: Control for Stepwise Regression 
 
 ##Description
 
Various parameters that control aspects of stepwise regression.
 
 
 ##Usage

```   
  rxStepControl(method = "stepwise", scope = NULL, 
      maxSteps = 1000, stepCriterion = "AIC",
      maxSigLevelToAdd = NULL, minSigLevelToDrop = NULL,
      refitEachStep = NULL, keepStepCoefs = FALSE,
      scale = 0, k = 2, test = NULL,   ...  )
 
```
 
 ##Arguments

   
    
 ### `method`
  a character string specifying the method of stepwise search:  
*   "stepwise": bi-directional search. 
*   "backward": backward elimination. 
*   "forward": forward selection.   
 Default is "stepwise" if the scope argument is not missing, otherwise "backward". 
  
    
 ### `scope`
  either a single formula, or a named list containing components upper and lower, both formulae, defining the range of models to be examined in the stepwise search. 
  
    
 ### `maxSteps`
  an integer specifying the maximum number of steps to be considered, typically used to stop the process early and the default is 1000. 
  
  
    
 ### `stepCriterion`
  a character string specifying the variable selection criterion:  
*   "AIC": Akaike's information criterion. 
*   "SigLevel": significance level, the traditional stepwise approach in SAS. 
 This argument is similar to the SELECT option of the GLMSELECT procedure in SAS.  Default is "AIC". 
  
    
 ### `maxSigLevelToAdd`
  a numeric scalar specifying the significance level for adding a variable to the model.  This argument is used only when stepCriterion = "SigLevel" and  is similar to the SLENTRY option of the GLMSELECT procedure in SAS. The defaults are 0.50 for "forward" and 0.15 for "stepwise". 
  
    
 ### `minSigLevelToDrop`
  a numeric scalar specifying the significance level for dropping a variable from the model. This argument is used only when stepCriterion = "SigLevel" and  is similar to the SLSTAY option of the GLMSELECT procedure in SAS. The defaults are 0.10 for "backward" and 0.15 for "stepwise". 
  
    
 ### `refitEachStep`
  a logical flag specifying whether or not to refit the model at each step. The default is `NULL`,  indicating to refit the model at each step for `rxLogit` and `rxGlm` but not for `rxLinMod`. 
  
    
 ### `keepStepCoefs`
  a logical flag specifying whether or not to keep the model coefficients at each step.  If `TRUE`, a data.frame `stepCoefs` will be returned with the fitted model with rows corresponding to the coefficients and columns corresponding to the iterations. Additional computation may be required to generate the coefficients at each step. Those stepwise coefficients can be visualized by plotting the fitted model with [rxStepPlot](rxStepPlot.md). 
  
    
 ### `scale`
  optional numeric scalar specifying the scale parameter of the model. It is used in computing the AIC statistics for selecting the models. The default 0 indicates it should be estimated by maximum likelihood. See "scale" in step for details. 
  
    
 ### `k`
  optional numeric scalar specifying the weight of the number of  equivalent degrees of freedom in computing AIC for the penalty.  See "k" in step for details. 
  
    
 ### `test`
  a character string specifying the test statistic to be included in the results, either "F" or "Chisq". Both test statistics are relative to the original model. 
  
    
 ### ` ...`
  additional arguments to be passed directly to the Microsoft R Services Compute Engine. 
  
 
 
 ##Details
 
Stepwise models must be computed on the same dataset in order to be compared so
rows with missing values in any of the variables in the upper model are removed
before the model fitting starts.
Consequently, the stepwise models might be different from the corresponding models 
fitted with only the selected variables if there are missing values in the data set.

When computing stepwise models with `rxLogit` or `rxGlm`, you can
sometimes improve the speed and quality of the fitting by setting `returnAlways=TRUE`
in the initial `rxLogit` or `rxGlm` call. When `returnAlways=TRUE`, 
`rxLogit` and `rxGlm` always return the solution tried so far that has the minimum deviance.
 
 
 ##Value
 
A list containing the options.
 
 ##Author(s)
 
Microsoft Corporation [`Microsoft Technical Support`](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409)

 
 
 ##References
 
Venables, W. N. and Ripley, B. D. (2002) 
*Modern Applied Statistics with S*. 
New York: Springer (4th ed).

Chambers, J. M. and Hastie, T. J. eds (1992)
*Statistical Models in S*.
Wadsworth & Brooks/Cole.

Goodnight, J. H. (1979)
A Tutorial on the SWEEP Operator.
*The American Statistician* 
Vol. **33** No. **3**, 149--158.
 
 
 ##See Also
 
step, [rxStepPlot](rxStepPlot.md).
   
 ##Examples

 ```
   
  ## setup
  form <- Sepal.Length ~ Sepal.Width + Petal.Length
  scope <- list(
      lower = ~ Sepal.Width,
      upper = ~ Sepal.Width + Petal.Length + Petal.Width * Species)
      
  ## lm/step
  ## We need to specify the contrasts for the factor variable Species,
  ## even though this is not part of the original model. This will 
  ## generate a warning, so we suppress that warning here.
  suppressWarnings(rlm.obj <- lm(form, data = iris, contrasts = list(Species = contr.SAS)))
  rlm.step <- step(rlm.obj, direction = "both", scope = scope, trace = 1)
  
  ## rxLinMod/variableSelection
  varsel <- rxStepControl(method = "stepwise", scope = scope)
  rxlm.step <- rxLinMod(form, data = iris, variableSelection = varsel,
      verbose = 1, dropMain = FALSE, coefLabelStyle = "R")
      
  ## compare lm/step and rxLinMod/variableSelection
  rlm.step$anova
  rxlm.step$anova
  
  as.matrix(coef(rlm.step))
  as.matrix(coef(rxlm.step))
  
  ## rxLinMod/variableSelection with keepStepCoefs = TRUE
  varsel <- rxStepControl(method = "stepwise", scope = scope, keepStepCoefs = TRUE)
  rxlm.step <- rxLinMod(form, data = iris, variableSelection = varsel,
      verbose = 1, dropMain = FALSE, coefLabelStyle = "R")
  rxStepPlot(rxlm.step)
 
```
 
 
 
