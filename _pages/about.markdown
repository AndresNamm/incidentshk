---
title: About
permalink: /about/
---

The goal of this project is to develop an automatic incident detection System for Hong Kong.  Accurate incident detection is important mostly for 2 reasons: Routing and Acknowledging. For routing algorithms, incident detection knowledge gives ground to a better estimate of travel times for different routes. Automatic incident detection can as well be useful for informing important agents like the police or traffic authority about the incident so they could take necessary action faster.

This webpage divides into 3 parts:

1. [Data preprocessing]({{site.baseurl}}/preprocessing) develops an universal system to parse gps, accident and speed panel info into a form which can be inputed to different incident detection models/algorithms. 
2. [LDA applicaton]({{site.baseurl}}/lda) - Bai Guangtong has implemented method offered in this paper:  [IS'15] Real-time Traffic Incident Detection Using a Probabilistic Topic Model . Our goal is to improve that method with additional data and find other means to improve incident detection accuracy based on available data. 
3. [Machine learning analysis]({{site.baseurl}}/lda) - Universal analysis from the machine learning method applied in 2. This analysys seeks to bee the baseline for comparing different incident detectoon methods on Hong Kong data. 


All work in this project is supervised by 

+ [Dr Reynold Cheng](https://scholar.google.com/citations?user=7R7MSb4AAAAJ)
+ [Yixiang Fang](https://scholar.google.com/citations?user=tArifqQAAAAJ&hl=en)


[jekyll-organization]: https://github.com/jekyll
