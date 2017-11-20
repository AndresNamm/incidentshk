---
title: "Matching accidents and records to segments"
permalink: /preprocessing/mapmatching/
excerpt: "Instructions for installing the theme for new and existing Jekyll based sites."
modified: 2016-08-01T09:36:36-04:00
---

+ To match 30 GB worth of GPS records (around 300 million records)  took around 2 hours without multiprocessing. Each record matched to only 1 segment.
+ [Code in Github](https://github.com/AndresNamm/Incident-Detection-Java/tree/master/src/main/java/bgt/MapMatching)
+ You can read more about the execution logic in the map matching part of [this document]({{site.baseurl}}/assets/files/detection.pdf)

## General


This part reads in the GPS records collected from Hong Kong taxis and accidents records received from the Hong Kong Transport department. Both of the records are then matched to specific road segments which are described in the previous section. This is a very important and very complicated part in the whole incident detection project because slight inaccuracies in matching the GPS data and accident data can have negative results in the overall performance of the incident detection model. To perform map matching the segmented map is read in and Segment objects with lists for Accidents and Records are created. Each generated segment will be assigned a direction. Next, a grid with 100 m X 100 m  squares will be generated over the whole Hong Kong area. Every segment is assigned into 1 or multiple grid squares.  After creation of the grid, the map is ready for matching GPS records and accidents to it. For each record that is matched, first its location inside the grid is calculated, then  the record is compared with all the segments inside the grid elements based on direction and distance. The matching of accidents is similar however without direction comparison.

## Accident matching

**MATCHING ACCIDENTS** To convert accident coordinates you have to use this site:  https://www.geodetic.gov.hk/smo/en/tform/tform.aspx   HK_1980 GRID TO WGS94 - However you have to add 800000 in front of the
Accident file coordinates. Otherwise this conversion wont work.


Accidents visualisation together with roads used in this project. It is seen that a lot of the accident have happened far from the currently analyzed area. Next step is to calulate the amount of distance actually under analysis.

All accidents visualised on Hong Kong map.   
![]({{site.baseurl}}/assets/images/accidents.png)    
Accidents Density on Hong Kong roads by segment.
![]({{site.baseurl}}/assets/images/diss/acc_density.png)   
![]({{site.baseurl}}/assets/images/diss/acc_density_legend.png)   



## GPS matching

GPS density on Hong Kong roads by segment.    
![]({{site.baseurl}}/assets/images/diss/gps_density.png)   
![]({{site.baseurl}}/assets/images/diss/gps_density_legend.png)   

## Execution logic

To perform map matching. A 100 m X 100 m GRID is placed over Hong Kong. Part of grid in Hong Kong . Each square area stands for GridElement

![]({{site.baseurl}}/assets/images/grid.png)   

Each segment is assigned to a grid or multiple grids.

![]({{site.baseurl}}/assets/images/geseg.png)   

![]({{site.baseurl}}/assets/images/geseg2.png)   

Legend for the above map

![]({{site.baseurl}}/assets/images/basis.png)
