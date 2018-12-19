---
title: My notes on Graphical Models and Bayesian Networks
image:  /images/bayesiannetworks/bayesheader.jpeg
category: environments
tags: [bayesian, bayes, network, bayesian network, graphical model, graphical, model, notes]
---

# My notes on Graphical Models and Bayesian Networks

## Probabilistic Graphical Models

A PGM (Probabilistic Graphical Model) is a probabilistic model for which a graph expresses the conditional dependence structure between random variables

The idea it's to be able to use graph algorithms to do inference and learning

Example: The following image shows a Bayesian Network who describes a conditional independent structure. The nodes of this network are random variable and the directed edges shows the conditional dependency between two random variables

![example](/assets/images/bayesiannetworks/example1.png)

What we are trying to describe with this kind of models is the [full joint probability distribution](https://en.wikipedia.org/wiki/Joint_probability_distribution) of L(Lottey), R(Rain) and W(Wet Ground)

<a href="https://www.codecogs.com/eqnedit.php?latex=P(L,&space;R&space;,&space;W)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?P(L,&space;R&space;,&space;W)" title="P(L, R , W)" /></a>

Which can be factorized as follows

<a href="https://www.codecogs.com/eqnedit.php?latex=P(L,&space;R&space;,&space;W)&space;=&space;P(L)&space;P(R)P(W&space;|&space;R)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?P(L,&space;R&space;,&space;W)&space;=&space;P(L)&space;P(R)P(W&space;|&space;R)" title="P(L, R , W) = P(L) P(R)P(W | R)" /></a>

P(L) doesn't have any connection with a node in the graph so it's completly independt of everything else and the variable of if the Weather is Rainning is dependent on/related to Wet Ground

Another example:

![example](/assets/images/bayesiannetworks/example2.png)

For this example the full joint probability can be obtained using the following factorization

<a href="https://www.codecogs.com/eqnedit.php?latex=P(R&space;,&space;W,&space;S,&space;C)&space;=&space;P(R)&space;P(C)&space;P(W&space;|&space;C,R)P(S&space;|&space;W)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?P(R&space;,&space;W,&space;S,&space;C)&space;=&space;P(R)&space;P(C)&space;P(W&space;|&space;C,R)P(S&space;|&space;W)" title="P(R , W, S, C) = P(R) P(C) P(W | C,R)P(S | W)" /></a>


## Bayesian Networks

## Naive Bayes and Logicstic Regressions as Bayes nets

## Inference in Bayes nets


## References 

- https://www.youtube.com/watch?v=zCWRTKnOYYg
