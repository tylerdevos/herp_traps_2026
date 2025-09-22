## Hybrid arrays
### Part 1: Hybrid array capture counts by species and trap type for individual arrays
Load data (see example data: [Three_Lakes_trap_data](https://github.com/tylerdevos/herp_traps_2026/Data/Three_Lakes_trap_data.csv), [Three_Lakes_camera_data](https://github.com/tylerdevos/herp_traps_2026/Data/Three_Lakes_camera_data.csv), [Three_Lakes_array_info](https://github.com/tylerdevos/herp_traps_2026/Data/Three_Lakes_array_info.csv))
```
trap_data_raw <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_trap_data.csv", fileEncoding="UTF-8-BOM")
camera_data_raw <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_camera_data.csv", fileEncoding="UTF-8-BOM")
array_info <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_array_info.csv", fileEncoding="UTF-8-BOM")
```
Expand condensed species data using the count column
```
trap_data <- trap_data_raw[rep(row.names(trap_data_raw), trap_data_raw$count_in_trap), 1:6]
camera_data <- camera_data_raw[rep(row.names(camera_data_raw), camera_data_raw$count), 1:9]
```
Join array type data to trap and camera data
```
trap_array_data <- merge(trap_data, array_info, by='array_ID')
camera_array_data <- merge(camera_data, array_info, by='array_ID')
```
Extract data from hybrid arrays only
```
hybrid_trap_data <- subset(trap_array_data, array_type=='hybrid')
hybrid_camera_data <- subset(camera_array_data, array_type=='hybrid')
```
Merge hybrid trap and hybrid camera data
```
hybrid_camera_data$trap_type <- NA
hybrid_camera_data$trap_type[is.na(hybrid_camera_data$trap_type)] <- "camera"
hybrid_camera_merge <- hybrid_camera_data[c("species","trap_type","array_ID")]
hybrid_trap_merge <- hybrid_trap_data[c("species","trap_type","array_ID")]
hybrid_trap_and_camera <- rbind(hybrid_trap_merge, hybrid_camera_merge)
```
Print a list of hybrid array IDs
```
sort(unique(hybrid_trap_and_camera$array_ID))
```
Subset data by array ID
```
array_1 <- subset(hybrid_trap_and_camera, array_ID=='Array 1')
array_3 <- subset(hybrid_trap_and_camera, array_ID=='Array 3')
array_4 <- subset(hybrid_trap_and_camera, array_ID=='Array 4')
array_6 <- subset(hybrid_trap_and_camera, array_ID=='Array 6')
array_7 <- subset(hybrid_trap_and_camera, array_ID=='Array 7')
array_11 <- subset(hybrid_trap_and_camera, array_ID=='Array 11')
```
Create and export tables of species counts by trap type for each array
```
array_1_table <- table(array_1$species, array_1$trap_type)
array_3_table <- table(array_3$species, array_3$trap_type)
array_4_table <- table(array_4$species, array_4$trap_type)
array_6_table <- table(array_6$species, array_6$trap_type)
array_7_table <- table(array_7$species, array_7$trap_type)
array_11_table <- table(array_11$species, array_11$trap_type)
setwd("C:/Users/tbd/Desktop/")
write.csv(array_1_table, "Three_Lakes_array_1_hybrid_results.csv", row.names=TRUE)
write.csv(array_3_table, "Three_Lakes_array_3_hybrid_results.csv", row.names=TRUE)
write.csv(array_4_table, "Three_Lakes_array_4_hybrid_results.csv", row.names=TRUE)
write.csv(array_6_table, "Three_Lakes_array_6_hybrid_results.csv", row.names=TRUE)
write.csv(array_7_table, "Three_Lakes_array_7_hybrid_results.csv", row.names=TRUE)
write.csv(array_11_table, "Three_Lakes_array_11_hybrid_results.csv", row.names=TRUE)
```
###### Note: we repeated the above procedure with an alternate version of the dataset including camera data only from times when funnel traps were open.
### Part 2: Statistical test
Load data (see example data: [hybrid_stats](https://github.com/tylerdevos/herp_traps_2026/Data/hybrid_stats.csv))
###### Note: input data are manually-created summaries of species detection totals for each trap type at each array, compiled using the tables generated above.
```
data <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/hybrid_stats.csv", fileEncoding="UTF-8-BOM")
```
Subset data by trap type
```
funnels <- data$funnels
camera <- data$camera
```
Wilcoxon signed rank test (difference between species counts at hybrid array funnels vs. cameras)
```
wilcox.test(camera, funnels, paired=TRUE, alternative="two.sided", exact=FALSE)
```
###### Note: we repeated this test for individual species groups (e.g., snake data only, frog data only, etc.) and for an alternate version of the dataset including camera data only from times when funnel traps were open.
