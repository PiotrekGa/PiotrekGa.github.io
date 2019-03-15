---
published: false
---
## 2019-03-14 Pruned Cross-Validation

There are several reasons why you should use cross-validation. 
It helps you to access the quality of the model, optimize it's hyperparameters and test various architectures.
There is also variable number of reasons why you wouldn't - it's equal to the number of folds. 
For each fold the model has to be trained. With computation intensive algorithms and big datasets
it may occur to be a cumbersome endeavor. This was a challenge I had to face in order to optimize
hyperparameters of a stacked model. I used a Bayesian optimization package to do it (I described it in my
Towards Data Science [article](https://towardsdatascience.com/how-to-make-your-model-awesome-with-optuna-b56d490368af))
with 12 folds. The effect was a raging frustration.