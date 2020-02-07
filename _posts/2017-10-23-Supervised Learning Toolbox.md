---
layout: post
title: A Toolbox for Supervised Learning Algorithms

---

Supervised machine learning utilizes various algorithms in building predictive models.  While the work done under the hood is different between models, the API for training every machine learning model is essentially the same in scikit-learn:  
`model.fit(x,y)`  
`model.predict(x)`

This post is not meant to be an in-depth analysis of each algorithm and its inner workings.  Instead, my hope that this gives a better idea of which models to choose for certain datasets and constraints.  I'll discuss factors including:
- Scalability
- Computation and memory
- Interpretability

The following table summarizes the pros/cons of several core machine learning algorithms.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;}
.tg .tg-w7sd{font-weight:bold;background-color:#9b9b9b;color:#ffffff;text-align:center;vertical-align:top}
.tg .tg-yw4l{vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-w7sd">Algorithm</th>
    <th class="tg-w7sd">Pros</th>
    <th class="tg-w7sd">Cons</th>
    <th class="tg-w7sd">Notes</th>
  </tr>
  <tr>
    <td class="tg-yw4l">Linear Regression</td>
    <td class="tg-yw4l">Interpretable <br><br>Fast training/prediction time<br><br>Robust<br> <br>Structure is simple; just a single weight vector</td>
    <td class="tg-yw4l">Requires several assumptions about error values<br><br>Can't model complex, nonlinear relationships</td>
    <td class="tg-yw4l">As simple as regression models can get<br><br>Good for numerical data with lots of features</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Logistic Regression</td>
    <td class="tg-yw4l">Probabilistically interpretable<br><br>Fast training/prediction time</td>
    <td class="tg-yw4l">Not inherently multiclass; requires building multiple one-vs-all classifiers</td>
    <td class="tg-yw4l">A binary extension to the linear regression model</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Naive Bayes</td>
    <td class="tg-yw4l">Probabilistically interpretable<br><br>Fast training/prediction<br><br>Good with high-dimensional data</td>
    <td class="tg-yw4l">Independence between features is a VERY strong assumption</td>
    <td class="tg-yw4l">Good for text data</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Decision Tree</td>
    <td class="tg-yw4l">Interpretable<br><br>Scale invariant; data does not need to be normalized before training<br><br>Inherent feature selection and multiclass support</td>
    <td class="tg-yw4l">VERY prone to overfitting</td>
    <td class="tg-yw4l">Great for categorical data</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Random Forest</td>
    <td class="tg-yw4l">Can train multiple trees in parallel<br><br>Very good with categorical features</td>
    <td class="tg-yw4l">Multiple decision trees may be memory intensive<br><br>Lot of hyperparameters to tune<br><br>Harder to interpret than a single decision tree</td>
    <td class="tg-yw4l">An ensemble variant to the decision tree</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Gradient Boosting</td>
    <td class="tg-yw4l">Surprisingly effective at regression<br><br>Very good with categorical features</td>
    <td class="tg-yw4l">Prone to overfit<br><br>Hard to interpret</td>
    <td class="tg-yw4l">Another ensemble variant to the decision tree</td>
  </tr>
  <tr>
    <td class="tg-yw4l">K-Nearest Neighbors</td>
    <td class="tg-yw4l">Simple to interpret<br><br>No training time<br><br>Inherent multiclass support</td>
    <td class="tg-yw4l">Offloads all computation to testing<br><br>Memory intensive<br><br>Prediction time does not scale well with dimensions or number of training points</td>
    <td class="tg-yw4l">I do not recommend this algorithm in practice</td>
  </tr>
  <tr>
    <td class="tg-yw4l">SVM</td>
    <td class="tg-yw4l">Good in high-dimensional spaces<br><br>Memory efficient</td>
    <td class="tg-yw4l">Difficult to interpret<br><br>Computation doesn't scale well with larger datasets<br><br>Doesn't provide probability estimates<br><br>Doesn't handle overlap/noise well</td>
    <td class="tg-yw4l">Good for data with more features than training points</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Neural Network</td>
    <td class="tg-yw4l">Can model very complex relationships<br><br>Inherent multiclass support</td>
    <td class="tg-yw4l">Lot of hyperparameters to tune<br><br>Computationally expensive<br><br>Memory intensive<br><br>Impossible to interpret</td>
    <td class="tg-yw4l">Good for image/video/sound data</td>
  </tr>
</table>

This [flowchart](https://docs.microsoft.com/en-us/azure/machine-learning/studio/media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png) from Microsoft Azure also gives a good basic idea.
