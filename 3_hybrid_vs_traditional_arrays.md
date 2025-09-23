## Hybrid vs. traditional arrays
### Part 1: Traditional array capture counts by species and trap type for individual arrays
Load data (see example data: [Three_Lakes_trap_data](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/Three_Lakes_trap_data.csv), [Three_Lakes_array_info](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/Three_Lakes_array_info.csv))
```
trap_data_raw <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_trap_data.csv", fileEncoding="UTF-8-BOM")
array_info <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_array_info.csv", fileEncoding="UTF-8-BOM")
```
Expand condensed species data using the count column
```
trap_data <- trap_data_raw[rep(row.names(trap_data_raw), trap_data_raw$count_in_trap), 1:6]
```
Join array type data to trap data
```
trap_array_data <- merge(trap_data, array_info, by='array_ID')
```
Extract data from traditional arrays only
```
box_trap_data <- subset(trap_array_data, array_type=='trap')
```
Print a list of traditional array IDs
```
sort(unique(box_trap_data$array_ID))
```
Subset data by array ID
```
array_2 <- subset(box_trap_data, array_ID=='Array 2')
array_5 <- subset(box_trap_data, array_ID=='Array 5')
array_8 <- subset(box_trap_data, array_ID=='Array 8')
array_9 <- subset(box_trap_data, array_ID=='Array 9')
array_10 <- subset(box_trap_data, array_ID=='Array 10')
```
Create and export tables of species counts by trap type for each array
```
array_2_table <- table(array_2$species, array_2$trap_type)
array_5_table <- table(array_5$species, array_5$trap_type)
array_8_table <- table(array_8$species, array_8$trap_type)
array_9_table <- table(array_9$species, array_9$trap_type)
array_10_table <- table(array_10$species, array_10$trap_type)
setwd("C:/Users/tbd/Desktop/")
write.csv(array_2_table, "Three_Lakes_array_2_results.csv", row.names=TRUE)
write.csv(array_5_table, "Three_Lakes_array_5_results.csv", row.names=TRUE)
write.csv(array_8_table, "Three_Lakes_array_8_results.csv", row.names=TRUE)
write.csv(array_9_table, "Three_Lakes_array_9_results.csv", row.names=TRUE)
write.csv(array_10_table, "Three_Lakes_array_10_results.csv", row.names=TRUE)
```
### Part 2: Statistical tests
Load packages
```
library(car)
```
Load data (see example data: [hybrid_camera_stats](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/hybrid_camera_stats.csv), [box_stats](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/box_stats.csv), [camera_v_box_stats](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/camera_v_box_stats.csv))
###### Note: input data are manually-created summaries of species detection totals for each trap type at each array, compiled using the tables generated above and in part 1 on page 2 (hybrid arrays).
```
data_hybrid_camera <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/hybrid_camera_stats.csv", fileEncoding="UTF-8-BOM")
data_traditional_box <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/box_stats.csv", fileEncoding="UTF-8-BOM")
data_both <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/camera_v_box_stats.csv", fileEncoding="UTF-8-BOM")
```
Subset hybrid array data by trap type
```
funnels_only <- data_hybrid_camera$funnels_only
funnels_and_camera <- data_hybrid_camera$funnels_and_camera
```
Wilcoxon signed rank test (difference between species counts at hybrid array funnels vs. funnels and camera combined)
```
wilcox.test(funnels_and_camera, funnels_only, paired=TRUE, alternative="greater", exact=FALSE)
```
Subset traditional array data by trap type
```
funnels_only_2 <- data_traditional_box$funnels_only
funnels_and_box <- data_traditional_box$funnels_and_box
```
Wilcoxon signed rank test (difference between species counts at traditional array funnels vs. funnels and box trap combined)
```
wilcox.test(funnels_and_box, funnels_only_2, paired=TRUE, alternative="greater", exact=FALSE)
```
Subset data by trap type
```
camera <-subset(data, trap_type=="camera")
box <-subset(data, trap_type=="box")
```
Mann-Whitney U-test (difference between percentage increase in species detection counts resulting from cameras vs. box traps)
```
wilcox.test(camera$perc_increase, box$perc_increase, alternative="two.sided", exact=FALSE)
```
###### Note: we repeated the above tests for individual species groups (e.g., snake data only, frog data only, etc.).
