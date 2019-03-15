---
published: true
---
## 2019-03-14 Pruned Cross-Validation

There are several reasons why you should use cross-validation. 
It helps you to access the quality of the model, optimize it's hyperparameters and test various
architectures.
There is also variable number of reasons why you wouldn't - it's equal to the number of folds. 
For each fold the model has to be trained. With computation intensive algorithms and big datasets
it may occur to be a cumbersome endeavor.