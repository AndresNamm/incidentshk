---
title: "SE design principles for this project"
modified: 2017-08-01T15:54:02-04:00
permalink: {{baseurl}}preprocessing/design/
category: software design
---

{% include toc %}

## Introduction
 Although this is a somewhat smaller project Ive decided to follow some simpler design principles 
to make modifying extending and understanding this software as easy as possible.
The preprocessing part is mostly implemented in Java so the design guidelines apply to code written in Java.


## Separation of Concerns

To make the data processing logic separated from data parsing implementations, Ive mainly categorized my code into 3 types of classes:

+ Model 
     + Grid.java - Separate Routes into 2 dimensional Matrix of smaller rectangle shaped areas called Gridelements
     + GridElement.java
	 + Node.java 
	 + Way.java 
	 + Routes.java
	 + Segment.java
	 + Accident.java
	 + Record.java 
	 + LabeledRecord.Java
+ Processer/Algorithm - deals only with implemented models. Does not do any parsing. 
     + MapMatcher.java
	 + RouteSegmentor.java 
	 + LinearInterpolation.java
	 + LabelData.java 
+ Parser  - loads data into model from database

### Model 

I loosely follow the UML standard 2.0 when designing models (and later on Sequence Diagrams) Loosely, because although this project is a giant from my perspective, I rather need UML
for helping to describe the overall System so its a secondary, but still helpful, tool.  I use [IBM rational](https://www-01.ibm.com/software/uk/rational/)
for modeling which is provided to me by Hong Kong University. 

![]({{baseurl}}assets/images/models.png)


To follow separation of concerns principle as much as possible Ive decided to use the Spring framework.

## Testing

To avoid wasting time in later phases of training due to some random errors in the first phases I create simple automated tests. I use [Junit](http://junit.org/junit4/). for that.
 
## Deployment 

I use a build tool to make the project eaily added to other systems.
 For Java source code build and dependency management I use [Gradle](https://gradle.org/) and [Maven](https://maven.apache.org/)




