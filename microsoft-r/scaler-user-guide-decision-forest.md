---

# required metadata
title: "RevoScaleR User's Guide--Estimating Decision Forest Models"
description: "Decision forests with RevoScaleR."
keywords: ""
author: "richcalaway"
manager: "mblythe"
ms.date: "03/17/2016"
ms.topic: "get-started-article"
ms.prod: "rserver"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

# Estimating Decision Forest Models

The *rxDForest* function in RevoScaleR fits a *decision forest*, which is an ensemble of decision trees. Each tree is fitted to a bootstrap sample of the original data, which leaves about 1/3 of the data unused in the fitting of each tree. Each data point in the original data is fed through each of the trees for which it was unused; the decision forest prediction for that data point is the statistical *mode* of the individual tree predictions, that is, the majority prediction (for classification; for regression problems, the prediction is the mean of the individual predictions).

Unlike individual decision trees, decision forests are not prone to overfitting, and they are consistently shown to be among the best machine learning algorithms. RevoScaleR implements decision forests in the *rxDForest* function, which uses the same basic tree-fitting algorithm as *rxDTree* (see ["The *rxDTree* Algorithm"](scaler-user-guide-decision-tree.md#the-rxdtree-algorithm)). To create the forest, you specify the number of trees using the *nTree* argument and the number of variables to consider for splitting in each tree using the *mTry* argument. In most cases, you will also want to specify the maximum depth to grow the individual trees: greater depth typically results in greater accuracy, but as with *rxDTree*, also results in significantly longer fitting times.

### A Simple Classification Forest

In Chapter 11, we fit a simple classification tree model to rpart’s kyphosis data. That model is easily recast as a classification decision forest using *rxDForest* as follows (we set the *seed* argument to ensure reproducibility; in most cases you can omit this):

	######################################################## 
	# Chapter 12: Estimating Decision Forest Models
	#  A Simple Classification Forest
	Ch12Start <- Sys.time()

	  
	data("kyphosis", package="rpart")
	kyphForest <- rxDForest(Kyphosis ~ Age + Start + Number, seed = 10,
		data = kyphosis, cp=0.01, nTree=500, mTry=3)
	kyphForest

	  Call:
	  rxDForest(formula = Kyphosis ~ Age + Start + Number, data = kyphosis, 
		  cp = 0.01, nTree = 500, mTry = 3, seed = 10)
	  
	  
				   Type of decision forest: class 
						   Number of trees: 500 
	  No. of variables tried at each split: 3 
	  
			   OOB estimate of error rate: 19.75%
	  Confusion matrix:
			   Predicted
	  Kyphosis  absent present class.error
		absent      56       8   0.1250000
		present      8       9   0.4705882

While decision forests do not produce a unified model, as logistic regression and decision trees do, they do produce reasonable predictions for each data point. In this case, we can obtain predictions using *rxPredict* as follows:

	dfPreds <- rxPredict(kyphForest, data=kyphosis)

Comparing this to the Kyphosis variable in the original kyphosis data, we see that approximately 88 percent of cases are classified correctly:

	sum(as.character(dfPreds[,1]) ==
		as.character(kyphosis$Kyphosis))/81

	  [1] 0.8765432

### A Simple Regression Forest

As a simple example of a regression forest, consider the classic *stackloss* data set, containing observations from a chemical plant producing nitric acid by the oxidation of ammonia, and let’s fit the stack loss (*stack.loss*) using air flow (*Air.Flow*), water temperature (*Water.Temp*), and acid concentration (*Acid.Conc.*) as predictors:

	#  A Simple Regression Forest
	  
	stackForest <- rxDForest(stack.loss ~ Air.Flow + Water.Temp + Acid.Conc.,
		data=stackloss, nTree=200, mTry=2)
	stackForest

	  Call:
	  rxDForest(formula = stack.loss ~ Air.Flow + Water.Temp + Acid.Conc., 
		  data = stackloss, maxDepth = 3, nTree = 200, mTry = 2)
	  
	  
				   Type of decision forest: anova 
						   Number of trees: 200 
	  No. of variables tried at each split: 2 
	  
				Mean of squared residuals: 44.54992
						  % Var explained: 65
	  
### A Larger Regression Forest Model

As a more complex example, we return to the censusWorkers data to which we earlier fit a decision tree. We will create a regression forest predicting wage income from age, sex, and weeks worked, using the *perwt* variable as probability weights (note that we retain the *maxDepth* and *minBucket* parameters from our earlier decision tree example):

	#  A Larger Regression Forest Model
	  
	censusWorkers <- file.path(rxGetOption("sampleDataDir"),
		"CensusWorkers.xdf")
	rxGetInfo(censusWorkers, getVarInfo=TRUE)
	incForest <- rxDForest(incwage ~ age + sex + wkswork1, pweights = "perwt", 
		maxDepth = 3, minBucket = 30000, mTry=2, nTree=200, data = censusWorkers)
	incForest

	  Call:
	  rxDForest(formula = incwage ~ age + sex + wkswork1, data = censusData, 
		  pweights = "perwt", maxDepth = 5, nTree = 200, mTry = 2)
	  
	  
				   Type of decision forest: anova 
						   Number of trees: 200 
	  No. of variables tried at each split: 2 
	  
				Mean of squared residuals: 1458969472
						  % Var explained: 11 
	  
### Large Data Decision Forest Models

As with decision trees, scaling decision forests to very large data sets should be done with caution—the wrong choice of model parameters can easily lead to models that take hours or longer to estimate, even in a distributed computing environment, or that simply cannot be fit at all. For non-binary classification problems, as with decision trees, categorical predictors should have a small to moderate number of levels.

As an example of a large data classification forest, consider the following simple model using the 7% subsample of the full airline data (this uses the variable *ArrDel15* indicating flights with an arrival delay of 15 minutes or more):

>The `blocksPerRead` argument is ignored if run locally using R Client. [Learn more...](scaler-getting-started.md#chunking)
	  
	#  Large Data Tree Models
	  
	bigDataDir <- "C:/MRS/Data"
	sampleAirData <- file.path(bigDataDir, "AirOnTime7Pct.xdf")	
	airlineForest <- rxDForest(ArrDel15 ~ CRSDepTime + DayOfWeek, 
		data = sampleAirData, blocksPerRead = 30, maxDepth = 5, 
		nTree=20, mTry=2, method="class", seed = 8)
	
This yields the following:

	airlineForest

	  Call:
	  rxDForest(formula = ArrDel15 ~ CRSDepTime + DayOfWeek, data = sampleAirData, 
	  	method = "class", maxDepth = 5, nTree = 20, mTry = 2, seed = 8, 
	  	blocksPerRead = 30)
      
      
	  			 Type of decision forest: class 
	  					 Number of trees: 20 
	  No. of variables tried at each split: 2 
      
	  		 OOB estimate of error rate: 20.01%
	  Confusion matrix:
	  		Predicted
	  ArrDel15   FALSE TRUE class.error
	     FALSE 8147274    0           0
	     TRUE  2037941    0           1
	
Looking at the fitted object’s forest component, we see that a number of the fitted trees do not split at all:

	airlineForest$forest

		[[1]]
		Number of valid observations:  6440007 
		Number of missing observations:  3959748 

		Tree representation: 
		n= 10186709 

		node), split, n, loss, yval, (yprob)
			  * denotes terminal node

		1) root 10186709 2038302 FALSE (0.7999057 0.2000943) *

		[[2]]
		Number of valid observations:  6440530 
		Number of missing observations:  3959225 

		Tree representation: 
		n= 10186445 

		node), split, n, loss, yval, (yprob)
			  * denotes terminal node

		1) root 10186445 2038249 FALSE (0.7999057 0.2000943) *

		[[3]]
		... 

		[[6]]
		Number of valid observations:  6439485 
		Number of missing observations:  3960270 

		Tree representation: 
		n= 10186656 

		node), split, n, loss, yval, (yprob)
			  * denotes terminal node

		1) root 10186656 2038291 FALSE (0.7999057 0.2000943) *

		[[7]]
		Number of valid observations:  6439307 
		Number of missing observations:  3960448 

		Tree representation: 
		n= 10186499 

		node), split, n, loss, yval, (yprob)
			  * denotes terminal node

		1) root 10186499 2038260 FALSE (0.7999057 0.2000943) *
		. . .


This may well be because our response is extremely unbalanced--that is, the percentage of flights that are late by 15 minutes or more is quite small. We can tune the fit by providing a *loss matrix*, which allows us to penalize certain predictions in favor of others. You specify the loss matrix using the parms argument, which takes a list with named components. The loss component is specified as either a matrix, or equivalently, a vector that can be coerced to a matrix. In the binary classification case, it can be useful to start with a loss matrix with a penalty roughly equivalent to the ratio of the two classes. So, in our case we know that the on-time flights outnumber the late flights approximately 4 to 1 :

	airlineForest2 <- rxDForest(ArrDel15 ~ CRSDepTime + DayOfWeek, 
		data = sampleAirData, blocksPerRead = 30, maxDepth = 5, seed = 8,
		nTree=20, mTry=2, method="class", parms=list(loss=c(0,4,1,0)))

		Call:
	  rxDForest(formula = ArrDel15 ~ CRSDepTime + DayOfWeek, data = sampleAirData, 
		  method = "class", parms = list(loss = c(0, 4, 1, 0)), maxDepth = 5, 
		  nTree = 20, mTry = 2, seed = 8, blocksPerRead = 30)
	  
	  
				   Type of decision forest: class 
						   Number of trees: 20 
	  No. of variables tried at each split: 2 
	  
			   OOB estimate of error rate: 42.27%
	  Confusion matrix:
			  Predicted
	  ArrDel15   FALSE    TRUE class.error
		 FALSE 4719374 3427900    0.420742
		 TRUE   877680 1160261    0.430670
	  
This model no longer predicts all flights as on time, but now over-predicts late flights. Adjusting the loss matrix again, this time reducing the penalty, yields the following:

	Call:
	rxDForest(formula = ArrDel15 ~ CRSDepTime + DayOfWeek, data = sampleAirData, 
	    method = "class", parms = list(loss = c(0, 3, 1, 0)), maxDepth = 5, 
	    nTree = 20, mTry = 2, seed = 8, blocksPerRead = 30)
	
	
	             Type of decision forest: class 
	                     Number of trees: 20 
	No. of variables tried at each split: 2 
	
	         OOB estimate of error rate: 30.15%
	Confusion matrix:
	        Predicted
	ArrDel15   FALSE    TRUE class.error
	   FALSE 6465439 1681835   0.2064292
	   TRUE  1389092  648849   0.6816154

### Controlling the Model Fit

The *rxDForest* function has a number of options for controlling the model fit. Most of these control parameters are identical to the same controls in *rxDTree*. A full listing of these options can be found in the *rxDForest* help file, but the following have been found in our testing to be the most useful at controlling the time required to fit a model with *rxDForest*:

-   *maxDepth*: this sets the maximum depth of any node of the tree. Computations grow rapidly more expensive as the depth increases, so we recommend a maxDepth of 10 to 15.
-   *maxNumBins*: this controls the maximum number of bins used for each variable. Managing the number of bins is important in controlling memory usage. The default is to use the larger of 101 and the square root of the number of observations for small to moderate size data sets (up to about one million observations), but for larger sets to use 1001 bins. For small data sets with continuous predictors, you may find that you need to increase the *maxNumBins* to obtain models that resemble those from rpart.
-   *minSplit*, *minBucket*: these determine how many observations must be in a node before a split is attempted (*minSplit*) and how many must remain in a terminal node (*minBucket*).
