---
title: "bgt/LinearInterpolation/LinearInterpolation.java"
modified: 2017-08-01T15:54:02-04:00
permalink: /preprocessing/linear interpolation/
category: Java-8
---



# LinearInterpolation.java

First this file generates trajectories from train_match_result.txt data. After that it generates new records for some trajectories which have skipped segments.
The end result of the execution of this file is a large file with key value pairs, where keys are segments and labels are speed observations collected from records.

Result Example:

![inpolat]({{site.baseurl}}/assets/images/inpolat.png)

## public void parseAndInterpolate(String filename, int fold) throws IOException, InterruptedException {

I had to add this function to the initial software, otherwise my Java Heap got full.

This codepiece
1. gets a hashmap of all gps-device id-s from the input file (train_match_result.txt).
2. Then it separates this hashmap into 5 100 element pieces.
3. Then it goes through the input file for every 100 gps-device bucket and generates Trajectories for those files.


~~~java
    public void parseAndInterpolate(String filename, int fold) throws IOException, InterruptedException {
        File file = FileUtils.getFile(filename);
        LineIterator it = FileUtils.lineIterator(file);
        HashMap<Integer, Integer > devMap = new HashMap<>();
        while(it.hasNext()){
            String line = it.next();
            StringTokenizer st = new StringTokenizer(line);
            String flag = st.nextToken();
            if (flag.equals("S")){  // this line corresponds to a segment
                String seg_id = st.nextToken();
            }else if (flag.equals("R")){ // this line belongs to a record
                long rec_id = Long.valueOf(st.nextToken());
                int dev_id = Integer.valueOf(st.nextToken());
                devMap.put(dev_id,dev_id);
            }
        }



        BufferedReader reader = new BufferedReader(new FileReader(filename));
        int lines = 0;
        while (reader.readLine() != null) lines++;
        reader.close();
        System.out.println("We have " +lines+ " lines" );
        //Checking LineNR

        int bucketSize = 100;
        int previous = 0;


        // Converting DevIDs hashmap into multiple submaps to iterate over
        ArrayList<HashMap<Integer,Integer>> buckets = new ArrayList<>();
        int nrOfBuckets = (int) Math.floor(devMap.size() / bucketSize) + 1;
        for(int j =0;j<nrOfBuckets;j++){
            buckets.add(new HashMap<Integer,Integer>());
        }

        List<Integer> devIDs = new ArrayList(devMap.values());
        for(int i = 0;i <devIDs.size();i++){
            buckets.get(i/bucketSize).put(devIDs.get(i),devIDs.get(i));
        }

        for(HashMap<Integer, Integer> fracDevID :  buckets){
            HashMap<Integer, List<List<Record>>> trajsMap = new HashMap<>();
            System.out.println(fracDevID);
            System.out.println(fracDevID.size());
            parseRecords(filename, fracDevID, trajsMap, lines);
            interpolate(trajsMap);
            outputResult("data/linear_interpolation/fold_"+fold+"/train_interpolation_result.csv", trajsMap );
            trajsMap=null;
            System.gc();
        }
        //this.parseRecords(filename, dev__id);
    }

~~~


## parseRecords(String filename)

Goes through mapmatching result files where each line is one record. Creates trajsMap from Records:
Assumes every way is directional and has only 1 direction.

+ Returns : HashMap of Trajectories consisting of records


~~~java
public HashMap<Integer, List<List<Record>>> trajsMap; // store (dev_id, trajectories)  
~~~

To separate trajectories from each other uses this condition + other minor checks. That means, 1 trajectory is inside 1 way.
~~~java
        if (thisSeg.way_id == lastSeg.way_id &&  // NEXT RECORD IS LOCATED AT THE SAME SEGMENT
                thisSeg.inner_id >= lastSeg.inner_id && // CANT UNDERSTAND WHY THIS IS A NECESSARY CONDITION. CAN IT BE THAT WE ARE ASSUMING 2 WAY ROADS , WHICH
                //WOULD MEAN THAT SEGMENTS ARE ADDED FROM WAYS IN THE RIGHT DIRECTION. YES. THATS LIKELY IT // THIS PART CAN BE USEFUL
                // BUT CAN ALSO BE VERY DANGEROUS
                rec.month == lastRec.month &&
                rec.day == lastRec.day)
~~~

Generation Segment.inner id

_In generation we are assuming that each road is 2 ways. Thus the ways are read in directional manner._
[Direction info](https://gis.stackexchange.com/questions/98435/does-osm-data-contain-the-direction-of-travel) All the WAYS are one way according to
~~~java

        Iterator<Element> wayItr = rootElem.elements("way").iterator();
        while (wayItr.hasNext()){
            Element wayElem = wayItr.next();
            ...
            List<Element> nodeElemList = wayElem.elements("nd");
            for (int i=0; i+1<nodeElemList.size(); i++){
                ...
                Segment seg = new Segment(way_id, i, startNode, endNode);
                segList.add(seg);
                ...
            }
        }


~~~

## interpolate()

### Summary

Goes through the data. Modifies the data, so that all segments have EXACLTY 1 record **from each previously generated trajectory**.

### Parts of code



GOES THROUGH ALL DEVICES

~~~java
Iterator trajsItr = trajsMap.entrySet().iterator();
while (trajsItr.hasNext()){ // ITERATER OVER ALL DEVICES
    Map.Entry dev_trajs_pair = (Map.Entry)trajsItr.next();
    int dev_id = (int)dev_trajs_pair.getKey(); //
    List<List<Record>> trajs = (List<List<Record>>) dev_trajs_pair.getValue(); // GET NEXT LIST OF TRAJECTORIES FOR A PARTICULAR DEVICE
~~~

GOES THROUGH ALL GENERATED TRAJECTORIES

~~~java
Iterator<List<Record>> trajItr = trajs.iterator();
while (trajItr.hasNext()){
    List<Record> traj = trajItr.next();
~~~

If trajectory takes place only in one segment, then we merge multiple records into one

~~~java
if (lastInner_id == firstInner_id){
    double distance = Node.calcDist(firstRec.getLocation(), lastRec.getLocation());
    double speed = distance/1000.0/(lastRec.time-firstRec.time); // GET SPEED
    double time = (firstRec.time + lastRec.time) / 2;
    Record newRec = new Record(dev_id, firstRec.month, firstRec.day, time, speed, firstRec.seg_id);
    newTraj.add(newRec);
~~~

If we have more segments than 1 in a trajectory. We go through each record in the trajectory and get their segments id.
If the difference of segment id-s between sequential records is larger than 1, the gps has skipped it and we have to interpolate a
record there.

~~~java
    while (j < traj.size()){
        ...

        if (eInner_id - sInner_id >= 1){// THIS MEANS THAT IN TRAJECTORY SOME SEGMENTS HAVE BEEND PASSED
                            double dist_s = Node.calcDist(sRec.getLocation(), segsMap.get(sRec.seg_id).endNode);
                            double dist_e = Node.calcDist(segsMap.get(eRec.seg_id).startNode, eRec.getLocation());
                            double dist = dist_s + dist_e;
                            String[] seg_id_frags = sRec.seg_id.split("_");
                            double[] inter_dists = new double[eInner_id-sInner_id-1];
                            for (int k=sInner_id+1; k<eInner_id; k++){
                                String seg_id = seg_id_frags[0]+"_"+k+"_"+seg_id_frags[2];
                                Segment inter_seg = segsMap.get(seg_id);// GET THE MIDDLE SEGMENT
                                double inter_dist = Node.calcDist(inter_seg.getStartNode(), inter_seg.getEndNode());//segmen length
                                inter_dists[k-sInner_id-1] = inter_dist;
                                dist += inter_dist;
                            }
        ...

    }
    ...

                            for (i = firstInner_id+1; i<lastInner_id; i++){
                            int pure_index = i - firstInner_id+1;
                            String innerSeg_id = first_seg_id_frags[0] + "_" +i + "_" + first_seg_id_frags[2];
                            double innerSpeed = 0.0;
                            innerSpeed = Node.calcDist(segsMap.get(innerSeg_id).startNode, segsMap.get(innerSeg_id).endNode)
                                    / 1000.0 / (times[pure_index] - times[pure_index - 1]);
                            double innerTime = (times[pure_index-1] + times[pure_index]) / 2.0;
                            Record newInnerRec = new Record(dev_id, firstRec.month, firstRec.day, innerTime, innerSpeed, innerSeg_id);
                            newTraj.add(newInnerRec);
                        }
    ...

~~~

## public void outputResult(String result_filename, HashMap<Integer, List<List<Record>>> trajsMap ) throws IOException{

Goes through all
devices -> all trajectories -> all records : prints out rec.seg_id info:(way_id, inner_id) with speed and time info to file

~~~java
Iterator trajsItr = trajsMap.entrySet().iterator();
while (trajsItr.hasNext()){
    Map.Entry dev_trajs_pair = (Map.Entry)trajsItr.next();
    List<List<Record>> trajs = (List<List<Record>>) dev_trajs_pair.getValue();
    Iterator<List<Record>> trajItr = trajs.iterator();
    while (trajItr.hasNext()){
        List<Record> traj = trajItr.next();
        Iterator<Record> recItr = traj.iterator();
        while (recItr.hasNext()){
            Record rec = recItr.next();
            String seg_id = rec.seg_id;// ELABORATION BELOW
            String[] seg_id_split = seg_id.split("_");
            fw_result.write(seg_id_split[0]+"_"+seg_id_split[1]+"_"+(int)rec.time + "," + (int)Math.round(rec.speed) + "\n");
        }
    }
}
~~~



String seg_id = rec.seg_id;// ELABORATION BELOW

~~~java
public void calcSeg_id() {
    this.seg_id = way_id +"_"+inner_id+"_"+startHour; // generate segment id
}
~~~
