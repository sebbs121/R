---
title: "RProject"
author: "Sebastian Karkos"
date: "16 grudzień, 2018"
output: 
  html_document:
    toc: true
    toc_depth: 3
    toc_float: true
    keep_md: true
    theme: united
---


## Kod wyliczający wykorzystane biblioteki.

```r
usedPackages <- as.data.frame(installed.packages()[,c(1,3:4)])
nrow(usedPackages)
```

```
## [1] 923
```

## Kod zapewniający powtarzalność wyników przy każdym uruchomieniu raportu na tych samych danych.


```r
set.seed(100)
```


## Kod pozwalający wczytać dane z pliku.

```r
myData <- read.csv(file="/Users/Lenovo/Desktop/R PROJEKT/all_summary/all_summary.csv", header = TRUE, sep = ";", nrows=10000)
nrow(myData)
```

```
## [1] 10000
```

## Kod usuwający z danych wiersze posiadające wartość zmiennej res_name równą..


```r
newData <- subset(myData, !(res_name %in% c('UNK','UNX','UNL','DUM','N','BLOB','ALA','ARG','ASN','ASP','CYS','GLN','GLU','GLY','HIS','ILE','LEU','LYS','MET','MSE','PHE','PRO','SEC','SER','THR','TRP','TYR','VAL','DA','DG','DT','DC','DU','A','G','T','C','U','HOH','H20','WAT')))
nrow(newData)
```

```
## [1] 9940
```

## Kod przetwarzający brakujące dane.

```r
#newData <- na.omit(newData)
```

## Sekcję podsumowującą rozmiar zbioru i podstawowe statystyki.

```r
summary(newData)
```

```
##                  blob_coverage                 res_coverage 
##  '{DN6:B:306:1: 100.0}' :   3   '{ZN:B:902:3: 100.0}':   3  
##  '{EDO:A:411:6: 100.0}' :   3   '{CL:A:406:0: 84.8}' :   2  
##  '{15P:A:1001:1: 100.0}':   2   '{FMN:D:500:2: 36.5}':   2  
##  '{2M3:H:290:0: 88.4}'  :   2   '{HEM:B:148:0: 10.3}':   2  
##  '{CDL:M:800:1: 87.2}'  :   2   '{HEM:B:750:0: 61.5}':   2  
##  '{CME:F:110:1: 63.8}'  :   2   '{PR2:A:401:0: 42.6}':   2  
##  (Other)                :9926   (Other)              :9927  
##              title         pdb_code       res_name        res_id      
##  2bl2 LHG 1158 F:   7   3i3d   : 164   SO4    :1007   Min.   :   0.0  
##  2hg9 CDL 800 M :   7   1jyv   : 139   GOL    : 632   1st Qu.: 301.0  
##  3dwf NAP 301 C :   7   2bl2   : 115   EDO    : 516   Median : 501.0  
##  1rz1 FAD 4200 D:   6   1nf4   : 114   NAG    : 464   Mean   : 994.9  
##  1rz1 FAD 5200 E:   6   3s6l   :  75   CL     : 387   3rd Qu.:1018.2  
##  1xa4 COA 701 B :   6   1rz1   :  60   (Other):6770   Max.   :9043.0  
##  (Other)        :9901   (Other):9273   NA's   : 164                   
##     chain_id    blob_volume_coverage blob_volume_coverage_second
##  A      :4673   Min.   :0.02305      Min.   :0.00000            
##  B      :2314   1st Qu.:0.51239      1st Qu.:0.00000            
##  C      : 912   Median :0.72932      Median :0.00000            
##  D      : 701   Mean   :0.67185      Mean   :0.02003            
##  E      : 310   3rd Qu.:0.86952      3rd Qu.:0.00000            
##  F      : 227   Max.   :1.00000      Max.   :0.95385            
##  (Other): 803                                                   
##  res_volume_coverage res_volume_coverage_second local_res_atom_count
##  Min.   :0.003291    Min.   :0.00000            Min.   : 1.00       
##  1st Qu.:0.251621    1st Qu.:0.00000            1st Qu.: 4.00       
##  Median :0.453179    Median :0.00000            Median : 6.00       
##  Mean   :0.493952    Mean   :0.06738            Mean   :13.74       
##  3rd Qu.:0.697003    3rd Qu.:0.00000            3rd Qu.:20.00       
##  Max.   :1.000000    Max.   :1.00000            Max.   :91.00       
##                                                                     
##  local_res_atom_non_h_count local_res_atom_non_h_occupancy_sum
##  Min.   : 1.00              Min.   :-7.38                     
##  1st Qu.: 4.00              1st Qu.: 4.00                     
##  Median : 6.00              Median : 6.00                     
##  Mean   :13.51              Mean   :12.85                     
##  3rd Qu.:20.00              3rd Qu.:19.00                     
##  Max.   :91.00              Max.   :91.00                     
##                                                               
##  local_res_atom_non_h_electron_sum
##  Min.   :  7.0                    
##  1st Qu.: 30.0                    
##  Median : 48.0                    
##  Mean   :100.4                    
##  3rd Qu.:137.0                    
##  Max.   :727.0                    
##                                   
##  local_res_atom_non_h_electron_occupancy_sum local_res_atom_C_count
##  Min.   :-45.91                              Min.   : 0.000        
##  1st Qu.: 28.80                              1st Qu.: 0.000        
##  Median : 48.00                              Median : 3.000        
##  Mean   : 94.94                              Mean   : 7.651        
##  3rd Qu.:132.00                              3rd Qu.:10.000        
##  Max.   :727.00                              Max.   :67.000        
##                                                                    
##  local_res_atom_N_count local_res_atom_O_count local_res_atom_S_count
##  Min.   : 0.000         Min.   : 0.000         Min.   :0.0000        
##  1st Qu.: 0.000         1st Qu.: 1.000         1st Qu.:0.0000        
##  Median : 0.000         Median : 3.000         Median :0.0000        
##  Mean   : 1.245         Mean   : 3.791         Mean   :0.2402        
##  3rd Qu.: 2.000         3rd Qu.: 5.000         3rd Qu.:0.0000        
##  Max.   :13.000         Max.   :49.000         Max.   :8.0000        
##                                                                      
##  dict_atom_non_h_count dict_atom_non_h_electron_sum dict_atom_C_count
##  Min.   :  1.00        Min.   :  7.0                Min.   : 0.000   
##  1st Qu.:  4.00        1st Qu.: 30.0                1st Qu.: 0.000   
##  Median :  6.00        Median : 48.0                Median : 3.000   
##  Mean   : 13.87        Mean   :103.1                Mean   : 7.764   
##  3rd Qu.: 20.00        3rd Qu.:141.0                3rd Qu.:10.000   
##  Max.   :104.00        Max.   :727.0                Max.   :81.000   
##  NA's   :174           NA's   :174                  NA's   :174      
##  dict_atom_N_count dict_atom_O_count dict_atom_S_count
##  Min.   : 0.00     Min.   : 0.000    Min.   :0.00     
##  1st Qu.: 0.00     1st Qu.: 1.000    1st Qu.:0.00     
##  Median : 0.00     Median : 3.000    Median :0.00     
##  Mean   : 1.22     Mean   : 4.051    Mean   :0.24     
##  3rd Qu.: 1.00     3rd Qu.: 6.000    3rd Qu.:0.00     
##  Max.   :13.00     Max.   :49.000    Max.   :8.00     
##  NA's   :174       NA's   :174       NA's   :174      
##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         skeleton_data 
##  [[-0.009,18.043,0.552],[1.664,17.663,2.206],[1.853,17.663,2.206],[2.042,17.853,2.206],[2.165,17.853,2.390],[2.354,17.853,2.390],[2.477,18.043,2.574],[2.411,18.043,2.758],[2.534,18.233,2.942],[2.468,18.233,3.126],[1.747,18.993,0.919],[1.870,18.993,1.103]]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                :   1  
##  [[-0.019,24.948,31.604],[0.208,24.750,31.410],[0.396,23.958,31.410],[0.396,24.156,31.410],[0.396,24.354,31.410],[0.357,24.552,31.604],[0.584,23.760,31.410],[0.553,23.562,30.634],[0.514,23.562,30.828],[0.475,23.562,31.022],[0.623,23.562,31.216],[0.593,23.364,30.440],[0.632,23.166,30.246],[0.483,23.166,30.053]]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        :   1  
##  [[-0.033,0.395,32.028],[-0.068,0.197,32.220],[-1.536,1.381,30.686],[-1.342,1.381,30.686]]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     :   1  
##  [[-0.065,29.356,9.851],[0.099,29.356,10.041],[0.263,29.356,10.230],[0.427,29.356,10.419],[0.591,29.169,10.609],[0.755,29.169,10.798],[0.918,29.169,10.988],[1.109,28.982,10.988],[1.327,28.982,10.798],[1.518,28.982,10.798],[1.709,28.796,10.798],[1.926,28.796,10.609],[2.144,28.796,10.419],[2.335,28.796,10.419],[2.553,28.982,10.230],[2.744,28.982,10.230],[2.961,29.169,10.041],[3.152,29.169,10.041],[3.343,29.169,10.041],[3.507,28.982,10.230],[3.534,29.169,10.041],[3.644,28.796,10.609],[3.671,28.982,10.419],[3.752,29.169,9.851],[3.482,27.300,11.746],[3.482,27.487,11.746],[3.482,27.674,11.746],[3.482,27.861,11.746],[3.482,28.048,11.746],[3.482,28.235,11.746],[3.781,28.609,10.988],[3.754,28.609,11.177],[3.808,28.796,10.798],[3.727,28.422,11.367],[3.700,28.422,11.556],[3.969,29.356,9.662],[3.646,27.113,11.935],[3.999,28.796,10.798],[4.187,29.543,9.472],[4.190,28.796,10.798],[4.405,29.730,9.283],[4.380,28.982,10.798],[4.596,29.730,9.283],[4.544,28.796,10.988],[4.787,29.917,9.283],[4.814,30.104,9.093],[4.708,28.796,11.177],[5.031,30.291,8.904],[4.899,28.796,11.177],[5.249,30.478,8.714],[5.276,30.665,8.525],[5.090,28.609,11.177],[5.575,30.665,7.767],[5.467,30.852,8.525],[5.281,28.609,11.177],[5.602,30.478,7.578],[5.739,30.852,7.957],[5.739,31.039,7.957],[5.712,31.039,8.146],[5.685,31.039,8.336],[5.658,31.039,8.525],[5.766,31.226,7.767],[5.631,31.226,8.714],[5.604,31.413,8.904],[5.498,28.422,10.988],[5.438,30.291,7.388],[6.010,31.413,7.388],[5.983,31.413,7.578],[5.795,31.600,8.904],[5.689,28.235,10.988],[5.274,30.104,7.199],[6.037,31.600,7.199],[5.880,28.048,10.988],[5.301,30.104,7.009],[6.064,31.600,7.009],[5.328,29.917,6.820],[6.091,31.600,6.820],[5.355,29.917,6.631],[6.309,31.787,6.631],[5.382,29.917,6.441],[6.336,31.787,6.441],[5.409,29.917,6.252],[6.554,31.974,6.252],[6.554,32.161,6.252],[5.435,30.104,6.062],[6.580,31.787,6.062],[6.771,32.348,6.062],[6.962,32.535,6.062],[7.153,32.722,6.062],[7.153,32.909,6.062],[5.462,30.104,5.873],[6.417,31.600,5.873],[5.680,30.291,5.683],[6.253,31.413,5.683],[5.707,30.478,5.494],[6.089,30.852,5.494],[6.280,31.226,5.494],[5.925,30.665,5.304],[6.116,31.039,5.304]]:   1  
##  [[-0.072,76.468,13.985],[-0.076,76.468,14.179],[-0.080,76.468,14.373],[-0.083,76.468,14.568],[-0.087,76.468,14.762],[0.108,76.273,14.956],[0.303,76.077,15.150],[0.299,75.882,15.345],[0.498,75.686,15.345],[0.494,75.491,15.539],[0.693,75.295,15.539],[0.689,75.099,15.733],[0.689,74.904,15.733],[0.686,74.708,15.927]]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    :   1  
##  [[-0.089,38.488,78.835],[-0.089,38.488,79.027],[0.108,38.488,79.220],[-0.879,38.290,78.642],[-0.681,38.290,78.642],[-0.484,38.488,78.642],[-0.287,38.488,78.642],[-0.089,38.488,78.642],[-1.076,38.290,78.642],[0.108,38.488,78.449],[0.306,38.488,78.256],[0.504,38.290,78.256],[0.702,38.091,78.064],[0.900,37.893,78.064],[1.097,37.893,78.064]]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           :   1  
##  (Other)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       :9934  
##  skeleton_cycle_4   skeleton_diameter skeleton_cycle_6 skeleton_cycle_7   
##  Min.   :  0.0000   Min.   :  0.00    Min.   : 0.000   Min.   : 0.000000  
##  1st Qu.:  0.0000   1st Qu.:  2.00    1st Qu.: 0.000   1st Qu.: 0.000000  
##  Median :  0.0000   Median : 13.00    Median : 0.000   Median : 0.000000  
##  Mean   :  0.4751   Mean   : 23.85    Mean   : 0.017   Mean   : 0.008149  
##  3rd Qu.:  0.0000   3rd Qu.: 32.00    3rd Qu.: 0.000   3rd Qu.: 0.000000  
##  Max.   :630.0000   Max.   :270.00    Max.   :33.000   Max.   :17.000000  
##                                                                           
##  skeleton_closeness_006_008 skeleton_closeness_002_004 skeleton_cycle_3  
##  Min.   :  0.000            Min.   :  0.00000          Min.   :  0.0000  
##  1st Qu.:  0.000            1st Qu.:  0.00000          1st Qu.:  0.0000  
##  Median :  0.000            Median :  0.00000          Median :  0.0000  
##  Mean   :  2.493            Mean   :  0.06137          Mean   :  0.1061  
##  3rd Qu.:  0.000            3rd Qu.:  0.00000          3rd Qu.:  0.0000  
##  Max.   :183.000            Max.   :172.00000          Max.   :191.0000  
##                                                                          
##  skeleton_avg_degree skeleton_closeness_004_006 skeleton_closeness_010_012
##  Min.   :0.000       Min.   :  0.0000           Min.   :  0.000           
##  1st Qu.:1.333       1st Qu.:  0.0000           1st Qu.:  0.000           
##  Median :1.867       Median :  0.0000           Median :  0.000           
##  Mean   :1.556       Mean   :  0.7929           Mean   :  3.488           
##  3rd Qu.:1.959       3rd Qu.:  0.0000           3rd Qu.:  0.000           
##  Max.   :5.976       Max.   :378.0000           Max.   :113.000           
##                                                                           
##  skeleton_closeness_012_014 skeleton_edges    skeleton_radius 
##  Min.   :  0.00             Min.   :   0.00   Min.   :  0.00  
##  1st Qu.:  0.00             1st Qu.:   2.00   1st Qu.:  1.00  
##  Median :  0.00             Median :  13.00   Median :  7.00  
##  Mean   :  3.23             Mean   :  35.22   Mean   : 12.21  
##  3rd Qu.:  0.00             3rd Qu.:  38.00   3rd Qu.: 16.00  
##  Max.   :121.00             Max.   :1996.00   Max.   :135.00  
##                                                               
##  skeleton_cycle_8_plus skeleton_closeness_020_030 skeleton_deg_5_plus
##  Min.   :  0.0000      Min.   :  0.000            Min.   :  0.000    
##  1st Qu.:  0.0000      1st Qu.:  0.000            1st Qu.:  0.000    
##  Median :  0.0000      Median :  0.000            Median :  0.000    
##  Mean   :  0.2928      Mean   :  5.064            Mean   :  0.321    
##  3rd Qu.:  0.0000      3rd Qu.:  1.000            3rd Qu.:  0.000    
##  Max.   :424.0000      Max.   :588.000            Max.   :633.000    
##                                                                      
##  skeleton_closeness_016_018 skeleton_closeness_008_010
##  Min.   : 0.000             Min.   :  0.000           
##  1st Qu.: 0.000             1st Qu.:  0.000           
##  Median : 0.000             Median :  0.000           
##  Mean   : 2.223             Mean   :  3.505           
##  3rd Qu.: 0.000             3rd Qu.:  0.000           
##  Max.   :61.000             Max.   :171.000           
##                                                       
##  skeleton_closeness_018_020 skeleton_average_clustering
##  Min.   : 0.000             Min.   :0.0000000          
##  1st Qu.: 0.000             1st Qu.:0.0000000          
##  Median : 0.000             Median :0.0000000          
##  Mean   : 1.706             Mean   :0.0001360          
##  3rd Qu.: 0.000             3rd Qu.:0.0000000          
##  Max.   :60.000             Max.   :0.1225906          
##                                                        
##  skeleton_closeness_040_050 skeleton_closeness_014_016 skeleton_center 
##  Min.   :  0.000            Min.   : 0.000             Min.   : 1.000  
##  1st Qu.:  0.000            1st Qu.: 0.000             1st Qu.: 1.000  
##  Median :  0.000            Median : 0.000             Median : 1.000  
##  Mean   :  1.865            Mean   : 2.786             Mean   : 1.618  
##  3rd Qu.:  1.000            3rd Qu.: 0.000             3rd Qu.: 2.000  
##  Max.   :520.000            Max.   :92.000             Max.   :84.000  
##                                                                        
##  skeleton_closeness_000_002 skeleton_density  skeleton_closeness_030_040
##  Min.   :0.0000             Min.   :0.00000   Min.   :  0.000           
##  1st Qu.:0.0000             1st Qu.:0.02500   1st Qu.:  0.000           
##  Median :0.0000             Median :0.09524   Median :  0.000           
##  Mean   :0.1151             Mean   :0.22750   Mean   :  2.729           
##  3rd Qu.:0.0000             3rd Qu.:0.25000   3rd Qu.:  1.000           
##  Max.   :1.0000             Max.   :1.00000   Max.   :508.000           
##                                                                         
##  skeleton_deg_4    skeleton_deg_0   skeleton_deg_1   skeleton_deg_2  
##  Min.   : 0.0000   Min.   :0.0000   Min.   : 0.000   Min.   :  0.00  
##  1st Qu.: 0.0000   1st Qu.:0.0000   1st Qu.: 2.000   1st Qu.:  1.00  
##  Median : 0.0000   Median :0.0000   Median : 2.000   Median : 11.00  
##  Mean   : 0.1061   Mean   :0.1151   Mean   : 3.172   Mean   : 29.73  
##  3rd Qu.: 0.0000   3rd Qu.:0.0000   3rd Qu.: 3.000   3rd Qu.: 34.00  
##  Max.   :36.0000   Max.   :1.0000   Max.   :54.000   Max.   :468.00  
##                                                                      
##  skeleton_deg_3  skeleton_graph_clique_number skeleton_nodes  
##  Min.   : 0.00   Min.   :1.000                Min.   :  1.00  
##  1st Qu.: 0.00   1st Qu.:2.000                1st Qu.:  3.00  
##  Median : 0.00   Median :2.000                Median : 14.00  
##  Mean   : 1.82   Mean   :1.891                Mean   : 35.26  
##  3rd Qu.: 2.00   3rd Qu.:2.000                3rd Qu.: 39.00  
##  Max.   :98.00   Max.   :4.000                Max.   :690.00  
##                                                               
##  skeleton_cycles     skeleton_cycle_5   skeleton_closeness_050_plus
##  Min.   :   0.0000   Min.   : 0.00000   Min.   : 0.000             
##  1st Qu.:   0.0000   1st Qu.: 0.00000   1st Qu.: 0.000             
##  Median :   0.0000   Median : 0.00000   Median : 3.000             
##  Mean   :   0.9606   Mean   : 0.06147   Mean   : 5.204             
##  3rd Qu.:   0.0000   3rd Qu.: 0.00000   3rd Qu.:11.000             
##  Max.   :1329.0000   Max.   :83.00000   Max.   :27.000             
##                                                                    
##  skeleton_periphery  local_volume     local_electrons     local_mean      
##  Min.   : 1         Min.   :   96.8   Min.   :  0.244   Min.   :0.001332  
##  1st Qu.: 2         1st Qu.:  206.0   1st Qu.:  3.342   1st Qu.:0.012203  
##  Median : 2         Median :  328.2   Median :  7.586   Median :0.018941  
##  Mean   : 2         Mean   :  831.4   Mean   : 17.548   Mean   :0.023739  
##  3rd Qu.: 2         3rd Qu.:  756.2   3rd Qu.: 19.458   3rd Qu.:0.028502  
##  Max.   :42         Max.   :36718.6   Max.   :242.358   Max.   :0.240600  
##                                                                           
##    local_std          local_min   local_max        local_max_over_std
##  Min.   :0.009638   Min.   :0   Min.   : 0.07639   Min.   :  3.076   
##  1st Qu.:0.069449   1st Qu.:0   1st Qu.: 0.57607   1st Qu.:  5.230   
##  Median :0.099135   Median :0   Median : 0.89419   Median :  7.207   
##  Mean   :0.122865   Mean   :0   Mean   : 1.34756   Mean   :  9.811   
##  3rd Qu.:0.142580   3rd Qu.:0   3rd Qu.: 1.49320   3rd Qu.: 11.269   
##  Max.   :1.399740   Max.   :0   Max.   :29.31470   Max.   :107.375   
##                                                                      
##  local_skewness    local_cut_by_mainchain_volume local_near_cut_count_C
##  Min.   :0.01853   Min.   : 0.0000               Min.   :  0.000       
##  1st Qu.:0.12308   1st Qu.: 0.0000               1st Qu.:  0.000       
##  Median :0.17425   Median : 0.0000               Median :  2.000       
##  Mean   :0.22274   Mean   : 0.3649               Mean   :  4.258       
##  3rd Qu.:0.25541   3rd Qu.: 0.0320               3rd Qu.:  6.000       
##  Max.   :3.15507   Max.   :24.1120               Max.   :100.000       
##                                                                        
##  local_near_cut_count_other local_near_cut_count_S local_near_cut_count_O
##  Min.   :0.00000            Min.   :0.0000         Min.   : 0.000        
##  1st Qu.:0.00000            1st Qu.:0.0000         1st Qu.: 0.000        
##  Median :0.00000            Median :0.0000         Median : 1.000        
##  Mean   :0.01087            Mean   :0.1259         Mean   : 2.061        
##  3rd Qu.:0.00000            3rd Qu.:0.0000         3rd Qu.: 3.000        
##  Max.   :3.00000            Max.   :9.0000         Max.   :31.000        
##                                                                          
##  local_near_cut_count_N part_00_shape_segments_count
##  Min.   : 0.000         Min.   :     1.0            
##  1st Qu.: 0.000         1st Qu.:     4.0            
##  Median : 1.000         Median :    23.0            
##  Mean   : 2.073         Mean   :   345.3            
##  3rd Qu.: 3.000         3rd Qu.:   134.0            
##  Max.   :34.000         Max.   :114577.0            
##                                                     
##  part_00_density_segments_count part_00_volume    part_00_electrons
##  Min.   :     1.0               Min.   :  2.048   Min.   :  0.244  
##  1st Qu.:     4.0               1st Qu.:  6.742   1st Qu.:  3.331  
##  Median :    23.0               Median : 13.620   Median :  7.529  
##  Mean   :   345.3               Mean   : 32.367   Mean   : 17.339  
##  3rd Qu.:   134.0               3rd Qu.: 32.810   3rd Qu.: 18.860  
##  Max.   :114577.0               Max.   :998.776   Max.   :242.358  
##                                                                    
##   part_00_mean      part_00_std        part_00_max      
##  Min.   :0.07102   Min.   :0.002262   Min.   : 0.07639  
##  1st Qu.:0.37295   1st Qu.:0.064717   1st Qu.: 0.57607  
##  Median :0.51168   Median :0.121043   Median : 0.89419  
##  Mean   :0.59839   Mean   :0.208430   Mean   : 1.34756  
##  3rd Qu.:0.70898   3rd Qu.:0.229274   3rd Qu.: 1.49320  
##  Max.   :6.76074   Max.   :6.159016   Max.   :29.31473  
##                                                         
##  part_00_max_over_std part_00_skewness   part_00_parts   
##  Min.   :  3.076      Min.   :0.001729   Min.   : 1.000  
##  1st Qu.:  5.230      1st Qu.:0.056066   1st Qu.: 1.000  
##  Median :  7.207      Median :0.110448   Median : 1.000  
##  Mean   :  9.811      Mean   :0.214761   Mean   : 1.069  
##  3rd Qu.: 11.269      3rd Qu.:0.232662   3rd Qu.: 1.000  
##  Max.   :107.375      Max.   :6.647288   Max.   :21.000  
##                                                          
##  part_00_shape_O3    part_00_shape_O4    part_00_shape_O5   
##  Min.   :     2507   Min.   :2.028e+06   Min.   :5.290e+08  
##  1st Qu.:    25449   1st Qu.:1.621e+08   1st Qu.:2.975e+11  
##  Median :    87652   Median :1.960e+09   Median :1.158e+13  
##  Mean   :  1635959   Mean   :7.458e+12   Mean   :2.809e+19  
##  3rd Qu.:   542865   3rd Qu.:6.207e+10   3rd Qu.:1.622e+15  
##  Max.   :181759457   Max.   :8.936e+15   Max.   :1.073e+23  
##                                                             
##  part_00_shape_FL    part_00_shape_O3_norm part_00_shape_O4_norm
##  Min.   :6.388e+04   Min.   :0.2311        Min.   :0.01780      
##  1st Qu.:7.342e+08   1st Qu.:0.2677        1st Qu.:0.02138      
##  Median :2.804e+10   Median :0.3756        Median :0.03284      
##  Mean   :8.512e+15   Mean   :0.4863        Mean   :0.06029      
##  3rd Qu.:4.707e+12   3rd Qu.:0.6007        3rd Qu.:0.06755      
##  Max.   :1.436e+19   Max.   :3.0243        Max.   :1.37240      
##                                                                 
##  part_00_shape_O5_norm part_00_shape_FL_norm part_00_shape_I1   
##  Min.   :0.0004538     Min.   : 0.000001     Min.   :3.031e+04  
##  1st Qu.:0.0005074     1st Qu.: 0.000565     1st Qu.:1.070e+06  
##  Median :0.0007430     Median : 0.005209     Median :6.635e+06  
##  Mean   :0.0018959     Mean   : 0.052230     Mean   :1.952e+09  
##  3rd Qu.:0.0016834     3rd Qu.: 0.031522     3rd Qu.:1.139e+08  
##  Max.   :0.1020111     Max.   :11.295451     Max.   :5.280e+11  
##                                                                 
##  part_00_shape_I2    part_00_shape_I3    part_00_shape_I4   
##  Min.   :2.302e+08   Min.   :1.926e+08   Min.   :2.594e+04  
##  1st Qu.:1.763e+11   1st Qu.:4.283e+11   1st Qu.:3.647e+08  
##  Median :6.395e+12   Median :1.788e+13   Median :1.524e+10  
##  Mean   :2.103e+19   Mean   :1.177e+20   Mean   :4.930e+15  
##  3rd Qu.:1.476e+15   3rd Qu.:6.395e+15   3rd Qu.:2.749e+12  
##  Max.   :3.894e+22   Max.   :1.615e+23   Max.   :7.692e+18  
##                                                             
##  part_00_shape_I5    part_00_shape_I6    part_00_shape_I1_norm
##  Min.   :1.460e+02   Min.   :2.681e+07   Min.   : 0.06365     
##  1st Qu.:5.196e+07   1st Qu.:1.196e+10   1st Qu.: 0.09602     
##  Median :4.615e+09   Median :2.631e+11   Median : 0.22217     
##  Mean   :2.541e+15   Mean   :4.849e+16   Mean   : 0.51797     
##  3rd Qu.:1.176e+12   3rd Qu.:3.153e+13   3rd Qu.: 0.58113     
##  Max.   :3.249e+18   Max.   :5.499e+19   Max.   :17.17392     
##                                                               
##  part_00_shape_I2_norm part_00_shape_I3_norm part_00_shape_I4_norm
##  Min.   : 0.00108      Min.   :  0.00081     Min.   :0.000001     
##  1st Qu.: 0.00187      1st Qu.:  0.00291     1st Qu.:0.000262     
##  Median : 0.00628      Median :  0.02318     Median :0.002903     
##  Mean   : 0.07609      Mean   :  0.66998     Mean   :0.035171     
##  3rd Qu.: 0.03293      3rd Qu.:  0.18915     3rd Qu.:0.019390     
##  Max.   :32.66772      Max.   :171.94542     Max.   :7.900524     
##                                                                   
##  part_00_shape_I5_norm part_00_shape_I6_norm part_00_shape_M000
##  Min.   :0.000000      Min.   : 0.00491      Min.   :   256.0  
##  1st Qu.:0.000037      1st Qu.: 0.01021      1st Qu.:   842.8  
##  Median :0.001002      Median : 0.04039      Median :  1702.5  
##  Mean   :0.023798      Mean   : 0.29181      Mean   :  4045.9  
##  3rd Qu.:0.009383      3rd Qu.: 0.19054      3rd Qu.:  4101.2  
##  Max.   :7.308983      Max.   :32.28797      Max.   :124847.0  
##                                                                
##  part_00_shape_CI    part_00_shape_E3_E1 part_00_shape_E2_E1
##  Min.   :-36.83657   Min.   :0.008551    Min.   :0.01244    
##  1st Qu.: -0.73128   1st Qu.:0.087277    1st Qu.:0.21742    
##  Median :  0.00015   Median :0.174404    Median :0.39266    
##  Mean   :  0.05592   Mean   :0.246055    Mean   :0.42848    
##  3rd Qu.:  0.78368   3rd Qu.:0.362703    3rd Qu.:0.61942    
##  Max.   : 35.22041   Max.   :0.987816    Max.   :1.00000    
##                                                             
##  part_00_shape_E3_E2 part_00_shape_sqrt_E1 part_00_shape_sqrt_E2
##  Min.   :0.0328      Min.   : 1.918        Min.   : 1.234       
##  1st Qu.:0.3699      1st Qu.: 3.928        1st Qu.: 2.570       
##  Median :0.5708      Median : 5.759        Median : 3.410       
##  Mean   :0.5530      Mean   : 7.918        Mean   : 4.394       
##  3rd Qu.:0.7423      3rd Qu.: 9.753        3rd Qu.: 5.195       
##  Max.   :0.9993      Max.   :40.364        Max.   :26.087       
##                                                                 
##  part_00_shape_sqrt_E3 part_00_density_O3 part_00_density_O4 
##  Min.   : 0.853        Min.   :     388   Min.   :3.937e+04  
##  1st Qu.: 1.952        1st Qu.:   12286   1st Qu.:3.652e+07  
##  Median : 2.544        Median :   43232   Median :4.698e+08  
##  Mean   : 2.901        Mean   :  773765   Mean   :1.241e+12  
##  3rd Qu.: 3.403        3rd Qu.:  241483   3rd Qu.:1.253e+10  
##  Max.   :12.805        Max.   :70068932   Max.   :1.101e+15  
##                                                              
##  part_00_density_O5  part_00_density_FL  part_00_density_O3_norm
##  Min.   :1.190e+06   Min.   :6.341e+03   Min.   :0.04382        
##  1st Qu.:3.105e+10   1st Qu.:1.354e+08   1st Qu.:0.37621        
##  Median :1.434e+12   Median :6.171e+09   Median :0.59548        
##  Mean   :9.496e+17   Mean   :1.588e+15   Mean   :0.73160        
##  3rd Qu.:1.519e+14   3rd Qu.:9.236e+11   3rd Qu.:0.93646        
##  Max.   :2.089e+21   Max.   :4.076e+18   Max.   :6.15261        
##                                                                 
##  part_00_density_O4_norm part_00_density_O5_norm part_00_density_FL_norm
##  Min.   :0.000631        Min.   :0.000003        Min.   :  0.00000      
##  1st Qu.:0.041194        1st Qu.:0.001263        1st Qu.:  0.00178      
##  Median :0.084885        Median :0.003063        Median :  0.01781      
##  Mean   :0.141015        Mean   :0.007141        Mean   :  0.26371      
##  3rd Qu.:0.173467        3rd Qu.:0.007404        3rd Qu.:  0.12745      
##  Max.   :5.692793        Max.   :0.572401        Max.   :197.49046      
##                                                                         
##  part_00_density_I1  part_00_density_I2  part_00_density_I3 
##  Min.   :5.581e+03   Min.   :6.416e+06   Min.   :6.765e+06  
##  1st Qu.:4.862e+05   1st Qu.:3.702e+10   1st Qu.:8.909e+10  
##  Median :3.000e+06   Median :1.315e+12   Median :3.686e+12  
##  Mean   :8.706e+08   Mean   :3.003e+18   Mean   :1.800e+19  
##  3rd Qu.:4.853e+07   3rd Qu.:2.705e+14   3rd Qu.:1.211e+15  
##  Max.   :2.241e+11   Max.   :9.670e+21   Max.   :2.508e+22  
##                                                             
##  part_00_density_I4  part_00_density_I5  part_00_density_I6 
##  Min.   :2.562e+03   Min.   :4.300e+01   Min.   :8.342e+05  
##  1st Qu.:7.042e+07   1st Qu.:1.428e+07   1st Qu.:2.588e+09  
##  Median :3.770e+09   Median :1.523e+09   Median :5.849e+10  
##  Mean   :9.508e+14   Mean   :5.262e+14   Mean   :7.703e+15  
##  3rd Qu.:6.417e+11   3rd Qu.:3.480e+11   3rd Qu.:6.068e+12  
##  Max.   :2.125e+18   Max.   :1.019e+18   Max.   :7.364e+18  
##                                                             
##  part_00_density_I1_norm part_00_density_I2_norm part_00_density_I3_norm
##  Min.   :  0.00303       Min.   :   0.0000       Min.   :   0.000       
##  1st Qu.:  0.20677       1st Qu.:   0.0083       1st Qu.:   0.014       
##  Median :  0.56036       Median :   0.0431       Median :   0.138       
##  Mean   :  1.34074       Mean   :   0.6992       Mean   :   6.829       
##  3rd Qu.:  1.46418       3rd Qu.:   0.2225       3rd Qu.:   1.193       
##  Max.   :102.77749       Max.   :1182.2106       Max.   :6758.274       
##                                                                         
##  part_00_density_I4_norm part_00_density_I5_norm part_00_density_I6_norm
##  Min.   :  0.00000       Min.   :  0.00000       Min.   :  0.0000       
##  1st Qu.:  0.00088       1st Qu.:  0.00017       1st Qu.:  0.0314       
##  Median :  0.01081       Median :  0.00448       Median :  0.1562       
##  Mean   :  0.20084       Mean   :  0.15892       Mean   :  1.3526       
##  3rd Qu.:  0.08325       3rd Qu.:  0.04546       3rd Qu.:  0.7373       
##  Max.   :171.31354       Max.   :153.86227       Max.   :350.5213       
##                                                                         
##  part_00_density_M000 part_00_density_CI  part_00_density_E3_E1
##  Min.   :   30.5      Min.   :-44.20442   Min.   :0.00819      
##  1st Qu.:  416.3      1st Qu.: -0.77139   1st Qu.:0.08411      
##  Median :  941.1      Median :  0.00023   Median :0.17362      
##  Mean   : 2167.4      Mean   :  0.07351   Mean   :0.24951      
##  3rd Qu.: 2357.4      3rd Qu.:  0.87353   3rd Qu.:0.37243      
##  Max.   :30294.7      Max.   : 43.00719   Max.   :0.98388      
##                                                                
##  part_00_density_E2_E1 part_00_density_E3_E2 part_00_density_sqrt_E1
##  Min.   :0.01229       Min.   :0.03514       Min.   : 1.664         
##  1st Qu.:0.21321       1st Qu.:0.36777       1st Qu.: 3.677         
##  Median :0.39413       Median :0.57392       Median : 5.418         
##  Mean   :0.42968       Mean   :0.55528       Mean   : 7.588         
##  3rd Qu.:0.62258       3rd Qu.:0.74854       3rd Qu.: 9.406         
##  Max.   :1.00000       Max.   :0.99954       Max.   :39.831         
##                                                                     
##  part_00_density_sqrt_E2 part_00_density_sqrt_E3 part_00_shape_Z_7_3
##  Min.   : 1.223          Min.   : 0.8457         Min.   :  8.044    
##  1st Qu.: 2.438          1st Qu.: 1.8701         1st Qu.: 15.021    
##  Median : 3.189          Median : 2.3960         Median : 25.768    
##  Mean   : 4.168          Mean   : 2.7410         Mean   : 40.398    
##  3rd Qu.: 4.892          3rd Qu.: 3.1650         3rd Qu.: 52.320    
##  Max.   :25.853          Max.   :11.8534         Max.   :336.806    
##                                                                     
##  part_00_shape_Z_0_0 part_00_shape_Z_7_0 part_00_shape_Z_7_1
##  Min.   :  7.818     Min.   :  1.806     Min.   :  4.875    
##  1st Qu.: 14.184     1st Qu.:  6.459     1st Qu.:  9.641    
##  Median : 20.160     Median :  9.762     Median : 16.884    
##  Mean   : 25.886     Mean   : 17.163     Mean   : 27.706    
##  3rd Qu.: 31.291     3rd Qu.: 21.697     3rd Qu.: 36.137    
##  Max.   :172.641     Max.   :143.492     Max.   :240.539    
##                                                             
##  part_00_shape_Z_3_0 part_00_shape_Z_5_2 part_00_shape_Z_6_1
##  Min.   :  1.067     Min.   :  5.994     Min.   :  2.431    
##  1st Qu.:  5.812     1st Qu.: 14.173     1st Qu.: 11.539    
##  Median : 10.335     Median : 23.669     Median : 20.018    
##  Mean   : 14.823     Mean   : 34.448     Mean   : 31.315    
##  3rd Qu.: 19.466     3rd Qu.: 44.817     3rd Qu.: 41.627    
##  Max.   :122.989     Max.   :291.527     Max.   :249.778    
##                                                             
##  part_00_shape_Z_3_1 part_00_shape_Z_6_0 part_00_shape_Z_2_1
##  Min.   :  4.041     Min.   :  0.08336   Min.   :  4.503    
##  1st Qu.: 11.037     1st Qu.:  5.41245   1st Qu.: 19.037    
##  Median : 17.534     Median :  9.64381   Median : 28.007    
##  Mean   : 24.064     Mean   : 14.76078   Mean   : 37.825    
##  3rd Qu.: 30.738     3rd Qu.: 19.13086   3rd Qu.: 46.757    
##  Max.   :199.398     Max.   :133.02247   Max.   :251.895    
##                                                             
##  part_00_shape_Z_6_3 part_00_shape_Z_2_0 part_00_shape_Z_6_2
##  Min.   :  6.003     Min.   :  2.532     Min.   :  4.645    
##  1st Qu.: 18.020     1st Qu.: 14.142     1st Qu.: 15.680    
##  Median : 29.819     Median : 21.255     Median : 26.704    
##  Mean   : 45.906     Mean   : 27.825     Mean   : 41.467    
##  3rd Qu.: 59.917     3rd Qu.: 34.707     3rd Qu.: 54.633    
##  Max.   :336.974     Max.   :185.947     Max.   :306.014    
##                                                             
##  part_00_shape_Z_5_0 part_00_shape_Z_5_1 part_00_shape_Z_4_2
##  Min.   :  1.224     Min.   :  4.44      Min.   :  5.519    
##  1st Qu.:  5.464     1st Qu.: 11.09      1st Qu.: 18.683    
##  Median : 11.737     Median : 19.46      Median : 30.452    
##  Mean   : 17.975     Mean   : 28.43      Mean   : 43.222    
##  3rd Qu.: 24.338     3rd Qu.: 37.35      3rd Qu.: 55.416    
##  Max.   :140.614     Max.   :243.47      Max.   :300.629    
##                                                             
##  part_00_shape_Z_1_0 part_00_shape_Z_4_1 part_00_shape_Z_7_2
##  Min.   :0.9021      Min.   :  4.007     Min.   :  7.019    
##  1st Qu.:1.2620      1st Qu.: 15.513     1st Qu.: 12.774    
##  Median :1.4097      Median : 26.207     Median : 22.526    
##  Mean   :1.4293      Mean   : 37.200     Mean   : 35.886    
##  3rd Qu.:1.5768      3rd Qu.: 48.476     3rd Qu.: 46.821    
##  Max.   :2.1742      Max.   :258.269     Max.   :302.181    
##                                                             
##  part_00_shape_Z_4_0 part_00_density_Z_7_3 part_00_density_Z_0_0
##  Min.   :  0.08809   Min.   :  5.763       Min.   : 2.699       
##  1st Qu.:  7.93420   1st Qu.: 10.340       1st Qu.: 9.969       
##  Median : 14.45545   Median : 18.858       Median :14.989       
##  Mean   : 20.30294   Mean   : 29.937       Mean   :18.925       
##  3rd Qu.: 27.11393   3rd Qu.: 38.474       3rd Qu.:23.723       
##  Max.   :169.90882   Max.   :193.941       Max.   :85.043       
##                                                                 
##  part_00_density_Z_7_0 part_00_density_Z_7_1 part_00_density_Z_3_0
##  Min.   :  1.335       Min.   :  4.493       Min.   : 1.242       
##  1st Qu.:  5.954       1st Qu.:  7.353       1st Qu.: 4.304       
##  Median :  8.213       Median : 13.549       Median : 7.570       
##  Mean   : 14.803       Mean   : 22.030       Mean   :11.141       
##  3rd Qu.: 18.714       3rd Qu.: 28.583       3rd Qu.:14.424       
##  Max.   :111.307       Max.   :150.532       Max.   :76.672       
##                                                                   
##  part_00_density_Z_5_2 part_00_density_Z_6_1 part_00_density_Z_3_1
##  Min.   :  3.899       Min.   :  1.290       Min.   :  2.466      
##  1st Qu.:  9.688       1st Qu.:  7.194       1st Qu.:  7.402      
##  Median : 17.242       Median : 16.745       Median : 12.124      
##  Mean   : 25.201       Mean   : 24.483       Mean   : 17.086      
##  3rd Qu.: 32.866       3rd Qu.: 33.745       3rd Qu.: 21.819      
##  Max.   :166.642       Max.   :133.516       Max.   :115.011      
##                                                                   
##  part_00_density_Z_6_0 part_00_density_Z_2_1 part_00_density_Z_6_3
##  Min.   : 0.1065       Min.   :  2.902       Min.   :  2.348      
##  1st Qu.: 3.4826       1st Qu.: 14.054       1st Qu.: 11.577      
##  Median : 7.9114       Median : 21.048       Median : 22.944      
##  Mean   :12.5803       Mean   : 27.793       Mean   : 33.829      
##  3rd Qu.:17.2022       3rd Qu.: 34.408       3rd Qu.: 44.437      
##  Max.   :97.0463       Max.   :126.304       Max.   :181.684      
##                                                                   
##  part_00_density_Z_2_0 part_00_density_Z_6_2 part_00_density_Z_5_0
##  Min.   : 2.067        Min.   :  1.902       Min.   :  1.016      
##  1st Qu.:10.775        1st Qu.: 10.113       1st Qu.:  5.043      
##  Median :16.699        Median : 21.146       Median :  9.502      
##  Mean   :21.467        Mean   : 31.145       Mean   : 14.712      
##  3rd Qu.:27.529        3rd Qu.: 41.347       3rd Qu.: 19.331      
##  Max.   :94.741        Max.   :173.363       Max.   :101.082      
##                                                                   
##  part_00_density_Z_5_1 part_00_density_Z_4_2 part_00_density_Z_1_0
##  Min.   :  3.150       Min.   :  2.282       Min.   :0.8848       
##  1st Qu.:  7.944       1st Qu.: 13.753       1st Qu.:1.2507       
##  Median : 14.782       Median : 23.099       Median :1.3983       
##  Mean   : 21.559       Mean   : 31.924       Mean   :1.4198       
##  3rd Qu.: 28.435       3rd Qu.: 41.215       3rd Qu.:1.5708       
##  Max.   :147.579       Max.   :158.602       Max.   :2.1713       
##                                                                   
##  part_00_density_Z_4_1 part_00_density_Z_7_2 part_00_density_Z_4_0
##  Min.   :  1.811       Min.   :  5.110       Min.   : 0.1146      
##  1st Qu.: 12.099       1st Qu.:  9.045       1st Qu.: 6.8465      
##  Median : 20.593       Median : 17.045       Median :12.5095      
##  Mean   : 28.255       Mean   : 27.270       Mean   :16.9774      
##  3rd Qu.: 36.786       3rd Qu.: 35.277       3rd Qu.:22.9285      
##  Max.   :143.250       Max.   :177.414       Max.   :93.7575      
##                                                                   
##  part_01_shape_segments_count part_01_density_segments_count
##  Min.   :    0.0              Min.   :    0.0               
##  1st Qu.:    3.0              1st Qu.:    3.0               
##  Median :   14.0              Median :   14.0               
##  Mean   :  287.9              Mean   :  287.9               
##  3rd Qu.:   90.0              3rd Qu.:   90.0               
##  Max.   :69202.0              Max.   :69202.0               
##                                                             
##  part_01_volume    part_01_electrons  part_01_mean     part_01_std     
##  Min.   :  0.000   Min.   :  0.000   Min.   :0.0000   Min.   :0.00000  
##  1st Qu.:  4.096   1st Qu.:  2.183   1st Qu.:0.4083   1st Qu.:0.05213  
##  Median :  9.888   Median :  5.977   Median :0.5608   Median :0.10776  
##  Mean   : 24.983   Mean   : 14.939   Mean   :0.6502   Mean   :0.19789  
##  3rd Qu.: 24.790   3rd Qu.: 16.414   3rd Qu.:0.7762   3rd Qu.:0.21873  
##  Max.   :827.504   Max.   :217.253   Max.   :7.6615   Max.   :6.16373  
##                                                                        
##   part_01_max      part_01_max_over_std part_01_skewness  part_01_parts   
##  Min.   : 0.0000   Min.   :  0.000      Min.   :0.00000   Min.   : 0.000  
##  1st Qu.: 0.5755   1st Qu.:  5.230      1st Qu.:0.04388   1st Qu.: 1.000  
##  Median : 0.8940   Median :  7.207      Median :0.09666   Median : 1.000  
##  Mean   : 1.3450   Mean   :  9.784      Mean   :0.20181   Mean   : 1.262  
##  3rd Qu.: 1.4932   3rd Qu.: 11.269      3rd Qu.:0.21830   3rd Qu.: 1.000  
##  Max.   :29.3147   Max.   :107.375      Max.   :6.68747   Max.   :21.000  
##                                                                           
##  part_01_shape_O3    part_01_shape_O4    part_01_shape_O5   
##  Min.   :       76   Min.   :1.854e+03   Min.   :1.413e+04  
##  1st Qu.:    12301   1st Qu.:3.722e+07   1st Qu.:3.049e+10  
##  Median :    52449   Median :6.971e+08   Median :2.565e+12  
##  Mean   :  1237071   Mean   :4.366e+12   Mean   :1.214e+19  
##  3rd Qu.:   347976   3rd Qu.:2.524e+10   3rd Qu.:4.453e+14  
##  Max.   :140189341   Max.   :5.397e+15   Max.   :5.008e+22  
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_shape_FL    part_01_shape_O3_norm part_01_shape_O4_norm
##  Min.   :6.100e+01   Min.   :0.2275        Min.   :0.01701      
##  1st Qu.:7.327e+07   1st Qu.:0.2582        1st Qu.:0.02027      
##  Median :7.264e+09   Median :0.3551        Median :0.02977      
##  Mean   :5.134e+15   Mean   :0.5231        Mean   :0.07041      
##  3rd Qu.:1.886e+12   3rd Qu.:0.6612        3rd Qu.:0.07749      
##  Max.   :9.410e+18   Max.   :5.0072        Max.   :2.59546      
##  NA's   :80          NA's   :80            NA's   :80           
##  part_01_shape_O5_norm part_01_shape_FL_norm part_01_shape_I1   
##  Min.   :0.00041       Min.   :  0.00000     Min.   :2.180e+02  
##  1st Qu.:0.00048       1st Qu.:  0.00032     1st Qu.:3.787e+05  
##  Median :0.00067       Median :  0.00389     Median :3.239e+06  
##  Mean   :0.00240       Mean   :  0.13631     Mean   :1.460e+09  
##  3rd Qu.:0.00195       3rd Qu.:  0.04588     3rd Qu.:6.506e+07  
##  Max.   :0.27844       Max.   :190.55980     Max.   :3.896e+11  
##  NA's   :80            NA's   :80            NA's   :80         
##  part_01_shape_I2    part_01_shape_I3    part_01_shape_I4   
##  Min.   :1.171e+04   Min.   :1.128e+04   Min.   :2.600e+01  
##  1st Qu.:2.213e+10   1st Qu.:4.764e+10   1st Qu.:3.606e+07  
##  Median :1.521e+12   Median :4.247e+12   Median :4.057e+09  
##  Mean   :1.157e+19   Mean   :6.909e+19   Mean   :3.038e+15  
##  3rd Qu.:4.711e+14   3rd Qu.:2.283e+15   3rd Qu.:1.165e+12  
##  Max.   :2.408e+22   Max.   :9.072e+22   Max.   :5.114e+18  
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_shape_I5    part_01_shape_I6    part_01_shape_I1_norm
##  Min.   :1.000e+00   Min.   :5.950e+03   Min.   : 0.06068     
##  1st Qu.:3.488e+06   1st Qu.:2.054e+09   1st Qu.: 0.08648     
##  Median :1.169e+09   Median :7.561e+10   Median : 0.19954     
##  Mean   :1.641e+15   Mean   :2.897e+16   Mean   : 0.69598     
##  3rd Qu.:5.322e+11   3rd Qu.:1.159e+13   3rd Qu.: 0.72185     
##  Max.   :2.279e+18   Max.   :3.569e+19   Max.   :69.56991     
##  NA's   :80          NA's   :80          NA's   :80           
##  part_01_shape_I2_norm part_01_shape_I3_norm part_01_shape_I4_norm
##  Min.   :  0.00095     Min.   :   0.001      Min.   :  0.00000    
##  1st Qu.:  0.00159     1st Qu.:   0.002      1st Qu.:  0.00014    
##  Median :  0.00507     Median :   0.019      Median :  0.00218    
##  Mean   :  0.16219     Mean   :   2.454      Mean   :  0.11129    
##  3rd Qu.:  0.04556     3rd Qu.:   0.299      3rd Qu.:  0.02970    
##  Max.   :111.21241     Max.   :4676.087      Max.   :196.96152    
##  NA's   :80            NA's   :80            NA's   :80           
##  part_01_shape_I5_norm part_01_shape_I6_norm part_01_shape_M000
##  Min.   :  0.00000     Min.   :  0.00470     Min.   :    32    
##  1st Qu.:  0.00001     1st Qu.:  0.00866     1st Qu.:   527    
##  Median :  0.00071     Median :  0.03434     Median :  1253    
##  Mean   :  0.09462     Mean   :  0.57360     Mean   :  3148    
##  3rd Qu.:  0.01478     3rd Qu.:  0.26323     3rd Qu.:  3123    
##  Max.   :201.22933     Max.   :242.73459     Max.   :103438    
##  NA's   :80            NA's   :80            NA's   :80        
##  part_01_shape_CI    part_01_shape_E3_E1 part_01_shape_E2_E1
##  Min.   :-57.42017   Min.   :0.00394     Min.   :0.01118    
##  1st Qu.: -0.55674   1st Qu.:0.08029     1st Qu.:0.21130    
##  Median : -0.00010   Median :0.18628     Median :0.40332    
##  Mean   :  0.04862   Mean   :0.25740     Mean   :0.43342    
##  3rd Qu.:  0.60971   3rd Qu.:0.39430     3rd Qu.:0.63745    
##  Max.   : 59.91494   Max.   :0.98119     Max.   :1.00000    
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_shape_E3_E2 part_01_shape_sqrt_E1 part_01_shape_sqrt_E2
##  Min.   :0.03144     Min.   : 0.9819       Min.   : 0.7163      
##  1st Qu.:0.37991     1st Qu.: 3.3242       1st Qu.: 2.1725      
##  Median :0.59655     Median : 5.1318       Median : 3.0768      
##  Mean   :0.56563     Mean   : 7.3343       Mean   : 3.9987      
##  3rd Qu.:0.75940     3rd Qu.: 9.1483       3rd Qu.: 4.7850      
##  Max.   :1.00000     Max.   :39.9848       Max.   :26.1732      
##  NA's   :80          NA's   :80            NA's   :80           
##  part_01_shape_sqrt_E3 part_01_density_O3 part_01_density_O4 
##  Min.   : 0.5316       Min.   :      23   Min.   :1.310e+02  
##  1st Qu.: 1.6823       1st Qu.:    6554   1st Qu.:1.018e+07  
##  Median : 2.3135       Median :   29930   Median :2.227e+08  
##  Mean   : 2.6286       Mean   :  653030   Mean   :9.268e+11  
##  3rd Qu.: 3.1531       3rd Qu.:  174538   3rd Qu.:6.453e+09  
##  Max.   :12.4320       Max.   :62031382   Max.   :8.648e+14  
##  NA's   :80            NA's   :80         NA's   :80         
##  part_01_density_O5  part_01_density_FL   part_01_density_O3_norm
##  Min.   :1.960e+02   Min.   :-2.000e+00   Min.   : 0.03937       
##  1st Qu.:4.458e+09   1st Qu.: 1.813e+07   1st Qu.: 0.34967       
##  Median :4.493e+11   Median : 2.015e+09   Median : 0.54996       
##  Mean   :6.048e+17   Mean   : 1.225e+15   Mean   : 0.74727       
##  3rd Qu.:5.719e+13   3rd Qu.: 4.828e+11   3rd Qu.: 0.94403       
##  Max.   :1.414e+21   Max.   : 3.257e+18   Max.   :12.68599       
##  NA's   :80          NA's   :80           NA's   :80             
##  part_01_density_O4_norm part_01_density_O5_norm part_01_density_FL_norm
##  Min.   :0.00051         Min.   :0.00000         Min.   :  -0.0001      
##  1st Qu.:0.03574         1st Qu.:0.00106         1st Qu.:   0.0009      
##  Median :0.07503         Median :0.00258         Median :   0.0105      
##  Mean   :0.14749         Mean   :0.00754         Mean   :   0.8599      
##  3rd Qu.:0.16694         3rd Qu.:0.00674         3rd Qu.:   0.1412      
##  Max.   :7.72338         Max.   :1.07234         Max.   :3090.7819      
##  NA's   :80              NA's   :80              NA's   :80             
##  part_01_density_I1  part_01_density_I2  part_01_density_I3 
##  Min.   :7.700e+01   Min.   :1.375e+03   Min.   :1.285e+03  
##  1st Qu.:1.934e+05   1st Qu.:5.854e+09   1st Qu.:1.299e+10  
##  Median :1.689e+06   Median :4.317e+11   Median :1.122e+12  
##  Mean   :7.299e+08   Mean   :2.173e+18   Mean   :1.349e+19  
##  3rd Qu.:3.166e+07   3rd Qu.:1.126e+14   3rd Qu.:5.628e+14  
##  Max.   :1.969e+11   Max.   :7.062e+21   Max.   :1.952e+22  
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_density_I4  part_01_density_I5  part_01_density_I6 
##  Min.   :0.000e+00   Min.   :0.000e+00   Min.   :7.010e+02  
##  1st Qu.:8.908e+06   1st Qu.:1.376e+06   1st Qu.:5.596e+08  
##  Median :1.204e+09   Median :4.396e+08   Median :2.258e+10  
##  Mean   :7.444e+14   Mean   :4.236e+14   Mean   :5.822e+15  
##  3rd Qu.:3.360e+11   3rd Qu.:1.828e+11   3rd Qu.:2.785e+12  
##  Max.   :1.662e+18   Max.   :8.271e+17   Max.   :5.729e+18  
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_density_I1_norm part_01_density_I2_norm part_01_density_I3_norm
##  Min.   :  0.0021        Min.   :   0.0000       Min.   :     0.00      
##  1st Qu.:  0.1737        1st Qu.:   0.0060       1st Qu.:     0.01      
##  Median :  0.4684        Median :   0.0315       Median :     0.09      
##  Mean   :  1.6821        Mean   :   1.4175       Mean   :    39.01      
##  3rd Qu.:  1.5080        3rd Qu.:   0.2170       3rd Qu.:     1.30      
##  Max.   :445.9705        Max.   :3021.6539       Max.   :192198.89      
##  NA's   :80              NA's   :80              NA's   :80             
##  part_01_density_I4_norm part_01_density_I5_norm part_01_density_I6_norm
##  Min.   :   0.000        Min.   :   0.000        Min.   :   0.000       
##  1st Qu.:   0.000        1st Qu.:   0.000        1st Qu.:   0.024       
##  Median :   0.006        Median :   0.003        Median :   0.118       
##  Mean   :   0.795        Mean   :   0.752        Mean   :   2.837       
##  3rd Qu.:   0.096        3rd Qu.:   0.055        3rd Qu.:   0.780       
##  Max.   :3197.241        Max.   :3268.213        Max.   :3943.530       
##  NA's   :80              NA's   :80              NA's   :80             
##  part_01_density_M000 part_01_density_CI  part_01_density_E3_E1
##  Min.   :    6.669    Min.   :-67.60415   Min.   :0.00391      
##  1st Qu.:  280.706    1st Qu.: -0.58862   1st Qu.:0.07861      
##  Median :  757.884    Median :  0.00024   Median :0.18711      
##  Mean   : 1882.535    Mean   :  0.06267   Mean   :0.26070      
##  3rd Qu.: 2073.749    3rd Qu.:  0.64039   3rd Qu.:0.40188      
##  Max.   :27156.585    Max.   : 67.64254   Max.   :0.97713      
##  NA's   :80           NA's   :80          NA's   :80           
##  part_01_density_E2_E1 part_01_density_E3_E2 part_01_density_sqrt_E1
##  Min.   :0.01123       Min.   :0.03203       Min.   : 0.972         
##  1st Qu.:0.20892       1st Qu.:0.37854       1st Qu.: 3.124         
##  Median :0.40642       Median :0.60013       Median : 4.791         
##  Mean   :0.43486       Mean   :0.56805       Mean   : 7.059         
##  3rd Qu.:0.64163       3rd Qu.:0.76378       3rd Qu.: 8.883         
##  Max.   :1.00000       Max.   :0.99961       Max.   :39.531         
##  NA's   :80            NA's   :80            NA's   :80             
##  part_01_density_sqrt_E2 part_01_density_sqrt_E3 part_01_shape_Z_7_3
##  Min.   : 0.7146         Min.   : 0.5302         Min.   :  6.191    
##  1st Qu.: 2.0865         1st Qu.: 1.6270         1st Qu.: 11.689    
##  Median : 2.8711         Median : 2.1909         Median : 20.922    
##  Mean   : 3.8152         Mean   : 2.5017         Mean   : 35.367    
##  3rd Qu.: 4.5188         3rd Qu.: 2.9608         3rd Qu.: 45.886    
##  Max.   :25.7790         Max.   :11.5369         Max.   :307.209    
##  NA's   :80              NA's   :80              NA's   :80         
##  part_01_shape_Z_0_0 part_01_shape_Z_7_0 part_01_shape_Z_7_1
##  Min.   :  2.764     Min.   :  1.557     Min.   :  4.486    
##  1st Qu.: 11.217     1st Qu.:  6.603     1st Qu.:  8.355    
##  Median : 17.295     Median :  8.408     Median : 13.579    
##  Mean   : 22.222     Mean   : 15.892     Mean   : 24.666    
##  3rd Qu.: 27.304     3rd Qu.: 19.396     3rd Qu.: 31.642    
##  Max.   :157.143     Max.   :142.901     Max.   :217.500    
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_shape_Z_3_0 part_01_shape_Z_5_2 part_01_shape_Z_6_1
##  Min.   :  1.097     Min.   :  4.559     Min.   :  1.760    
##  1st Qu.:  4.526     1st Qu.: 10.361     1st Qu.:  8.754    
##  Median :  8.616     Median : 19.426     Median : 16.267    
##  Mean   : 13.148     Mean   : 29.801     Mean   : 27.062    
##  3rd Qu.: 17.277     3rd Qu.: 39.275     3rd Qu.: 36.190    
##  Max.   :111.383     Max.   :263.074     Max.   :235.198    
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_shape_Z_3_1 part_01_shape_Z_6_0 part_01_shape_Z_2_1
##  Min.   :  3.417     Min.   :  0.07402   Min.   :  2.031    
##  1st Qu.:  8.419     1st Qu.:  4.17546   1st Qu.: 14.779    
##  Median : 14.787     Median :  7.97972   Median : 23.480    
##  Mean   : 20.936     Mean   : 13.13547   Mean   : 32.296    
##  3rd Qu.: 26.945     3rd Qu.: 17.07145   3rd Qu.: 40.484    
##  Max.   :179.513     Max.   :126.38312   Max.   :227.318    
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_shape_Z_6_3 part_01_shape_Z_2_0 part_01_shape_Z_6_2
##  Min.   :  4.054     Min.   :  0.3275    Min.   :  2.675    
##  1st Qu.: 13.457     1st Qu.: 10.6777    1st Qu.: 11.655    
##  Median : 24.622     Median : 17.6158    Median : 21.751    
##  Mean   : 39.636     Mean   : 23.6036    Mean   : 35.640    
##  3rd Qu.: 52.280     3rd Qu.: 29.9455    3rd Qu.: 47.308    
##  Max.   :318.958     Max.   :171.6858    Max.   :290.411    
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_shape_Z_5_0 part_01_shape_Z_5_1 part_01_shape_Z_4_2
##  Min.   :  1.075     Min.   :  3.289     Min.   :  2.861    
##  1st Qu.:  5.381     1st Qu.:  7.939     1st Qu.: 13.738    
##  Median :  9.242     Median : 15.671     Median : 24.910    
##  Mean   : 16.118     Mean   : 24.550     Mean   : 36.875    
##  3rd Qu.: 21.319     3rd Qu.: 32.537     3rd Qu.: 48.155    
##  Max.   :127.672     Max.   :219.824     Max.   :286.815    
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_shape_Z_1_0 part_01_shape_Z_4_1 part_01_shape_Z_7_2
##  Min.   :0.8159      Min.   :  1.586     Min.   :  5.046    
##  1st Qu.:1.3061      1st Qu.: 10.906     1st Qu.: 10.071    
##  Median :1.4996      Median : 20.997     Median : 17.944    
##  Mean   :1.5461      Mean   : 31.501     Mean   : 31.355    
##  3rd Qu.:1.7391      3rd Qu.: 41.754     3rd Qu.: 40.661    
##  Max.   :3.5838      Max.   :245.247     Max.   :276.559    
##  NA's   :80          NA's   :80          NA's   :80         
##  part_01_shape_Z_4_0 part_01_density_Z_7_3 part_01_density_Z_0_0
##  Min.   :  0.04122   Min.   :  5.272       Min.   : 1.262       
##  1st Qu.:  5.64270   1st Qu.:  9.218       1st Qu.: 8.186       
##  Median : 11.29689   Median : 15.726       Median :13.451       
##  Mean   : 17.31015   Mean   : 27.669       Mean   :17.168       
##  3rd Qu.: 23.48139   3rd Qu.: 35.426       3rd Qu.:22.250       
##  Max.   :142.13052   Max.   :185.160       Max.   :80.518       
##  NA's   :80          NA's   :80            NA's   :80           
##  part_01_density_Z_7_0 part_01_density_Z_7_1 part_01_density_Z_3_0
##  Min.   :  2.012       Min.   :  3.927       Min.   : 1.222       
##  1st Qu.:  6.247       1st Qu.:  7.423       1st Qu.: 4.024       
##  Median :  7.638       Median : 10.995       Median : 6.602       
##  Mean   : 14.212       Mean   : 20.546       Mean   :10.492       
##  3rd Qu.: 17.284       3rd Qu.: 26.101       3rd Qu.:13.428       
##  Max.   :109.065       Max.   :145.976       Max.   :72.443       
##  NA's   :80            NA's   :80            NA's   :80           
##  part_01_density_Z_5_2 part_01_density_Z_6_1 part_01_density_Z_3_1
##  Min.   :  3.752       Min.   :  0.7294      Min.   :  2.441      
##  1st Qu.:  7.982       1st Qu.:  5.7223      1st Qu.:  6.236      
##  Median : 14.645       Median : 13.3493      Median : 10.765      
##  Mean   : 23.106       Mean   : 21.9016      Mean   : 15.825      
##  3rd Qu.: 30.362       3rd Qu.: 30.6952      3rd Qu.: 20.334      
##  Max.   :157.155       Max.   :130.6053      Max.   :107.809      
##  NA's   :80            NA's   :80            NA's   :80           
##  part_01_density_Z_6_0 part_01_density_Z_2_1 part_01_density_Z_6_3
##  Min.   : 0.03706      Min.   :  1.152       Min.   :  1.906      
##  1st Qu.: 2.79102      1st Qu.: 11.429       1st Qu.:  8.924      
##  Median : 6.04225      Median : 18.678       Median : 19.066      
##  Mean   :11.41561      Mean   : 25.059       Mean   : 30.527      
##  3rd Qu.:15.67157      3rd Qu.: 31.584       3rd Qu.: 41.233      
##  Max.   :94.49897      Max.   :119.472       Max.   :178.642      
##  NA's   :80            NA's   :80            NA's   :80           
##  part_01_density_Z_2_0 part_01_density_Z_6_2 part_01_density_Z_5_0
##  Min.   : 0.2386       Min.   :  1.318       Min.   :  1.232      
##  1st Qu.: 8.5807       1st Qu.:  7.700       1st Qu.:  5.175      
##  Median :14.6134       Median : 17.214       Median :  7.815      
##  Mean   :19.2075       Mean   : 27.910       Mean   : 13.814      
##  3rd Qu.:25.5138       3rd Qu.: 38.053       3rd Qu.: 17.820      
##  Max.   :90.9074       Max.   :170.518       Max.   :100.094      
##  NA's   :80            NA's   :80            NA's   :80           
##  part_01_density_Z_5_1 part_01_density_Z_4_2 part_01_density_Z_1_0
##  Min.   :  2.319       Min.   :  1.449       Min.   :0.8034       
##  1st Qu.:  6.633       1st Qu.: 10.326       1st Qu.:1.2931       
##  Median : 12.395       Median : 19.788       Median :1.4905       
##  Mean   : 19.703       Mean   : 28.554       Mean   :1.5379       
##  3rd Qu.: 26.068       3rd Qu.: 37.602       3rd Qu.:1.7357       
##  Max.   :139.875       Max.   :155.731       Max.   :3.5821       
##  NA's   :80            NA's   :80            NA's   :80           
##  part_01_density_Z_4_1 part_01_density_Z_7_2 part_01_density_Z_4_0
##  Min.   :  0.6882      Min.   :  4.777       Min.   : 0.08173     
##  1st Qu.:  8.6645      1st Qu.:  8.359       1st Qu.: 4.46256     
##  Median : 17.4373      Median : 13.944       Median :10.26513     
##  Mean   : 25.0238      Mean   : 25.117       Mean   :14.94675     
##  3rd Qu.: 33.3538      3rd Qu.: 32.188       3rd Qu.:20.67911     
##  Max.   :140.1980      Max.   :170.417       Max.   :90.63116     
##  NA's   :80            NA's   :80            NA's   :80           
##  part_02_shape_segments_count part_02_density_segments_count
##  Min.   :    0.0              Min.   :    0.0               
##  1st Qu.:    2.0              1st Qu.:    2.0               
##  Median :    7.0              Median :    7.0               
##  Mean   :  239.1              Mean   :  239.1               
##  3rd Qu.:   59.0              3rd Qu.:   59.0               
##  Max.   :43653.0              Max.   :43653.0               
##                                                             
##  part_02_volume    part_02_electrons  part_02_mean     part_02_std     
##  Min.   :  0.000   Min.   :  0.000   Min.   :0.0000   Min.   :0.00000  
##  1st Qu.:  2.272   1st Qu.:  1.315   1st Qu.:0.4191   1st Qu.:0.03940  
##  Median :  7.128   Median :  4.679   Median :0.6000   Median :0.09486  
##  Mean   : 19.335   Mean   : 12.776   Mean   :0.6741   Mean   :0.18679  
##  3rd Qu.: 18.944   3rd Qu.: 14.057   3rd Qu.:0.8348   3rd Qu.:0.20860  
##  Max.   :689.944   Max.   :192.152   Max.   :7.9722   Max.   :6.13902  
##                                                                        
##   part_02_max      part_02_max_over_std part_02_skewness  part_02_parts   
##  Min.   : 0.0000   Min.   :  0.000      Min.   :0.00000   Min.   : 0.000  
##  1st Qu.: 0.5574   1st Qu.:  5.230      1st Qu.:0.03174   1st Qu.: 1.000  
##  Median : 0.8880   Median :  7.207      Median :0.08371   Median : 1.000  
##  Mean   : 1.3166   Mean   :  9.545      Mean   :0.18889   Mean   : 1.287  
##  3rd Qu.: 1.4932   3rd Qu.: 11.269      3rd Qu.:0.20314   3rd Qu.: 1.000  
##  Max.   :29.3147   Max.   :107.375      Max.   :6.54564   Max.   :16.000  
##                                                                           
##  part_02_shape_O3    part_02_shape_O4    part_02_shape_O5   
##  Min.   :       75   Min.   :1.830e+03   Min.   :1.341e+04  
##  1st Qu.:     7630   1st Qu.:1.389e+07   1st Qu.:7.112e+09  
##  Median :    37029   Median :3.434e+08   Median :8.611e+11  
##  Mean   :   992268   Mean   :2.776e+12   Mean   :5.935e+18  
##  3rd Qu.:   262327   3rd Qu.:1.407e+10   3rd Qu.:1.761e+14  
##  Max.   :115722779   Max.   :3.312e+15   Max.   :2.395e+22  
##  NA's   :704         NA's   :704         NA's   :704        
##  part_02_shape_FL    part_02_shape_O3_norm part_02_shape_O4_norm
##  Min.   :0.000e+00   Min.   : 0.2304       Min.   :0.0174       
##  1st Qu.:1.293e+07   1st Qu.: 0.2514       1st Qu.:0.0196       
##  Median :2.580e+09   Median : 0.3282       Median :0.0266       
##  Mean   :3.342e+15   Mean   : 0.5678       Mean   :0.0841       
##  3rd Qu.:1.055e+12   3rd Qu.: 0.7118       3rd Qu.:0.0860       
##  Max.   :6.486e+18   Max.   :19.1003       Max.   :6.1699       
##  NA's   :704         NA's   :704           NA's   :704          
##  part_02_shape_O5_norm part_02_shape_FL_norm part_02_shape_I1   
##  Min.   :0.0004        Min.   :  0.0000      Min.   :2.080e+02  
##  1st Qu.:0.0005        1st Qu.:  0.0002      1st Qu.:1.885e+05  
##  Median :0.0006        Median :  0.0024      Median :1.931e+06  
##  Mean   :0.0032        Mean   :  0.2733      Mean   :1.158e+09  
##  3rd Qu.:0.0022        3rd Qu.:  0.0596      3rd Qu.:4.892e+07  
##  Max.   :0.7800        Max.   :155.8484      Max.   :3.214e+11  
##  NA's   :704           NA's   :704           NA's   :704        
##  part_02_shape_I2    part_02_shape_I3    part_02_shape_I4   
##  Min.   :1.118e+04   Min.   :9.865e+03   Min.   :0.000e+00  
##  1st Qu.:5.304e+09   1st Qu.:1.125e+10   1st Qu.:6.047e+06  
##  Median :5.717e+11   Median :1.466e+12   Median :1.353e+09  
##  Mean   :6.918e+18   Mean   :4.453e+19   Mean   :2.008e+15  
##  3rd Qu.:2.468e+14   3rd Qu.:1.244e+15   3rd Qu.:7.123e+11  
##  Max.   :1.407e+22   Max.   :6.477e+22   Max.   :3.230e+18  
##  NA's   :704         NA's   :704         NA's   :704        
##  part_02_shape_I5    part_02_shape_I6    part_02_shape_I1_norm
##  Min.   :0.000e+00   Min.   :5.422e+03   Min.   :  0.0631     
##  1st Qu.:5.215e+05   1st Qu.:6.232e+08   1st Qu.:  0.0804     
##  Median :3.046e+08   Median :3.215e+10   Median :  0.1664     
##  Mean   :1.119e+15   Mean   :1.895e+16   Mean   :  1.0245     
##  3rd Qu.:3.388e+11   3rd Qu.:6.870e+12   3rd Qu.:  0.8499     
##  Max.   :1.477e+18   Max.   :2.578e+19   Max.   :451.2704     
##  NA's   :704         NA's   :704         NA's   :704          
##  part_02_shape_I2_norm part_02_shape_I3_norm part_02_shape_I4_norm
##  Min.   :   0.0010     Min.   :     0.00     Min.   :  0.0000     
##  1st Qu.:   0.0014     1st Qu.:     0.00     1st Qu.:  0.0001     
##  Median :   0.0036     Median :     0.01     Median :  0.0013     
##  Mean   :   0.7656     Mean   :    35.95     Mean   :  0.2311     
##  3rd Qu.:   0.0604     3rd Qu.:     0.43     3rd Qu.:  0.0397     
##  Max.   :2425.7461     Max.   :194700.71     Max.   :154.2622     
##  NA's   :704           NA's   :704           NA's   :704          
##  part_02_shape_I5_norm part_02_shape_I6_norm part_02_shape_M000
##  Min.   :  0.0000      Min.   :   0.005      Min.   :   32     
##  1st Qu.:  0.0000      1st Qu.:   0.008      1st Qu.:  392     
##  Median :  0.0004      Median :   0.026      Median : 1018     
##  Mean   :  0.2030      Mean   :   2.220      Mean   : 2601     
##  3rd Qu.:  0.0207      3rd Qu.:   0.337      3rd Qu.: 2560     
##  Max.   :153.2048      Max.   :5890.923      Max.   :86243     
##  NA's   :704           NA's   :704           NA's   :704       
##  part_02_shape_CI   part_02_shape_E3_E1 part_02_shape_E2_E1
##  Min.   :-98.0386   Min.   :0.0020      Min.   :0.0035     
##  1st Qu.: -0.4176   1st Qu.:0.0770      1st Qu.:0.2125     
##  Median :  0.0001   Median :0.2077      Median :0.4277     
##  Mean   :  0.0725   Mean   :0.2729      Mean   :0.4458     
##  3rd Qu.:  0.4551   3rd Qu.:0.4342      3rd Qu.:0.6614     
##  Max.   : 57.6653   Max.   :0.9795      Max.   :1.0000     
##  NA's   :704        NA's   :704         NA's   :704        
##  part_02_shape_E3_E2 part_02_shape_sqrt_E1 part_02_shape_sqrt_E2
##  Min.   :0.0297      Min.   : 0.9531       Min.   : 0.6291      
##  1st Qu.:0.3875      1st Qu.: 2.9275       1st Qu.: 1.9523      
##  Median :0.6177      Median : 4.6042       Median : 2.8722      
##  Mean   :0.5757      Mean   : 7.0093       Mean   : 3.7749      
##  3rd Qu.:0.7750      3rd Qu.: 8.9844       3rd Qu.: 4.5344      
##  Max.   :0.9988      Max.   :39.5459       Max.   :26.1461      
##  NA's   :704         NA's   :704           NA's   :704          
##  part_02_shape_sqrt_E3 part_02_density_O3 part_02_density_O4 
##  Min.   : 0.4467       Min.   :      20   Min.   :9.300e+01  
##  1st Qu.: 1.5164       1st Qu.:    4340   1st Qu.:4.309e+06  
##  Median : 2.1747       Median :   23618   Median :1.341e+08  
##  Mean   : 2.4711       Mean   :  576045   Mean   :7.178e+11  
##  3rd Qu.: 3.0078       3rd Qu.:  154347   3rd Qu.:4.706e+09  
##  Max.   :12.0224       Max.   :54194483   Max.   :6.641e+14  
##  NA's   :704           NA's   :704        NA's   :704        
##  part_02_density_O5  part_02_density_FL   part_02_density_O3_norm
##  Min.   :1.260e+02   Min.   :-4.000e+00   Min.   : 0.0391        
##  1st Qu.:1.192e+09   1st Qu.: 4.676e+06   1st Qu.: 0.3184        
##  Median :2.096e+11   Median : 8.608e+08   Median : 0.4928        
##  Mean   :3.992e+17   Mean   : 9.741e+14   Mean   : 0.7640        
##  3rd Qu.:3.440e+13   3rd Qu.: 3.519e+11   3rd Qu.: 0.9422        
##  Max.   :9.348e+20   Max.   : 2.554e+18   Max.   :38.7933        
##  NA's   :704         NA's   :704          NA's   :704            
##  part_02_density_O4_norm part_02_density_O5_norm part_02_density_FL_norm
##  Min.   : 0.0005         Min.   :0.0000          Min.   :  -0.0004      
##  1st Qu.: 0.0300         1st Qu.:0.0008          1st Qu.:   0.0005      
##  Median : 0.0625         Median :0.0021          Median :   0.0053      
##  Mean   : 0.1563         Mean   :0.0081          Mean   :   1.2472      
##  3rd Qu.: 0.1591         3rd Qu.:0.0060          3rd Qu.:   0.1533      
##  Max.   :25.2102         Max.   :2.0747          Max.   :1044.2645      
##  NA's   :704             NA's   :704             NA's   :704            
##  part_02_density_I1  part_02_density_I2  part_02_density_I3 
##  Min.   :6.700e+01   Min.   :7.940e+02   Min.   :1.413e+03  
##  1st Qu.:1.043e+05   1st Qu.:1.652e+09   1st Qu.:3.641e+09  
##  Median :1.182e+06   Median :2.094e+11   Median :5.152e+11  
##  Mean   :6.391e+08   Mean   :1.625e+18   Mean   :1.046e+19  
##  3rd Qu.:2.669e+07   3rd Qu.:7.275e+13   3rd Qu.:3.835e+14  
##  Max.   :1.711e+11   Max.   :4.842e+21   Max.   :1.480e+22  
##  NA's   :704         NA's   :704         NA's   :704        
##  part_02_density_I4   part_02_density_I5  part_02_density_I6 
##  Min.   :-1.000e+00   Min.   :0.000e+00   Min.   :5.760e+02  
##  1st Qu.: 2.204e+06   1st Qu.:2.715e+05   1st Qu.:2.012e+08  
##  Median : 4.879e+08   Median :1.348e+08   Median :1.272e+10  
##  Mean   : 6.008e+14   Mean   :3.519e+14   Mean   :4.557e+15  
##  3rd Qu.: 2.479e+11   3rd Qu.:1.391e+11   3rd Qu.:2.126e+12  
##  Max.   : 1.323e+18   Max.   :6.305e+17   Max.   :4.337e+18  
##  NA's   :704          NA's   :704         NA's   :704        
##  part_02_density_I1_norm part_02_density_I2_norm part_02_density_I3_norm
##  Min.   :   0.0021       Min.   :    0.00        Min.   :      0        
##  1st Qu.:   0.1394       1st Qu.:    0.00        1st Qu.:      0        
##  Median :   0.3665       Median :    0.02        Median :      0        
##  Mean   :   2.3655       Mean   :   10.42        Mean   :    559        
##  3rd Qu.:   1.5647       3rd Qu.:    0.21        3rd Qu.:      1        
##  Max.   :1857.5496       Max.   :41005.52        Max.   :3300967        
##  NA's   :704             NA's   :704             NA's   :704            
##  part_02_density_I4_norm part_02_density_I5_norm part_02_density_I6_norm
##  Min.   : -0.0001        Min.   :  0.0000        Min.   :    0.00       
##  1st Qu.:  0.0002        1st Qu.:  0.0000        1st Qu.:    0.02       
##  Median :  0.0031        Median :  0.0011        Median :    0.08       
##  Mean   :  1.1324        Mean   :  1.0558        Mean   :   13.10       
##  3rd Qu.:  0.1074        3rd Qu.:  0.0630        3rd Qu.:    0.82       
##  Max.   :961.2606        Max.   :955.4421        Max.   :49261.82       
##  NA's   :704             NA's   :704             NA's   :704            
##  part_02_density_M000 part_02_density_CI  part_02_density_E3_E1
##  Min.   :    6.516    Min.   :-107.8668   Min.   :0.0020       
##  1st Qu.:  226.643    1st Qu.:  -0.4448   1st Qu.:0.0751       
##  Median :  673.127    Median :   0.0003   Median :0.2107       
##  Mean   : 1718.678    Mean   :   0.0812   Mean   :0.2762       
##  3rd Qu.: 1948.957    3rd Qu.:   0.4828   3rd Qu.:0.4407       
##  Max.   :24018.954    Max.   :  71.7621   Max.   :0.9758       
##  NA's   :704          NA's   :704         NA's   :704          
##  part_02_density_E2_E1 part_02_density_E3_E2 part_02_density_sqrt_E1
##  Min.   :0.0035        Min.   :0.0335        Min.   : 0.9319        
##  1st Qu.:0.2104        1st Qu.:0.3884        1st Qu.: 2.7863        
##  Median :0.4300        Median :0.6212        Median : 4.3037        
##  Mean   :0.4474        Mean   :0.5783        Mean   : 6.7636        
##  3rd Qu.:0.6660        3rd Qu.:0.7792        3rd Qu.: 8.7178        
##  Max.   :1.0000        Max.   :0.9992        Max.   :39.6678        
##  NA's   :704           NA's   :704           NA's   :704            
##  part_02_density_sqrt_E2 part_02_density_sqrt_E3 part_02_shape_Z_7_3
##  Min.   : 0.6268         Min.   : 0.4459         Min.   :  7.064    
##  1st Qu.: 1.8917         1st Qu.: 1.4783         1st Qu.: 10.817    
##  Median : 2.7033         Median : 2.0729         Median : 17.902    
##  Mean   : 3.6143         Mean   : 2.3625         Mean   : 32.352    
##  3rd Qu.: 4.3042         3rd Qu.: 2.8273         3rd Qu.: 42.351    
##  Max.   :25.5768         Max.   :11.1804         Max.   :280.091    
##  NA's   :704             NA's   :704             NA's   :704        
##  part_02_shape_Z_0_0 part_02_shape_Z_7_0 part_02_shape_Z_7_1
##  Min.   :  2.764     Min.   :  1.202     Min.   :  4.885    
##  1st Qu.:  9.674     1st Qu.:  6.744     1st Qu.:  8.386    
##  Median : 15.586     Median :  8.365     Median : 11.703    
##  Mean   : 19.951     Mean   : 15.179     Mean   : 22.878    
##  3rd Qu.: 24.719     3rd Qu.: 18.123     3rd Qu.: 29.153    
##  Max.   :143.489     Max.   :145.425     Max.   :198.159    
##  NA's   :704         NA's   :704         NA's   :704        
##  part_02_shape_Z_3_0 part_02_shape_Z_5_2 part_02_shape_Z_6_1
##  Min.   : 1.033      Min.   :  5.356     Min.   :  1.298    
##  1st Qu.: 4.383      1st Qu.:  8.934     1st Qu.:  7.059    
##  Median : 7.311      Median : 16.709     Median : 13.783    
##  Mean   :12.151      Mean   : 27.024     Mean   : 24.255    
##  3rd Qu.:15.834      3rd Qu.: 35.670     3rd Qu.: 32.584    
##  Max.   :99.782      Max.   :237.935     Max.   :224.450    
##  NA's   :704         NA's   :704         NA's   :704        
##  part_02_shape_Z_3_1 part_02_shape_Z_6_0 part_02_shape_Z_2_1
##  Min.   :  3.528     Min.   :  0.0156    Min.   :  1.982    
##  1st Qu.:  7.097     1st Qu.:  3.4308    1st Qu.: 12.464    
##  Median : 13.131     Median :  6.9059    Median : 20.384    
##  Mean   : 19.039     Mean   : 11.9556    Mean   : 28.774    
##  3rd Qu.: 24.659     3rd Qu.: 15.4084    3rd Qu.: 36.469    
##  Max.   :160.388     Max.   :119.9098    Max.   :212.662    
##  NA's   :704         NA's   :704         NA's   :704        
##  part_02_shape_Z_6_3 part_02_shape_Z_2_0 part_02_shape_Z_6_2
##  Min.   :  3.614     Min.   :  0.3719    Min.   :  2.389    
##  1st Qu.: 10.890     1st Qu.:  8.7258    1st Qu.:  9.349    
##  Median : 21.180     Median : 15.2224    Median : 18.363    
##  Mean   : 35.591     Mean   : 20.8955    Mean   : 31.867    
##  3rd Qu.: 47.323     3rd Qu.: 26.7826    3rd Qu.: 42.412    
##  Max.   :306.290     Max.   :159.3042    Max.   :279.772    
##  NA's   :704         NA's   :704         NA's   :704        
##  part_02_shape_Z_5_0 part_02_shape_Z_5_1 part_02_shape_Z_4_2
##  Min.   :  1.159     Min.   :  3.423     Min.   :  2.467    
##  1st Qu.:  5.499     1st Qu.:  7.309     1st Qu.: 11.102    
##  Median :  7.662     Median : 13.353     Median : 21.094    
##  Mean   : 15.054     Mean   : 22.280     Mean   : 32.803    
##  3rd Qu.: 19.423     3rd Qu.: 29.661     3rd Qu.: 43.762    
##  Max.   :130.325     Max.   :200.009     Max.   :273.753    
##  NA's   :704         NA's   :704         NA's   :704        
##  part_02_shape_Z_1_0 part_02_shape_Z_4_1 part_02_shape_Z_7_2
##  Min.   :0.7772      Min.   :  1.246     Min.   :  5.388    
##  1st Qu.:1.3404      1st Qu.:  8.692     1st Qu.:  9.623    
##  Median :1.5952      Median : 17.541     Median : 15.237    
##  Mean   :1.6549      Mean   : 27.826     Mean   : 28.675    
##  3rd Qu.:1.8756      3rd Qu.: 37.550     3rd Qu.: 37.543    
##  Max.   :3.9866      Max.   :233.554     Max.   :253.288    
##  NA's   :704         NA's   :704         NA's   :704        
##  part_02_shape_Z_4_0 part_02_density_Z_7_3 part_02_density_Z_0_0
##  Min.   :  0.1461    Min.   :  4.990       Min.   : 1.247       
##  1st Qu.:  4.4663    1st Qu.:  9.291       1st Qu.: 7.356       
##  Median :  9.3408    Median : 13.978       Median :12.677       
##  Mean   : 15.3681    Mean   : 26.584       Mean   :16.242       
##  3rd Qu.: 20.8374    3rd Qu.: 34.057       3rd Qu.:21.570       
##  Max.   :117.6043    Max.   :174.056       Max.   :75.724       
##  NA's   :704         NA's   :704           NA's   :704          
##  part_02_density_Z_7_0 part_02_density_Z_7_1 part_02_density_Z_3_0
##  Min.   :  1.733       Min.   :  4.087       Min.   : 0.6542      
##  1st Qu.:  6.487       1st Qu.:  7.641       1st Qu.: 4.0957      
##  Median :  7.987       Median : 10.039       Median : 6.0304      
##  Mean   : 14.078       Mean   : 19.899       Mean   :10.2178      
##  3rd Qu.: 16.772       3rd Qu.: 24.893       3rd Qu.:13.0276      
##  Max.   :106.528       Max.   :139.538       Max.   :70.4909      
##  NA's   :704           NA's   :704           NA's   :704          
##  part_02_density_Z_5_2 part_02_density_Z_6_1 part_02_density_Z_3_1
##  Min.   :  4.146       Min.   :  0.5755      Min.   : 1.981       
##  1st Qu.:  7.640       1st Qu.:  4.9795      1st Qu.: 5.760       
##  Median : 13.173       Median : 10.6803      Median : 9.994       
##  Mean   : 22.062       Mean   : 20.4210      Mean   :15.210       
##  3rd Qu.: 29.151       3rd Qu.: 29.1025      3rd Qu.:19.553       
##  Max.   :146.645       Max.   :125.0562      Max.   :99.253       
##  NA's   :704           NA's   :704           NA's   :704          
##  part_02_density_Z_6_0 part_02_density_Z_2_1 part_02_density_Z_6_3
##  Min.   : 0.0852       Min.   :  1.391       Min.   :  1.524      
##  1st Qu.: 2.3849       1st Qu.: 10.097       1st Qu.:  7.679      
##  Median : 5.0445       Median : 17.232       Median : 16.300      
##  Mean   :10.7492       Mean   : 23.520       Mean   : 28.656      
##  3rd Qu.:14.7138       3rd Qu.: 30.348       3rd Qu.: 39.578      
##  Max.   :91.7772       Max.   :116.713       Max.   :174.841      
##  NA's   :704           NA's   :704           NA's   :704          
##  part_02_density_Z_2_0 part_02_density_Z_6_2 part_02_density_Z_5_0
##  Min.   : 0.1134       Min.   :  1.06        Min.   : 1.600       
##  1st Qu.: 7.3496       1st Qu.:  6.57        1st Qu.: 5.333       
##  Median :13.3436       Median : 14.56        Median : 7.283       
##  Mean   :17.9223       Mean   : 26.05        Mean   :13.460       
##  3rd Qu.:24.2675       3rd Qu.: 36.26        3rd Qu.:17.141       
##  Max.   :86.9563       Max.   :166.09        Max.   :97.914       
##  NA's   :704           NA's   :704           NA's   :704          
##  part_02_density_Z_5_1 part_02_density_Z_4_2 part_02_density_Z_1_0
##  Min.   :  3.020       Min.   :  1.389       Min.   :0.7485       
##  1st Qu.:  6.637       1st Qu.:  8.262       1st Qu.:1.3299       
##  Median : 10.850       Median : 17.621       Median :1.5857       
##  Mean   : 18.790       Mean   : 26.581       Mean   :1.6475       
##  3rd Qu.: 24.621       3rd Qu.: 36.077       3rd Qu.:1.8726       
##  Max.   :131.323       Max.   :152.782       Max.   :3.9866       
##  NA's   :704           NA's   :704           NA's   :704          
##  part_02_density_Z_4_1 part_02_density_Z_7_2 part_02_density_Z_4_0
##  Min.   :  0.8051      Min.   :  4.511       Min.   : 0.0262      
##  1st Qu.:  6.6085      1st Qu.:  8.531       1st Qu.: 3.2397      
##  Median : 15.2242      Median : 12.268       Median : 8.7131      
##  Mean   : 23.0896      Mean   : 24.082       Mean   :13.7511      
##  3rd Qu.: 31.8240      3rd Qu.: 30.577       3rd Qu.:19.1438      
##  Max.   :136.0006      Max.   :161.371       Max.   :87.2901      
##  NA's   :704           NA's   :704           NA's   :704          
##     fo_col         fc_col     weight_col       grid_space  solvent_radius
##  DELFWT:9940   PHDELWT:9940   Mode:logical   Min.   :0.2   Min.   :1.9   
##                               NA's:9940      1st Qu.:0.2   1st Qu.:1.9   
##                                              Median :0.2   Median :1.9   
##                                              Mean   :0.2   Mean   :1.9   
##                                              3rd Qu.:0.2   3rd Qu.:1.9   
##                                              Max.   :0.2   Max.   :1.9   
##                                                                          
##  solvent_opening_radius resolution_max_limit   resolution    
##  Min.   :1.4            Min.   :1            Min.   :0.6644  
##  1st Qu.:1.4            1st Qu.:1            1st Qu.:1.8000  
##  Median :1.4            Median :1            Median :2.0997  
##  Mean   :1.4            Mean   :1            Mean   :2.1387  
##  3rd Qu.:1.4            3rd Qu.:1            3rd Qu.:2.4922  
##  Max.   :1.4            Max.   :1            Max.   :4.2346  
##                                                              
##    FoFc_mean             FoFc_std       FoFc_square_std    
##  Min.   :-6.051e-08   Min.   :0.02416   Min.   :0.0005837  
##  1st Qu.:-4.894e-11   1st Qu.:0.09230   1st Qu.:0.0085198  
##  Median : 1.700e-13   Median :0.12126   Median :0.0147049  
##  Mean   :-2.340e-11   Mean   :0.12832   Mean   :0.0190105  
##  3rd Qu.: 5.276e-11   3rd Qu.:0.15250   3rd Qu.:0.0232561  
##  Max.   : 5.131e-08   Max.   :0.42034   Max.   :0.1766856  
##                                                            
##     FoFc_min          FoFc_max       part_step_FoFc_std_min
##  Min.   :-2.9385   Min.   : 0.1679   Min.   :2.8           
##  1st Qu.:-0.8604   1st Qu.: 1.1819   1st Qu.:2.8           
##  Median :-0.6582   Median : 1.9475   Median :2.8           
##  Mean   :-0.7001   Mean   : 2.6308   Mean   :2.8           
##  3rd Qu.:-0.5048   3rd Qu.: 3.0742   3rd Qu.:2.8           
##  Max.   :-0.1475   Max.   :30.4462   Max.   :2.8           
##                                                            
##  part_step_FoFc_std_max part_step_FoFc_std_step
##  Min.   :4.05           Min.   :0.5            
##  1st Qu.:4.05           1st Qu.:0.5            
##  Median :4.05           Median :0.5            
##  Mean   :4.05           Mean   :0.5            
##  3rd Qu.:4.05           3rd Qu.:0.5            
##  Max.   :4.05           Max.   :0.5            
## 
```

## Kod ograniczający liczbę klas (res_name) do 50 najpopularniejszych wartości.
## Określenie ile przykładów ma każda z klas (res_name).

```r
firstFifty <- names(sort(table(newData$res_name), decreasing = TRUE))
firstFifty[(1:50)]
```

```
##  [1] "SO4" "GOL" "EDO" "NAG" "CL"  "DMS" "ZN"  "CA"  "HEM" "MG"  "PO4"
## [12] "NAD" "ACT" "PEG" "FAD" "IOD" "CD"  "PG4" "K"   "LHG" "MLY" "MN" 
## [23] "NAP" "FMN" "AMP" "NDP" "EPE" "ADP" "FE2" "PLP" "HEC" "MAN" "FMT"
## [34] "ACY" "C8E" "PGE" "BR"  "PLC" "CU"  "FE"  "MPD" "CIT" "UMQ" "CME"
## [45] "H4B" "FEC" "FES" "GLC" "MES" "SGN"
```

```r
firstFiftyWithValues <- sort(table(newData$res_name), decreasing = TRUE)
firstFiftyWithValues[1:50]
```

```
## 
##  SO4  GOL  EDO  NAG   CL  DMS   ZN   CA  HEM   MG  PO4  NAD  ACT  PEG  FAD 
## 1007  632  516  464  387  340  323  284  260  206  175  143  125  118  115 
##  IOD   CD  PG4    K  LHG  MLY   MN  NAP  FMN  AMP  NDP  EPE  ADP  FE2  PLP 
##   96   89   78   77   75   62   59   59   56   48   46   45   44   44   41 
##  HEC  MAN  FMT  ACY  C8E  PGE   BR  PLC   CU   FE  MPD  CIT  UMQ  CME  H4B 
##   39   39   38   37   36   34   33   33   32   32   32   31   30   29   28 
##  FEC  FES  GLC  MES  SGN 
##   27   27   27   27   27
```

## Sekcję sprawdzającą korelacje między zmiennymi; sekcja ta powinna zawierać jakąś formę graficznej prezentacji korelacji.

```r
numericCols <- dplyr::select_if(newData, is.numeric)
variableCorrelation <- numericCols[,2:length(numericCols)]
corDF <- as.data.frame(as.table(cor(variableCorrelation)))
```

```
## Warning in cor(variableCorrelation): odchylenie standardowe wynosi zero
```

```r
corDF <- subset(corDF, Freq >= 0.99)
corDF <- subset(corDF, Var1 != Var2)
corDF <- corDF[!duplicated(t(apply(corDF, 1, sort))),]
summary(corDF)
```

```
##                     Var1                    Var2         Freq       
##  skeleton_cycles      : 4   skeleton_cycle_4  : 4   Min.   :0.9900  
##  part_02_electrons    : 4   local_electrons   : 4   1st Qu.:0.9934  
##  skeleton_deg_5_plus  : 3   skeleton_cycle_3  : 3   Median :0.9968  
##  part_00_shape_Z_4_2  : 3   local_max         : 3   Mean   :0.9961  
##  part_00_density_Z_7_2: 3   local_max_over_std: 3   3rd Qu.:0.9989  
##  part_01_electrons    : 3   part_00_electrons : 3   Max.   :1.0000  
##  (Other)              :62   (Other)           :62
```

```r
for(row in 1:nrow(corDF))
{
  x <- as.vector(corDF$Var1[[row]])
  y <- as.vector(corDF$Var2[[row]])
  plot(newData[,x], newData[,y], xlab=x, ylab=y)
}
```

![](RProject_files/figure-html/correlation-1.png)<!-- -->![](RProject_files/figure-html/correlation-2.png)<!-- -->![](RProject_files/figure-html/correlation-3.png)<!-- -->![](RProject_files/figure-html/correlation-4.png)<!-- -->![](RProject_files/figure-html/correlation-5.png)<!-- -->![](RProject_files/figure-html/correlation-6.png)<!-- -->![](RProject_files/figure-html/correlation-7.png)<!-- -->![](RProject_files/figure-html/correlation-8.png)<!-- -->![](RProject_files/figure-html/correlation-9.png)<!-- -->![](RProject_files/figure-html/correlation-10.png)<!-- -->![](RProject_files/figure-html/correlation-11.png)<!-- -->![](RProject_files/figure-html/correlation-12.png)<!-- -->![](RProject_files/figure-html/correlation-13.png)<!-- -->![](RProject_files/figure-html/correlation-14.png)<!-- -->![](RProject_files/figure-html/correlation-15.png)<!-- -->![](RProject_files/figure-html/correlation-16.png)<!-- -->![](RProject_files/figure-html/correlation-17.png)<!-- -->![](RProject_files/figure-html/correlation-18.png)<!-- -->![](RProject_files/figure-html/correlation-19.png)<!-- -->![](RProject_files/figure-html/correlation-20.png)<!-- -->![](RProject_files/figure-html/correlation-21.png)<!-- -->![](RProject_files/figure-html/correlation-22.png)<!-- -->![](RProject_files/figure-html/correlation-23.png)<!-- -->![](RProject_files/figure-html/correlation-24.png)<!-- -->![](RProject_files/figure-html/correlation-25.png)<!-- -->![](RProject_files/figure-html/correlation-26.png)<!-- -->![](RProject_files/figure-html/correlation-27.png)<!-- -->![](RProject_files/figure-html/correlation-28.png)<!-- -->![](RProject_files/figure-html/correlation-29.png)<!-- -->![](RProject_files/figure-html/correlation-30.png)<!-- -->![](RProject_files/figure-html/correlation-31.png)<!-- -->![](RProject_files/figure-html/correlation-32.png)<!-- -->![](RProject_files/figure-html/correlation-33.png)<!-- -->![](RProject_files/figure-html/correlation-34.png)<!-- -->![](RProject_files/figure-html/correlation-35.png)<!-- -->![](RProject_files/figure-html/correlation-36.png)<!-- -->![](RProject_files/figure-html/correlation-37.png)<!-- -->![](RProject_files/figure-html/correlation-38.png)<!-- -->![](RProject_files/figure-html/correlation-39.png)<!-- -->![](RProject_files/figure-html/correlation-40.png)<!-- -->![](RProject_files/figure-html/correlation-41.png)<!-- -->![](RProject_files/figure-html/correlation-42.png)<!-- -->![](RProject_files/figure-html/correlation-43.png)<!-- -->![](RProject_files/figure-html/correlation-44.png)<!-- -->![](RProject_files/figure-html/correlation-45.png)<!-- -->![](RProject_files/figure-html/correlation-46.png)<!-- -->![](RProject_files/figure-html/correlation-47.png)<!-- -->![](RProject_files/figure-html/correlation-48.png)<!-- -->![](RProject_files/figure-html/correlation-49.png)<!-- -->![](RProject_files/figure-html/correlation-50.png)<!-- -->![](RProject_files/figure-html/correlation-51.png)<!-- -->![](RProject_files/figure-html/correlation-52.png)<!-- -->![](RProject_files/figure-html/correlation-53.png)<!-- -->![](RProject_files/figure-html/correlation-54.png)<!-- -->![](RProject_files/figure-html/correlation-55.png)<!-- -->![](RProject_files/figure-html/correlation-56.png)<!-- -->![](RProject_files/figure-html/correlation-57.png)<!-- -->![](RProject_files/figure-html/correlation-58.png)<!-- -->![](RProject_files/figure-html/correlation-59.png)<!-- -->![](RProject_files/figure-html/correlation-60.png)<!-- -->![](RProject_files/figure-html/correlation-61.png)<!-- -->![](RProject_files/figure-html/correlation-62.png)<!-- -->![](RProject_files/figure-html/correlation-63.png)<!-- -->![](RProject_files/figure-html/correlation-64.png)<!-- -->![](RProject_files/figure-html/correlation-65.png)<!-- -->![](RProject_files/figure-html/correlation-66.png)<!-- -->![](RProject_files/figure-html/correlation-67.png)<!-- -->![](RProject_files/figure-html/correlation-68.png)<!-- -->![](RProject_files/figure-html/correlation-69.png)<!-- -->![](RProject_files/figure-html/correlation-70.png)<!-- -->![](RProject_files/figure-html/correlation-71.png)<!-- -->![](RProject_files/figure-html/correlation-72.png)<!-- -->![](RProject_files/figure-html/correlation-73.png)<!-- -->![](RProject_files/figure-html/correlation-74.png)<!-- -->![](RProject_files/figure-html/correlation-75.png)<!-- -->![](RProject_files/figure-html/correlation-76.png)<!-- -->![](RProject_files/figure-html/correlation-77.png)<!-- -->![](RProject_files/figure-html/correlation-78.png)<!-- -->![](RProject_files/figure-html/correlation-79.png)<!-- -->![](RProject_files/figure-html/correlation-80.png)<!-- -->![](RProject_files/figure-html/correlation-81.png)<!-- -->![](RProject_files/figure-html/correlation-82.png)<!-- -->

## Wykresy rozkładów liczby atomów (local_res_atom_non_h_count) i elektronów (local_res_atom_non_h_electron_sum).

```r
qplot(res_id, local_res_atom_non_h_count, data = newData)
```

![](RProject_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
qplot(res_id, local_res_atom_non_h_electron_sum, data = newData)
```

![](RProject_files/figure-html/unnamed-chunk-5-2.png)<!-- -->

## Tabelę pokazującą 10 klas z największą niezgodnością liczby atomów (local_res_atom_non_h_count vs dict_atom_non_h_count) i tabelę pokazującą 10 klas z największą niezgodnością liczby elektronów (local_res_atom_non_h_electron_sum vs dict_atom_non_h_electron_sum;)

```r
atomsDiffs <- select(newData, res_id, res_name, local_res_atom_non_h_count , dict_atom_non_h_count)
atomsDiffs <- mutate(atomsDiffs,diff=local_res_atom_non_h_count - dict_atom_non_h_count)
atomsDiffs <- atomsDiffs[order(atomsDiffs$diff, decreasing=FALSE), ]
 
prettyTable <- function(table_df, round_columns=numeric(), round_digits=2) {
    DT::datatable(table_df, style="bootstrap", filter = "top", rownames = FALSE, extensions = "Buttons", options = list(dom = 'Bfrtip', buttons = c('copy', 'csv', 'excel', 'pdf', 'print'))) %>%
    formatRound(round_columns, round_digits)
}
 
prettyTable(head(atomsDiffs, 10))
```

<!--html_preserve--><div id="htmlwidget-4e418d0e777bcf5e8b2b" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-4e418d0e777bcf5e8b2b">{"x":{"style":"bootstrap","filter":"top","filterHTML":"<tr>\n  <td data-type=\"integer\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"602\" data-max=\"1161\"><\/div>\n      <span style=\"float: left;\"><\/span>\n      <span style=\"float: right;\"><\/span>\n    <\/div>\n  <\/td>\n  <td data-type=\"factor\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"width: 100%; display: none;\">\n      <select multiple=\"multiple\" style=\"width: 100%;\" data-options=\"[&quot;15P&quot;,&quot;DSL&quot;,&quot;LHG&quot;]\"><\/select>\n    <\/div>\n  <\/td>\n  <td data-type=\"integer\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"8\" data-max=\"48\"><\/div>\n      <span style=\"float: left;\"><\/span>\n      <span style=\"float: right;\"><\/span>\n    <\/div>\n  <\/td>\n  <td data-type=\"number\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"49\" data-max=\"104\"><\/div>\n      <span style=\"float: left;\"><\/span>\n      <span style=\"float: right;\"><\/span>\n    <\/div>\n  <\/td>\n  <td data-type=\"number\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"-56\" data-max=\"-38\"><\/div>\n      <span style=\"float: left;\"><\/span>\n      <span style=\"float: right;\"><\/span>\n    <\/div>\n  <\/td>\n<\/tr>","extensions":["Buttons"],"data":[[1001,1001,1001,1001,602,1161,1160,1160,1159,1160],["15P","15P","15P","15P","DSL","LHG","LHG","LHG","LHG","LHG"],[48,48,48,48,14,8,8,8,9,11],[104,104,104,104,55,49,49,49,49,49],[-56,-56,-56,-56,-41,-41,-41,-41,-40,-38]],"container":"<table class=\"table table-striped table-hover\">\n  <thead>\n    <tr>\n      <th>res_id<\/th>\n      <th>res_name<\/th>\n      <th>local_res_atom_non_h_count<\/th>\n      <th>dict_atom_non_h_count<\/th>\n      <th>diff<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"dom":"Bfrtip","buttons":["copy","csv","excel","pdf","print"],"columnDefs":[{"className":"dt-right","targets":[0,2,3,4]}],"order":[],"autoWidth":false,"orderClasses":false,"orderCellsTop":true,"rowCallback":"function(row, data) {\n}"}},"evals":["options.rowCallback"],"jsHooks":[]}</script><!--/html_preserve-->

```r
electronDiffs <- select(newData, res_id, res_name, local_res_atom_non_h_electron_sum, dict_atom_non_h_electron_sum)
electronDiffs <- mutate(electronDiffs,diff=local_res_atom_non_h_electron_sum - dict_atom_non_h_electron_sum)
electronDiffs <- electronDiffs[order(electronDiffs$diff, decreasing=FALSE), ]
 
prettyTable(head(electronDiffs, 10))
```

<!--html_preserve--><div id="htmlwidget-9fe14766c3ab345b5cb0" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-9fe14766c3ab345b5cb0">{"x":{"style":"bootstrap","filter":"top","filterHTML":"<tr>\n  <td data-type=\"integer\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"602\" data-max=\"1161\"><\/div>\n      <span style=\"float: left;\"><\/span>\n      <span style=\"float: right;\"><\/span>\n    <\/div>\n  <\/td>\n  <td data-type=\"factor\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"width: 100%; display: none;\">\n      <select multiple=\"multiple\" style=\"width: 100%;\" data-options=\"[&quot;15P&quot;,&quot;DSL&quot;,&quot;LHG&quot;]\"><\/select>\n    <\/div>\n  <\/td>\n  <td data-type=\"integer\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"48\" data-max=\"320\"><\/div>\n      <span style=\"float: left;\"><\/span>\n      <span style=\"float: right;\"><\/span>\n    <\/div>\n  <\/td>\n  <td data-type=\"number\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"323\" data-max=\"694\"><\/div>\n      <span style=\"float: left;\"><\/span>\n      <span style=\"float: right;\"><\/span>\n    <\/div>\n  <\/td>\n  <td data-type=\"number\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\"><\/span>\n    <\/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"-374\" data-max=\"-257\"><\/div>\n      <span style=\"float: left;\"><\/span>\n      <span style=\"float: right;\"><\/span>\n    <\/div>\n  <\/td>\n<\/tr>","extensions":["Buttons"],"data":[[1001,1001,1001,1001,1161,1160,1160,1159,602,1160],["15P","15P","15P","15P","LHG","LHG","LHG","LHG","DSL","LHG"],[320,320,320,320,48,48,48,54,84,66],[694,694,694,694,323,323,323,323,347,323],[-374,-374,-374,-374,-275,-275,-275,-269,-263,-257]],"container":"<table class=\"table table-striped table-hover\">\n  <thead>\n    <tr>\n      <th>res_id<\/th>\n      <th>res_name<\/th>\n      <th>local_res_atom_non_h_electron_sum<\/th>\n      <th>dict_atom_non_h_electron_sum<\/th>\n      <th>diff<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"dom":"Bfrtip","buttons":["copy","csv","excel","pdf","print"],"columnDefs":[{"className":"dt-right","targets":[0,2,3,4]}],"order":[],"autoWidth":false,"orderClasses":false,"orderCellsTop":true,"rowCallback":"function(row, data) {\n}"}},"evals":["options.rowCallback"],"jsHooks":[]}</script><!--/html_preserve-->

## Sekcja pokazujÄ…ca rozkĹ‚ad wartoĹ›ci wszystkich kolumn zaczynajÄ…cych siÄ™ od part_01 z zaznaczeniem (graficznym i liczbowym) Ĺ›redniej wartoĹ›ci.

```r
partsVar <-  dplyr::select(newData, res_id, starts_with("part_01"))
selectInput("column", label = "Kolumna:",
            choices = colnames(partsVar)[2:length(colnames(partsVar))], selected = "part_01_shape_sqrt_E1")
```

<!--html_preserve--><div class="form-group shiny-input-container">
<label class="control-label" for="column">Kolumna:</label>
<div>
<select id="column"><option value="part_01_shape_segments_count">part_01_shape_segments_count</option>
<option value="part_01_density_segments_count">part_01_density_segments_count</option>
<option value="part_01_volume">part_01_volume</option>
<option value="part_01_electrons">part_01_electrons</option>
<option value="part_01_mean">part_01_mean</option>
<option value="part_01_std">part_01_std</option>
<option value="part_01_max">part_01_max</option>
<option value="part_01_max_over_std">part_01_max_over_std</option>
<option value="part_01_skewness">part_01_skewness</option>
<option value="part_01_parts">part_01_parts</option>
<option value="part_01_shape_O3">part_01_shape_O3</option>
<option value="part_01_shape_O4">part_01_shape_O4</option>
<option value="part_01_shape_O5">part_01_shape_O5</option>
<option value="part_01_shape_FL">part_01_shape_FL</option>
<option value="part_01_shape_O3_norm">part_01_shape_O3_norm</option>
<option value="part_01_shape_O4_norm">part_01_shape_O4_norm</option>
<option value="part_01_shape_O5_norm">part_01_shape_O5_norm</option>
<option value="part_01_shape_FL_norm">part_01_shape_FL_norm</option>
<option value="part_01_shape_I1">part_01_shape_I1</option>
<option value="part_01_shape_I2">part_01_shape_I2</option>
<option value="part_01_shape_I3">part_01_shape_I3</option>
<option value="part_01_shape_I4">part_01_shape_I4</option>
<option value="part_01_shape_I5">part_01_shape_I5</option>
<option value="part_01_shape_I6">part_01_shape_I6</option>
<option value="part_01_shape_I1_norm">part_01_shape_I1_norm</option>
<option value="part_01_shape_I2_norm">part_01_shape_I2_norm</option>
<option value="part_01_shape_I3_norm">part_01_shape_I3_norm</option>
<option value="part_01_shape_I4_norm">part_01_shape_I4_norm</option>
<option value="part_01_shape_I5_norm">part_01_shape_I5_norm</option>
<option value="part_01_shape_I6_norm">part_01_shape_I6_norm</option>
<option value="part_01_shape_M000">part_01_shape_M000</option>
<option value="part_01_shape_CI">part_01_shape_CI</option>
<option value="part_01_shape_E3_E1">part_01_shape_E3_E1</option>
<option value="part_01_shape_E2_E1">part_01_shape_E2_E1</option>
<option value="part_01_shape_E3_E2">part_01_shape_E3_E2</option>
<option value="part_01_shape_sqrt_E1" selected>part_01_shape_sqrt_E1</option>
<option value="part_01_shape_sqrt_E2">part_01_shape_sqrt_E2</option>
<option value="part_01_shape_sqrt_E3">part_01_shape_sqrt_E3</option>
<option value="part_01_density_O3">part_01_density_O3</option>
<option value="part_01_density_O4">part_01_density_O4</option>
<option value="part_01_density_O5">part_01_density_O5</option>
<option value="part_01_density_FL">part_01_density_FL</option>
<option value="part_01_density_O3_norm">part_01_density_O3_norm</option>
<option value="part_01_density_O4_norm">part_01_density_O4_norm</option>
<option value="part_01_density_O5_norm">part_01_density_O5_norm</option>
<option value="part_01_density_FL_norm">part_01_density_FL_norm</option>
<option value="part_01_density_I1">part_01_density_I1</option>
<option value="part_01_density_I2">part_01_density_I2</option>
<option value="part_01_density_I3">part_01_density_I3</option>
<option value="part_01_density_I4">part_01_density_I4</option>
<option value="part_01_density_I5">part_01_density_I5</option>
<option value="part_01_density_I6">part_01_density_I6</option>
<option value="part_01_density_I1_norm">part_01_density_I1_norm</option>
<option value="part_01_density_I2_norm">part_01_density_I2_norm</option>
<option value="part_01_density_I3_norm">part_01_density_I3_norm</option>
<option value="part_01_density_I4_norm">part_01_density_I4_norm</option>
<option value="part_01_density_I5_norm">part_01_density_I5_norm</option>
<option value="part_01_density_I6_norm">part_01_density_I6_norm</option>
<option value="part_01_density_M000">part_01_density_M000</option>
<option value="part_01_density_CI">part_01_density_CI</option>
<option value="part_01_density_E3_E1">part_01_density_E3_E1</option>
<option value="part_01_density_E2_E1">part_01_density_E2_E1</option>
<option value="part_01_density_E3_E2">part_01_density_E3_E2</option>
<option value="part_01_density_sqrt_E1">part_01_density_sqrt_E1</option>
<option value="part_01_density_sqrt_E2">part_01_density_sqrt_E2</option>
<option value="part_01_density_sqrt_E3">part_01_density_sqrt_E3</option>
<option value="part_01_shape_Z_7_3">part_01_shape_Z_7_3</option>
<option value="part_01_shape_Z_0_0">part_01_shape_Z_0_0</option>
<option value="part_01_shape_Z_7_0">part_01_shape_Z_7_0</option>
<option value="part_01_shape_Z_7_1">part_01_shape_Z_7_1</option>
<option value="part_01_shape_Z_3_0">part_01_shape_Z_3_0</option>
<option value="part_01_shape_Z_5_2">part_01_shape_Z_5_2</option>
<option value="part_01_shape_Z_6_1">part_01_shape_Z_6_1</option>
<option value="part_01_shape_Z_3_1">part_01_shape_Z_3_1</option>
<option value="part_01_shape_Z_6_0">part_01_shape_Z_6_0</option>
<option value="part_01_shape_Z_2_1">part_01_shape_Z_2_1</option>
<option value="part_01_shape_Z_6_3">part_01_shape_Z_6_3</option>
<option value="part_01_shape_Z_2_0">part_01_shape_Z_2_0</option>
<option value="part_01_shape_Z_6_2">part_01_shape_Z_6_2</option>
<option value="part_01_shape_Z_5_0">part_01_shape_Z_5_0</option>
<option value="part_01_shape_Z_5_1">part_01_shape_Z_5_1</option>
<option value="part_01_shape_Z_4_2">part_01_shape_Z_4_2</option>
<option value="part_01_shape_Z_1_0">part_01_shape_Z_1_0</option>
<option value="part_01_shape_Z_4_1">part_01_shape_Z_4_1</option>
<option value="part_01_shape_Z_7_2">part_01_shape_Z_7_2</option>
<option value="part_01_shape_Z_4_0">part_01_shape_Z_4_0</option>
<option value="part_01_density_Z_7_3">part_01_density_Z_7_3</option>
<option value="part_01_density_Z_0_0">part_01_density_Z_0_0</option>
<option value="part_01_density_Z_7_0">part_01_density_Z_7_0</option>
<option value="part_01_density_Z_7_1">part_01_density_Z_7_1</option>
<option value="part_01_density_Z_3_0">part_01_density_Z_3_0</option>
<option value="part_01_density_Z_5_2">part_01_density_Z_5_2</option>
<option value="part_01_density_Z_6_1">part_01_density_Z_6_1</option>
<option value="part_01_density_Z_3_1">part_01_density_Z_3_1</option>
<option value="part_01_density_Z_6_0">part_01_density_Z_6_0</option>
<option value="part_01_density_Z_2_1">part_01_density_Z_2_1</option>
<option value="part_01_density_Z_6_3">part_01_density_Z_6_3</option>
<option value="part_01_density_Z_2_0">part_01_density_Z_2_0</option>
<option value="part_01_density_Z_6_2">part_01_density_Z_6_2</option>
<option value="part_01_density_Z_5_0">part_01_density_Z_5_0</option>
<option value="part_01_density_Z_5_1">part_01_density_Z_5_1</option>
<option value="part_01_density_Z_4_2">part_01_density_Z_4_2</option>
<option value="part_01_density_Z_1_0">part_01_density_Z_1_0</option>
<option value="part_01_density_Z_4_1">part_01_density_Z_4_1</option>
<option value="part_01_density_Z_7_2">part_01_density_Z_7_2</option>
<option value="part_01_density_Z_4_0">part_01_density_Z_4_0</option></select>
<script type="application/json" data-for="column" data-nonempty="">{}</script>
</div>
</div><!--/html_preserve-->

```r
renderPlot({
  qplot(partsVar[,1], partsVar[,input$column], data = partsVar, ylab=  input$column, xlab =colnames(partsVar)[1] ) + stat_summary(fun.y=mean, geom="point", group="res_id", color="red", fill="red")
})
```

<!--html_preserve--><div id="out5b6f6bd537fea2b2" class="shiny-plot-output" style="width: 100% ; height: 400px"></div><!--/html_preserve-->

```r
renderPlot({
  helperMean <- mean(partsVar[,input$column], na.rm=T)
  ggplot(partsVar, aes(partsVar[,input$column], xlab=input$column, alpha=0.5)) + geom_density() +   geom_vline(aes(xintercept=helperMean), color="red", linetype="dashed", size=1, show_guide = TRUE) +  geom_text(aes(x=mean(partsVar[,input$column], na.rm=T), label=round(helperMean,2), y=0), colour="blue", angle=0, text=element_text(size=12))
})
```

<!--html_preserve--><div id="out96bd0eb0dfe81f28" class="shiny-plot-output" style="width: 100% ; height: 400px"></div><!--/html_preserve-->


## Sekcję sprawdzającą czy na podstawie wartości innych kolumn można przewidzieć liczbę elektronów i atomów oraz z jaką dokładnością można dokonać takiej predykcji; trafność regresji powinna zostać oszacowana na podstawie miar R^2 i RMSE;

```r
numericCols <- numericCols %>% select(-one_of(c(
"weight_col",
"title",
"pbd_code",
"res_name",
"res_id",
"chain_id",
"local_BAa",
"local_NPa",
"local_Ra",
"local_RGa",
"local_SRGa",
"local_CCSa",
"local_CCPa",
"local_ZOa",
"local_ZDa",
"local_ZD_minus_a",
"local_ZD_plus_a",
"local_res_atom_count",
"local_res_atom_non_h_count",
"local_res_atom_non_h_occupancy_sum", 
#"local_res_atom_non_h_electron_sum", 
"local_res_atom_non_h_electron_occupancy_sum", "local_res_atom_C_count", 
"local_res_atom_N_count", 
"local_res_atom_O_count", 
"local_res_atom_S_count",
"dict_atom_non_h_count",
"dict_atom_non_h_electron_sum",
"dict_atom_C_count",
"dict_atom_N_count", 
"dict_atom_O_count", 
"dict_atom_S_count",
"fo_col",
"fc_col",
"weight_col",
"grid_space",
"solvent_radius",
"solvent_opening_radius",
"part_step_FoFc_std_min",
"part_step_FoFc_std_max",
"part_step_FoFc_std_step"
)))

numericCols <- as.data.frame(numericCols)
class(numericCols)
```

```
## [1] "data.frame"
```

```r
set.seed(100)
inTrain <- createDataPartition(y = numericCols$local_electrons, 
                               p = 0.8, list = FALSE)
training <- numericCols[inTrain,]
testing <- numericCols[-inTrain,]
colNo = which( colnames(training)=="local_res_atom_non_h_electron_sum" )
preProcValues <- preProcess(training, method = c("knnImpute","center","scale"))
set.seed(100)
train_processed <- predict(preProcValues, training)
set.seed(100)
my_lm = train(train_processed[,-colNo], train_processed[,colNo],
               method = "lm",
               preProc = c("center", "scale")
              )
my_lm
```

```
## Linear Regression 
## 
## 7952 samples
##  380 predictor
## 
## Pre-processing: centered (380), scaled (380) 
## Resampling: Bootstrapped (25 reps) 
## Summary of sample sizes: 7952, 7952, 7952, 7952, 7952, 7952, ... 
## Resampling results:
## 
##   RMSE      Rsquared    MAE      
##   3.596238  0.07312249  0.6112807
## 
## Tuning parameter 'intercept' was held constant at a value of TRUE
```

```r
p <- predict(my_lm, testing)
summary(p)
```

```
##       Min.    1st Qu.     Median       Mean    3rd Qu.       Max. 
## -7.805e+19  2.939e+12  9.864e+13  2.338e+20  2.607e+16  2.160e+23 
##       NA's 
##        125
```

```r
Rsquared <- summary(my_lm)$r.squared
print(Rsquared)
```

```
## [1] 0.5469711
```

```r
#p
```



```r
memory.limit()
```

```
## [1] 16178
```

```r
memory.limit(size = 56000)
```

```
## [1] 56000
```


```r
#vars <- c("res_name","blob_volume_coverage")
#catcorrm <- function(vars, data) sapply(vars, function(y) sapply(vars, function(x) assocstats(table(dat[,x], #dat[,y]))$cramer))
#dat <- newData
#catcorrm(vars,dat)
```

## Sekcję próbującą stworzyć klasyfikator przewidujący wartość atrybutu res_name (w tej sekcji należy wykorzystać wiedzę z pozostałych punktów oraz wykonać dodatkowe czynności, które mogą poprawić trafność klasyfikacji); trafność klasyfikacji powinna zostać oszacowana na danych inne niż uczące za pomocą mechanizmu (stratyfikowanej!) oceny krzyżowej lub (stratyfikowanego!) zbioru testowego.

```r
gc()
```

```
##            used  (Mb) gc trigger  (Mb) max used  (Mb)
## Ncells  2288646 122.3    4069657 217.4  4069657 217.4
## Vcells 31460768 240.1   79264985 604.8 79264894 604.8
```

```r
completeFun <- function(data, desiredCols) {
  completeVec <- complete.cases(data[, desiredCols])
  return(data[completeVec, ])
}
interestedColumns <- c("res_volume_coverage", "blob_volume_coverage", "part_01_max", "local_max", "local_electrons", "part_00_skewness", "part_00_std", "part_00_electrons", "part_00_density_sqrt_E2", "part_00_density_sqrt_E3", "part_00_shape_Z_7_0", "part_00_shape_Z_7_1", "part_00_shape_Z_3_0", "part_00_shape_Z_5_2", "part_00_shape_Z_6_1", "part_00_shape_Z_3_1", "part_00_shape_Z_6_3", "part_00_shape_Z_6_2", "part_00_shape_Z_4_2", "part_00_density_Z_7_3", "part_00_density_Z_7_0",  "part_00_density_Z_2_1", "part_00_density_Z_6_3", "part_00_density_Z_4_2", "part_01_max", "part_01_skewness", "part_01_shape_O4")
nrow(newData)
```

```
## [1] 9940
```

```r
newData <- subset(newData, (res_name %in% firstFifty[(1:50)]))
nrow(newData)
```

```
## [1] 6652
```

```r
newData <- completeFun(newData, interestedColumns)
nrow(newData)
```

```
## [1] 6606
```

```r
newData <- newData[!is.na(newData$res_name),]
newData <- as.data.frame(newData)
newData <- droplevels(newData)
set.seed(100)
inTrain <- createDataPartition(y = newData$res_name,
                               p = 0.8, list = FALSE)
 training <- newData[inTrain,]
 testing <- newData[-inTrain,]
control <- trainControl(method="cv", number=10)
metric <- "Accuracy"
set.seed(100)
preProcValues <- preProcess(training, method = c("center","scale"))
train_processed <- predict(preProcValues, training)
fit.lda <- train(res_name ~  res_volume_coverage + blob_volume_coverage + part_01_max + local_max + local_electrons + part_00_skewness + part_00_std + part_00_electrons + part_00_density_sqrt_E2 + part_00_density_sqrt_E3 + part_00_shape_Z_7_0 + part_00_shape_Z_7_1 + part_00_shape_Z_3_0 + part_00_shape_Z_5_2 + part_00_shape_Z_6_1 + part_00_shape_Z_3_1 + part_00_shape_Z_6_3 + part_00_shape_Z_6_2 + part_00_shape_Z_4_2 + part_00_density_Z_7_3 + part_00_density_Z_7_0 +  part_00_density_Z_2_1 + part_00_density_Z_6_3 + part_00_density_Z_4_2, data=train_processed, method="lda", metric=metric, trControl=control,na.action = na.pass)
# CART
set.seed(100)
fit.cart <- train(res_name ~ res_volume_coverage + blob_volume_coverage + part_01_max + local_max + local_electrons + part_00_skewness + part_00_std + part_00_electrons + part_00_density_sqrt_E2 + part_00_density_sqrt_E3 + part_00_shape_Z_7_0 + part_00_shape_Z_7_1 + part_00_shape_Z_3_0 + part_00_shape_Z_5_2 + part_00_shape_Z_6_1 + part_00_shape_Z_3_1 + part_00_shape_Z_6_3 + part_00_shape_Z_6_2 + part_00_shape_Z_4_2 + part_00_density_Z_7_3 + part_00_density_Z_7_0 +  part_00_density_Z_2_1 + part_00_density_Z_6_3 + part_00_density_Z_4_2 + part_01_max + part_01_skewness + part_01_shape_O4, data=train_processed, method="rpart", metric=metric, trControl=control,na.action = na.pass)
# kNN
#set.seed(100)
#fit.knn <- train(res_name~res_volume_coverage + blob_volume_coverage + part_01_max + local_max + local_electrons + part_00_skewness + part_00_std + part_00_electrons + part_00_density_sqrt_E2 + part_00_density_sqrt_E3 + part_00_shape_Z_7_0 + part_00_shape_Z_7_1 + part_00_shape_Z_3_0 + part_00_shape_Z_5_2 + part_00_shape_Z_6_1 + part_00_shape_Z_3_1 + part_00_shape_Z_6_3 + part_00_shape_Z_6_2 + part_00_shape_Z_4_2 + part_00_density_Z_7_3 + part_00_density_Z_7_0 +  part_00_density_Z_2_1 + part_00_density_Z_6_3 + part_00_density_Z_4_2 + part_01_max + part_01_skewness + part_01_shape_O4, data=training, method="knn", metric=metric, trControl=control,na.action = na.pass)
#set.seed(100)
#fit.svm <- train(res_name~res_volume_coverage + blob_volume_coverage + part_01_max + local_max + local_electrons + part_00_skewness + part_00_std + part_00_electrons + part_00_density_sqrt_E2 + part_00_density_sqrt_E3 + part_00_shape_Z_7_0 + part_00_shape_Z_7_1 + part_00_shape_Z_3_0 + part_00_shape_Z_5_2 + part_00_shape_Z_6_1 + part_00_shape_Z_3_1 + part_00_shape_Z_6_3 + part_00_shape_Z_6_2 + part_00_shape_Z_4_2 + part_00_density_Z_7_3 + part_00_density_Z_7_0 +  part_00_density_Z_2_1 + part_00_density_Z_6_3 + part_00_density_Z_4_2 + part_01_max + part_01_skewness + part_01_shape_O4, data=train_processed, method="svmRadial", metric=metric, trControl=control,na.action = na.pass)
set.seed(100)
fit.rf <- train(res_name~res_volume_coverage + blob_volume_coverage + part_01_max + local_max + local_electrons + part_00_skewness + part_00_std + part_00_electrons + part_00_density_sqrt_E2 + part_00_density_sqrt_E3 + part_00_shape_Z_7_0 + part_00_shape_Z_7_1 + part_00_shape_Z_3_0 + part_00_shape_Z_5_2 + part_00_shape_Z_6_1 + part_00_shape_Z_3_1 + part_00_shape_Z_6_3 + part_00_shape_Z_6_2 + part_00_shape_Z_4_2 + part_00_density_Z_7_3 + part_00_density_Z_7_0 +  part_00_density_Z_2_1 + part_00_density_Z_6_3 + part_00_density_Z_4_2, data=train_processed, method="rf", metric=metric, trControl=control,na.action = na.pass)
 
results <- resamples(list( rf=fit.rf, lda= fit.lda, cart=fit.cart))
summary(results)
```

```
## 
## Call:
## summary.resamples(object = results)
## 
## Models: rf, lda, cart 
## Number of resamples: 10 
## 
## Accuracy 
##           Min.   1st Qu.    Median      Mean   3rd Qu.      Max. NA's
## rf   0.5158879 0.5194129 0.5292453 0.5320415 0.5425732 0.5557656    0
## lda  0.4011407 0.4107211 0.4245283 0.4234422 0.4325491 0.4559099    0
## cart 0.2215909 0.2384345 0.2483521 0.2453659 0.2538618 0.2608696    0
## 
## Kappa 
##           Min.   1st Qu.    Median      Mean   3rd Qu.      Max. NA's
## rf   0.4760416 0.4799782 0.4913335 0.4942987 0.5066707 0.5203767    0
## lda  0.3506656 0.3602936 0.3766127 0.3749229 0.3843909 0.4103555    0
## cart 0.1193072 0.1366570 0.1498060 0.1459825 0.1555824 0.1638511    0
```

```r
predictions <- predict(fit.lda, testing)
summary(predictions)
```

```
## ACT ACY ADP AMP  BR C8E  CA  CD CIT  CL CME  CU DMS EDO EPE FAD  FE FE2 
##   0   0 172   0   0   0   0   0   0   0   0   0   0   0   1 579   0   0 
## FEC FES FMN FMT GLC GOL H4B HEC HEM IOD   K LHG MAN MES  MG MLY  MN MPD 
##   1  35   3   0   0   0 470   0  22   0   0   0   0  10   0   0   0   0 
## NAD NAG NAP NDP PEG PG4 PGE PLC PLP PO4 SGN SO4 UMQ  ZN 
##   0   0   0   0   0   0   0   0   7   0   0   0   0   0
```





