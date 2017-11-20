---
title: "Data Models "
modified: 2017-08-01T15:54:02-04:00
permalink: /preprocessing/codereference/
category: Java-8
---

{% include toc %}


## bgt/Model/Routes.java

This class generally represents the OSM file in java context under this project.

### Variables  

+ private Boundaries boundaries;
+ private List<Way> wayList= new ArrayList<>();
+ private List<Node> nodeList=new ArrayList<>();

### Most Important functions

+ public void resegmentWays(double LOW_THRES, double HIGH_THRES){...}



### Most Important functions

None

## bgt/Model/Node.java

### Variables  
Represents a Node in standard OSM format in java context under this project.  

+ public static int numOfGenNodes = 0;  Counter for number of nodes
+ public long id;
+ public double lat;
+ public double lon;
+ public String visible= "true";
+ public String ref="untyped";

### Most Important functions

+ public static double toRad(double coord){return coord/180.0*Math.PI;} - Convert coordinate(Latitude/Longitude) into radian
+ public static double calcDist(double aLat, double aLon, double bLat, double bLon){...} - Calculate the distance between two points
+ public static double calcDist(Node aNode, Node bNode){...} - Calculate the distance between two points


## bgt/Model/Way.java

Represents a WAy in standard OSM format in java context under this project.  


### Variables

+ public long id;
+ public List<Node> nodeList;
+ public boolean isBridge;
+ public boolean isTunnel;
+ public String name;
+ public String name_en;
+ public String name_zh;
+ public boolean isOneWay; - This is true for all
+ public static final double LOW_THRES = 50.0;
+ public static final double HIGH_THRES = 100.0;

### Most Important functions

+ public List<Node> resegmentWay(double LOW_THRES,double HIGH_THRES){...} -In 1 way, changes the  distances between its nodes so each distance bewteen nodes would be roughly equal.  

## bgt/Model/Segment.java

### Variables

+ public String seg_id; -  22371280_0_0
+ public long way_id; $\equiv$ to route id.
+ public int inner_id;// id inside a way differing on from other
+ public Node startNode;
+ public Node endNode;
+ public double direction;
+ public int startHour; - what the fuck is this . In our model, this shit is always = 0..  
+ public List<Record> recordList;
+ public List<Accident> accList;

### Most Important functions

+ public void calcSeg_id() {this.seg_id = way_id +"_"+inner_id+"_"+ startHour; // generate segment i;}
+ public void calcDirection(){ // calculate the azimuth of the segment } Triggered at constructor when creating the segment at
+ public Segment[] splitSegByHour(){

## bgt/Model/Grid.java

### Variables

+ private Boundaries boundaries;
+ private GridElement[][] listGrid;

### Most Important functions

 public Routes convertToRoutes(int rowID, int colID) Helps to print out Grid as routes


## bgt/Model/GridElement.java

### Variables

+ Position boundaries
    + final double LON_MIN;
    + final double LON_MAX;
    + final double LAT_MIN;
    + final double LAT_MAX;
+ private List<Segment> segmentList=new ArrayList<Segment>();

### Most Important functions

## bgt/MapMatching/Accident.java

### Variables

+ public int acc_no;
+ public int severity;
+ public int month;
+ public int day;
+ public double acc_time;
+ public Node acc_location;

### Most Important functions

## bgt/MapMatching/Record.java

### Variables

+ public long rec_id;
+ public int dev_id;
+ public int month;
+ public int day;
+ public double time;
+ public Node location;
+ public double speed; // in km/h
+ public double direction;
+ public String seg_id; - this.seg_id = way_id +"_"+inner_id+"_"+ startHour;
+ public int acc_flag; // whether encounter an accident or not

### Most Important functions

## bgt/IncidentDetection/Models/EvaluateResult

Used to keep anomaly values and whether there is accident

### Variables  

+ public double anomaly_val;
+ public double anomaly_val_weighted;
+ public int acc_label;

### Most Important functions

## bgt/IncidentDetection/Models/LabeledRecord

Store the record read in from labeling result file.

### Variables  

+ public double time;
+ public int speed;
+ public int acc_flag; // 1 for accident, 0 for no accident

### Most Important functions

## bgt/IncidentDetection/Models/SegDistribution

Store the parameters that have been traines

### Variables  

+ public static int NUM_MIXTURE;  // number of traffic states
+ public static double[] lambdas; // global parameter of each poisson distribution - total K
+ public static double[][] deltas; // the pair-wise KL distance between states - some math jargon
+ public String seg_id;   // segment id this distribution parameters belong to
+ public double[] pis; // weights for each state/poisson distribution in this segment
+ public double[] sigmas; // prepared for weighted KL divergence - math jargon


## bgt/Model/Boundaries.java

### Variables

Based on the XML boundary attribute on top of every XML file

_Example_

~~~
 <bounds minlat="22.10531" minlon="113.78242" maxlat="22.60856"
maxlon="114.49179" origin="osmconvert 0.8.5"/>  
~~~

+ private final double minlat;
+ private final double maxlat;
+ private final double maxlon;
+ private final double minlon;
+ private String origin="osmconvert 0.8.5";
