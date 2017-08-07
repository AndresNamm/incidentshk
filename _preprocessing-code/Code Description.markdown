---
title: "General Code Description"
modified: 2017-08-01T15:54:02-04:00
permalink: /preprocessing/codereference/
category: Java-8
---

# Code Description
## Incident Detection (Java)
- RouteSegmentation
	- Models
		- [Node.java](./Incident Detection (Java)/src/bgt/RouteSegmentation/Models/Node.java)
			
			```
			[Model Class] 
			Model of route nodes.
			```
		- [Way.java](./Incident Detection (Java)/src/bgt/RouteSegmentation/Models/Way.java)
			
			```
			[Model Class]
			Model of routes.
			```
	- [RouteSegmentor.java](./Incident Detection (Java)/src/bgt/RouteSegmentation/RouteSegmentor.java)
		
		```
		[Function Class]
		Function: Split routes into segments of specific lengths.
		Input Files: data/route_segmentation/Hong_Kong_Highways-Merged-Remove_Deleted.osm
		Input Desc: Hong Kong roadmap extracted from OpenStreetMap (in OSM format, downloaded from 
					https://mapzen.com/data/metro-extracts/), with route fragments manually merged.
		Output File: data/route_segmentation/Hong_Kong-result.osm, 
					 data/route_segmentation/Hong_Kong-result.txt
		Output Desc: Hong Kong roadmap with routes segmented. OSM file is used for visualization use, 
					 TXT file is used for reading use.
		```

- MapMatching
	- Models
		- [Accident.java](./Incident Detection (Java)/src/bgt/MapMatching/Models/Accident.java)
		
			```
			[Model Class]
			Model of traffic accidents.
			``` 
		- [Record.java](./Incident Detection (Java)/src/bgt/MapMatching/Models/Record.java)
		
			```
			[Model Class]
			Model of GPS records.
			```
		- [Segment.java](./Incident Detection (Java)/src/bgt/MapMatching/Models/Segment.java)

			```
			[Model Class]
			Model of road segments.
			```
	- [CountRecords.java](./Incident Detection (Java)/src/bgt/MapMatching/CountRecords.java)

		```
		[Function Class]
		Function: Count the total number of GPS records.
		Input Files: data/GPS_data/TaxiData201000.mdb - TaxiData201052.mdb (obtained from Civil Engineering 
					 guys)
		Input Desc: All GPS record files.
		Output Files: None
		Output Desc: Total number of GPS records.
		```
	- [MapMatcher.java](./Incident Detection (Java)/src/bgt/MapMatching/MapMatcher.java)
		
	  ```
	  [Function Class]
	  Function: Map GPS records/traffic accidents onto road segments.
	  Input Files: roadmap file => data/route_segmentation/Hong_Kong-result.osm
	  				 record files => data/route_segmentation/TaxiData2010XX.mdb-TaxiData2010YY.mdb (decided 
	  				 				 by fold number) 
	  				 accident file => data/map_matching/acc_info.xls
	  Input Desc: Input roadmap file+record files when mapping records, input roadmap file+accident file when 
	  				mapping accidents.
	  Output Files: record mapping => data/map_matching/fold_x/train_match_result.txt (x is fold number)
	  									data/map_matching/fold_x/test_match_result.txt (x is fold number)
	  				  accident mapping => data/map_matching/acc_match_result.txt
	  Output Desc: Will output record mapping result to different folders with different fold numbers.
	  ```

- LineaInterpolation
	- [LinearInterpolation.java](./Incident Detection (Java)/src/bgt/LinearInterpolation/LinearInterpolation.java)

		```
		[Function Class]
		Function: Estimate the average speed value of taxis on passed road segments with zero or more than 
				  one training GPS records.
		Input Files: data/map_matching/fold_x/train_match_result.txt (x is fold number)
		Input Desc: None
		Output Files: data/map_matching/fold_x/train_interpolation_result.csv (x is fold number)
		Output Desc: None
		```

- LabelData
	- [LabelData.java](./Incident Detection (Java)/src/bgt/LabelData/LabelData.java)
	
		```
		[Function Class]
		Function: Interpolate the average speed value and label accident data on test GPS records. 
		Input Files: test record files => data/map_matching/fold_x/test_match_result.txt (x is fold number)
					 accident file => data/map_matching/acc_match_result.txt
		Input Desc: None
		Output Files: data/label_data/fold_x/label_result_y.txt (x is fold number, y is route id)
					  data/label_data/fold_x/label_result_list.txt
		Output Desc: label_result_y records the labeling result for route y, while label_result_list is used 
					to record the list of routes with at least one record labelled with accident
		```

- IncidentDetection (src/bgt/IncidentDetection)
	- Models
		- [EvaluateResult.java](./Incident Detection (Java)/src/bgt/IncidentDetection/Models/EvaluateResult.java)
		
			```
			[Model Class]
			Model of the evaluation result of a set by the detection algorithm.
			```
		- [LabeledRecord.java](./Incident Detection (Java)/src/bgt/IncidentDetection/Models/LabeledRecord.java)
		
			```
			[Model Class]
			Model of a GPS record with label.
			```
		- [SegDistribution.java](./Incident Detection (Java)/src/bgt/IncidentDetection/Models/SegDistribution.java)
		
			```
			[Model Class]
			Model of the speed distribution on a segment.
			```
	
	- [IncidentDetection.java](./Incident Detection (Java)/src/bgt/IncidentDetection/IncidentDetection.java)
	
		```
		[Function Class]
		Function: Detect traffic incidents in real time with trained model and labeled test data.
		Input Files: distribution file => data/estimate_result/fold_x/estimate.out (x is fold number)
					 labeled route list file => data/label_data/fold_x/label_result_list.txt (x is fold number)
					 labeled test data file => data/label_data/fold_x/label_result_y.txt (x is fold number, 
					 y is route id)
		Input Desc: From the labeled route list file can know the list of routes containing at least 
					one record labeled incident.
		Output Files: data/incident_detection/fold_x/detect_result_y.txt (x is fold number, y is route id)
		Output Desc: None
		```

## Parameter Estimation (C++)
- pmm (poisson mixture model)
	- [csv2dump.cpp](./Parameter Estimation (C++)/pmm/src/csv2dump.cpp)
		
		```
		Function: Convert CSV input to a dump file that 'estimate' reads.
		Input Files: Inicident Detection (Java)/data/linear_interpolation/fold_x/train_interpolation_result.csv (x is fold number)
		Input Desc: None
		Output Files: fold_x/train_interpolation_dump
		Output Desc: Corresponding dump file for the input CSV file.
		```
	- [estimate.cpp](./Parameter Estimation (C++)/pmm/src/estimate.cpp)

	
		```
		Function: Maximum-likelihood estimate the parameters of the poisson mixture model (pmm).
		Input Files: fold_x/train_interpolation_dump (x is fold number)
		Input Desc: None
		Output Files: Inicident Detection (Java)/data/estimate_result/fold_x/estimate.out (x is fold number)
		Output Desc: The estimated speed distribution for each road segment.
		```

## Convert Coordinates & Plot Results (Python)
- [ConvertCoords.py](./Convert Coordinates & Plot Results (Python)/ConvertCoords.py)

	```
	Function: Convert GPS records from HK80 Grid system into WGS84 system using the web-based tool on the 
			  official website of HK Survey and Mapping Office / Lands Department.
	Input Files: data/convert_coords/only_coords.xlsx
	Input Desc: Unconverted coordinates in HK80 Grid system.
	Output Files: data/convert_coords/ConvertedCoords.xls
	Output Desc: Converted coordinates in WGS84 system.
	```
- [PlotResult.py](./Convert Coordinates & Plot Results (Python)/PlotResult.py)

	```
	Function: Plot the detection results to get the receiver-operating characteristic (ROC) curve and get  
			 the area under curve (AUC) value.
	Input Files: data/plot_result/TW_z/fold_x/detect_result_y.txt (x is the fold number, y is the route id, 
				 z is the time window size)
	Input Desc: None
	Output Files: data/plot_result/averaged/TW_z/result.txt (z is the fold number)
	Output Desc: The AUC values of the detection result for each route in each fold.
	
	```

		
			
		
			 
