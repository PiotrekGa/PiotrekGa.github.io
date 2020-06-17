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
_X<sub>t-l</sub> = i<sub>l</sub>,...,X<sub>t-1</sub> = i<sub>1</sub>,X<sub>t</sub> = i<sub>0</sub>_ in a dataset.

## Information criteria

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn24.png">
</p>

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn25.png">
</p>

## Implementation

## Usage example

## Summary

## Final notes
LaTeX formulas were generated with [latex.codecogs.com](https://www.codecogs.com/latex/eqneditor.php)

## Bibliography
1. BERCHTOLD, RAFTERY, The mixture Transition Distribution Model for High-Order Markov Chains
and Non-Gaussian Time Series , 2002., Statistical Science Vol. 17, No. 3, 328-356
2. LEBRE, BOURGUIGNOM, An EM algorithm for estimation in the Mixture Transition Distribution
model , Laboratoire Statistique et Genome, Universite Evry Val d'Essonne, Evry, 2009
3. HUDSON, WON SUN KIM, KEATLEY, Series of Discrete State Processes: With an Application to Modelling Flowering 
Synchronisation with Respect to Climate Dynamics, 2019