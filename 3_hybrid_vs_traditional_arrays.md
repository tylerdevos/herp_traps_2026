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
TO BE CONTINUED
