# Datasets and Code of Our SIGIR18 Paper

Please contact the following author if you need further clarification.  
Author:  Hongwei Liang  
Contact: hit.henryleung@gmail.com  

## 1. SUMMARY

This folder includes the datasets and the code for the following paper:  
Hongwei Liang and Ke Wang. 2018. Top-k Route Search through Submodularity Modeling of Recurrent POI Features. In SIGIR'18: The 41st International ACM SIGIR Conference on Research & Development in Information Retrieval, 2018, 10 pages.  

```
@inproceedings{liang2018route,  
  title={Top-k Route Search through Submodularity Modeling of Recurrent POI Features},  
  author={Liang, Hongwei and Wang, Ke},  
  booktitle={Proceedings of the 41st International ACM SIGIR Conference on Research & Development in Information Retrieval},  
  year={2018},  
  organization={ACM}  
}  
```
Please cite this work if either the datasets or the code are used by your works.

Datasets: They are in the folder "Data". The subfolder "raw_data" contains the original check-in data for two datasets and some files we generated based on the check-in data. The subfolder “input_data” contains the real files that will be read by the program.

Code: The code in folder "OfflineIndexing" are for offline indices building; the code in folder "OnlineRouteSearch" (core part) are for loading the built indices and other input data, retrieving the POI candidates and searching for the desired routes. All the code are written in C++ (except the simple code for generating the inverted feature index) and are run on Ubuntu (Linux).

See the detailed description of the datasets and the code below.


## 2. DESCRIPTION OF THE DATASETS

### (1) "raw_data" folder
There are two datasets: sg (Singapore) and as (Austin). 

#### i. "xx_original_checkin.txt"  
The original check-in datasets we obtained from the authors of the following paper:  
Yifeng Zeng, Xuefeng Chen, Xin Cao, Shengchao Qin, Marc Cavazza, and Yanping Xiang. 2015. Optimal Route Search with the Coverage of Users' Preferences. In 24th IJCAI. AAAI Press, 2118–2124.  
Format (seperated by '\t'): user_ID	POI_ID	coordinates	checkin_time(hour:min)	checkin_date_id	city_name	categories(features)

#### ii. "xx_coors.txt"
The location information is extracted from the above check-in datasets.  
Format (seperated by '\t'): POI_ID	coordinates(latitude,longitude)

#### iii. "xx_cat_map2.txt"  
The digitalization (mapping table) of POI categories/features.  
Format (seperated by ':'): number of check-ins containing the feature:feature_name:mapped_integer  

### (2) “input_data” folder
The files in this folder are generated based on the above raw data, and are fed to the programs.  

#### i. "xx_undir_edges.txt"
The POI network we built based on the above check-in datasets following the procedures in Section 7.1.1 of our SIGIR'18 paper as mentioned in the beginning of this README.md file. We built undirected graphs for simplicity.
Format: The first line is the total number of POIs. Each of the rest lines has three fields separated by single space: source_POI destination_POI traveling_time_in_minites(by Google map under driving mode).

#### ii. "xx_undir_edges.txt.deg" and "xx_undir_edges.txt.out" 
The 2-hop labeling files produced by the code in the folder "OfflineIndexing\Hop-2 labeling-undirected\" based on the POI network files "xx_undir_edges.txt". These two are binary files, will be loaded by the program in "OnlineRouteSearch\" as the HI index.

#### iii. "xx_mat_loc_kwds.txt"
The POI * feature_ratings matrix. The feature ratings are obtained in the way as Eqn. (14) in our SIGIR'18 paper.  
Format: POI_ID:a vector of feature ratings.  
The coordinates information of each POI_ID can be found in the "xx_coors.txt" file.  
The semantic meaning of each feature_ID (column number starting from 0) can be found in the "xx_cat_map2.txt" file.  

#### iv. "xx_inverted_kwds.txt"
The inverted feature index (FI) built from the file xx_mat_loc_kwds.txt  
Format: feature_ID:POI_ID feature_rating POI_ID feature_rating ...

#### v. "as_stayTime.txt"
The staying time generated by the Gaussian distribution as described in Section 7.1.1.  
Format: POI_ID:staying_time_in_minutes

## 3. DESCRIPTION OF THE CODE

### (1) "OfflineIndexing" folder
The code in "Hop-2 labeling-directed" and "Hop-2 labeling-undirected" are for building up the 2-hop labeling from directed and undirected graph, respectively. The code are provided by the authors of the following paper:  
Jiang, Minhao, Ada Wai-Chee Fu, Raymond Chi-Wing Wong, and Yanyan Xu. "Hop doubling label indexing for point-to-point distance querying on scale-free networks." Proceedings of the VLDB Endowment 7, no. 12 (2014): 1203-1214.   

It's unnecessary to run the code in this folder if you are using our provided datasets, i.e., sg and as. The files "xx_undir_edges.txt.deg" and "xx_undir_edges.txt.out" are the input 2-hop labelling files that will be used by our program, which are the outputs by running the code in "Hop-2 labeling-undirected".   
Note that the 2-hop labels in "xx_undir_edges.txt.deg" and "xx_undir_edges.txt.out" are not in the format we want. We load and organize them by the code in "OnlineRouteSearch/Index.cpp".  
If you want to produce the 2-hop labels for other graphs, you can follow the procedures in the readme.txt file in each subfolder.  

The simple java code "FIBuilding.java" is for producing the FI index file "xx_inverted_kwds.txt" from "xx_mat_loc_kwds.txt", which is useful only when you want to user other datasets.


### (2) "OnlineRouteSearch" folder
The code in this folder are for loading the built indices and other input data, retrieving the POI candidates and searching for the desired routes.  
The provided Makefile helps you to compile the source code. See the comments in the source code files for more information.  
A sample query is provided in the file "Sample Query.txt".

## Building Tools
C++ &
Java

## Platform
Ubuntu (Linux)

## License
MIT License
