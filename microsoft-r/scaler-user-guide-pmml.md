---

# required metadata
title: "RevoScaleR User's Guide--Converting RevoScaleR Model Objects for Use in PMML"
description: "Using PMML with RevoScaleR model objects."
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

# Converting RevoScaleR Model Objects for Use in PMML

The objects returned by RevoScaleR predictive analytics functions have similarities to objects returned by the predictive analytics functions provided by base and recommended packages in R. A key difference between these two types of model objects is that RevoScaleR model objects typically do not contain any components that have the same length as the number of rows in the original data set. For example, if you use base R's lm function to estimate a linear model, the result object contains not only all of the data used to estimate the model, but components such as residuals and fitted.values that contain values corresponding to every observation in the data set. For estimating models using big data, this is not appropriate.

However, there is overlap in the model object components that can be very useful when working with other packages. For example, the *pmml* package will generate PMML (Predictive Model Markup Language) code for a number of R model types. PMML is an XML-base language which allows users to share models between PMML compliant applications. For more information about PMML, visit <http://www.dmg.org> .

RevoScaleR provides a set of coercion methods to convert a RevoScaleR model object to a standard R model object: *as.lm*, *as.glm*, *as.rpart*, and *as.kmeans*. As suggested above, these coerced model objects do not have all of the information available in a standard R model object, but do contain information about the fitted model that is similar to standard R.

For example, let’s create an rxLinMod object by estimating a linear regression on a very large dataset: the airline data with almost 150 million observations.

	######################################################## 
	# Chapter 17: Converting RevoScaleR Model Objects for
	#  Use in PMML
	########################################################
	Ch17Start <- Sys.time()

	bigDataDir <- "C:/MRS/Data"
	bigAirData <- file.path(bigDataDir, "AirOnTime87to12/AirOnTime87to12.xdf")	

	# About 150 million observations
	rxLinModObj <- rxLinMod(ArrDelay~Year + DayOfWeek, data = bigAirData, 
	    blocksPerRead = 10)

>The `blocksPerRead` argument is ignored if run locally using R Client. [Learn more...](scaler-getting-started.md#chunking)

Next, the R *pmml* package should be downloaded and installed from CRAN. After you have done so, generate the RevoScaleR *rxLinMod* object, and use the *as.lm* method when generating the PMML output:

	library(pmml)
	pmml(as.lm(rxLinModObj))

The generated output looks as follows:

	> pmml(as.lm(rxLinModObj))
	<PMML version="4.1" xmlns="http://www.dmg.org/PMML-4_1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.dmg.org/PMML-4_1 http://www.dmg.org/v4-1/pmml-4-1.xsd">
	 <Header copyright="Copyright (c) 2013 sue" description="Linear Regression Model">
	  <Extension name="user" value="yourname" extender="Rattle/PMML"/>
	  <Application name="Rattle/PMML" version="1.3"/>
	  <Timestamp>2013-09-05 11:39:28</Timestamp>
	 </Header>
	 <DataDictionary numberOfFields="3">
	  <DataField name="ArrDelay" optype="continuous" dataType="double"/>
	  <DataField name="Year" optype="continuous" dataType="double"/>
	  <DataField name="DayOfWeek" optype="categorical" dataType="string">
	   <Value value="Mon"/>
	   <Value value="Tues"/>
	   <Value value="Wed"/>
	   <Value value="Thur"/>
	   <Value value="Fri"/>
	   <Value value="Sat"/>
	   <Value value="Sun"/>
	  </DataField>
	 </DataDictionary>
	 <RegressionModel modelName="Linear_Regression_Model" functionName="regression" algorithmName="least squares" targetFieldName="ArrDelay">
	  <MiningSchema>
	   <MiningField name="ArrDelay" usageType="predicted"/>
	   <MiningField name="Year" usageType="active"/>
	   <MiningField name="DayOfWeek" usageType="active"/>
	  </MiningSchema>
	  <Output>
	   <OutputField name="Predicted_ArrDelay" feature="predictedValue"/>
	  </Output>
	  <RegressionTable intercept="112.856953212332">
	   <NumericPredictor name="Year" exponent="1" coefficient="-0.0533799688606163"/>
	   <CategoricalPredictor name="DayOfWeek" value="Mon" coefficient="0.303666227836942"/>
	   <CategoricalPredictor name="DayOfWeek" value="Tues" coefficient="-0.593700918634743"/>
	   <CategoricalPredictor name="DayOfWeek" value="Wed" coefficient="0.473693749859764"/>
	   <CategoricalPredictor name="DayOfWeek" value="Thur" coefficient="2.3393087933048"/>
	   <CategoricalPredictor name="DayOfWeek" value="Fri" coefficient="2.91671831592945"/>
	   <CategoricalPredictor name="DayOfWeek" value="Sat" coefficient="-2.31671307739631"/>
	   <CategoricalPredictor name="DayOfWeek" value="Sun" coefficient="0"/>
	  </RegressionTable>
	 </RegressionModel>
	</PMML>

Similarly, rxLogit and rxGlm functions have as.glm methods:

	form <- case ~ age + parity + spontaneous + induced
	rxLogitObj <- rxLogit(form, data =infert)
	pmml(as.glm(rxLogitObj))
	    
	rxGlmObj <- rxGlm(form, data=infert, family = binomial())
	pmml(as.glm(rxGlmObj))


RevoScaleR's *rxKmeans* objects can be coerced to *kmeans* objects:

	set.seed(17)
	irow <- unique(sample.int(nrow(women), 4L, 
	    replace = TRUE))[seq(2)]
	centers <- women[irow,, drop = FALSE]
	rxKmeansObj <- rxKmeans(~height + weight, data = women, 
	    centers = centers)
	pmml(as.kmeans(rxKmeansObj))

And RevoScaleR's *rxDTree* objects can be coerced to *rpart* objects. (Note that *rpart* is a recommended package that you may need to load.)

	library(rpart)
	method <- "class"
	form <- Kyphosis ~ Number + Start
	parms <- list(prior = c(0.8, 0.2), loss = c(0, 2, 3, 0), 
	    split = "gini")
	control <- rpart.control(minsplit = 5, minbucket = 2, cp = 0.01, 
	    maxdepth = 10, maxcompete = 4, maxsurrogate = 5, 
	    usesurrogate = 2, surrogatestyle = 0, xval = 0)
	cost <- 1 + seq(length(attr(terms(form), "term.labels")))
	   
	rxDTreeObj <- rxDTree(formula = Kyphosis ~ Number + Start, 
	    data = kyphosis, method = method, parms = parms, 
	    control = control, cost = cost)      
	pmml(as.rpart(rxDTreeObj))

