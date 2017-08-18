---

# required metadata
title: "Solution templates | Machine Learning Server | Microsoft Docs"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "08/05/2017"
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

# Solution templates for Machine Learning Server and Microsoft R Server

Several solution templates exist to help you accelerate time to value when using Machine Learning Server (or Microsoft R Server).

We offer solutions tailored to specific industries and problem sets, such as:
+ Predicting Length of Stay in Hospitals

+ Campaign Optimization with SQL Server

+ Campaign Optimization with Azure HDInsight Spark Clusters

+ Loan Credit Risk with SQL Server

Explore machine learning how-to solutions at https://aka.ms/rsolutiontemplates.


 
## Marketing solutions 

**Campaign Optimization** (R-based):  

+ **Solution page**: https://microsoft.github.io/r-server-campaign-optimization
+ **Source code**: https://github.com/Microsoft/r-server-campaign-optimization
+ **Summary**: When a business launches a marketing campaign to interest customers in new or existing product(s), they often use a set of business rules to select leads for their campaign to target. Machine learning can be used to help increase the response rate from these leads. 
  
  This solution demonstrates how to use a model to predict actions that are expected to maximize the purchase rate of leads targeted by the campaign. These predictions serve as the basis for recommendations to be used by a renewed campaign on how to contact (for example, e-mail, SMS, or cold call) and when to contact (day of week and time of day) the targeted leads.

+ **Deploy on Cortana Gallery**:
  + [SQL Server](https://aka.ms/campaignoptimization)     
  + [HDInsight Spark Cluster](https://aka.ms/campaign-hdi)

## Medical solutions

**Predicting Hospital Length of Stay**

+ **Summary**: This solution enables a predictive model for Length of Stay for in-hospital admissions. Length of Stay (LOS) is defined in number of days from the initial admit date to the date that the patient is discharged from any given hospital facility. There can be significant variation of LOS across various facilities and across disease conditions and specialties even within the same healthcare system. Advanced LOS prediction at the time of admission can greatly enhance the quality of care as well as operational workload efficiency and help with accurate planning for discharges resulting in lowering of various other quality measures such as readmissions.

+ **Solution page**: https://microsoft.github.io/r-server-hospital-length-of-stay

+ **Source code**: https://github.com/Microsoft/r-server-hospital-length-of-stay

+ **Deploy on Cortana Gallery**:
  + [SQL Server](https://aka.ms/hospital-los)

## Financial Solutions

**Loan Credit Risk**

If we had a crystal ball, we would only loan money to someone if we knew they would pay us back. A lending institution can make use of predictive analytics to reduce the number of loans it offers to those borrowers most likely to default, increasing the profitability of its loan portfolio. This solution is based on simulated data for a small personal loan financial institution, containing the borrower’s financial history as well as information about the requested loan. It uses predictive analytics to help decide whether or not to grant a loan for each borrower.

+ **Solution page**: https://microsoft.github.io/r-server-fraud-detection/

+ **Source code**: https://github.com/Microsoft/r-server-fraud-detection

+ **Deploy on Cortana Gallery**:
  + [SQL Server](https://aka.ms/loan-credit-risk 
HDInsight Spark Cluster https://aka.ms/loan-credit-risk-hdi 

**Loan ChargeOff**

+ **Summary**: 
  A charged off loan is a loan that is declared by a creditor (usually a lending institution) that an amount of debt is unlikely to be collected, usually when the loan repayment is severely delinquent by the debtor. Given that high chargeoff has negative impact on lending institutions’ year end financials, lending institutions often monitor loan chargeoff risk very closely to prevent loans from getting charged-off. Using Azure HDInsight R Server, a lending institution can leverage machine learning predictive analytics to predict the likelihood of loans getting charged off and run a report on the analytics result stored in HDFS and hive tables.

+ **Solution page**: https://microsoft.github.io/r-server-loan-chargeoff

+ **Source code**: https://github.com/Microsoft/r-server-loan-chargeoff/

+ **Deploy on Cortana Gallery**:
  + [SQL Server](https://aka.ms/loanchargeoffsql)
  + [HDInsight Spark Cluster](https://aka.ms/loanchargeoffhdi)

### Fraud Detection

Fraud detection is one of the earliest industrial applications of data mining and machine learning. This solution shows how to build and deploy a machine learning model for online retailers to detect fraudulent purchase transactions.

**Solution page**: https://microsoft.github.io/r-server-fraud-detection/

**Source code**: https://github.com/Microsoft/r-server-fraud-detection

**Deploy on Cortana Gallery**:
  + [SQL Server](https://aka.ms/fraud-detection)
  + [HDInsight Spark Cluster](https://aka.ms/fraud-detectinon-hdi)
