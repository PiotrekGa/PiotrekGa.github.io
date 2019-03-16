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

Some of the trials took extremely long to go, and to my despair,
yielded much worse results than the ten time quicker ones. 
At some point I started to print each folds score, it's hyperparameters, 
and execution time just to watch it go and understand what should I change in the search space. 
As you can imagine I got frustrated by the fact that I was manually overseeing an automatic process! 
Something went wrong.

By looking at folds scores I realized that only with first 2-3 folds' scores I was able to tell whether 
the total score will be decent or not. 
So there were some very long trials which at fold 3 out of 12 I was sure would yield poor results. 
As you can imagine that made me think what would happen if I just stopped after this first folds, 
evaluate results and go on if the trial is promising or prune it if it's not.
 But it's not a proper cross-validation, right?