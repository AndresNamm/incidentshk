---
title: "Interpolation"
permalink: /preprocessing/interlabel/
excerpt: "Instructions and suggestions for upgrading the theme."
modified: 2016-08-01T09:43:46-04:00
---


+ For approximately 30 million GPS records this part took around 2 hours to complete
+ [Code in Github](https://github.com/AndresNamm/Incident-Detection-Java/tree/master/src/main/java/bgt/LinearInterpolation)
+ You can read more about the execution logic in the linear interpolation part of [this document]({{site.baseurl}}/assets/files/detection.pdf)


## General

On faster roads, the taxis' travelling speed is so high that GPS readings which take place in 30 second time intervals cannot create an observation in each sequential segment. On the other hand, on slower routes with heavy congestion, taxis can generate multiple observations inside one segment. In this phase, trajectory generation is done for each device. If inside a trajectory, there are some segments skipped as shown in the upper part of Figure 4, interpolated records are going to be created between the sequential segments. At the same time, one trajectory can only contribute an observation to one segment.

## Idea

Input   
![this document]({{site.baseurl}}/assets/images/diss/n_inter.png)   
Output   
![this document]({{site.baseurl}}/assets/images/diss/y_inter.png)  

## Longest Trajectories visualised

![this document]({{site.baseurl}}/assets/images/diss/trajectories.png)  
