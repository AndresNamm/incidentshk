---
title: "Segmenting roads into specific length and time segments"
permalink: /preprocessing/segmenting/
excerpt: "How the theme is organized and what all of the files are for."
modified: 2016-08-08T16:25:30-04:00
---


+ This step runs through quickly (Less than 10 seconds).
+ [Controller code in Github](https://github.com/AndresNamm/Incident-Detection-Java/blob/master/src/main/java/bgt/RouteSegmentation/RouteSegmentor.java)
+ You can read more about the execution logic in the segmentation part of [this document]({{site.baseurl}}/assets/files/detection.pdf)


This is the first step for the Data preprocessing part. Most importan codepiece here is  
processer:RouteSegmentor.java and also models: way.java and node.java .. In addition to that I use tests to make sure that distances between
segments are correct and visualization code.


+ The input for this step is this [file]({{site.baseurl}}/assets/files/Hong_Kong_Highways-Merged-Remove_Deleted.osm)
+ Output for this step is this [file]({{site.baseurl}}/assets/files/Hong_Kong-result.osm)



This step basically takes ways and nodes and modifies them so that in each way the distance between sequential nodes is inside the predefined interval.
PS the ways both in the input and ouput have only one direction.



Input example visualised in OSM

![]({{site.baseurl}}/assets/images/diss/segment_in.png)


Result example visualised   in OSM

![]({{site.baseurl}}/assets/images/diss/segment_out.png)
