--- 
 
# required metadata 
title: "MISSING TITLE" 
description: "" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "HeidiSteen" 
manager: "" 
ms.date: "06/21/2017" 
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

# BaseLearner

class microsoftml.modules.base_learner.BaseLearner(**kwargs)


base class for all the learners

coef_


get model coefficients

fit(formula, data, ml_transforms=None, ml_transform_vars=None, row_selection=None, transforms=None, transform_objects=None, transform_func=None, transform_vars=None, transform_packages=None, transform_envir=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>, **kargs)


fit the model

get_algo_args()


get algorithm arguments

get_model_summary()


get model summary

get_train_node(**kargs)


get train node

predict(data, out_data=None, write_model_vars=False, extra_vars_to_write=None, suffix=None, overwrite=False, data_threads=None, blocks_per_read=1, report_progress=2, verbose=1, compute_context=<revoscalepy.computecontext.RxLocalSeq.RxLocalSeq object>)

