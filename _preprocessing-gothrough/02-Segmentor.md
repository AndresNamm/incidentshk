---
title: "Segmenting roads into specific length and time segments"
permalink: /preprocessing/segmenting/
excerpt: "How the theme is organized and what all of the files are for."
modified: 2016-08-08T16:25:30-04:00
---

This is the first step for the Data preprocessing part. Most importan codepiece here is  
processer:RouteSegmentor.java and also models: way.java and node.java .. In addition to that I use tests to make sure that distances between 
segments are correct and visualization code. 


+ The input for this step is this [file]({{site.baseurl}}/assets/files/Hong_Kong_Highways-Merged-Remove_Deleted.osm) 
+ Output for this step is this [file]({{site.baseurl}}/assets/files/Hong_Kong-result.osm)



This step basically takes ways and nodes and modifies them so that in each way the distance between sequential nodes is inside the predefined interval. 
PS the ways both in the input and ouput have only one direction. 



Input file in OSM 

![]({{site.baseurl}}/assets/images/hk-roads.png)


Result file in OSM 

![]({{site.baseurl}}/assets/images/seg-result.png)




