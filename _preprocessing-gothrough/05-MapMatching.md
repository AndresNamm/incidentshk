---
title: "Matching speed panels to segments"
permalink: /preprocessing/mapmatchingsp/
excerpt: "Instructions for installing the theme for new and existing Jekyll based sites."
modified: 2016-08-01T09:36:36-04:00
---

+ This part took for 62 road sensors and 200 segments around 20 minutes to complete
+ [Code in Github](https://github.com/AndresNamm/Incident-Detection-Java/tree/master/src/main/java/bgt)
+ You can read more about the execution logic in the speedpanel integration part of [this document]({{site.baseurl}}/assets/files/detection.pdf)
One of the key contributions of this dissertation was to integrate
speedpanel data with GPS data. The speedpanel dataset consists of average speed observations in 2 minute time intervals. As the taxi dataset was available for year 2010, only speedpanels that were installed in year 2010 have been integrated as a data source to this project. The distribution of speedpanels in Hong Kong can be seen below


![]({{site.baseurl}}/assets/images/diss/speedpanels.png)  
The road sensors analysed in this project are shown in beige. Newer road sensors installed after 2013 are shown in blue.


# SPEED PANEL DATA TRANSFORMATION MODEL

## Initial Table speedpanel.xls

This table contains spedpanel information transorfmed to xls based ont the file tsm_dataspec.pdf


| Link ID   | Start Node | Start Node Eastings | Start Node Northings  | End Node | End Node Eastings | End Node Northings | Region | Road Type   |
|-----------|------------|---------------------|-----------------------|----------|-------------------|--------------------|--------|-------------|
| 722-50059 | 722        | 834038.674          | 816345.067            | 50059    | 833862.7          | 816441.553         | HK     | MAJOR ROUTE |
| 724-722   | 724        | 834148.783          | 816250.647            | 722      | 834038.674        | 816345.067         | HK     | URBAN ROAD  |
| 752-875   | 752        | 835099.22           | 815634.373            | 875      | 834918.334        | 815759.379         | HK     | URBAN ROAD  |
| 756-752   | 756        | 835352.749          | 815640.774            | 752      | 835099.22         | 815634.373         | HK     | URBAN ROAD  |
| 760-756   | 760        | 835602.186          | 815600.695            | 756      | 835352.749        | 815640.774         |

## StartNode.csv->20170925_163111_StartNode.csv, EndNode.csv->20170925_163111_EndNode.csv

Generated Based on the speedpanel.xls.

1. I took the [1-3] (start form 0, inclusive) columns to generate StartNode.xls and then took the [4-6] columns to generate EndNode.xls
2. I used the converter in the [Hong Kong Mapping Office site](https://www.geodetic.gov.hk/smo/en/tform/tform.aspx) to transform and add to table the corresponding coordinates in wgs84 format.  
**Settings:**  
   + HK80 Grid Coordinates(Northing, Easting) to WGS85 Geographic Coordinates (Latitude/Longitude)
   + Format Decimal Degree(DDD.DDD) for input and output both.


| ID  | Easting    | Northing   | Latitude(DDD.DDDDD) | Longitude(DDD.DDDDD) |
|-----|------------|------------|---------------------|----------------------|
| 722 | 834038.674 | 816345.067 | 22.28599503         | 114.155241726        |
| 724 | 834148.783 | 816250.647 | 22.285142496        | 114.156310311        |
| 752 | 835099.22  | 815634.373 | 22.279578054        | 114.16553342         |
| 756 | 835352.749 | 815640.774 | 22.279636012        | 114.167993464        |


## panels.xls

Just a regular speedpanel observation file with data where in columns there are around 70 speed panels listed and in rows there are observations for every 2 minutes in 24 hour sequence.

| Date     | Time    | Node 3402-8979 Speed (km/h) | Node 3402-8979 Color | Node 3402-8979 Travel Time (mins) | Node 3651-4632 Speed (km/h) | Node 3651-4632 Color |
|----------|---------|-----------------------------|----------------------|-----------------------------------|-----------------------------|----------------------|
| 1/1/2010 | 0:00:35 | 55.55472633                 | G                    | 0.2                               | 77.77887123                 | G                    |
| 1/1/2010 | 0:02:35 | 55.55472633                 | G                    | 0.2                               | 69.5602234                  | G                    |
| 1/1/2010 | 0:04:35 | 54.42624134                 | G                    | 0.21                              | 72.99038487                 | G                    |
| 1/1/2010 | 0:06:35 | 55.55472633                 | G                    | 0.2                               | 77.77887123                 | G                    |
| 1/1/2010 | 0:08:35 | 48.56595273                 | G                    | 0.23                              | 77.77887123                 | G                    |


## StartNodes.csv-result.csv, EndNodes.csv-result.csv  

Here we store the filtered file based on the StartNode.xls, EndNode.xls. To do the filtering I retrieved (from panels.xls) speedpanel id-s we had in year 2010. I did the filtering because the files generated from speedpanel.xls contain all available speed panels  in year 2017 which is a lot more.

| ID    | Easting    | Northing   | Latitude(DDD.DDDDD) | Longitude(DDD.DDDDD) |
|-------|------------|------------|---------------------|----------------------|
| 7882  | 836603.538 | 815878.324 | 22.282              | 114.18               |
| 7881  | 836592.753 | 815897.731 | 22.282              | 114.18               |
| 46488 | 835804.774 | 815605.457 | 22.279              | 114.172              |
| 4650  | 839919.319 | 816768.747 | 22.29               | 114.212              |
| 7883  | 836596.707 | 815890.625 | 22.282              | 114.18               |
| 4651  | 836884.007 | 816087.665 | 22.284              | 114.183              |
| 4652  | 833101.677 | 816778.101 | 22.29               | 114.146              |


## Data formating


### Base Map, against which incident detection is done data files
Andres Incident Detection Folder:
**/data/route_segmentation/**
1. ~~InitialRoutesMergedWhkCityArea.osm --  Hong_Kong_Highways-Merged-Remove_Deleted.osm and  HkCityArea.osm merged manually using JOSM program~~
2. ~~resultInitialRoutesMergedWhkCityArea.osm filtered out ways and nodes except for which belonged
under ways categories : trunk, motorway_link, motorway;~~
1. Hong_Kong_Highways-Merged-Remove_deleted.osm
1. HkCityArea.osm
1. result_filtered-Hong_Kong_Highways-Merged-Remove_Deleted.osm
1. result_filtered-HkCityArea.osm  
1. merged-filtered.osm - based on 2 previous merges(Merge done with JOSM*.jar in the downloads folder). If 2 nodes had same id, HkCityArea is preferred because this is newer.
4. Hong-Kong-Result.osm -- segmented routes created based on resultInitialRoutesMergedWhkCityArea.osm file



### Specific descriptions

3 phases

1. Formating the speedpanel nodes and speedpanel nodepairs into osm formating with wgs84 coordinates System
2. Mapping speedpanels to mapmathing project base map. speedpanels.osm file with Hong-Kong-Result.osm file.
3. Parsing speedpanel observations so they could be included under this project training and incident detection phase.

#### bgt.SpeedPanel.SpeedPanelToProjectFormater

0. Manual Preprocessing - Based tsm_dataspec.pdf - Steps described above until generation of: 20170925_163111_StartNode.csv ,  20170925_163111_EndNode.csv
1. returns HashMap<String, Integer> panelNodeMap reads in speedpanel nodepairs and nodes from observation files in year 2010 (panels.xls example above)  
    + getCurrentSpeedPanelsOld() based on 1 file - reads in only speedpanels that existed in 1 day january 2010
    + getCurrentSpeedPanels() based on all files - read in all speedpanels that existed or were installed in 2010
2.  filterSpeedPanelNodesCSV(String fileName) Filters speedpanels out from the files:
20170925_163111_StartNode.csv ,  20170925_163111_EndNode.csv -> generates files: StartNodes.csv-result.csv, EndNodes.csv-result.csv
3. speedPanels = k.readInSpeedPanelsCSV(String fileName) -> reads in speedpanel nodes from filtered files as rows.
4. readSpeedPanelsToNodes(speedPanels);
5. generateWaysFromNodeCombos();
6. printWays("old"); -  


Input files    

+ tsm_dataspec.pdf

Intermediate files

+ panels.xls || all files that contain speedpanel observations
+ StartNode.csv
+ EndNode.csv
+ 20170925_163111_StartNode.csv
+ 20170925_163111_EndNode.csv
+ 20170925_163111_StartNode.csv-result.csv - filtered files with speedpanel nodes  
+ 20170925_163111_EndNode.csv-result.csv - filtered files with speedpanel nodes

Output files

+ oFileName +"speedPanelWays.osm"  - file which contains speepanels pairs as ways with 2 nodes.

#### bgt.SpeedPanel.MatchSpeedPanelSegments  extends MapMatcher


+ MatchSpeedPanelSegments matchSpeedPanelSegments = new MatchSpeedPanelSegments("speedPanelWays.osm"); - reads in speedPanels file as MapMatcher base map.
+ matchSpeedPanelSegments.indexSegs(); - indexes the base map just like in gps observations matching
+ matchSpeedPanelSegments.doMatching(); - matches Hong_Kong-result.osm segments to speed_panel based ways map.
    + this.speedPanelSegs = matchToSpeedPanels(segRecs);
    + printOutPanelsWSegments();
~~~java
        Routes segmentResultRoutes = OSMparser.parseNodesAndWays("data/route_segmentation/Hong_Kong-result.osm");
        SegmentStructures ss = parseSegments(segmentResultRoutes);
        this.segmentsToMatch = ss.segMap;

        HashMap<String,List<Record>> segRecs = segToRecs(ss.segList);
        this.speedPanelSegs = matchToSpeedPanels(segRecs);
        System.out.println(this.speedPanelSegs);
        System.out.println(this.speedPanelSegs.size());
        printOutPanelsWSegments();
        printOutResult("data/speed_panels/spMatched.txt");
        return true;
~~~


Input

+ data/speed_panels/intermediate_data/speedPanelWays.osm
+ data/route_segmentation/Hong_Kong-result.osm

Output

+ data/speed_panels/SpeedPanelSegments.osm
+ data/speed_panels/spMatched.txt

####  bgt.SpeedPanel.AppendSpeedPanel ; bgt.SpeedPanel.SpeedPanelObsParser  

After LInearInetporlation has finished. This class is responsible for adding speedPanel observations to the project.

+ AppendSpeedPanel speedPanelParser = new AppendSpeedPanel(AppendSpeedPanel.PREFIX + "spMatched.txt");
~~~java
AppendSpeedPanel(String fileN) throws IOException {
    this.speedPanelSegs = SpeedPanelObsParser.readInSpSegs(fileN);
}
~~~
+ speedPanelParser.appendAllObservations(GenerationType.TRAIN); - builds input for model training phase and appends this to linear interpolation result.
+ speedPanelParser.appendAllObservations(GenerationType.TEST); - builds input for labeling phase, which the labeling phase must read in separately.

Input

+ data/speed_panels/spMatched.txt
+ all speedpanel observations files

Output

+ data/speed_panels/append_train/train_interpolation_result" + foldNr + ".csv
>251368918_0_0,73   
251368918_0_0,76  
251368918_0_0,73  
251368918_0_0,73  
251368918_0_0,77  
251368918_0_0,74   
251368918_0_0,76   
251368918_0_0,75   

+ data/speed_panels/labeling/observations/+ foldNr + ".csv"

>251368918_0_0,0:00:35,73,14/1/2010  
251368918_0_0,0:02:35,76,14/1/2010  
251368918_0_0,0:04:35,73,14/1/2010   
251368918_0_0,0:06:35,73,14/1/2010   
251368918_0_0,0:08:35,77,14/1/2010   
251368918_0_0,0:10:35,74,14/1/2010   
251368918_0_0,0:12:35,76,14/1/2010   
251368918_0_0,0:14:35,75,14/1/2010   
251368918_0_0,0:16:35,71,14/1/2010   
251368918_0_0,0:18:35,75,14/1/2010   
251368918_0_0,0:20:35,73,14/1/2010   
251368918_0_0,0:22:35,70,14/1/2010   
251368918_0_0,0:24:35,70,14/1/2010   


### Code pieces that contain speedpanel data formating code and short descriptions


**After Route Segmentation Phase**

bgt.parsing.OSMparser - added function: public static Routes parseFilterNodesAndWays(String fileName) which filters input files so the only ways that are accepted are: "trunk","motorway_link","motorway":
bgt.SpeedPanel.SpeedPanelToProjectFormater    - formats speedpanels so their locations could be added to map.
bgt.SpeedPanel.MatchSpeedPanelSegments  extends MapMatcher - matches speedpanels to base map

**After Linear Interpolation Phase**

bgt.SpeedPanel.AppendSpeedPanel implements bgt.SpeedPanel.SpeedPanelBase   - controller, which handles overall generatsion of data
bgt.SpeedPanel.SpeedPanelObsParser  bgt.SpeedPanel.SpeedPanelBase  - parses observation files, copies append files. File to project handling.
bgt.LabelData.LabelData - modified LoadRecsIntoSegs function here.



# AMOUNT OF OBSERVATIONS

$70*24*30*365$ ~ 19 000 000
