---
layout: post
title: "Coursera: Exploratory Data Analysis 1"
date: 2020-09-28
excerpt: "Demonstrating basic ETL and reconstruct the plots from the examples provided by Lecturers. This is an assignment of Exploratory Data Analysis course from John Hopkins University in Coursera."
tags: [r programming, project, data visualization]
project: true
---

## Synopsis

### Introduction

This assignment uses data from the [UC Irvine Machine Learning Repository](http://archive.ics.uci.edu/ml/), a popular repository for machine learning datasets. In particular, we will be using the "Individual household electric power consumption Data Set" which I have made available on the course web site:

- **Dataset**: Electric power consumption [[20MB]](https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2Fhousehold_power_consumption.zip)

- **Description**: Measurements of electric power consumption in one household with a one-minute sampling rate over a period of almost 4 years. Different electrical quantities and some sub-metering values are available. The detailed description of these dataset could be obtained in [UCI website](https://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption).

Our overall goal here is simply to examine how household energy usage varies over a 2-day period in February, 2007. The task is to reconstruct the plots provided by lecturer, all of which were constructed using the base plotting system. The detailed documentation could be obtained in my [Github Repository](https://github.com/rdwin/ExData_Plotting1).

### Processing Steps

- Loading the data. Note that the dataset missing vaules are coded as `?`.
- Subsetting the dates to **2007-02-01** and **2007-02-02**.
- Converting date & time variables to Date/Time classes in R using `strptime()` and/or `as.Date()` functions.
- Construct the plot and save it to a PNG file with **480x480 px** size.

## Loading the Data


```R
# Download the Data set
    fileurl <- 'https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2Fhousehold_power_consumption.zip'
    download.file(fileurl, destfile = 'household_power_consumption.zip')
      
# Unzip the Data set
    unzip('./household_power_consumption.zip')
```


```R
# Read data from local directory
     rawData <- read.table('./household_power_consumption.txt', header = T,sep = ';', na.strings = '?')
```


```R
print(paste("Observation: ", nrow(rawData),"Column: ", ncol(rawData)))
head(rawData)
```

    [1] "Observation:  2075259 Column:  9"
    


<table>
<thead><tr><th scope=col>Date</th><th scope=col>Time</th><th scope=col>Global_active_power</th><th scope=col>Global_reactive_power</th><th scope=col>Voltage</th><th scope=col>Global_intensity</th><th scope=col>Sub_metering_1</th><th scope=col>Sub_metering_2</th><th scope=col>Sub_metering_3</th></tr></thead>
<tbody>
	<tr><td>16/12/2006</td><td>17:24:00  </td><td>4.216     </td><td>0.418     </td><td>234.84    </td><td>18.4      </td><td>0         </td><td>1         </td><td>17        </td></tr>
	<tr><td>16/12/2006</td><td>17:25:00  </td><td>5.360     </td><td>0.436     </td><td>233.63    </td><td>23.0      </td><td>0         </td><td>1         </td><td>16        </td></tr>
	<tr><td>16/12/2006</td><td>17:26:00  </td><td>5.374     </td><td>0.498     </td><td>233.29    </td><td>23.0      </td><td>0         </td><td>2         </td><td>17        </td></tr>
	<tr><td>16/12/2006</td><td>17:27:00  </td><td>5.388     </td><td>0.502     </td><td>233.74    </td><td>23.0      </td><td>0         </td><td>1         </td><td>17        </td></tr>
	<tr><td>16/12/2006</td><td>17:28:00  </td><td>3.666     </td><td>0.528     </td><td>235.68    </td><td>15.8      </td><td>0         </td><td>1         </td><td>17        </td></tr>
	<tr><td>16/12/2006</td><td>17:29:00  </td><td>3.520     </td><td>0.522     </td><td>235.02    </td><td>15.0      </td><td>0         </td><td>2         </td><td>17        </td></tr>
</tbody>
</table>




```R
# Subset data from 2007-02-01 and 2007-02-02
     data <- subset(rawData, Date == '1/2/2007' | Date == '2/2/2007')

# Correct date and time variable to the correct class
     data$Date <- as.Date(data$Date, format = '%d/%m/%Y')

# Add new variable called DateTime, consist of Variable Date and Time
     dateTime <- paste(data$Date, data$Time)
     data$DateTime <- strptime(dateTime, tz = "", '%Y-%m-%d %H:%M:%S')
```


```R
str(data)
head(data)
```

    'data.frame':	2880 obs. of  10 variables:
     $ Date                 : Date, format: "2007-02-01" "2007-02-01" ...
     $ Time                 : Factor w/ 1440 levels "00:00:00","00:01:00",..: 1 2 3 4 5 6 7 8 9 10 ...
     $ Global_active_power  : num  0.326 0.326 0.324 0.324 0.322 0.32 0.32 0.32 0.32 0.236 ...
     $ Global_reactive_power: num  0.128 0.13 0.132 0.134 0.13 0.126 0.126 0.126 0.128 0 ...
     $ Voltage              : num  243 243 244 244 243 ...
     $ Global_intensity     : num  1.4 1.4 1.4 1.4 1.4 1.4 1.4 1.4 1.4 1 ...
     $ Sub_metering_1       : num  0 0 0 0 0 0 0 0 0 0 ...
     $ Sub_metering_2       : num  0 0 0 0 0 0 0 0 0 0 ...
     $ Sub_metering_3       : num  0 0 0 0 0 0 0 0 0 0 ...
     $ DateTime             : POSIXlt, format: "2007-02-01 00:00:00" "2007-02-01 00:01:00" ...
    


<table>
<thead><tr><th></th><th scope=col>Date</th><th scope=col>Time</th><th scope=col>Global_active_power</th><th scope=col>Global_reactive_power</th><th scope=col>Voltage</th><th scope=col>Global_intensity</th><th scope=col>Sub_metering_1</th><th scope=col>Sub_metering_2</th><th scope=col>Sub_metering_3</th><th scope=col>DateTime</th></tr></thead>
<tbody>
	<tr><th scope=row>66637</th><td>2007-02-01         </td><td>00:00:00           </td><td>0.326              </td><td>0.128              </td><td>243.15             </td><td>1.4                </td><td>0                  </td><td>0                  </td><td>0                  </td><td>2007-02-01 00:00:00</td></tr>
	<tr><th scope=row>66638</th><td>2007-02-01         </td><td>00:01:00           </td><td>0.326              </td><td>0.130              </td><td>243.32             </td><td>1.4                </td><td>0                  </td><td>0                  </td><td>0                  </td><td>2007-02-01 00:01:00</td></tr>
	<tr><th scope=row>66639</th><td>2007-02-01         </td><td>00:02:00           </td><td>0.324              </td><td>0.132              </td><td>243.51             </td><td>1.4                </td><td>0                  </td><td>0                  </td><td>0                  </td><td>2007-02-01 00:02:00</td></tr>
	<tr><th scope=row>66640</th><td>2007-02-01         </td><td>00:03:00           </td><td>0.324              </td><td>0.134              </td><td>243.90             </td><td>1.4                </td><td>0                  </td><td>0                  </td><td>0                  </td><td>2007-02-01 00:03:00</td></tr>
	<tr><th scope=row>66641</th><td>2007-02-01         </td><td>00:04:00           </td><td>0.322              </td><td>0.130              </td><td>243.16             </td><td>1.4                </td><td>0                  </td><td>0                  </td><td>0                  </td><td>2007-02-01 00:04:00</td></tr>
	<tr><th scope=row>66642</th><td>2007-02-01         </td><td>00:05:00           </td><td>0.320              </td><td>0.126              </td><td>242.29             </td><td>1.4                </td><td>0                  </td><td>0                  </td><td>0                  </td><td>2007-02-01 00:05:00</td></tr>
</tbody>
</table>



## Making Plots

### Plot 1


```R
# Construct plot and save it to PNG file
     hist(data$Global_active_power,
          col = 'red',
          xlab = 'Global Active Power (kilowatts)',
          main = 'Global Active Power')
     
     dev.copy(png, 'plot1.png', height = 480, width = 480)
     dev.off()
```


<figure>
    <a href="/images/eda1/plot1.png"><img src="/assets/img/project/eda-1/output_14_2.png"></a>
</figure>    


### Plot 2


```R
# Construct plot and save it to PNG file
     plot(data$DateTime,
          data$Global_active_power,
          type = 'l',
          ylab = 'Global Active Power (kilowatts)', 
          xlab = "")
     
     dev.copy(png, 'plot2.png', width = 480, height = 480)
     dev.off()
```


<figure>
    <a href="/images/eda1/plot2.png"><img src="/assets/img/project/eda-1/output_16_2.png"></a>
</figure>      


### Plot 3


```R
# Construct plot and save it to PNG file
     plot(data$DateTime, 
          data$Sub_metering_1,
          type = 'l', xlab = '',
          ylab = 'Energy sub metering')
     points(data$DateTime, 
            data$Sub_metering_2, 
            col = 'red', type = 'l')
     points(data$DateTime,
            data$Sub_metering_3,
            col = 'blue', type = 'l')
     legend('topright',c('Sub_metering_1', 'Sub_metering_2', 'Sub_metering_3'),
            col = c('black', 'red', ' blue'),
            lty = 1, lwd = 2, cex = 0.9)
     
     dev.copy(png, 'plot3.png', width = 480, height = 480)
     dev.off()
```


<figure>
    <a href="/images/eda1/plot3.png"><img src="/assets/img/project/eda-1/output_18_2.png"></a>
</figure>      


### Plot 4


```R
# Construct plot and save it to PNG file
     par(mfrow = c(2,2), mar = c(4,4,1,1))
     
     with(data, plot(DateTime, Global_active_power, type = 'l',
                        xlab = '', ylab = 'Global Active Power'))
     
     with(data, plot(DateTime, Voltage, type = 'l',
                        xlab = 'datetime',
                        ylab = 'Voltage'))
     
     with(data,{
          plot(DateTime, Sub_metering_1, type = 'l',
               xlab = '', ylab = 'Energy sub metering')
          lines(DateTime, Sub_metering_2, col = 'red')
          lines(DateTime, Sub_metering_3, col = 'blue')
          legend('topright', c('Sub_metering_1', 'Sub_metering_2', 'Sub_metering_3'),
                 col = c('black', 'red','blue'), lty = 1, lwd = 2, cex = 0.9)})
     
     with(data,
          plot(DateTime, Global_reactive_power,
               type = 'l', xlab = 'datetime',
               ylab = 'Global_reactive_power'))
     
     dev.copy(png, 'plot4.png', width = 480, height = 480)
     dev.off()
```


<figure>
    <a href="/images/eda1/plot4.png"><img src="/assets/img/project/eda-1/output_20_2.png"></a>
</figure>    

