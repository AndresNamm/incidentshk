---
title: "bgt/LabelData/LabelData.java"
modified: 2017-08-01T15:54:02-04:00
permalink: {{baseurl}}preprocessing/labeldata/
category: Java-8
---


## main method 


Here you can see the principle order of execution of this piece of code. This example is modified compared to the actual code. 

~~~java
public static void main(String[] args) throws DocumentException, IOException, InterruptedException{
    ...
    MapMatcher mm = new MapMatcher();
    Labeldata ld = new LabelData();
    LinearInterpolation li = new LinearInterpolation();
    ld.trajsMap = li.trajsMap;
    // Get the segments
    ld.segList = mm.segList;
    ld.segsMap = mm.segMap;
    ld.LoadRecsIntoSegs();
    // Parse and label accidents
    ld.parseAccidents("data/map_matching/acc_match_result.txt");
    // Output labeling result
    ld.outputLabelingResult("data/label_data/fold_"+i+"/label_result");
    ... 
}
~~~


##     public void parseAccidents (String filename) throws FileNotFoundException, IOException


Goes through all the mapMatcher matched accidents and assign those accidents to Segments AND nearby segments based on accident severity ...

~~~java
int affected_seg_num = (severity==2?AFFECTED_SEG_NUM_SEVERITY_2:AFFECTED_SEG_NUM_SEVERITY_3);
for (int i=0; i<= affected_seg_num; i++){
    if (i == 0){
        Segment seg = segsMap.get(seg_id);
        labelAccOnSegment(seg, severity, month, day, time);
    }else{
        // Label the accident on upstream segement
        String upstream_seg_id = seg_id_split[0] + (inner_id-i) + seg_id_split[2];
        if (segsMap.containsKey(upstream_seg_id)){
            Segment upstream_seg = segsMap.get(upstream_seg_id);
            labelAccOnSegment(upstream_seg, severity, month, day, time);
        }

        // Label the accident on downstream segment
        String downstream_seg_id = seg_id_split[0] + (inner_id+i) + seg_id_split[2];
        if (segsMap.containsKey(downstream_seg_id)){
            Segment downstream_seg = segsMap.get(downstream_seg_id);
            labelAccOnSegment(downstream_seg, severity, month, day, time);
        }
    }
}
~~~ 
  


##    ld.outputLabelingResult("data/label_data/fold_"+i+"/label_result");

This code pretty much outputs segments together with accidents and records. 