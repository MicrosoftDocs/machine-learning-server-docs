---

# required metadata
title: "RevoScaleR User's Guide--Estimating Models using Stochastic Gradient Boosting"
description: "Boosted trees with RevoScaleR."
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

# Estimating Models Using Stochastic Gradient Boosting

The *rxBTrees* function in RevoScaleR, like *rxDForest*, fits a decision forest to your data, but the forest is generated using a stochastic gradient boosting algorithm. This is similar to the decision forest algorithm in that each tree is fitted to a subsample of the training set (sampling without replacement) and predictions are made by aggregating over the predictions of all trees. Unlike the *rxDForest* algorithm, the boosted trees are added one at a time, and at each iteration, a regression tree is fitted to the current pseudo-residuals, i.e., the gradient of the loss functional being minimized.

Like the *rxDForest* function, *rxBTrees* uses the same basic tree-fitting algorithm as *rxDTree* (see ["The *rxDTree* Algorithm"](how-to-revoscaler-decision-tree.md#the-rxdtree-algorithm)). To create the forest, you specify the number of trees using the *nTree* argument and the number of variables to consider for splitting at each tree node using the *mTry* argument. In most cases, you will also want to specify the maximum depth to grow the individual trees: shallow trees seem to be sufficient in many applications, but as with *rxDTree* and *rxDForest*, greater depth typically results in longer fitting times.

Different model types are supported by specifying different loss functions, as follows:

-   *"gaussian"*, for regression models
-   *"bernoulli"*, for binary classification models
-   *"multinomial"*, for multi-class classification models

### A Simple Binary Classification Forest

In [Logistic Regression](how-to-revoscaler-logistic-regression.md), we fit a simple classification tree model to rpart’s kyphosis data. That model is easily recast as a classification decision forest using *rxBTrees* as follows (we set the *seed* argument to ensure reproducibility; in most cases you can omit this):

	#  A Simple Classification Forest
	  
	data("kyphosis", package="rpart")
	kyphBTrees <- rxBTrees(Kyphosis ~ Age + Start + Number, seed = 10,
		data = kyphosis, cp=0.01, nTree=500, mTry=3, lossFunction="bernoulli")
	kyphBTrees

	  Call:
	  rxBTrees(formula = Kyphosis ~ Age + Start + Number, data = kyphosis, 
		  cp = 0.01, nTree = 500, mTry = 3, seed = 10, lossFunction = "bernoulli")
	  
	  
			Loss function of boosted trees: bernoulli 
					  Number of iterations: 500 
				  OOB estimate of deviance: 0.1441952 
	  

In this case, we don’t need to explicitly specify the loss function; *"bernoulli"* is the default.

### A Simple Regression Forest

As a simple example of a regression forest, consider the classic *stackloss* data set, containing observations from a chemical plant producing nitric acid by the oxidation of ammonia, and let’s fit the stack loss (*stack.loss*) using air flow (*Air.Flow*), water temperature (*Water.Temp*), and acid concentration (*Acid.Conc.*) as predictors:

	#  A Simple Regression Forest
	  
	stackBTrees <- rxDForest(stack.loss ~ Air.Flow + Water.Temp + Acid.Conc.,
		data=stackloss, nTree=200, mTry=2, lossFunction="gaussian")
	stackBTrees

	  Call:
	  rxDForest(formula = stack.loss ~ Air.Flow + Water.Temp + Acid.Conc., 
		  data = stackloss, nTree = 200, mTry = 2, lossFunction = "gaussian")
	  
	  
			Loss function of boosted trees: gaussian 
					  Number of iterations: 200 
				  OOB estimate of deviance: 1.46797
	  
### A Simple Multinomial Forest Model

As a simple multinomial example, we will fit an *rxBTrees* model to the iris data:

	#  A Multinomial Forest Model
	  
	irisBTrees <- rxBTrees(Species ~ Sepal.Length + Sepal.Width + 
		Petal.Length + Petal.Width, data=iris,
		nTree=50, seed=0, maxDepth=3, lossFunction="multinomial")
	irisBTrees


	  Call:
	  rxBTrees(formula = Species ~ Sepal.Length + Sepal.Width + Petal.Length + 
		  Petal.Width, data = iris, maxDepth = 3, nTree = 50, seed = 0, 
		  lossFunction = "multinomial")
	  
	  
			Loss function of boosted trees: multinomial 
			 Number of boosting iterations: 50 
	  No. of variables tried at each split: 1 
	  
				  OOB estimate of deviance: 0.02720805  
	  
### Controlling the Model Fit

The principal parameter controlling the boosting algorithm itself is the *learning rate*. The learning rate (or shrinkage) is used to scale the contribution of each tree when it is added to the ensemble. The default learning rate is 0.1.

The *rxBTrees* function has a number of other options for controlling the model fit. Most of these control parameters are identical to the same controls in *rxDTree*. A full listing of these options can be found in the *rxBTrees* help file, but the following have been found in our testing to be the most useful at controlling the time required to fit a model with *rxBTrees*:

-   *maxDepth*: this sets the maximum depth of any node of the tree. Computations grow more expensive as the depth increases, so we recommend starting with maxDepth=1, i.e., boosted stumps.
-   *maxNumBins*: this controls the maximum number of bins used for each variable. Managing the number of bins is important in controlling memory usage. The default is to use the larger of 101 and the square root of the number of observations for small to moderate size data sets (up to about one million observations), but for larger sets to use 1001 bins. For small data sets with continuous predictors, you may find that you need to increase the *maxNumBins* to obtain models that resemble those from rpart.
-   *minSplit*, *minBucket*: these determine how many observations must be in a node before a split is attempted (*minSplit*) and how many must remain in a terminal node (*minBucket*).
