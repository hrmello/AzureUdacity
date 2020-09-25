# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
**In 1-2 sentences, explain the problem statement: e.g "This dataset contains data about... we seek to predict..."**
This dataset contains data about a bank marketing campaign, which will be used to predict whether a client will enroll in a marketing promotion. 

**In 1-2 sentences, explain the solution: e.g. "The best performing model was a ..."**
We have used both Logistic Regression and Voting Ensemble(using AutoML) to make the predictions. They had the same performance, 91% of accuracy. 

## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**

After feeding the algorithm with data, we preprocessed it to one-hot-encode categorical variables:
- marital
- default
- housing
- loan
- y (the target variable)
- education
- contact

and changed other two categorical variables to numeric:
- month
- day_of_week

After that, we split the dataset into train and test sets (67% of the data for training and 33% for test) and used Azure to select multiple configurations of Logistic Regression's parameters C and max_iter to check which one was the best.

**What are the benefits of the parameter sampler you chose?**
The sampler used was the RandomParameterSampling class. Using it, the parameters are chosen at random from a grid previously specified and the algorithm learns using them. I've select a few discrete values for each parameter:
- C: 0.01, 0.1, 1
- max_iter: 100, 200, 300

**What are the benefits of the early stopping policy you chose?**
If the algorithm is not performing, early stopping is called to reduce resources' consumption and use those resources to explore other parameters in the grid previously specified. In this case, Bandit Policy was chosen. This policy early terminates any runs where the primary metric (here, the accuracy) is not within the specified slack factor / slack amount with respect to the best performing training run.
 

## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**
AutoML's best run was using Voting Ensemble. 

## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**
Voting Ensemble and Logistic Regession's both were 0.91. Their architecture are very different, as the former is based on classification trees and voting, and the other one is based on regression: totally different techniques. The results are about the same probably because the dataset is a very good one, with enough observations of both classes of the target variable that any algorithm could reach the highest possible accuracy without overfitting. 

## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**
That fact that both models have the same accuracy suggest that this would be around the limit of the possible highest accuracy for that specific dataset. If there is improvement to be done, I think it would be by getting more data. As always, the more data one has the more improved can become their model. 
Another possible improvement would be to use Deep Learning. However, by doing so the explainability of the model will become compromised and one has to decide if it is worthy to lose explanatory power to increase two or three percentage points in model's accuracy.

## Proof of cluster clean up
Cluster was deleted in the notebook. It is the last code cell.
