---
published: false
---
# 2020-06-06 Mixture Transition Distribution
_Model, usage and example_

## Intention

The intention of the article is to outline the Mixture Transition Distribution model.
A reader will find here definitions of Markov Chain and MTD models and presentation of a Python package 
[mtd-learn](https://github.com/PiotrekGa/mtd-learn) for estimating them. This post does not intend to be 
exhaustive on the subject. References to more detailed sources will be listed at the end of the post.


Note to R users. There is a R package [march](https://cran.r-project.org/web/packages/march/) developed and
maintained by Andre Berchtold which can be used for estimation of the MTD models.

## Introduction

The Mixture Transition Distribution (MTD) model was proposed by Raftery in 1985<sup>[1]</sup>. Its initial intent of 
was to approximate high order Markov Chains (MC), but it can serve as an independent model too. The main advantage of 
the MTD model is that its number of independent parameters of the MTD model grows linearly with the order comparing with 
exponential growth of Markov Chains models.


### Model definition

#### Markov Chains recap

#### First-order Markov Chains

First order Markov Chain is a sequence of random variables _(X<sub>n</sub>)_ such that:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn.png">
</p>

such that:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn1.png">
</p>

for every _i<sub>t</sub>...i<sub>0</sub> ∈ S_, where _S_ is a state space.

The probability can be written in an abbreviated form:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn3.png">
</p>

If we assume that for every _t_:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn4.png">
</p>

we obtain time-homogeneous Markov Chain.

A transition matrix of a first order Markov Chain is build of all possible combination of process states _i<sub>0</sub>_
and _i<sub>1</sub>_: 

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn5.png">
</p>

_Q_ is a stochastic matrix. It means that all its values are [0, 1] and sum of every row equals 1.

#### High-order Markov Chains

A _l_-order Markov Chain, is a stochastic process in which current state depends on _l_ last observations:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn6.png">
</p>

and

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn7.png">
</p>

such that:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn8.png">
</p>

for every _i<sub>t</sub>...i<sub>0</sub> ∈ S_, where _S_ is a state space.


It is possible to represent a high-order Markov Chain as a first-order Markov Chain. Some probabilities in the 
transition matrix _Q_ will be equal to zero by definition (structural zeros). 
Below is an example od 2-order MC with 3 possible states:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn9.png">
</p>

where

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn10.png">
</p>

If we remove structural zeros from the _Q_ matrix we obtain its reduced form:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn11.png">
</p>

Please note, that the representation of _Q_ and _R_ is slightly different that the one from original paper<sup>[2]</sup>. 
It's due to implementation convenience of the Python package `mtd-learn`.

The umber of independent parameters of high-order Markov Chain is equal to _m<sup>l</sup>(m-1)_, where _m_ represents
number of states and _l_ is the order of the model.

Rw log likelihood of the Markov Chain model is given by formula:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn19.png">
</p>

where _n<sub>i<sub>l</sub>...i<sub>0</sub></sub>_ means number of transitions of type 
_X<sub>t-l</sub> = i<sub>l</sub>,...,X<sub>t-1</sub> = i<sub>1</sub>,X<sub>t</sub> = i<sub>0</sub>_ in a dataset.

The maximum likelihood estimator of a transition _q<sub>i<sub>l</sub>...i<sub>0</sub></sub>_ is:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn20.png">
</p>

where 

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn21.png">
</p>

### MTD models

#### Mixture Transition  Distribution model

The MTD model is a sequence of random variables _(X<sub>n</sub>)_ such that:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn12.png">
</p>

where _i<sub>t</sub>...i<sub>0</sub> ∈ N_, probabilities _q<sub>i<sub>l</sub>i<sub>0</sub></sub>_ are elements of a
_m ⨯ m Q_ matrix and _𝜆  = (𝜆 <sub>l</sub>,...,𝜆 <sub>1</sub>)<sup>T</sup>_ is a weight vector.

Following conditions has to be met for the model to produce probabilities:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn13.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn14.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn15.png">
</p>

The log-likelihood function of the MTD model is given by:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn22.png">
</p>

where _n<sub>i<sub>l</sub>...i<sub>0</sub></sub>_ means number of transitions of type 
_X<sub>t-l</sub> = i<sub>l</sub>,...,X<sub>t-1</sub> = i<sub>1</sub>,X<sub>t</sub> = i<sub>0</sub>_ in a dataset.

#### Generalized Mixture Transition  Distribution model

The MTDg model is a sequence of random variables _(X<sub>n</sub>)_ such that:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn16.png">
</p>

where _i<sub>t</sub>...i<sub>0</sub> ∈ N_, 𝜆  = (𝜆 <sub>l</sub>,...,𝜆 <sub>1</sub>)<sup>T</sup>_ is a weight vector and
_Q<sub>g</sub> = [q<sup>(g)</sup><sub>i<sub>g</sub>i<sub>0</sub></sub>]_ is a _m ⨯ m_ matrix representing association
between _g_ lag and the current state.

Following conditions has to be met for the model to produce probabilities:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn17.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn18.png">
</p>

The log-likelihood function of the MTDg model is given by:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn23.png">
</p>

where _n<sub>i<sub>l</sub>...i<sub>0</sub></sub>_ means number of transitions of type 
_X<sub>t-l</sub> = i<sub>l</sub>,...,X<sub>t-1</sub> = i<sub>1</sub>,X<sub>t</sub> = i<sub>0</sub>_ in a dataset.iizbo

### MTD models intuition

You can think about MTDg model as a weighted average of transition probabilities from various orders.  The example below 
shows how to calculate a probability of transition B->C->A->B from an order 3 MTDg model:

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/mtd.png">
</p>

In case of the MTD model all the Q<sup>(1)</sup>, Q<sup>(2)</sup>, and Q<sup>(3)</sup> matrices would be the same.

### Number of independent parameters

According to [1] the number independent parameters of the MTDg model equals _lm(m-1) + l - 1_. In [2] Lebre and 
Bourguignon proved that the true number of independent parameters equals _(ml - m + 1)(l - 1)_. Since the `mtd-learn`
package uses the estimation method proposed in [2] the number of parameters is calculated with the latest formula.

For Markov Chain the number of parameters equals _m<sup>l</sup>(m-1)_. 
You can find a comparison of the number of parameters below:

| States   |      Order    | Markov Chain | MTDg<sup>[1]</sup>  | MTDg|
|----------|:-------------:|-------------:|--------------------:|----:|
| 2        | 1             |     2        | 2                   | 2   |
| 2        | 2             |     4        | 3                   | 5   |
| 2        | 3             |     8        | 4                   | 8   |
| 2        | 4             |    16        | 5                   | 11  |
| 3        | 1             |     6        | 6                   | 6   |
| 3        | 2             |    18        | 10                  | 13  |
| 3        | 3             |    54        | 14                  | 20  |
| 3        | 4             |   162        | 18                  | 27  |
| 5        | 1             |    20        | 20                  | 20  |
| 5        | 2             |   100        | 36                  | 41  |
| 5        | 3             |   500        | 52                  | 62  |
| 5        | 4             |  2500        | 68                  | 83  |
| 10       | 1             |    90        | 90                  | 90  |
| 10       | 2             |   900        | 171                 | 181 |
| 10       | 3             |  9000        | 252                 | 272 |
| 10       | 4             | 90000        | 333                 | 363 |

## Information criteria

To determine the proper order of the MTD / MTDg model you can use one of the two information criteria - 
Akaike's and Bayesian (know also as Schwarz):

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn24.png">
</p>

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn25.png">
</p>

The first part of each equation is model's log-likelihood _ln(L)_. The second one is a penalty for number of independent 
parameters. In case of BIC number of samples is also taken into consideration.

In the `mtd-learn` package you can access then with `MTD.aic` and `MTD.bic` properties. 
You should choose a model with the minimal value of the chosen criterion.

## Implementation details

### Estimation algorithm

You can find the Python implementation of the model here: [mtd-learn](https://github.com/PiotrekGa/mtd-learn). The
models are estimated using a version of EM algorithm proposed by Lebre and Bourguignon in [2]. Explanation of the 
algorithm is out of scope of the post. For details please refer to the original article.

### Number of independent parameters

The number of independent parameters for MTDg models is calculated with the formula _(ml - m + 1)(l - 1)_ following [2].

### Output of the model

#### Transition matrices and lambda vector

The `mtd-learn` package returns _Q_ matrices and the 𝜆 vector in following order:
```
mtd.transition_matrices.round(3) = array([[[0.521, 0.479], # Q3
                                           [0.404, 0.596]],
                                   
                                          [[0.254, 0.746], # Q2
                                           [0.569, 0.431]],
                                   
                                          [[0.797, 0.203], # Q1
                                           [0.542, 0.458]]])

#                            lambda3, lambda2, lambda1
mtd.lambdas.round(3) = array([0.082,   0.122,   0.796])

```

#### Reconstructed MC matrices

Based on the MTDg model it's possible to construct a MC transition matrix. The matrix looks like follows:

```
mtd.transition_matrix.round(3) = array([[0.708, 0.292],
                                        [0.505, 0.495],
                                        [0.746, 0.254],
                                        [0.543, 0.457],
                                        [0.698, 0.302],
                                        [0.495, 0.505],
                                        [0.737, 0.263],
                                        [0.534, 0.466]])
```

The expanded version of the matrix (simulating first order MC from third order MC) looks as follows:

```
mtd.expanded_matrix.round(3) = array([[0.708, 0.292, 0.   , 0.   , 0.   , 0.   , 0.   , 0.   ],
                                      [0.   , 0.   , 0.505, 0.495, 0.   , 0.   , 0.   , 0.   ],
                                      [0.   , 0.   , 0.   , 0.   , 0.746, 0.254, 0.   , 0.   ],
                                      [0.   , 0.   , 0.   , 0.   , 0.   , 0.   , 0.543, 0.457],
                                      [0.698, 0.302, 0.   , 0.   , 0.   , 0.   , 0.   , 0.   ],
                                      [0.   , 0.   , 0.495, 0.505, 0.   , 0.   , 0.   , 0.   ],
                                      [0.   , 0.   , 0.   , 0.   , 0.737, 0.263, 0.   , 0.   ],
                                      [0.   , 0.   , 0.   , 0.   , 0.   , 0.   , 0.534, 0.466]])
```

With states indexes:

```
        t-3, t-2, t-1
IDX = [( 0,   0,   0 ),
       ( 0,   0,   1 ),
       ( 0,   1,   0 ),
       ( 0,   1,   1 ),
       ( 1,   0,   0 ),
       ( 1,   0,   1 ),
       ( 1,   1,   0 ),
       ( 1,   1,   1 )]
```

For example, if we would like to find a transition probabilities after 1->0->1, we have to choose row 6 of R (`[0.495, 0.505]`) 
and Q (`[0.   , 0.   , 0.495, 0.505, 0.   , 0.   , 0.   , 0.   ]`).

## Usage example

## Summary

## Final notes
LaTeX formulas were generated with [latex.codecogs.com](https://www.codecogs.com/latex/eqneditor.php)

## Bibliography
1. BERCHTOLD, RAFTERY, The mixture Transition Distribution Model for High-Order Markov Chains
and Non-Gaussian Time Series , 2002., Statistical Science Vol. 17, No. 3, 328-356
2. LEBRE, BOURGUIGNON, An EM algorithm for estimation in the Mixture Transition Distribution
model , Laboratoire Statistique et Genome, Universite Evry Val d'Essonne, Evry, 2009
3. HUDSON, WON SUN KIM, KEATLEY, Series of Discrete State Processes: With an Application to Modelling Flowering 
Synchronisation with Respect to Climate Dynamics, 2019