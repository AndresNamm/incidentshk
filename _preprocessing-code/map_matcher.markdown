---
title: "bgt/MapMatching/MapMatcher.java"
modified: 2017-08-01T15:54:02-04:00
permalink: {{baseurl}}preprocessing/mapmatcher/
category: Java-8
---

{% include toc %}

## public static void main(String[] args)


Adds records and accidents to segments. Outputs 2 files one for accidents, one for records. Outputs record file with format: 

>
>Segment1 id  
Record inf 1  
Record inf 2  
...  
Segment2 id  
Record inf 1  
Record inf 2  
.......  

Example:  
![]({{baseurl}}assets/images/mathresult.png)

Calls pretty much these methods in the order below. 

~~~java
MapMatcher mm = new MapMatcher();
mm.indexSegs()
// The procedure below is executed into 4 folds
mm.parseRecords() 
mm.outputRecordResults("data/map_matching/fold_"+i+"/<train>||<test>_match_result.txt");
mm.parseAccidents();
mm.outputAccResults("data/map_matching/acc_match_result.txt");
~~~


## MapMatcher mm = new MapMatcher();
loads in RouteSegmentation result file in osm format
## mm.indexSegs() 
Adds segments to gridElements
## mm.parseRecords() // Does that into 4 folds 
Loads in records - Uses mapRecord _below_ to map records to segments **THIS FUNCTION RUNS AROUND 4 HOURS"
## mm.outputRecordResults("data/map_matching/fold_"+i+"/<train>||<test>_match_result.txt");
Outputs segments with recors 
## mm.parseAccidents();
Same as parseRecord 
## mm.outputAccResults("data/map_matching/acc_match_result.txt");
Same as outputRecordResults, but with accidents. 
## mapRecord(Record record)



based on 2 attributes finds closest Segment to each record that fits according to the minimal criteria. 


1 Direction 
~~~java
if (seg.calcRecordDirectDist(record) > 45){ // CHECKS THAT THE DIRECTION 
//THE TAXI IS DRIVING CLOSE TO THE DIRECTION THE ROAD IS SET UP. 
    continue;
~~~
2 Distance 
~~~java
}else{
    // Calculates the distance between Record Node ad 
    double dist = seg.calcNodeDist(recNode);
    if (dist < minDist){
        minDist = dist;
        minSegID = segID;
    }
}
~~~

~~~java
 public void mapRecord(Record record){
        Node recNode = record.getLocation();

        // THIS PART MAPS THE THING INTO THE RIGHT PLACE IN GRID - MAPS IT INTO 3 PLACES IN THE GRID  -- TOTALING 300 * 300 m
        int latIndex = (int)(Node.calcDist(recNode.getLat(), MIN_LON, MIN_LAT, MIN_LON) / GRID_SIZE);
        int lonIndex = (int)(Node.calcDist(MIN_LAT, recNode.getLon(), MIN_LAT, MIN_LON) / GRID_SIZE);
        int minLatIndex = latIndex-1>=0 ? latIndex-1:0;
        int maxLatIndex = latIndex+1<=MAX_LAT_INDEX ? latIndex+1:MAX_LAT_INDEX;
        int minLonIndex = lonIndex-1>=0 ? lonIndex-1:0;
        int maxLonIndex = lonIndex+1<=MAX_LON_INDEX ? lonIndex+1:MAX_LON_INDEX;
        // THIS PART MAPS THE THING INTO THE RIGHT PLACE IN THE GRID
        List<String> checkedSegIDList = new ArrayList<>(); //Initialize empty ArrayList of // Segment ID-s
        double minDist = 100;
        String minSegID = null;
        for (int i=minLatIndex; i<=maxLatIndex; i++){
            for (int j=minLonIndex; j<=maxLonIndex; j++){         // ASSIGNS TO CLOSEST SEGMENT IN GRID
                // map the segments in each grid
                List<String> segIDList = this.segIDGrid[i][j];// here are the segments that belong to the specific gridelement . these segments are mapped in indexSegs()
                for (String segID : segIDList){
                    if (checkedSegIDList.indexOf(segID) == -1){// OII
                        // this segment has not been checked
                        Segment seg = this.segMap.get(segID);
                        if (seg.calcRecordDirectDist(record) > 45){ // CHECKS THAT THE DIRECTION 
                        //THE TAXI IS DRIVING IS CLOSE TO THE DIRECTION THE ROAD IS SET UP. 
                            continue;
                        }else{
                            // Calculates the distance between Record Node ad 
                            double dist = seg.calcNodeDist(recNode);
                            if (dist < minDist){
                                minDist = dist;
                                minSegID = segID;
                            }
                        }
                    }
                }
            }
        }

        if (minDist < 100){
            // there exists a segment that has a distance less than 100 from the record,
            // and their direction difference is less than 45 degree.
            segMap.get(minSegID).addRecord(record); // adds TRAFFIC RECORD TO CLOSEST SEGMENT
        }
    }


~~~

## Segment calcdirection()
What is direct distance? Calculated in Segment Constructor calcDirection(); Direction is between 0 and 360. If direction difference is less than 45 degrees, we calculate the distance betweern record and segment. 
~~~java 

    public void calcDirection(){
        // calculate the azimuth of the segment
        double s_lat = convertToRad(startNode.getLat());
        double s_lon = convertToRad(startNode.getLon());
        double e_lat = convertToRad(endNode.getLat());
        double e_lon = convertToRad(endNode.getLon());

        double direction = Math.sin(s_lat)*Math.sin(e_lat) +
                Math.cos(s_lat)*Math.cos(e_lat)*Math.cos(e_lon-s_lon);
        direction = Math.sqrt(1-direction*direction);
        direction = Math.cos(e_lat)*Math.sin(e_lon-s_lon)/direction;
        direction = Math.asin(direction)*180/Math.PI;

        if (e_lon > s_lon && e_lat > s_lat){
            // in 1st quadrant
            this.direction = direction;
        }else if (e_lon < s_lon && e_lat > s_lat){
            // in 2nd quadrant
            this.direction = 360 + direction;
        }else if (e_lat < s_lat ){
            // in 3rd or 4th quadrant
            this.direction = 180 - direction;
        }else {
            this.direction = direction;
        }
        return;
    }



~~~


## Segment.java -> seg.calcRecordDirectDist(record) - just absolute distance


~~~java
    public double calcRecordDirectDist(Record r){
        // calculate the direction difference between record and segment
        return Math.abs(this.direction - r.direction);
    }
~~~