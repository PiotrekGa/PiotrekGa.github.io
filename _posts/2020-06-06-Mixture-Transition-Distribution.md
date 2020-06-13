---
published: false
---
## 2020-06-06 Mixture Transition Distribution
_Model, usage and example_

### Introduction

The Mixture Transition Distribution (MTD) model was proposed by Raftery in 1985<sup>[1]</sup>. Its initial intent of 
was to approximate higher order Markov Chains (MC), but it can serve as an independent model too. The main advantage of 
the MTD model is that its number of independent parameters of the MTD model grows linearly with the order comparing with 
exponential growth of Markov Chains models.

The aim of the post is to introduce the definitions of Markov Chain and MTD models and present a Python package 
[mtd-learn](https://github.com/PiotrekGa/mtd-learn) for estimating them.

Note to R users. There is a R package [march](https://cran.r-project.org/web/packages/march/) developed and
maintained by Andre Berchtold which can be used for estimation of the MTD models.

### Model definition

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn.png">
</p>

<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn1.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn2.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn3.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn4.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn5.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn6.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn7.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn8.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn9.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn10.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn11.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn12.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn13.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn14.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn15.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn16.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn17.png">
</p>


<p align="center">
  <img src="https://github.com/PiotrekGa/PiotrekGa.github.io/blob/master/images/CodeCogsEqn18.png">
</p>

### Implementation

### Usage example

### Summary

### Final notes
LaTeX formulas were generated with [latex.codecogs.com](https://www.codecogs.com/latex/eqneditor.php)

### Bibliography
1. BERCHTOLD, RAFTERY, The mixture Transition Distribution Model for High-Order Markov Chains
and Non-Gaussian Time Series , 2002., Statistical Science Vol. 17, No. 3, 328-356
2. LEBRE, BOURGUIGNOM, An EM algorithm for estimation in the Mixture Transition Distribution
model , Laboratoire Statistique et Genome, Universite Evry Val d'Essonne, Evry, 2009
3. HUDSON, WON SUN KIM, KEATLEY, Series of Discrete State Processes: With an Application to Modelling Flowering 
Synchronisation with Respect to Climate Dynamics, 2019