---
title: "OSM - the base map format for this project"
permalink: /preprocessing/osm/
excerpt: "How to quickly install and setup Minimal Mistakes for use with GitHub Pages."
modified: 2016-04-13T15:54:02-04:00
---

{% include toc %}

## Introduction 


In this project we are goint to use OSM format maps, because they are open source and have good support. There are multiple different types of ways to format an [OSM map](http://wiki.openstreetmap.org/wiki/OSM_file_formats). We have chosen to use .osm files for formating and as a data source. 
With mapping, its important to have a tool to visualize the documents to cross check that everything is correct. To our knowledge [Qgis](http://www.qgis.org/en/site/) is the most used tool for that, so we are basing our project on qgis, however as OSM is a widespread format if necessary its also possible to you use other tools to process analyse the maps. 

## Intorductory tutorial about coordinate systems 

A coordinate system can be 
+ projected - coordinates in northin, easting. Example HK1980 Grid - The accidents file is in this format. These are coordinates converted to a flat paper kind of surface
+ spherical - coordinates in longitude, latitude. Example wgs84. Coordinates on the sphere. 


To conver files in HK 1980 coordinate to wgs1984 you can use this site. 
https://www.geodetic.gov.hk/smo/en/tform/tform.aspx 

__References__


1. [Good intro pdf]({{site.baseurl}}/assets/files/maps.pdf)
2. [Hk grid system info]({{site.baseurl}}/assets/files/hksystem.ppt)
3. https://www.geodetic.gov.hk/smo/en/tform/tform.aspx  


## OSM 

The cornestone of OSM are 

1. Nodes
2. Ways

Nodes carry the geographic coordinates in the OSM data model. A way only gets geometry via the nodes that are members of it. Most of the time such nodes will be untagged and only serve to determine the way geometry. There are also nodes with tags, these show points of interests.


You can extract OSM maps for different locations from. 
https://mapzen.com/documentation/metro-extracts/tutorial/
 

Analysis have been currently based on this [file]({{site.baseurl}}/assets/files/Hong_Kong_Highways-Merged-Remove_Deleted.osm)

__References__

1.  http://en.flossmanuals.net/openstreetmap/the-osm-data-model/
2.  http://wiki.openstreetmap.org/wiki/Node





## QGIS 

### How to open  a file with .osm extension in qgis 

#### Option A

In the left sidebar go to the folder where .osm file is located at. Double click on it. In the opening window click on "choose all" and then click okay. The file will be opened fully. with bigger sets, this could take time. Overall, opening full Hong Kong data for example renders very slow. 

#### Option B Sqlite Database 

In menu click:

1. Vector 
2. OpenStreetMap 
3. Import Topology from XML -> choose osm file u waant to use. Then Import database

then

1. Vector 
2. OpenStreetMap 
3. Export Topology to SpatialLite 
4. Choose polylines, lines or points 
5. Click on load 
6. Choose Features -> NOT NULL SECTOR IN TABLE MEANS FILTERING 

If gives error, it might not import all data.

### Import from csv 

http://www.qgistutorials.com/en/docs/importing_spreadsheets_csv.html


### GROUPING AND COLOURING NODES

PS - to show node in QGIS you have to add a tag to it. Otherwise this node is only considered a way node and wont be visualized. 

https://gis.stackexchange.com/questions/20404/how-to-give-multiple-colors-to-features-within-a-single-shapefile

tags for grouping nodes <tag k="ref" v="4"/> 
http://wiki.openstreetmap.org/wiki/Key:ref
			