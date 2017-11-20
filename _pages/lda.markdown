---
layout: single
title: Description and Analysis
permalink: /lda/
---


## Introduction

This part focuses on the implementation and model analysis for the proposed real time incident detection method.
The method is based on article [IS'15] Real-time Traffic Incident Detection Using a Probabilistic Topic Model.

## The road to a deeper insight into the theory behind this methodology

To get somewhat a deeper idea about topic modeling e.g. the methdodology for modeling traffic,  deeper insights are needed in:

1. Bayes Statistics
    1. Random Variables
    2. Conjugacy
    3. Likelihood
    4. Difference between point estimation and variable inference
2. Approximation methodologies for getting the posterior distribution
3. Probabalistic Graphical Modeling
4. Different distributions like Poisson, Multinomial, Gaussian, Dirichlet. Distribution families
5. KL distance
6. Clustering - soft and hard
7. General methods for approximating maximum likelihood paramaters - e.g. analytical method, gradient descent and so on
8. EM algorithm


### This is what I started with

I think these are good starting points for understanding Topic Modeling. At least these tutorials generally go in the order of how I have learned this topic. They describe Latent Dirchlet Allocation which is not exactly the same methodology as used in this project but the general concepts behind it are similiar. These tutorials bring out the importance of Bayes machinery with such modeling. The second source also goes into describing the process of graphical modeling. In addition the second source brings out the problem with Bayes statistic, e.g that in most real life cases its intractable to compute and numerical approximations is necessary. For a novice like me it still a bit hard to understand the overall necessity exact reasoning. In addition, as there is an ample of different methodologys for approximating the posterior distribution with seemingly different underlying theorys, it is hard to not get overwhelmed on this all.

+ https://www.quora.com/What-is-a-good-explanation-of-Latent-Dirichlet-Allocation
+ http://videolectures.net/mlss09uk_blei_tm/ - Clear and **fun** lecture from the creator of this method.
+ LDA is based on bayes statistics so its good to get some understaning of [that]({{site.baseurl}}/assets/files/bayes.pdf)

## Topic modeling

{% for cookie in site.lda %}
  <div class="cookie">
    <h2><a href="{{site.baseurl}}{{ cookie.url }}">{{ cookie.title }}</a></h2>
  </div>
{% endfor %}



## Code

+ The code which trains the Topic Model for Hong Kong Traffic is available in [Github](https://github.com/AndresNamm/Incident-Detection-Model-Training-)

+ Incident detection code which uses the trained model is also available in [Github](https://github.com/AndresNamm/Incident-Detection-Java/tree/master/src/main/java/bgt/IncidentDetection)

So far,  I have created a [tutorial](https://andresnamm.github.io/blog/formating/2017/08/03/C++BOOST.html) for setting up for windows the C++ enviroment to run this code. Suprise suprise, its messy and not conscise.
