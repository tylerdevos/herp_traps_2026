## Camera arrays
### Part 1: Camera array capture counts by species and camera position type for individual arrays
Load data (see example data: [Three_Lakes_camera_data_unfiltered](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/Three_Lakes_camera_data_unfiltered.csv), [Three_Lakes_camera_info](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/Three_Lakes_camera_info.csv))
```
camera_data_unfiltered <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2024_season/data/Three_Lakes_camera_data_unfiltered.csv", fileEncoding="UTF-8-BOM")
camera_info <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2024_season/data/Three_Lakes_camera_info.csv", fileEncoding="UTF-8-BOM")
```
Expand condensed species data using the count column
```
camera_data <- camera_data_unfiltered[rep(row.names(camera_data_unfiltered), camera_data_unfiltered$count), 1:7]
```
Join array ID and camera position data to camera data
```
camera_array_data <- merge(camera_data, camera_info, by='camera_ID')
```
Extract data from arrays with multiple cameras only (i.e., camera arrays, not hybrid arrays)
```
camera_array_data_subset <- subset(camera_array_data, multiple_cameras=="yes")
```
Print a list of camera array IDs
```
sort(unique(camera_array_data_subset$array_ID))
```
Subset data by array ID
```
array_12 <- subset(camera_array_data_subset, array_ID=='Array 12')
array_13 <- subset(camera_array_data_subset, array_ID=='Array 13')
array_14 <- subset(camera_array_data_subset, array_ID=='Array 14')
array_15 <- subset(camera_array_data_subset, array_ID=='Array 15')
```
Create and export tables of species counts by camera location for each array
```
array_12_table <- table(array_12$species, array_12$camera_position)
array_13_table <- table(array_13$species, array_13$camera_position)
array_14_table <- table(array_14$species, array_14$camera_position)
array_15_table <- table(array_15$species, array_15$camera_position)
setwd("C:/Users/tbd/Desktop/")
write.csv(array_12_table, "Three_Lakes_array_12_results.csv", row.names=TRUE)
write.csv(array_13_table, "Three_Lakes_array_13_results.csv", row.names=TRUE)
write.csv(array_14_table, "Three_Lakes_array_14_results.csv", row.names=TRUE)
write.csv(array_15_table, "Three_Lakes_array_15_results.csv", row.names=TRUE)
```
### Part 2: Statistical test
Load packages
```
library(car)
```
Load data (see example data: [camera_stats](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/camera_stats.csv))
###### Note: input data are manually-created summaries of species detection totals for each camera position at each array, compiled using the tables generated above.
```
data <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/camera_stats.csv", fileEncoding="UTF-8-BOM")
```
Subset data by position type
```
centers <- data$centers
arms <- data$arms
```
Wilcoxon signed rank test (difference between species counts at camera array arm cameras vs. center camera)
```
wilcox.test(arms, centers, paired=TRUE, alternative="two.sided", exact=FALSE)
```
###### Note: we repeated this test for individual species groups (e.g., snake data only, frog data only, etc.).
