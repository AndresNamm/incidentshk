---
title: "Matching accidents and records to segments"
permalink: /preprocessing/mapmatching/
excerpt: "Instructions for installing the theme for new and existing Jekyll based sites."
modified: 2016-08-01T09:36:36-04:00
---

This code ran aroun  4 Hours on my computer.

Importan files 

Each record matched to only 1 segment.

To convert accident coordinates you have to use this site:  https://www.geodetic.gov.hk/smo/en/tform/tform.aspx   HK_1980 GRID TO WGS94 - However you have to add 800000 in front of the 
Accident file coordinates. Otherwise this conversion wont work. 



Model: 

+ Grid.java
+ GridElement.java
+ Segment.java 
+ Accident.java
+ Routes.java
+ Record.java 

Processing:

+ [MapMatcher.java]({{site.baseurl}}/preprocessing/mapmatcher/)


Todo - write this to the end. 


Accidents visualisation together with roads used in this project. It is seen that a lot of the accident have happened far from the currently analyzed area. Next step is to calulate the amount of distance actually under analysis. 

![]({{site.baseurl}}/assets/images/accidents.png)

Part of grid in Hong Kong . Each square area stands for GridElement

![]({{site.baseurl}}/assets/images/grid.png)


GrideElement and the segment inside this gridelement . 1 Segment belongs to 9 gridElemenst actually-

![]({{site.baseurl}}/assets/images/geseg.png)

![]({{site.baseurl}}/assets/images/geseg2.png)

Basis for the above maps

![]({{site.baseurl}}/assets/images/basis.png)


Histogram for how many segments (y) there ara which have (x) amount of records assigned to them. Altogether there are around 14000 segments in our experiment.

![]({{site.baseurl}}/assets/images/recordhist.png)




