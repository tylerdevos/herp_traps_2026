## Hybrid arrays
### Part 1: Hybrid array counts by trap type for individual arrays
#load data
trap_data_raw <- read.csv("C:/Users/tbd22/OneDrive - Florida State University/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_trap_data.csv", fileEncoding="UTF-8-BOM")
camera_data_raw <- read.csv("C:/Users/tbd22/OneDrive - Florida State University/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_camera_data.csv", fileEncoding="UTF-8-BOM")
array_info <- read.csv("C:/Users/tbd22/OneDrive - Florida State University/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_array_info.csv", fileEncoding="UTF-8-BOM")

#expand condensed species data using the count column
trap_data <- trap_data_raw[rep(row.names(trap_data_raw), trap_data_raw$count_in_trap), 1:6]
camera_data <- camera_data_raw[rep(row.names(camera_data_raw), camera_data_raw$count), 1:9]

#join array type data to trap and camera data
trap_array_data <- merge(trap_data, array_info, by='array_ID')
camera_array_data <- merge(camera_data, array_info, by='array_ID')

#subset hybrid array data
hybrid_trap_data <- subset(trap_array_data, array_type=='hybrid')
hybrid_camera_data <- subset(camera_array_data, array_type=='hybrid')

#merge hybrid trap array and hybrid camera data
hybrid_camera_data$trap_type <- NA
hybrid_camera_data$trap_type[is.na(hybrid_camera_data$trap_type)] <- "camera"
hybrid_camera_merge <- hybrid_camera_data[c("species","trap_type","array_ID")]
hybrid_trap_merge <- hybrid_trap_data[c("species","trap_type","array_ID")]
hybrid_trap_and_camera <- rbind(hybrid_trap_merge, hybrid_camera_merge)

#get list of hybrid array IDs
sort(unique(hybrid_trap_and_camera$array_ID))

#subset data by array ID
array_1 <- subset(hybrid_trap_and_camera, array_ID=='Array 1')
array_2 <- subset(hybrid_trap_and_camera, array_ID=='Array 2')
array_5 <- subset(hybrid_trap_and_camera, array_ID=='Array 5')
array_6 <- subset(hybrid_trap_and_camera, array_ID=='Array 6')
array_8 <- subset(hybrid_trap_and_camera, array_ID=='Array 8')
array_11 <- subset(hybrid_trap_and_camera, array_ID=='Array 11')

#create and export tables of species counts by trap type for each array
array_1_table <- table(array_1$species, array_1$trap_type)
array_2_table <- table(array_2$species, array_2$trap_type)
array_5_table <- table(array_5$species, array_5$trap_type)
array_6_table <- table(array_6$species, array_6$trap_type)
array_8_table <- table(array_8$species, array_8$trap_type)
array_11_table <- table(array_11$species, array_11$trap_type)
setwd("C:/Users/tbd22/OneDrive - Florida State University/Desktop/")
write.csv(array_1_table, "Three_Lakes_array_1_results.csv", row.names=TRUE)
write.csv(array_2_table, "Three_Lakes_array_2_results.csv", row.names=TRUE)
write.csv(array_5_table, "Three_Lakes_array_5_results.csv", row.names=TRUE)
write.csv(array_6_table, "Three_Lakes_array_6_results.csv", row.names=TRUE)
write.csv(array_8_table, "Three_Lakes_array_8_results.csv", row.names=TRUE)
write.csv(array_11_table, "Three_Lakes_array_11_results.csv", row.names=TRUE)


