## Species detections by trap type
### Part 1: General summary table
Load data (see example data: [Three_Lakes_trap_data](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/Three_Lakes_trap_data.csv), [Three_Lakes_camera_data](https://github.com/tylerdevos/herp_traps_2026/blob/main/Data/Three_Lakes_camera_data.csv))
```
trap_data_raw <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_trap_data.csv", fileEncoding="UTF-8-BOM")
camera_data_raw <- read.csv("C:/Users/tbd/Desktop/Research/Herp Capture Data/2025_season/data/Three_Lakes_camera_data.csv", fileEncoding="UTF-8-BOM")
```
Expand condensed species data using the count column
```
trap_data <- trap_data_raw[rep(row.names(trap_data_raw), trap_data_raw$count_in_trap), 1:6]
camera_data <- camera_data_raw[rep(row.names(camera_data_raw), camera_data_raw$count), 1:9]
```
Merge trap and camera data
```
camera_data$trap_type <- NA
camera_data$trap_type[is.na(camera_data$trap_type)] <- "camera"
camera_merge <- camera_data[c("species","trap_type")]
trap_merge <- trap_data[c("species","trap_type")]
trap_and_camera <- rbind(trap_merge, camera_merge)
```
Create table of capture counts by species and trap type and export as a .csv file
```
species_trap_table <- table(trap_and_camera$species, trap_and_camera$trap_type)
setwd("C:/Users/tbd/Desktop")
write.csv(species_trap_table, "Three_Lakes_species_counts_by_trap_type.csv", row.names=TRUE)
```
###### Note: we repeated the above steps for each relevant survey site
### Part 2: Venn diagrams
Load packages
```
library(VennDiagram)
```
Plot Venn diagram for Three Lakes reptiles
###### Note: circle and intersection sizes must be specified manually; these values can be determined using the table from part 1.
```
grid.newpage()
TR <- draw.triple.venn(area1=21, area2=11, area3=23, 
                 n12=10, n23=9, n13=15, n123=8, 
                 category=c("","",""),
                 col="Black", fill=c("#5B9BD5","#F35353","#FFC000"), alpha=0.75)
setwd("C:/Users/tbd/Desktop/")
ggsave(TR, filename="Three_Lakes_reptiles_venn_diagram.jpg", width=3.0, height=3.0, units="in")
```
<img src="/Graphics/Three_Lakes_reptiles_raw_venn_diagram.jpg" alt="Three_Lakes_reptiles_raw_venn_diagram" width="400"/>

Plot Venn diagram for Three Lakes amphibians
###### Note: circle and intersection sizes must be specified manually; these values can be determined using the table from part 1.
```
grid.newpage()
TA <- draw.triple.venn(area1=6, area2=9, area3=11, 
                 n12=4, n23=7, n13=6, n123=4, 
                 category=c("","",""),
                 col="Black", fill=c("#5B9BD5","#F35353","#FFC000"), alpha=0.75)
setwd("C:/Users/tbd/Desktop/")
ggsave(TA, filename="Three_Lakes_amphibians_venn_diagram.jpg", width=3.0, height=3.0, units="in")
```
<img src="/Graphics/Three_Lakes_amphibians_raw_venn_diagram.jpg" alt="Three_Lakes_amphibians_raw_venn_diagram" width="400"/>

###### Note: we manually added animal symbols to the base plots generated in R to show the breakdown of species groups within each trap type; Venn diagrams from both sites were then combined to create the final figure:
<img src="/Graphics/Figure_2.jpg" alt="Figure_2" width="800"/>



### [To page 2: hybrid arrays >>>](https://github.com/tylerdevos/herp_traps_2026/blob/main/2_hybrid_arrays.md)
