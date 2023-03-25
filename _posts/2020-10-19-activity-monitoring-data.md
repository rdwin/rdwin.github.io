---
layout: post
title: "Activity Monitoring Data"
date: 2020-10-19
excerpt: "Coursera: Reproducible Research, Course Project 1. Doing Exploratory Data Analysis in personal movement using activity monitoring device."
tags: [r programming, data visualization]
project: true
comments: true
---

## Synopsis

### Data

It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a [Fitbit](http://www.fitbit.com/), [Nike Fuelband](http://www.nike.com/us/en_us/c/nikeplus-fuelband), or [Jawbone Up](https://jawbone.com/up). These type of devices are part of the “quantified self” movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.

The data for this assignment can be downloaded from the course web site:

- Dataset: Activity monitoring data [[52K]](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip)

The variables included in this dataset are:

- **steps**: Number of steps taking in a 5-minute interval (missing values are coded as `NA`)

- **date**: The date on which the measurement was taken in YYYY-MM-DD format

- **interval**: Identifier for the 5-minute interval in which measurement was taken

The dataset is stored in a comma-separated-value (CSV) file and there are a total of 17,568 observations in this dataset.

### Processing Step and Questions

- Loading and preprocessing the data
- What is mean total number of steps taken per day?
- What is the average daily activity pattern?
- Imputing missing values
- Are there differences in activity patterns between weekdays and weekends?

## Loading and preprocessing the data


```R
# Download the Data set
    fileurl <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
    download.file(fileurl, destfile = 'activity.zip')

# unziping the data set
    unzip('activity.zip', exdir = '.')
```


```R
# loading the data set
    data <- read.csv('activity.csv', header = T)
```


```R
# Show first 10 rows in the data set
    head(data,10)
```


<table>
<thead><tr><th scope=col>steps</th><th scope=col>date</th><th scope=col>interval</th></tr></thead>
<tbody>
	<tr><td>NA        </td><td>2012-10-01</td><td> 0        </td></tr>
	<tr><td>NA        </td><td>2012-10-01</td><td> 5        </td></tr>
	<tr><td>NA        </td><td>2012-10-01</td><td>10        </td></tr>
	<tr><td>NA        </td><td>2012-10-01</td><td>15        </td></tr>
	<tr><td>NA        </td><td>2012-10-01</td><td>20        </td></tr>
	<tr><td>NA        </td><td>2012-10-01</td><td>25        </td></tr>
	<tr><td>NA        </td><td>2012-10-01</td><td>30        </td></tr>
	<tr><td>NA        </td><td>2012-10-01</td><td>35        </td></tr>
	<tr><td>NA        </td><td>2012-10-01</td><td>40        </td></tr>
	<tr><td>NA        </td><td>2012-10-01</td><td>45        </td></tr>
</tbody>
</table>



## What is mean total number of steps taken per day?


```R
# Aggregating the total number of steps taken per day
    stepByDay <- aggregate(steps ~ date, data = data, sum)
```

**Making a histogram of the total number of steps:**


```R
hist(stepByDay$steps,
     xlab = 'Total Steps',
     main = 'Histogram of the Steps per Day',
     col = c(1,2,3,4,5))
```

<figure>
    <a href="/images/rep1/plot1.png"><img src="/assets/img/project/rep-1/output_13_0.png"></a>
</figure>     


**Calculating  the mean and median of the total number of steps taken per day**


```R
# calculate mean
    round(mean(stepByDay$steps, na.rm = T),2)
```


10766.19



```R
# calculate median
    median(stepByDay$steps, na.rm = T)
```


10765


## What is the average daily activity pattern?

**Make a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)**


```R
# Aggregating the data set
    avgStepByInterval <- aggregate(steps ~ interval,
                         data = data,
                         mean,
                         na.rm = T)
```


```R
# Create time series plot
    with(avgStepByInterval,
         plot(interval,
              steps,
              type = 'l',
              main = 'Average Number of Steps per Interval',
              xlab = 'Interval',
              ylab = 'Average Number of Steps'))
```

<figure>
    <a href="/images/rep1/plot2.png"><img src="/assets/img/project/rep-1/output_20_0.png"></a>
</figure>      


**5-minute interval contains the maximum number of steps on average across all the days in the data set**


```R
# Identify maximum number of steps
    maxAverage <- max(avgStepByInterval$steps)

# Show the interval on maximum number of steps
    avgStepByInterval[avgStepByInterval$steps==maxAverage, 1]
```


835


## Imputing missing values

**Calculate and report the total number of missing values in the dat aset (i.e. the total number of rows with (NAs)**


```R
# Identify the total number of missing value
    missingValue <- sum(is.na(data$steps))

    missingValue
```

2304


**Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.**
**Create a new dataset that is equal to the original dataset but with the missing data filled in.**
**I'm choosing to use mean of the Total Number Steps to replace NA Value**


```R
# Create new data set from original data
    newData <- data

# Replace missing value with average
    newData$steps[is.na(newData$steps)] <- mean(data$steps, na.rm = T)
```


```R
# Show the changes in first 10 rows of the new data set
    head(newData,10)
```


<table>
<thead><tr><th scope=col>steps</th><th scope=col>date</th><th scope=col>interval</th></tr></thead>
<tbody>
	<tr><td>37.3826   </td><td>2012-10-01</td><td> 0        </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td> 5        </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>10        </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>15        </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>20        </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>25        </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>30        </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>35        </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>40        </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>45        </td></tr>
</tbody>
</table>



**Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day.**


```R
# Aggregating the total number of steps taken per day
    stepByDay <- aggregate(steps ~ date, data = newData, sum)
```

**Making a histogram of the total number of steps:**


```R
hist(stepByDay$steps,
     xlab = 'Total Steps',
     main = 'Histogram of the Steps per Day',
     col = c(1,2,3,4,5))
```

<figure>
    <a href="/images/rep1/plot3.png"><img src="/assets/img/project/rep-1/output_32_0.png"></a>
</figure>     


```R
# calculate mean
    round(mean(stepByDay$steps),2)
```


10766.19



```R
# calculate median
    round(median(stepByDay$steps),2)
```


10766.19


## Are there differences in activity patterns between weekdays and weekends?

**Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day**


```R
# Create new column dayType to indicating whether it's weekday or weekend day.
    # Detemine what day in given date
    newData$dayType <- weekdays(as.Date(newData$date))

    # Replace it with two level - "weekday" and "weekend"
    newData$dayType <- ifelse(newData$dayType %in% c('Saturday','Sunday'),'Weekends','Weekdays')
```


```R
# Show the changes in first 10 rows of the new data set
    head(newData,10)
```


<table>
<thead><tr><th scope=col>steps</th><th scope=col>date</th><th scope=col>interval</th><th scope=col>dayType</th></tr></thead>
<tbody>
	<tr><td>37.3826   </td><td>2012-10-01</td><td> 0        </td><td>Weekdays  </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td> 5        </td><td>Weekdays  </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>10        </td><td>Weekdays  </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>15        </td><td>Weekdays  </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>20        </td><td>Weekdays  </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>25        </td><td>Weekdays  </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>30        </td><td>Weekdays  </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>35        </td><td>Weekdays  </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>40        </td><td>Weekdays  </td></tr>
	<tr><td>37.3826   </td><td>2012-10-01</td><td>45        </td><td>Weekdays  </td></tr>
</tbody>
</table>



**Make a panel plot containing a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis)**


```R
# Aggregating the total number of steps taken per day
    stepByDayType <- aggregate(newData['steps'], newData[c('interval','dayType')], mean)
```


```R
# Import Necessary library
    library(lattice)

# Make time series plot
    xyplot(steps ~ interval | dayType, 
           data = stepByDayType,
           type = 'l',
           layout = c(1,2), 
           main="Average Steps per Interval by Type of Day", 
           ylab="Average Number of Steps", 
           xlab="Interval")
```

<figure>
    <a href="/images/rep1/plot4.png"><img src="/assets/img/project/rep-1/output_41_0.png"></a>
</figure>       

