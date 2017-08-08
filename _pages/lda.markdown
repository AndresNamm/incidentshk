---
layout: single
title: Lda tutorial
permalink: /lda/
---


## Introduction

This part focuses on the implementation of specific algorithm, LDA on Hong Kong data. 
The method is based on article [IS'15] Real-time Traffic Incident Detection Using a Probabilistic Topic Model.




## Theoretical analysis

I think these are good starting points for understaning this unsupervised learning method

+ https://www.quora.com/What-is-a-good-explanation-of-Latent-Dirichlet-Allocation
+ http://videolectures.net/mlss09uk_blei_tm/ - Clear and **fun** lecture from the creator of this method.
+ LDA is based on bayes statistics so its good to get some understaning of [that]({{site.baseurl}}/assets/files/bayes.pdf)

### My rather messy mathematical summaries 


{% for cookie in site.lda %}
  <div class="cookie">
    <h2><a href="{{site.baseurl}}{{ cookie.url }}">{{ cookie.title }}</a></h2>
  </div>
{% endfor %}


## Code

I will make it available as soon as I get the approval of the authors of this paper. 

So far I have created a [tutorial](https://andresnamm.github.io/blog/formating/2017/08/03/C++BOOST.html) for setting up the C++ enviroment to run this code:

