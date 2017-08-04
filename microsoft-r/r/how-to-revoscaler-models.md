---

# required metadata
title: "RevoScaleR User's Guide--Models in RevoScaleR"
description: "Models in RevoScaleR."
keywords: ""
author: "HeidiSteen"
ms.author: "heidist"
manager: "jhubbard"
ms.date: "03/17/2016"
ms.topic: "get-started-article"
ms.prod: "microsoft-r"

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

# Models in RevoScaleR

Specifying a model with RevoScaleR is similar to specifying a model with the standard R statistical modeling functions, but there are some significant differences. Understanding these differences can help you make better use of RevoScaleR. The purpose of this article is to explain these differences at a high level so that when we begin fitting models in script, the terminology does not seem too foreign.

### External Memory Algorithms


The first thing to know about RevoScaleR’s statistical algorithms is that they are *external memory* algorithms that work on one chunk of data at time. All of RevoScaleR’s algorithms share a basic underlying structure:

-   An *initialization* step, in which data structures are created and given initial values, a data source is identified and opened for reading, and any other initial conditions are satisfied.
-   A *process data* step, in which a chunk of data is read, some calculations are performed, and the result is passed back to the master process.
-   An *update results* step, in which the results of the process data step are merged with previous results.
-   A *finalize results* step, in which any final processing is performed and a result is returned.

An important consideration in using external memory algorithms is deciding how big a *chunk* of data is. You want to use a chunk size that’s as large as possible while still allowing the process data step to complete without swapping. All of the RevoScaleR modeling functions allow you to specify a *blocksPerRead* argument. To make effective use of this, you need to know how many blocks your data file contains and how large the data file is. The number of blocks can be obtained using the *rxGetInfo* function.

>The `blocksPerRead` argument is ignored if run locally using R Client. [Learn more...](tutorial-revoscaler-data-import-transform.md#chunking)

### Formulas in RevoScaleR

RevoScaleR uses a variant of the Wilkinson-Rogers formula notation that is similar to, but not exactly the same as, that used in the standard R modeling functions. The response, or dependent, variables are separated from the predictor, or independent, variables by a tilde (~). In most of the modeling functions, multiple response variables are permitted, specified using the *cbind* function to combine them.

Independent variables (*predictors*) are separated by plus signs (+). Interaction terms can be created by joining two or more variables with a colon (:). Interactions between two categorical variables add one coefficient to the model for every combination of levels of the two variables. For example, if we have the categorical variables sex with two levels and education with four levels, the interaction of sex and education will add eight coefficients to the fitted model. Interactions between a continuous variable and a categorical variable add one coefficient to the model representing the continuous variable for each level of the categorical variable. (However, in RevoScaleR, such interactions cannot be used with the *rxCube* or *rxCrossTabs* functions.) The interaction of two continuous variables is the same as multiplying the two variables.

An asterisk (\*) between two variables adds all subsets of interactions to the model. Thus, sex\*education is equivalent to sex + education + sex:education.

The special function syntax *F(x)* can be used to have RevoScaleR treat a numeric variable *x* as a categorical variable (factor) for the purposes of the analysis. You can include additional arguments *low* and *high* to specify the minimum and maximum values to be included in the factor variable; RevoScaleR creates a bin for each integer value from the low to high values. You can use the logical flag *exclude* to specify whether values outside that range are omitted from the model (*exclude=TRUE*) or included as a separate level (*exclude=FALSE*).

Similarly, the special function syntax *N(x)* can be used to have RevoScaleR treat *x* as a continuous numeric variable. (This syntax is provided for completeness, but is not recommended. In general, if you want to recover numeric data from a factor, you will want to do so from the *levels* of the factor.)

In RevoScaleR, formulas in which the first independent variable is categorical may be handled specially, as *cubes*. See the following section for more details.

## Letting the Data Speak For Itself

Classical statistics is largely concerned with fitting predictive models to limited observations. Data analysts and statisticians use exploratory techniques to visualize the data and then make informed guesses as to the appropriate form for a predictive model. Large data analysis, on the other hand, provides the opportunity to let the data speak for itself—with millions of observations in hand, we don’t have to guess about the form of relationships between variables: they become clear. A general approach to finding the relationship between two numeric variables x and y might be as follows. First, bin the x values. Then, for each bin, plot the mean of the y values contained in that bin. As the bins become narrower and narrower, the resulting plot becomes a plot of E(y|x) for each value of x.

## Cubes and Cube Regression

In RevoScaleR, a *cube* is a type of multi-dimensional array. It may have any number of dimensions, thus making it a hypercube, and each dimension consists of categorical data. The most common operation on a cube is simple cross-tabulation, computing the cell counts for every combination of levels within the cube; this is done using the *rxCube* function. But cubes can also be passed as the first independent variable in a linear or logistic regression, in which case the regression can be computed in a special way. Because the cube consists of categorical data, one part of the moment matrix is diagonal. This makes it possible to compute the regression using a partitioned inverse method, which may be faster and may use less memory than the usual regression method. When a linear or logistic regression is fitted with the argument *cube=TRUE* (and a categorical variable as the first predictor), the partitioned inverse method is used and the model is fitted without an intercept term. Because the first term in the regression is categorical, it is equivalent to a complete set of dummy variables and thus is collinear with a constant term. By dropping the intercept term, the collinearity is resolved and a coefficient can be computed for each level of the categorical predictor. The R-squared, however, is computed as if an intercept were included.
