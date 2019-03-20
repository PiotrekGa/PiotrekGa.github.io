---
published: false
---
## 2019-03-14 Pruned Cross-Validation

### Introduction

There are several reasons why you should use cross-validation. 
It helps you to access the quality of the model, optimize its hyperparameters and test various architectures. 
There is also a variable number of reasons why you wouldn't - it's equal to the number of folds. 
For each fold, the model has to be trained. With computation intensive algorithms and big datasets, 
it may occur to be a cumbersome endeavor. 
That was a challenge I had to face to optimize hyperparameters of a stacked model.
I used a Bayesian optimization package to do it (I described it in my 
Towards Data Science [article](https://towardsdatascience.com/how-to-make-your-model-awesome-with-optuna-b56d490368af))
with 12 folds. The effect was a raging frustration.

Some of the trials took extremely long to go, and to my despair, 
yielded much worse results than the ten time quicker ones. 
At some point I started to print each folds score, it's hyperparameters, and execution time only to watch 
it go and understand what should I change in the search space. 
As you can imagine I got frustrated by the fact that I was manually overseeing an automatic process! 
Something went wrong.

By looking at folds scores, I realized that only with first 2-3 folds' scores I was able to tell whether the 
total score will be decent or not. 
So there were some very long trials which at fold 3 out of 12 I was sure would yield poor results. 
As you can imagine that made me think what would happen if I just stopped after this first folds, 
evaluate results and go on if the trial is promising or prune it if it's not. 
But it's not proper cross-validation, right?

### Cross-validation recap

Cross-validation is a technique of model's quality. First you should choose a metric, which would represent the quality
of your model. Then you split your training dataset into _n_ number of subsets, called folds (note you should use
stratification if possible to ensure similar distributions in each set). 
Then you should train your model _n_ times at each time the training set be composed of _n - 1_ subsets. 
The remaining one be treated as a validation set to evaluate the metric on. 
Then the scores are averaged across the folds as presented as the final metric.

The technique allows you to verify you're model's quality of the whole dataset available and therefore present the best
possible predictions of its performance in unseen data. Its main disadvantage is a necessity of training the same model
_n_ times. In the hyperparameters optimization case many folds are required and therefore the computation time if high.

### Pruning idea

As you can imagine scores from the folds and the final score are dependent on each other. I've made some simulations 
studies to evaluate the correlation between cumulative metrics value after each fold and the final score.
As you can see the correlation with the final score rises very fast with subsequent folds reaching 0.98 on fold 3
out of 8. You can find a broader study of correlations distributions on the graph below.

![Correlations](https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/correlations.png)

The idea of pruned cross-validation is based on the high correlations and our ability to partially assess the 
hyperparameters set without calculating all the folds.

### The pruned cross-validation algorithm

The algorithm is base on deterministic comparison between equivalent folds' scores. There's also a probabilistic 
version, but it's beyond scope of this post.

Parameters:
* _n_ - number of folds (integer, >= 2)
* _t_ - tolerance (float, >=0.0, default=0.1)
* _k_ - fold number at which first pruning may happen (integer, <= _n_, default=2)

1. Define a model, a hyperparameters space and pruning parameters
1. Choose an initial hyperparameters set to evaluate
1. Calculate full cross-validation, save scores for all folds and the final score
1. Choose hyperparamterse set to evaluate
1. Calculate fold's score
    * if number of fold is lower than _k_, got to point 5.
    * if number of folds is equal to _n_ calculate the final score
        * If the score is lower than the best score so far, set it's hyperparameters and best scores as the best one
1. Evaluate whether current trial's mean score is below mean value of the best trial's scores (the same number as the 
current trial) multiplied by (1 + _t_)
    * If yes, got to point 5.
    * Prune the trial, estimate the final score and go to point 4. otherwise
    
The algorithm ensures that the best hyperparameters set is validation on all the folds, but it does not guarantee to
indicate the best hyperparameters out of evaluated ones. The model can strongly underperform on initial folds and
outperform on the latter ones. Even with medium sized datasets and proper data shuffle it's highly unlikely.

### Speed benchmarking

The main advantage of the pruned cross-validation is search speed increase. If the hyperparameters set yields poor
results the cross-validation is pruned and therefore time and computation resources are saved.

Below you can find a comparison between normal grid search and pruned grid search:

![GridSearch vs PrunedGridSearch](https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/gs_vs_pgs.png)

Grid Serach with pruned cross-validation was over 3 times faster than the traditional full validation search. The code to the experiment may be found in this [notebook](https://github.com/PiotrekGa/pruned-cv/blob/master/examples/GridSearchCV_Benchmark.ipynb).

#### Lower and higher speed bonds compared to full cross-validation

The current implementation of the algorithm is based on simple lists operations, so it's computation cost may be considered non-existent. Because of that the upper boundary of the time is equal to the time needed for full cross-validation. The lower boundary is equal to computing full cross-validation in the first trial and _k / n_ foldsin following ones, where _k_ is the first folds do try pruning and _n_ is the number of folds for cross-validation. With hight number of trials the value will converge to _k / n_.

### Implementation

The method was implemented in `pruned-cv` Python package (you can find it [here](https://github.com/PiotrekGa/pruned-cv)). Two search algorithms were implemented: ` PrunedGridSearchCV` and `PrunedRandomizedSearchCV`. They have similar API as scikit-learn implementations of `GridSearchCV` and `RandomizedSearchCV`. They don't offer final model refit though. Please refer to docstrings for more details.

The package provides also `PrunedCV` object. It's the working horse of the package and can be used with other search algorithms like Bayesian Hyperparameter Optimization. Below you can find a pseudocode example with Optuna package:

```
import optuna
from prunedcv import PrunedCV

prun = PrunedCV(12, 0.1)

def objective(trial):

	params = choose parameters for the trial
    model.set_params(**params)

    return prun.cross_val_score(model, x, y)

study = optuna.create_study()
study.optimize(objective, timeout=120)
```
You can find a benchmarking notebook with Optuna [here](https://github.com/PiotrekGa/pruned-cv/blob/master/examples/Usage_with_Optuna.ipynb) and Hyperopt [here](https://github.com/PiotrekGa/pruned-cv/blob/master/examples/Usage_with_Hyperopt.ipynb). In the examples provided pruning limited trial's duration 2.8 and 5.7 times respectively leading to better final scores in both cases.