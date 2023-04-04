---
layout: post
title: "Human Activity Data Set"
date: 2020-09-26
excerpt: "Coursera: Getting and Cleaning Data, Course Project. Doing data cleaning in the data set of human activity that are recorded using smartphone. Merge train-test data set and changed the labels to be more descriptive."
tags: [r programming, data cleaning]
project: true
comments: true
---

## Synopsis

### Data

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data.

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. 

A full description is available at the site where the data was obtained: [Here](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones)

The purpose of this project is to demonstrating how to collect, load, and clean a data set. The goal is to prepare tidy data that can be processed easily for later analysis.
- Data set [[60MB](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip)]

### Processing Steps

1. Collecting the data.
2. Merges training and test data into one data set.
3. Extract the measurement by mean and standard deviations.
4. Change to descriptive activities label.
5. Change column name to descriptive labels.
6. Create independent data set with the average of each variable for each activity and each subject.

## 1. Collecting the Data


```R
# Download the Data set
    fileurl <- 'https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip'
    download.file(fileurl, destfile = 'projectdataset.zip')
      
# Unzip the Data set
    unzip('./projectdataset.zip')
```


```R
## 0 Start to reading files

    # Read training data
    activity.train <- read.table('./UCI HAR Dataset/train/y_train.txt', header = F)
    feature.train <- read.table('./UCI HAR Dataset/train/X_train.txt', header = F)
    subject.train <- read.table('./UCI HAR Dataset/train/subject_train.txt', header = F)

    # Read test data
    activity.test <- read.table('./UCI HAR Dataset/test/y_test.txt', header = F)
    feature.test <- read.table('./UCI HAR Dataset/test/X_test.txt', header = F)
    subject.test <- read.table('./UCI HAR Dataset/test/subject_test.txt', header = F)

    # Read activity labels
    activity.label <- read.table('./UCI HAR Dataset/activity_labels.txt', header = F)

    # Read feature names
    feature.names <- read.table('./UCI HAR Dataset/features.txt', header = F)
```

## 2. Merge Training and Test Data into One Data Set


```R
# 1.1 Assigning variable names
    names(activity.train) <- 'Activity'
    names(feature.train) <- feature.names[,2]
    names(subject.train) <- 'Subject'

    names(activity.test) <- 'Activity'
    names(feature.test) <- feature.names[,2]
    names(subject.test) <- 'Subject'

    names(activity.label) <- c('Activity', 'ActivityType')

# 1.2 Merge all data frame into one set
    train <- cbind(subject.train, activity.train, feature.train)
    test <- cbind(subject.test, activity.test, feature.test)
    data <- rbind(train, test)
```


```R
print(paste("Observation: ", nrow(data),"Column: ", ncol(data)))
head(data[1:10])
```

    [1] "Observation:  10299 Column:  563"
    


<table>
<thead><tr><th scope=col>Subject</th><th scope=col>Activity</th><th scope=col>tBodyAcc-mean()-X</th><th scope=col>tBodyAcc-mean()-Y</th><th scope=col>tBodyAcc-mean()-Z</th><th scope=col>tBodyAcc-std()-X</th><th scope=col>tBodyAcc-std()-Y</th><th scope=col>tBodyAcc-std()-Z</th><th scope=col>tBodyAcc-mad()-X</th><th scope=col>tBodyAcc-mad()-Y</th></tr></thead>
<tbody>
	<tr><td>1          </td><td>5          </td><td>0.2885845  </td><td>-0.02029417</td><td>-0.1329051 </td><td>-0.9952786 </td><td>-0.9831106 </td><td>-0.9135264 </td><td>-0.9951121 </td><td>-0.9831846 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2784188  </td><td>-0.01641057</td><td>-0.1235202 </td><td>-0.9982453 </td><td>-0.9753002 </td><td>-0.9603220 </td><td>-0.9988072 </td><td>-0.9749144 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2796531  </td><td>-0.01946716</td><td>-0.1134617 </td><td>-0.9953796 </td><td>-0.9671870 </td><td>-0.9789440 </td><td>-0.9965199 </td><td>-0.9636684 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2791739  </td><td>-0.02620065</td><td>-0.1232826 </td><td>-0.9960915 </td><td>-0.9834027 </td><td>-0.9906751 </td><td>-0.9970995 </td><td>-0.9827498 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2766288  </td><td>-0.01656965</td><td>-0.1153619 </td><td>-0.9981386 </td><td>-0.9808173 </td><td>-0.9904816 </td><td>-0.9983211 </td><td>-0.9796719 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2771988  </td><td>-0.01009785</td><td>-0.1051373 </td><td>-0.9973350 </td><td>-0.9904868 </td><td>-0.9954200 </td><td>-0.9976274 </td><td>-0.9902177 </td></tr>
</tbody>
</table>



## 3. Extract the Measurement by Mean and Standard Deviations


```R
    subset.feature <- feature.names$V2[grep("mean\\(\\)|std\\(\\)",feature.names$V2)]
    subset.data <- c('Subject', 'Activity', as.character(subset.feature))
    data <- subset(data, select = subset.data)
```


```R
print(paste("Observation: ", nrow(data),"Column: ", ncol(data)))
head(data[1:10])
```

    [1] "Observation:  10299 Column:  68"
    


<table>
<thead><tr><th scope=col>Subject</th><th scope=col>Activity</th><th scope=col>tBodyAcc-mean()-X</th><th scope=col>tBodyAcc-mean()-Y</th><th scope=col>tBodyAcc-mean()-Z</th><th scope=col>tBodyAcc-std()-X</th><th scope=col>tBodyAcc-std()-Y</th><th scope=col>tBodyAcc-std()-Z</th><th scope=col>tGravityAcc-mean()-X</th><th scope=col>tGravityAcc-mean()-Y</th></tr></thead>
<tbody>
	<tr><td>1          </td><td>5          </td><td>0.2885845  </td><td>-0.02029417</td><td>-0.1329051 </td><td>-0.9952786 </td><td>-0.9831106 </td><td>-0.9135264 </td><td>0.9633961  </td><td>-0.1408397 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2784188  </td><td>-0.01641057</td><td>-0.1235202 </td><td>-0.9982453 </td><td>-0.9753002 </td><td>-0.9603220 </td><td>0.9665611  </td><td>-0.1415513 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2796531  </td><td>-0.01946716</td><td>-0.1134617 </td><td>-0.9953796 </td><td>-0.9671870 </td><td>-0.9789440 </td><td>0.9668781  </td><td>-0.1420098 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2791739  </td><td>-0.02620065</td><td>-0.1232826 </td><td>-0.9960915 </td><td>-0.9834027 </td><td>-0.9906751 </td><td>0.9676152  </td><td>-0.1439765 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2766288  </td><td>-0.01656965</td><td>-0.1153619 </td><td>-0.9981386 </td><td>-0.9808173 </td><td>-0.9904816 </td><td>0.9682244  </td><td>-0.1487502 </td></tr>
	<tr><td>1          </td><td>5          </td><td>0.2771988  </td><td>-0.01009785</td><td>-0.1051373 </td><td>-0.9973350 </td><td>-0.9904868 </td><td>-0.9954200 </td><td>0.9679482  </td><td>-0.1482100 </td></tr>
</tbody>
</table>



## 4. Change to Descriptive Activities Labels


```R
    for (x in 1:6) {data$Activity [(as.character(data$Activity) == x)] <- as.character(activity.label[x,2])
    }
```


```R
print(paste("Observation: ", nrow(data),"Column: ", ncol(data)))
head(data[1:10])
```

    [1] "Observation:  10299 Column:  68"
    


<table>
<thead><tr><th scope=col>Subject</th><th scope=col>Activity</th><th scope=col>tBodyAcc-mean()-X</th><th scope=col>tBodyAcc-mean()-Y</th><th scope=col>tBodyAcc-mean()-Z</th><th scope=col>tBodyAcc-std()-X</th><th scope=col>tBodyAcc-std()-Y</th><th scope=col>tBodyAcc-std()-Z</th><th scope=col>tGravityAcc-mean()-X</th><th scope=col>tGravityAcc-mean()-Y</th></tr></thead>
<tbody>
	<tr><td>1          </td><td>STANDING   </td><td>0.2885845  </td><td>-0.02029417</td><td>-0.1329051 </td><td>-0.9952786 </td><td>-0.9831106 </td><td>-0.9135264 </td><td>0.9633961  </td><td>-0.1408397 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2784188  </td><td>-0.01641057</td><td>-0.1235202 </td><td>-0.9982453 </td><td>-0.9753002 </td><td>-0.9603220 </td><td>0.9665611  </td><td>-0.1415513 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2796531  </td><td>-0.01946716</td><td>-0.1134617 </td><td>-0.9953796 </td><td>-0.9671870 </td><td>-0.9789440 </td><td>0.9668781  </td><td>-0.1420098 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2791739  </td><td>-0.02620065</td><td>-0.1232826 </td><td>-0.9960915 </td><td>-0.9834027 </td><td>-0.9906751 </td><td>0.9676152  </td><td>-0.1439765 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2766288  </td><td>-0.01656965</td><td>-0.1153619 </td><td>-0.9981386 </td><td>-0.9808173 </td><td>-0.9904816 </td><td>0.9682244  </td><td>-0.1487502 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2771988  </td><td>-0.01009785</td><td>-0.1051373 </td><td>-0.9973350 </td><td>-0.9904868 </td><td>-0.9954200 </td><td>0.9679482  </td><td>-0.1482100 </td></tr>
</tbody>
</table>



## 5. Change Column Names to Descriptive Labels


```R
    names(data) <- gsub('^t','Time', names(data))
    names(data) <- gsub('^f', 'Frequency', names(data))
    names(data) <- gsub('Acc', 'Accelerometer', names(data))
    names(data) <- gsub('BodyBody', 'Body', names(data))
    names(data) <- gsub('Gyro', 'Gyroscope', names(data))
    names(data) <- gsub('Mag', 'Magnitude', names(data))
```


```R
print(paste("Observation: ", nrow(data),"Column: ", ncol(data)))
head(data[1:10])
```

    [1] "Observation:  10299 Column:  68"
    


<table>
<thead><tr><th scope=col>Subject</th><th scope=col>Activity</th><th scope=col>TimeBodyAccelerometer-mean()-X</th><th scope=col>TimeBodyAccelerometer-mean()-Y</th><th scope=col>TimeBodyAccelerometer-mean()-Z</th><th scope=col>TimeBodyAccelerometer-std()-X</th><th scope=col>TimeBodyAccelerometer-std()-Y</th><th scope=col>TimeBodyAccelerometer-std()-Z</th><th scope=col>TimeGravityAccelerometer-mean()-X</th><th scope=col>TimeGravityAccelerometer-mean()-Y</th></tr></thead>
<tbody>
	<tr><td>1          </td><td>STANDING   </td><td>0.2885845  </td><td>-0.02029417</td><td>-0.1329051 </td><td>-0.9952786 </td><td>-0.9831106 </td><td>-0.9135264 </td><td>0.9633961  </td><td>-0.1408397 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2784188  </td><td>-0.01641057</td><td>-0.1235202 </td><td>-0.9982453 </td><td>-0.9753002 </td><td>-0.9603220 </td><td>0.9665611  </td><td>-0.1415513 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2796531  </td><td>-0.01946716</td><td>-0.1134617 </td><td>-0.9953796 </td><td>-0.9671870 </td><td>-0.9789440 </td><td>0.9668781  </td><td>-0.1420098 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2791739  </td><td>-0.02620065</td><td>-0.1232826 </td><td>-0.9960915 </td><td>-0.9834027 </td><td>-0.9906751 </td><td>0.9676152  </td><td>-0.1439765 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2766288  </td><td>-0.01656965</td><td>-0.1153619 </td><td>-0.9981386 </td><td>-0.9808173 </td><td>-0.9904816 </td><td>0.9682244  </td><td>-0.1487502 </td></tr>
	<tr><td>1          </td><td>STANDING   </td><td>0.2771988  </td><td>-0.01009785</td><td>-0.1051373 </td><td>-0.9973350 </td><td>-0.9904868 </td><td>-0.9954200 </td><td>0.9679482  </td><td>-0.1482100 </td></tr>
</tbody>
</table>



## 6. Create Independent Data Set with the Average of Each Variable for Each Activity and Each Subject


```R
# Creates a second independent data set
    data2 <- aggregate(.~ Subject + Activity, data, mean)
    data2 <- data2[order(data2$Subject,data2$Activity), ]
```


```R
print(paste("Observation: ", nrow(data2),"Column: ", ncol(data2)))
head(data2[1:10])
```

    [1] "Observation:  180 Column:  68"
    


<table>
<thead><tr><th></th><th scope=col>Subject</th><th scope=col>Activity</th><th scope=col>TimeBodyAccelerometer-mean()-X</th><th scope=col>TimeBodyAccelerometer-mean()-Y</th><th scope=col>TimeBodyAccelerometer-mean()-Z</th><th scope=col>TimeBodyAccelerometer-std()-X</th><th scope=col>TimeBodyAccelerometer-std()-Y</th><th scope=col>TimeBodyAccelerometer-std()-Z</th><th scope=col>TimeGravityAccelerometer-mean()-X</th><th scope=col>TimeGravityAccelerometer-mean()-Y</th></tr></thead>
<tbody>
	<tr><th scope=row>1</th><td>1                 </td><td>LAYING            </td><td>0.2215982         </td><td>-0.040513953      </td><td>-0.1132036        </td><td>-0.92805647       </td><td>-0.836827406      </td><td>-0.82606140       </td><td>-0.2488818        </td><td> 0.7055498        </td></tr>
	<tr><th scope=row>31</th><td>1                 </td><td>SITTING           </td><td>0.2612376         </td><td>-0.001308288      </td><td>-0.1045442        </td><td>-0.97722901       </td><td>-0.922618642      </td><td>-0.93958629       </td><td> 0.8315099        </td><td> 0.2044116        </td></tr>
	<tr><th scope=row>61</th><td>1                 </td><td>STANDING          </td><td>0.2789176         </td><td>-0.016137590      </td><td>-0.1106018        </td><td>-0.99575990       </td><td>-0.973190056      </td><td>-0.97977588       </td><td> 0.9429520        </td><td>-0.2729838        </td></tr>
	<tr><th scope=row>91</th><td>1                 </td><td>WALKING           </td><td>0.2773308         </td><td>-0.017383819      </td><td>-0.1111481        </td><td>-0.28374026       </td><td> 0.114461337      </td><td>-0.26002790       </td><td> 0.9352232        </td><td>-0.2821650        </td></tr>
	<tr><th scope=row>121</th><td>1                 </td><td>WALKING_DOWNSTAIRS</td><td>0.2891883         </td><td>-0.009918505      </td><td>-0.1075662        </td><td> 0.03003534       </td><td>-0.031935943      </td><td>-0.23043421       </td><td> 0.9318744        </td><td>-0.2666103        </td></tr>
	<tr><th scope=row>151</th><td>1                 </td><td>WALKING_UPSTAIRS  </td><td>0.2554617         </td><td>-0.023953149      </td><td>-0.0973020        </td><td>-0.35470803       </td><td>-0.002320265      </td><td>-0.01947924       </td><td> 0.8933511        </td><td>-0.3621534        </td></tr>
</tbody>
</table>




```R
# Create txt file from this tidy data
    write.table(data2, file = 'TidyData.txt', row.names = F)
      
# E N D
```
