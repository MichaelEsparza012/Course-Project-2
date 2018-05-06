# Course-Project-2
Peer Graded Assignment Course Project 2 - Week 4 Exploratory Data Analysis

The Git Hub URL for this assignement is: https://github.com/MichaelEsparza012/Course-Project-2

Data Frame provided: "summarySCC_PM25.RDS" and "Source_Classification_Code.rds" available at: "Data for Peer Assessment."

# Review Criteria 
1. Does the Plot appear the address the questions asked?
2. Is the submitted R Code appropriate for the construction of the submitted lot?
# Overall goal of the assignment is to conduct analysis the of National Emissions Inventory (NEI) database and see what its days about fine particulate matter pollution in the United State over the 10 year period of 1999-2008.

# Questions

1. Have total emissions from PM2.5 decreased in the United States from 1999-2008? Using the BASE plotting system, make a plot showing the TOTAL PM2.5 emission from all sources for each of the years: 1999, 2002, 2005 and 2008.

#Load the given Data Frames (.RDS files)

+     NEI <- readRDS("summarySCC_PM25.rds")

+     SCC <- readRDS("Source_Classification_Code.rds")

#NEI shows 6,497,561 observations of 6 variables

#SCC shows 11,717 observations of 15 variables

#Review the data frames for NEI and SCC with th R call "head"

#NEI

+     head(NEI)

    fips      SCC Pollutant Emissions  type year
    
4  09001 10100401  PM25-PRI    15.714 POINT 1999

8  09001 10100404  PM25-PRI   234.178 POINT 1999

12 09001 10100501  PM25-PRI     0.128 POINT 1999

16 09001 10200401  PM25-PRI     2.036 POINT 1999

20 09001 10200504  PM25-PRI     0.388 POINT 1999

24 09001 10200602  PM25-PRI     1.490 POINT 1999

+     head(SCC)

       SCC Data.Category                                                                 Short.Name
       
1 10100101         Point                   Ext Comb /Electric Gen /Anthracite Coal /Pulverized Coal

2 10100102         Point Ext Comb /Electric Gen /Anthracite Coal /Traveling Grate (Overfeed) Stoker

3 10100201         Point       Ext Comb /Electric Gen /Bituminous Coal /Pulverized Coal: Wet Bottom

4 10100202         Point       Ext Comb /Electric Gen /Bituminous Coal /Pulverized Coal: Dry Bottom

5 10100203         Point                   Ext Comb /Electric Gen /Bituminous Coal /Cyclone Furnace

6 10100204         Point                   Ext Comb /Electric Gen /Bituminous Coal /Spreader Stoker

                               EI.Sector Option.Group Option.Set               SCC.Level.One       SCC.Level.Two
                               
1 Fuel Comb - Electric Generation - Coal                         External Combustion Boilers Electric Generation

2 Fuel Comb - Electric Generation - Coal                         External Combustion Boilers Electric Generation

3 Fuel Comb - Electric Generation - Coal                         External Combustion Boilers Electric Generation

4 Fuel Comb - Electric Generation - Coal                         External Combustion Boilers Electric Generation

5 Fuel Comb - Electric Generation - Coal                         External Combustion Boilers Electric Generation

6 Fuel Comb - Electric Generation - Coal                         External Combustion Boilers Electric Generation

                SCC.Level.Three                                SCC.Level.Four Map.To Last.Inventory.Year
                
1               Anthracite Coal                               Pulverized Coal     NA                  NA

2               Anthracite Coal             Traveling Grate (Overfeed) Stoker     NA                  NA

3 Bituminous/Subbituminous Coal Pulverized Coal: Wet Bottom (Bituminous Coal)     NA                  NA

4 Bituminous/Subbituminous Coal Pulverized Coal: Dry Bottom (Bituminous Coal)     NA                  NA

5 Bituminous/Subbituminous Coal             Cyclone Furnace (Bituminous Coal)     NA                  NA

6 Bituminous/Subbituminous Coal             Spreader Stoker (Bituminous Coal)     NA                  NA

  Created_Date Revised_Date Usage.Notes
  
#First, using the provided data frame, calculate the total PM2.5 emissions from 1999+2002+2005+2008. 

Totals <- aggregate(Emissions ~ year,NEI, sum)

Totals

  year Emissions
  
1 1999   7332967

2 2002   5635780

3 2005   5454703

4 2008   3464206

#Results in 4 observations of 2 variables

#BASE Plotting will be used, with the R call "barplot" for plot of Total PM2.5 emissions from all data sources

+     barplot(

+     (aggTotals$Emissions)/10^6,
  
+     names.arg=aggTotals$year,
  
+     xlab="Year",
  
+     ylab="PM2.5 Emissions (10^6 Tons)",
  
+     main="Total PM2.5 Emissions From All US Sources"
  
 +     )

#resulting BASE Plot is saved, as a PNG file, showing a DECREASE in total emissions from 1999 to 2008.

2. Have total emissions from PM2.5 decreased in Baltimore City, Maryland, (fips = "24510) from 1999 to 2008? Use the BASE Plotting System to make a lot answering this question.

#Using the provided data frame, calculate the total PM2.5 for BALTIMORE CITY Maryland (fips "24510") emissions from 1999 to 2008. 

+     baltNEI <- NEI[NEI$fips=="24510",]

+     TotalsBaltimore <- aggregate(Emissions ~ year, baltNEI,sum)

#BASE Plotting will be used, with the R call "barplot" for plot of Total PM2.5 emissions from all Baltimore City, Maryland Sources

+     barplot(
 
+     TotalsBaltimore$Emissions,

+     names.arg=TotalsBaltimore$year,

+     xlab="Year",

+     ylab="PM2.5 Emissions (Tons)",

+     main="Total PM2.5 Emissions From All Baltimore City Sources"

+      )

#From 1999 to 2008, overall PM2.5 Emissions have DECREASED in Baltimore City, Maryland.

3. Of the four types of sources indicated by the type (point, nonpoint, onroad, nonroad) variable, which of theses four sources have seen decreases in emissions from 1999-2008 for Baltimore City? Which have seen incresases in emissions from 1999-2008? Use ggplot2 plotting system to make a plot to answer this question.

#Using the ggplot2 package, one can plot all four typed in order to answer the given questions.

+     library(ggplot2)

+     ggp <- ggplot(baltNEI,aes(factor(year),Emissions,fill=type)) +

+     geom_bar(stat="identity") +

+     theme_bw() + guides(fill=FALSE)+

+     facet_grid(.~type,scales = "free",space="free") +

+     labs(x="year", y=expression("Total PM"[2.5]*" Emission (Tons)")) +

+     labs(title=expression("PM"[2.5]*" Emissions, Baltimore City 1999-2008 by Source Type"))

+     print(ggp)

#Of the four source types, 3 sources have seen decreases from 1999-2008: NON-ROAD, NONPOINT, ON-ROAD.

4. Across the United States, how have emissions from coal combustion-related sources changed from 1999-2008 in Baltimore City?

#First Cousre of Action (COA) would be to sub-set the Coal Combustion elements from the NEI Data.

+     combustionRelated <- grepl("comb", SCC$SCC.Level.One, ignore.case=TRUE)

+     coalRelated <- grepl("coal", SCC$SCC.Level.Four, ignore.case=TRUE)

+     coalCombustion <- (combustionRelated & coalRelated)

+     combustionSCC <- SCC[coalCombustion,]$SCC

+     combustionNEI <- NEI[NEI$SCC %in% combustionSCC,]

#Once Subset, we use ggplot2 to plot the prepared data.

+     library(ggplot2)

+     ggp <- ggplot(combustionNEI,aes(factor(year),Emissions/10^5)) +

+     geom_bar(stat="identity",fill="grey",width=0.75) +
  
+     theme_bw() +  guides(fill=FALSE) +
  
+     labs(x="year", y=expression("Total PM"[2.5]*" Emission (10^5 Tons)")) +
  
+     labs(title=expression("PM"[2.5]*" Coal Combustion Source Emissions Across US from 1999-2008"))

+     print(ggp)

5. How have emissions from motor vehicles sources changed from 1999-2008 in Baltimore City?

#First prepare the data frame to look at motor vehicles by subset.

+     vehicles <- grepl("vehicle", SCC$SCC.Level.Two, ignore.case=TRUE)

+     vehiclesSCC <- SCC[vehicles,]$SCC

+     vehiclesNEI <- NEI[NEI$SCC %in% vehiclesSCC,]

#Second Course of Action (COA) is subset motor vehicles in Baltimore City, Maryland.

#To complete the analysis, we call ggplot2 and plot the prepared data.

+     library(ggplot2)

+     ggp <- ggplot(baltVehiclesNEI,aes(factor(year),Emissions)) +

+     geom_bar(stat="identity",fill="grey",width=0.75) +
  
+     theme_bw() +  guides(fill=FALSE) +
  
+     labs(x="year", y=expression("Total PM"[2.5]*" Emission (10^5 Tons)")) +
  
+     labs(title=expression("PM"[2.5]*" Motor Vehicle Source Emissions in Baltimore from 1999-2008"))
  
+     print(ggp)

6. Compare emissions from motor vehicles sources in Baltimore City with emisssions from motor vehicles in Los Angeles County, California (fips = "06037"). Which city has seen greater changes over time in motor vehicle emissions?

#Subset the Baltimore City and Los Angeles County Motor Vehicle Data 

+     vehiclesBaltNEI <- vehiclesNEI[vehiclesNEI$fips == 24510,]

+     vehiclesBaltNEI$city <- "Baltimore City"

+     vehiclesLANEI <- vehiclesNEI[vehiclesNEI$fips=="06037",]

+     vehiclesLANEI$city <- "Los Angeles County"

+     bothNEI <- rbind(vehiclesBaltNEI,vehiclesLANEI)

#Call ggplot2 and plot the subset data.

+     library(ggplot2)
 
+     ggp <- ggplot(bothNEI, aes(x=factor(year), y=Emissions, fill=city)) +
 
+     geom_bar(aes(fill=year),stat="identity") +

+     facet_grid(scales="free", space="free", .~city) +

+     guides(fill=FALSE) + theme_bw() +

+     labs(x="year", y=expression("Total PM"[2.5]*" Emission (Kilo-Tons)")) + 

+     labs(title=expression("PM"[2.5]*" Motor Vehicle Source Emissions in Baltimore & LA, 1999-2008"))
 
+     print(ggp)
