---

# required metadata
title: "RevoScaleR User's Guide--Naive Bayes Classifier"
description: "Naive Bayes Classifier in RevoScaleR."
keywords: ""
author: "richcalaway"
manager: "jhubbard"
ms.date: "03/17/2016"
ms.topic: "get-started-article"
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

# Naïve Bayes Classifier

In this article, we describe one simple and effective family of classification methods known as Naïve Bayes. In RevoScaleR, Naïve Bayes classifiers can be implemented using the *rxNaiveBayes* function. Classification, simply put, is the act of dividing observations into classes or categories. Some examples of this are the classification of product reviews into positive or negative categories or the detection of email spam. These classification examples can be achieved manually using a set of rules. However, this is not efficient or scalable. In Naïve Bayes and other machine learning based classification algorithms, the decision criteria for assigning class are learned from a training data set, which has classes assigned manually to each observation.

### The rxNaiveBayes Algorithm

The Naïves Bayes classification method is simple, effective, and robust. This method can be applied to data large or small, it requires minimal training data, and is unlikely to produce a classifier that performs poorly compared to more complex algorthims. This family of classifiers utilizes Bayes Theorem to determine the probability that an observation belongs to a certain class. A training dataset is used to calculate prior probabilities of an observation occurring in a class within the predefined set of classes. In RevoScaleR this is done using the *rxNaiveBayes* function. These probabilities are then used to calculate posterior probabilities that an observation belongs to each class. The class membership is decided by choosing the class with the largest posterior probability for each observation. This is accomplished with the *rxPredict* function using the Naïve Bayes object from a call to *rxNaiveBayes*. Part of the beauty of Naïve Bayes is its simplicity due to the conditional independent assumption: that the values of each predictor are independent of each other given the class. This assumption reduces the number of parameters needed and in turn makes the algorithm extremely efficient. Naïve Bayes methods differ in their choice of distribution for any continuous independent variables. Our implementation via the *rxNaiveBayes* function assumes the distribution to be Gaussian.

### A Simple Naïve Bayes Classifier

In [Logistic Regression Models](scaler-user-guide-logistic-regression.md), we fit a simple logistic regression model to rpart’s kyphosis data and in [Decision Trees](scaler-user-guide-decision-tree.md) and [Decision Forests](scaler-user-guide-decision-forest.md) we used the kyphosis data again to create classification and regression trees. We can use the same data with our Naïve Bayes classifier to see which patients are more likely to acquire Kyphosis based on age, number, and start. We can train and test our classifier on the kyphosis data for the sake of illustration. We use the *rxNaiveBayes* function to construct a classifier for the kyphosis data:

	#  A Simple Naïve Bayes Classifier
	
	data("kyphosis", package="rpart")
	kyphNaiveBayes <- rxNaiveBayes(Kyphosis ~ Age + Start + Number, data = kyphosis)
	kyphNaiveBayes

	  Call:
	  rxNaiveBayes(formula = Kyphosis ~ Age + Start + Number, data = kyphosis)
	  
	  A priori probabilities:
	  Kyphosis
		 absent   present 
	  0.7901235 0.2098765 
	  
	  Predictor types:
		Variable    Type
	  1      Age numeric
	  2    Start numeric
	  3   Number numeric
	  
	  Conditional probabilities:
	  $Age
				 Means   StdDev
	  absent  79.89062 61.86111
	  present 97.82353 39.27505
	  
	  $Start
				  Means   StdDev
	  absent  12.609375 4.427967
	  present  7.294118 4.283175
	  
	  $Number
				 Means   StdDev
	  absent  3.750000 1.414214
	  present 5.176471 1.878673
	  
The returned object *kyphNaiveBayes* is an object of class *rxNaiveBayes*. Objects of this class provide the following useful components: *apriori* and *tables*. The *apriori* component contains the conditional probabilities for the response variable, in this case the Kyphosis variable. The *tables* component contains a list of tables, one for each predictor variable. For a categorical variable, the table contains the conditional probabilities of the variable given the target level of the response variable. For a numeric variable, the table contains the mean and standard deviation of the variable given the target level of the response variable. These components are printed in the output above.

We can use our Naïve Bayes object with *rxPredict* to re-classify the Kyphosis variable for each child in our original dataset:

	kyphPred <- rxPredict(kyphNaiveBayes,kyphosis)

When we table the results from the Naïve Bayes classifier with the original Kyphosis variable, it appears that 13 of 81 children are misclassified:

	table(kyphPred[["Kyphosis_Pred"]], kyphosis[["Kyphosis"]])

	          absent present
	  absent      59       8
	  present      5       9


### A Larger Naïve Bayes Classifier

As a more complex example, consider the mortgage default example. For that example, there are ten input files total and we use nine input data files to create the training data set. We then use the model built from those files to make predictions on the final dataset. In this section we will use the same strategy to build a Naïve Bayes classifier on the first nine data sets and assign the outcome variable for the tenth data set.

The mortgage default data sets are available for download [online](http://go.microsoft.com/fwlink/?LinkID=698896&clcid=0x409). With the data downloaded we can create the training data set and test data set as follows (remember to modify the first line to match the location of the mortgage default text data files on your own system):

	#  A Larger Naïve Bayes Classifier
	
	
	bigDataDir <- "C:/MRS/Data"
	mortCsvDataName <- file.path(bigDataDir, "mortDefault", "mortDefault")
	trainingDataFileName <- "mortDefaultTraining"
	mortCsv2009 <- paste(mortCsvDataName, "2009.csv", sep = "")
	targetDataFileName <- "mortDefault2009.xdf"
	defaultLevels <- as.character(c(0,1))
	ageLevels <- as.character(c(0:40))
	yearLevels <- as.character(c(2000:2009))
	colInfo <- list(list(name  = "default", type = "factor",
	    levels = defaultLevels), list(name = "houseAge", type = "factor",
	    levels = ageLevels), list(name = "year", type = "factor",
	    levels = yearLevels))
	append= FALSE
	for (i in 2000:2008)
	{
	    importFile <- paste(mortCsvDataName, i, ".csv", sep = "")
	    rxImport(inData = importFile, outFile = trainingDataFileName,
	    colInfo = colInfo, append = append, overwrite=TRUE)
	    append = TRUE
	}
	
	rxImport(inData = mortCsv2009, outFile = targetDataFileName, 
	    colInfo = colInfo)
	
In the above code the response variable *default* is converted to a factor using the *colInfo* argument to *rxImport*. For the *rxNaiveBayes* function, the response variable must be a factor or you will get an error.

Now that we have training and test data sets we can fit a Naïve Bayes classifier with our training data using *rxNaiveBayes* and assign values of the *default* variable for observations within the test data using *rxPredict*:

	mortNB <- rxNaiveBayes(default ~ year + creditScore + yearsEmploy + ccDebt,
		data = trainingDataFileName, smoothingFactor = 1)
	mortNBPred <- rxPredict(mortNB, data = targetDataFileName)

Notice that we added an additional argument, *smoothingFactor*, to our rxNaiveBayes call. This is a useful argument when your data are missing levels of a certain variable that you expect to be in your test data. Based on our training data, the conditional probability for year 2009 will be 0, since it only includes data between the years of 2000 and 2008. If we try to use our classifier on the test data without specifying a smoothing factor in our call to *rxNaiveBayes* the function *rxPredict* produces no results since our test data only has data from 2009. In general, smoothing is used to avoid overfitting your model. It follows that to achieve the optimal classifier you may want to smooth the conditional probabilities even if every level of each variable is observed.

We can compare the predicted values of the *default* variable from the Naïve Bayes classifier with the actual data in the test dataset:

	results <- table(mortNBPred[["default_Pred"]], rxDataStep(targetDataFileName, 
	    maxRowsByCols=6000000)[["default"]])
	results

	         0      1
	  0 877272   3792
	  1  97987  20949

	pctMisclassified <- sum(results[2:3])/sum(results)*100
	pctMisclassified

	  [1] 10.1779

These results demonstrate a 10.2% misclassification rate using our Naïve Bayes classifier.

### Naïve Bayes with Missing Data

You can control the handling of missing data using the *byTerm* argument in the *rxNaiveBayes* function. By default, *byTerm* is set to *TRUE*, which means that missings are removed by variable before computing the conditional probabilities. If you prefer to remove observations with missings in any variable before computations are done, set the *byTerm* argument to *FALSE*.
