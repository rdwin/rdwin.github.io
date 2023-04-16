---
layout: post
title: "Storm Data"
date: 2020-11-11
excerpt: "Coursera: Reproducible Research, Course Project 2. Doing Data Manipulation and Exploratory Data Analysis in Storms and other severe weather events start in the year 1950 and end in November 2011."
tags: [r programming, data visualization]
project: true
comments: true
---

## Synopsis

### Data

Storms and other severe weather events can cause both public health and economic problems for communities and municipalities. Many severe events can result in fatalities, injuries, and property damage, and preventing such outcomes to the extent possible is a key concern.

This project involves exploring the U.S. National Oceanic and Atmospheric Administration’s (NOAA) storm database. This database tracks characteristics of major storms and weather events in the United States, including when and where they occur, as well as estimates of any fatalities, injuries, and property damage.

The data for this assignment come in the form of a comma-separated-value file compressed via the bzip2 algorithm to reduce its size. You can download the file from the course web site:
- Storm Data [[47Mb]](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2)

There is also some documentation of the database available. Here you will find how some of the variables are constructed/defined.
- National Weather [Service Storm Data Documentation](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf)
- National Climatic Data Center Storm Events [FAQ](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2FNCDC%20Storm%20Events-FAQ%20Page.pdf)

The events in the database start in the year 1950 and end in November 2011. In the earlier years of the database there are generally fewer events recorded, most likely due to a lack of good records. More recent years should be considered more complete.

### Assignment

1. Across the United States, which types of events (as indicated in the `EVTYPE` variable) are most harmful with respect to population health?
2. Across the United States, which types of events have the greatest economic consequences

## Data Processing

### - Loading data

- Downloading the data set


```R
# Download the Data set
    fileurl <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2"
    download.file(fileurl, destfile = 'repdata_data_StormData.csv.bz2')
```

- Loading the data set

First, reading data set from .bz2 file extension.


```R
# Loading the data set
    rawData <- read.csv("repdata_data_StormData.csv.bz2", header = T)
```

After reading the data set, check the dimension and the column names.


```R
# Checking the dimension of the data set
    dim(rawData)

# Checking first 5 rows of the data set
    head(rawData, 5)
```


902297

37



<table>
<thead><tr><th scope=col>STATE__</th><th scope=col>BGN_DATE</th><th scope=col>BGN_TIME</th><th scope=col>TIME_ZONE</th><th scope=col>COUNTY</th><th scope=col>COUNTYNAME</th><th scope=col>STATE</th><th scope=col>EVTYPE</th><th scope=col>BGN_RANGE</th><th scope=col>BGN_AZI</th><th scope=col>...</th><th scope=col>CROPDMGEXP</th><th scope=col>WFO</th><th scope=col>STATEOFFIC</th><th scope=col>ZONENAMES</th><th scope=col>LATITUDE</th><th scope=col>LONGITUDE</th><th scope=col>LATITUDE_E</th><th scope=col>LONGITUDE_</th><th scope=col>REMARKS</th><th scope=col>REFNUM</th></tr></thead>
<tbody>
	<tr><td>1                 </td><td>4/18/1950 0:00:00 </td><td>0130              </td><td>CST               </td><td>97                </td><td>MOBILE            </td><td>AL                </td><td>TORNADO           </td><td>0                 </td><td>                  </td><td>...               </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>3040              </td><td>8812              </td><td>3051              </td><td>8806              </td><td>                  </td><td>1                 </td></tr>
	<tr><td>1                 </td><td>4/18/1950 0:00:00 </td><td>0145              </td><td>CST               </td><td> 3                </td><td>BALDWIN           </td><td>AL                </td><td>TORNADO           </td><td>0                 </td><td>                  </td><td>...               </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>3042              </td><td>8755              </td><td>   0              </td><td>   0              </td><td>                  </td><td>2                 </td></tr>
	<tr><td>1                 </td><td>2/20/1951 0:00:00 </td><td>1600              </td><td>CST               </td><td>57                </td><td>FAYETTE           </td><td>AL                </td><td>TORNADO           </td><td>0                 </td><td>                  </td><td>...               </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>3340              </td><td>8742              </td><td>   0              </td><td>   0              </td><td>                  </td><td>3                 </td></tr>
	<tr><td>1                 </td><td>6/8/1951 0:00:00  </td><td>0900              </td><td>CST               </td><td>89                </td><td>MADISON           </td><td>AL                </td><td>TORNADO           </td><td>0                 </td><td>                  </td><td>...               </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>3458              </td><td>8626              </td><td>   0              </td><td>   0              </td><td>                  </td><td>4                 </td></tr>
	<tr><td>1                 </td><td>11/15/1951 0:00:00</td><td>1500              </td><td>CST               </td><td>43                </td><td>CULLMAN           </td><td>AL                </td><td>TORNADO           </td><td>0                 </td><td>                  </td><td>...               </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>3412              </td><td>8642              </td><td>   0              </td><td>   0              </td><td>                  </td><td>5                 </td></tr>
</tbody>
</table>

- Subset the data set

This step is about to subset column that will be used in the data analysis. These are the column :
     
     - EVTYPE            : Event Type
     - FATALITIES        : Number of Fatalities
     - INJURIES          : Number of Injuries
     - PROPDMG           : Property Damages
     - PROPDMGEXP        : Units for Property Damages (Magnitude - K, B, M)
     - CROPDMG           : Crop Damages
     - CROPDMGEXP        : Units for Property Damages (Magnitude - K, B, M)


```R
# Subsetting the data set
    data <- rawData[,c("EVTYPE", "FATALITIES", "INJURIES", "PROPDMG", "PROPDMGEXP", "CROPDMG", "CROPDMGEXP")]
```

- Data Re-formatting

**Event Type**
     
If we see the content of ***EVTYPE*** variable, it shown that the type of climate too detailed in explain every event. It differentiated some event that basically has same characteristics. For example, "TSTM WIND", "HIGH WIND". "THUNDERSTORM WIND" and so forth. So, I used several categories of climate type to make it more simple.


```R
# Assign categories in old and new objects
    old <- c("WIND", "RAIN", "FLOOD", "STROM|STORM", "HAIL|BLIZZARD", "HEAT", "SNOW", "COLD|ICE|FREEZE|FROST", "WINTER", "WILD", "FOG", "LIGHTNING", "DRY|DROUGHT", "TORNADO|HURRICANE")
    new <- c("WIND", "RAIN", "FLOOD", "STORM", "BLIZZARD", "HEAT","SNOW", "FROST", "WINTER", "WILDFIRE", "FOG", "LIGHTNING", "DROUGHT", "TORNADO")
```


```R
# Replacing EVTYPE with assigned categories
    for (i in 1:14){
        data$EVTYPE[grep(old[i], data$EVTYPE, ignore.case = T)] <- new[i]
    }
```

**Units of Property Damages & Crop Damages**
     
Units of Property & Crop Damages are the damages of every type of event caused. These damages are represented the amount of money in dollars. The magnitudes of the damages measured by 3 type, which is K (per 1000), M (per  1000 000) and B (per 1000 000 000)


```R
# Assign types of measurement in old and new objects
    old <- c("K|M|B", "K", "M", "B")
    new <- c(0, 10^3, 10^6, 10^9)
```


```R
# Replacing value of PROPDMGEXP and CROPDMGEXP variable
    data$PROPDMGEXP[!grepl(old[1], data$PROPDMGEXP, ignore.case = T)] <- new[1]
    data$CROPDMGEXP[!grepl(old[1], data$CROPDMGEXP, ignore.case = T)] <- new[1]

    for (i in 2:4){
        data$PROPDMGEXP <- gsub(old[i], new[i], data$PROPDMGEXP, ignore.case = T)
        data$CROPDMGEXP <- gsub(old[i], new[i], data$CROPDMGEXP, ignore.case = T)
    }
```


```R
# Converting PROPDMGEXP and CROPDMGEXP to numeric
    data$PROPDMGEXP <- as.numeric(data$PROPDMGEXP)
    data$CROPDMGEXP <- as.numeric(data$CROPDMGEXP)
```

### - Across the United States, which types of events (as indicated in the `EVTYPE` variable) are most harmful with respect to population health?

Processing data from the used data set is important. So, the data set need to be subsetted, in order to make the data more easy to analysed. According to the question, the answer only required 3 Column which are **EVTYPE, FATALITIES** and **INJURIES**.


```R
# Making a new data set consist of 3 variable, which are EVTYPE, FATALITIES, and INJURIES
    eventHealth <- aggregate(data[c("FATALITIES", "INJURIES")], data["EVTYPE"], sum)
```


```R
# Arrange the type of event based on INJURIES
    eventHealth <- eventHealth[order(eventHealth$INJURIES, decreasing = T),]

# Subset the top 10 observations in the arranged data set
    eventHealth <- head(eventHealth, 10)
```


```R
eventHealth
```


<table>
<thead><tr><th></th><th scope=col>EVTYPE</th><th scope=col>FATALITIES</th><th scope=col>INJURIES</th></tr></thead>
<tbody>
	<tr><th scope=row>238</th><td>TORNADO  </td><td>5769     </td><td>92735    </td></tr>
	<tr><th scope=row>289</th><td>WIND     </td><td>1451     </td><td>11498    </td></tr>
	<tr><th scope=row>62</th><td>HEAT     </td><td>3138     </td><td> 9224    </td></tr>
	<tr><th scope=row>41</th><td>FLOOD    </td><td>1524     </td><td> 8604    </td></tr>
	<tr><th scope=row>102</th><td>LIGHTNING</td><td> 817     </td><td> 5231    </td></tr>
	<tr><th scope=row>15</th><td>BLIZZARD </td><td> 116     </td><td> 2177    </td></tr>
	<tr><th scope=row>288</th><td>WILDFIRE </td><td>  90     </td><td> 1606    </td></tr>
	<tr><th scope=row>169</th><td>SNOW     </td><td> 159     </td><td> 1120    </td></tr>
	<tr><th scope=row>42</th><td>FOG      </td><td>  80     </td><td> 1076    </td></tr>
	<tr><th scope=row>49</th><td>FROST    </td><td> 225     </td><td>  445    </td></tr>
</tbody>
</table>



```R
# Reformatting new data set
    # 1. Fatalities
        # Subsetting the data set to only show EVTYPE and FATALITIES variable
            fatalities <- eventHealth[-3] # exclude INJURIES variable
        # Change colnames FATALITIES to total
            colnames(fatalities)[2] <- "Total"
        # Add new column Type with vales "Fatalities" to identify total fatalities
            fatalities$Type <- "Fatalities"

    # 2. Do the same thing to INJURIES variable
        injuries <- eventHealth[-2] # exclude FATALITIES variable
        colnames(injuries)[2] <- "Total"
        injuries$Type <- "Injuries"

    # 3. Merge both FATALITIES and INJURIES data set into one object
        eventHealth <- rbind(fatalities, injuries)
```


```R
eventHealth
```


<table>
<thead><tr><th></th><th scope=col>EVTYPE</th><th scope=col>Total</th><th scope=col>Type</th></tr></thead>
<tbody>
	<tr><th scope=row>238</th><td>TORNADO   </td><td> 5769     </td><td>Fatalities</td></tr>
	<tr><th scope=row>289</th><td>WIND      </td><td> 1451     </td><td>Fatalities</td></tr>
	<tr><th scope=row>62</th><td>HEAT      </td><td> 3138     </td><td>Fatalities</td></tr>
	<tr><th scope=row>41</th><td>FLOOD     </td><td> 1524     </td><td>Fatalities</td></tr>
	<tr><th scope=row>102</th><td>LIGHTNING </td><td>  817     </td><td>Fatalities</td></tr>
	<tr><th scope=row>15</th><td>BLIZZARD  </td><td>  116     </td><td>Fatalities</td></tr>
	<tr><th scope=row>288</th><td>WILDFIRE  </td><td>   90     </td><td>Fatalities</td></tr>
	<tr><th scope=row>169</th><td>SNOW      </td><td>  159     </td><td>Fatalities</td></tr>
	<tr><th scope=row>42</th><td>FOG       </td><td>   80     </td><td>Fatalities</td></tr>
	<tr><th scope=row>49</th><td>FROST     </td><td>  225     </td><td>Fatalities</td></tr>
	<tr><th scope=row>2381</th><td>TORNADO   </td><td>92735     </td><td>Injuries  </td></tr>
	<tr><th scope=row>2891</th><td>WIND      </td><td>11498     </td><td>Injuries  </td></tr>
	<tr><th scope=row>621</th><td>HEAT      </td><td> 9224     </td><td>Injuries  </td></tr>
	<tr><th scope=row>411</th><td>FLOOD     </td><td> 8604     </td><td>Injuries  </td></tr>
	<tr><th scope=row>1021</th><td>LIGHTNING </td><td> 5231     </td><td>Injuries  </td></tr>
	<tr><th scope=row>151</th><td>BLIZZARD  </td><td> 2177     </td><td>Injuries  </td></tr>
	<tr><th scope=row>2881</th><td>WILDFIRE  </td><td> 1606     </td><td>Injuries  </td></tr>
	<tr><th scope=row>1691</th><td>SNOW      </td><td> 1120     </td><td>Injuries  </td></tr>
	<tr><th scope=row>421</th><td>FOG       </td><td> 1076     </td><td>Injuries  </td></tr>
	<tr><th scope=row>491</th><td>FROST     </td><td>  445     </td><td>Injuries  </td></tr>
</tbody>
</table>



### - Across the United States, which types of events have the greatest economic consequences?

According to the question, the answer only require 5 Column which are **EVTYPE, PROPDMG, PROPDMGEXP, CROPDMG** and **CROPDMGEXP**.


```R
# Making new data set for examine the economic consequences
    eventEconomy <- data[, -(2:3)] # Excluding FATALITIES and INJURIES variable

# Make some variable to examine the Damages variable
    library(dplyr)
    eventEconomy <- eventEconomy %>% select(everything()) %>% mutate(PROPTTLDMG = PROPDMG * PROPDMGEXP,
                                          CROPTTLDMG = CROPDMG * CROPDMGEXP,
                                          TOTALDMG = PROPTTLDMG + CROPTTLDMG) %>% select(EVTYPE, PROPTTLDMG, CROPTTLDMG, TOTALDMG)
```


```R
# Subset the data set to show ONLY EVTYPE and Total Damage variables
    eventEconomy <- aggregate(eventEconomy[-1], eventEconomy[1], sum)

# Arrange the type of event based on TOTALDMG
    eventEconomy <- eventEconomy[order(eventEconomy$TOTALDMG, decreasing = T),]

# Subset the top 10 observations in the arranged data set
    eventEconomy <- head(eventEconomy, 10)
```


```R
eventEconomy
```


<table>
<thead><tr><th></th><th scope=col>EVTYPE</th><th scope=col>PROPTTLDMG</th><th scope=col>CROPTTLDMG</th><th scope=col>TOTALDMG</th></tr></thead>
<tbody>
	<tr><th scope=row>41</th><td>FLOOD       </td><td>167507743320</td><td>12266906100 </td><td>179774649420</td></tr>
	<tr><th scope=row>238</th><td>TORNADO     </td><td>141649277690</td><td> 5920254110 </td><td>147569531800</td></tr>
	<tr><th scope=row>289</th><td>WIND        </td><td> 17742574470</td><td> 2159304450 </td><td> 19901878920</td></tr>
	<tr><th scope=row>15</th><td>BLIZZARD    </td><td> 16397406670</td><td> 3158897450 </td><td> 19556304120</td></tr>
	<tr><th scope=row>30</th><td>DROUGHT     </td><td>  1052838600</td><td>13972581000 </td><td> 15025419600</td></tr>
	<tr><th scope=row>288</th><td>WILDFIRE    </td><td>  8491563500</td><td>  402781630 </td><td>  8894345130</td></tr>
	<tr><th scope=row>131</th><td>RAIN        </td><td>  3262721190</td><td>  806505800 </td><td>  4069226990</td></tr>
	<tr><th scope=row>49</th><td>FROST       </td><td>   156667950</td><td> 3406177350 </td><td>  3562845300</td></tr>
	<tr><th scope=row>169</th><td>SNOW        </td><td>  1009919740</td><td>  134663100 </td><td>  1144582840</td></tr>
	<tr><th scope=row>102</th><td>LIGHTNING   </td><td>   933689280</td><td>   12092090 </td><td>   945781370</td></tr>
</tbody>
</table>




```R
# Reformatting new data set
    # 1. Properties
        # Subsetting the data set to only show EVTYPE and PROPTTLDMG variable
            property <- eventEconomy[c(1,2)] 
        # Change colnames PROPTTLDMG to damage
            colnames(property)[2] <- "Damage"
        # Add new column Type with vales "Fatalities" to identify total fatalities
            property$Type <- "Property"

    # 2. Do the same thing to CROPTTLDMG variable
        crop <- eventEconomy[c(1,3)]
        colnames(crop)[2] <- "Damage"
        crop$Type <- "Crop"

    # 3. Merge both FATALITIES and INJURIES data set into one object
        eventEconomy <- rbind(property, crop)
```

```R
eventEconomy
```

<table>
<thead><tr><th></th><th scope=col>EVTYPE</th><th scope=col>Damage</th><th scope=col>Type</th></tr></thead>
<tbody>
	<tr><th scope=row>41</th><td>FLOOD       </td><td>167507743320</td><td>Property    </td></tr>
	<tr><th scope=row>238</th><td>TORNADO     </td><td>141649277690</td><td>Property    </td></tr>
	<tr><th scope=row>289</th><td>WIND        </td><td> 17742574470</td><td>Property    </td></tr>
	<tr><th scope=row>15</th><td>BLIZZARD    </td><td> 16397406670</td><td>Property    </td></tr>
	<tr><th scope=row>30</th><td>DROUGHT     </td><td>  1052838600</td><td>Property    </td></tr>
	<tr><th scope=row>288</th><td>WILDFIRE    </td><td>  8491563500</td><td>Property    </td></tr>
	<tr><th scope=row>131</th><td>RAIN        </td><td>  3262721190</td><td>Property    </td></tr>
	<tr><th scope=row>49</th><td>FROST       </td><td>   156667950</td><td>Property    </td></tr>
	<tr><th scope=row>169</th><td>SNOW        </td><td>  1009919740</td><td>Property    </td></tr>
	<tr><th scope=row>102</th><td>LIGHTNING   </td><td>   933689280</td><td>Property    </td></tr>
	<tr><th scope=row>411</th><td>FLOOD       </td><td> 12266906100</td><td>Crop        </td></tr>
	<tr><th scope=row>2381</th><td>TORNADO     </td><td>  5920254110</td><td>Crop        </td></tr>
	<tr><th scope=row>2891</th><td>WIND        </td><td>  2159304450</td><td>Crop        </td></tr>
	<tr><th scope=row>151</th><td>BLIZZARD    </td><td>  3158897450</td><td>Crop        </td></tr>
	<tr><th scope=row>301</th><td>DROUGHT     </td><td> 13972581000</td><td>Crop        </td></tr>
	<tr><th scope=row>2881</th><td>WILDFIRE    </td><td>   402781630</td><td>Crop        </td></tr>
	<tr><th scope=row>1311</th><td>RAIN        </td><td>   806505800</td><td>Crop        </td></tr>
	<tr><th scope=row>491</th><td>FROST       </td><td>  3406177350</td><td>Crop        </td></tr>
	<tr><th scope=row>1691</th><td>SNOW        </td><td>   134663100</td><td>Crop        </td></tr>
	<tr><th scope=row>1021</th><td>LIGHTNING   </td><td>    12092090</td><td>Crop        </td></tr>
</tbody>
</table>



## Result & Conclusions

### - Across the United States, which types of events (as indicated in the `EVTYPE` variable) are most harmful with respect to population health?

These question can be answered by comparing the total number of both fatalities and injuries in several type of Event.


```R
# Import necessary library
    library(ggplot2)

# Create plot with eventHealth object
    ggplot(eventHealth, aes(x = reorder(EVTYPE, -Total), y = Total, fill = Type)) + 
            facet_wrap(Type~., scales = "free") +
            geom_bar(stat = 'identity') + 
            ggtitle("Health Impact in US based on top 10 Weather Events") + 
            xlab('Type of Weather') + ylab("Number of Health Impact") +
            theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

[![]({{site.url}}/assets/img/project/rep-2/output_44_0.png)]({{site.url}}/assets/img/project/rep-2/output_44_0.png)

From the graph above, I conclude that **Tornado** has big impact into population health, both in injuries or fatalities, Across United States.

### - Across the United States, which types of events have the greatest economic consequences?

These question can be answered by comparing the total damages caused by every type Weather event. These total damages indicated by the total money are used to repair both property or crop.


```R
# Create plot with eventEconomy object
    ggplot(eventEconomy, aes(x = reorder(EVTYPE, -Damage), y = Damage/10^6, fill = Type)) + 
            facet_wrap(Type~., scales = "free") +
            geom_bar(stat = 'identity') +
            ggtitle("Comparing Property and Crop Total Damages in US\n based on top 10 Weather Events") + 
            xlab("Type of Weather") + 
            ylab(expression("Total Damages per 10"^6* " Dollars")) +
            theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

[![]({{site.url}}/assets/img/project/rep-2/output_48_0.png)]({{site.url}}/assets/img/project/rep-2/output_48_0.png)
      
From the graph above, I conclude that the Weather event categoried by **DROUGHT** have the greatest repair cost in **Crop Repair**. While in property, We could see that the greatest cost is event categoried by **FLOOD**.
