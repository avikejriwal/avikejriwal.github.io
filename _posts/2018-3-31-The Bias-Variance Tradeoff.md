---
layout: post
title: The Bias-Variance Tradeoff

---

The Bias-Variance Tradeoff is a fairly prevalent problem in machine learning and predictive modeling.  At a high level, it can be summarized in a brief story called "Goldilocks and the Three Data Scientists":  
- "This model is too simple."  
- "This model is too complex."  
- "This model is JUST right."  

The tradeoff describes the effects of model complexity on performance, and the consequences of not having a good balance.  Andrew Ng's Coursera lecture goes into more detail [here](https://www.coursera.org/learn/machine-learning/lecture/4VDlf/regularization-and-bias-variance).

Model complexity can take a number of forms, and factors that contribute to complexity vary from model to model, but one consistent factor is the number of training features.  Here are some examples of factors that affect complexity for machine learning models:
- Linear/Logistic Regression: Polynomial order of features
- Random Forest: Depth of trees
- Gradient Boosting: Number of trees
- Support Vector Machine: Kernel function

A low-level formula, as well as its derivation, can be found [here](https://artofsoftware.org/2012/09/13/derivation-of-bias-variance-decomposition/).

This chart from scikit-learn demonstrates the tradeoff in the context of regression models with polynomial feature transformations (using the nth power of the x value as a feature).  The left shows a model with high bias, and the right shows a model with high variance.

<img src="http://scikit-learn.org/stable/_images/sphx_glr_plot_underfitting_overfitting_0011.png">

__Bias__: Error due to assumptions in the model (underfitting).  In this case, the model is not complex enough to capture all of the meaningful variations in the data.  This can be identified by high training AND testing error.  

Unfortunately, in this case it can be hard to distinguish whether the error is due to deficiencies in the model, or due to deficiencies in the data itself.  It's possible that the training features don't capture enough information to make accurate predictions.

__Variance__: Error due to variations in the data (overfitting).  In the case of high variance, a model is too complex, and fits fluctuations in the data too closely.  As a result, it doesn't generalize well outside of the training dataset.  This can be identified by a testing error that is notably higher than the training error.

Generally in practice, testing error will be higher than training error, due to the fact that the model is trying to predict points it's never seen before.  Though a small margin, say a difference of 2% in classification error, is not indicative of overfitting.



If you feel like diving further, [this article](https://medium.com/mlreview/making-sense-of-the-bias-variance-trade-off-in-deep-reinforcement-learning-79cf1e83d565) goes into more detail about the tradeoff in the context of other topics in AI and Machine Learning.
