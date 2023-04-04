---
layout: post
title: "Data Cleaning Challenge"
date: 2021-10-23
excerpt: "More or less are just an exercise to cleaning and manipulating random dataset."
tags: [r programming, data cleaning]
project: true
comments: true
---

## Synopsis

### Data

It's been a while since last time I do exercise in data analysis. In this exercise, I'm going to do data cleaning to a dataset that I got from one of Shashank Kalanithi's video tutorial, someone how share how to do data analytics on youtube. This dataset is an anonymized version of a dataset He received from a client. To be honest, I don't really know much about the dataset, so I'm just do what he does in different programming language. For any further information about the dataset, you can click link url below.

- Data source [[2.7MB]](https://drive.google.com/drive/folders/1EiiZfKBOgCtibdSAMfD42NnA_l0PqL_q)
- [Video Link](https://www.youtube.com/watch?v=sSnbmbRmtSA)
- [Youtube Channel](https://www.youtube.com/@ShashankData)

### Processing Steps

1. Loading and Do Pre-processing to the Dataset
2. Making a new column for ID
2. Subsetting the Dataset
3. Merging all Subsetted data
4. Export the output
6. Additional Steps

## 1. Loading the Dataset


```R
# import data
    data_import <- read.csv("data_cleaning_challenge.csv")
```


```R
# Show a portion of the dataset
    head(data_import,12)
```


<table>
<thead><tr><th scope=col>Row.Type</th><th scope=col>Iter.Number</th><th scope=col>Power1</th><th scope=col>Speed1</th><th scope=col>Speed2</th><th scope=col>Electricity</th><th scope=col>Effort</th><th scope=col>Weight</th><th scope=col>Torque</th><th scope=col>X</th><th scope=col>X.1</th></tr></thead>
<tbody>
	<tr><td>first name: Person</td><td>last name: Human  </td><td>date: end of time </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>NA                </td><td>                  </td></tr>
	<tr><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>                  </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Row Type          </td><td>Iter Number       </td><td>Power1            </td><td>Speed1            </td><td>Speed2            </td><td>Electricity       </td><td>Effort            </td><td>Weight            </td><td>Torque            </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Iter              </td><td>1                 </td><td>360               </td><td>108               </td><td>863               </td><td>599               </td><td>680               </td><td>442               </td><td>982               </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Iter              </td><td>2                 </td><td>684               </td><td>508               </td><td>613               </td><td>241               </td><td>249               </td><td>758               </td><td>639               </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Iter              </td><td>3                 </td><td>365               </td><td>126               </td><td>825               </td><td>407               </td><td>855               </td><td>164               </td><td>86                </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Iter              </td><td>4                 </td><td>764               </td><td>594               </td><td>304               </td><td>718               </td><td>278               </td><td>674               </td><td>774               </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Iter              </td><td>5                 </td><td>487               </td><td>97                </td><td>593               </td><td>206               </td><td>779               </td><td>800               </td><td>123               </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Average           </td><td>182               </td><td>361               </td><td>741               </td><td>231               </td><td>731               </td><td>493               </td><td>847               </td><td>237               </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Maximum           </td><td>276               </td><td>33                </td><td>97                </td><td>154               </td><td>25                </td><td>922               </td><td>9                 </td><td>312               </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Std.Dev.          </td><td>523               </td><td>1000              </td><td>34                </td><td>904               </td><td>237               </td><td>600               </td><td>170               </td><td>553               </td><td>NA                </td><td>                  </td></tr>
	<tr><td>Total             </td><td>336               </td><td>-                 </td><td>-                 </td><td>-                 </td><td>-                 </td><td>977               </td><td>744               </td><td>652               </td><td>NA                </td><td>                  </td></tr>
</tbody>
</table>




```R
# Check the description of the dataset
    str(data_import)
```

    'data.frame':	76377 obs. of  11 variables:
     $ Row.Type   : Factor w/ 8 levels "","Average","first name: Person",..: 3 1 6 4 4 4 4 4 2 5 ...
     $ Iter.Number: Factor w/ 1003 levels "","1","10","100",..: 1003 1 1002 2 114 225 336 447 95 199 ...
     $ Power1     : Factor w/ 1004 levels "","-","1","10",..: 1003 1 1004 294 653 299 742 434 295 260 ...
     $ Speed1     : Factor w/ 1003 levels "","-","1","10",..: 1 1 1003 14 458 34 553 970 717 970 ...
     $ Speed2     : Factor w/ 1003 levels "","-","1","10",..: 1 1 1003 852 575 810 232 552 151 65 ...
     $ Electricity: Factor w/ 1003 levels "","-","1","10",..: 1 1 1003 558 162 346 691 123 706 171 ...
     $ Effort     : Factor w/ 1002 levels "","1","10","100",..: 1 1 1002 648 169 842 201 757 440 917 ...
     $ Weight     : Factor w/ 1002 levels "","1","10","100",..: 1 1 1002 384 734 75 641 782 833 891 ...
     $ Torque     : Factor w/ 1002 levels "","1","10","100",..: 1 1 1002 983 602 847 752 30 156 240 ...
     $ X          : logi  NA NA NA NA NA NA ...
     $ X.1        : Factor w/ 7 levels "","notes: 2BLOCK",..: 1 1 1 1 1 1 1 1 1 1 ...
    

As you can see, variable `X` and `X.1` doesn't really give us any information. So, I'm not going to use it.


```R
# Subsetting the dataset
    data_import <- data_import[1:9]
```

## 2. Making a New Column for Person Index

The purpose of this step was to assign an ID to each person in the dataset. With these ID, we can differentiate each record and iteration for each person.


```R
# Make a function for iterating person index
    iteration <- function(data){
        Iteration <- c()
        itr <- 0
        for (i in 1:nrow(data)){
            if (grepl("first name:", data$Row.Type[i]) == T){
                itr <- itr + 1
                Iteration <- c(Iteration, itr)
            } else{
                Iteration <- c(Iteration, Iteration[i-1])
            }}
        data <- cbind(data, Iteration)
        return(data)
    }
    
# add the iteration and overwrite the dataset
    data_import <- iteration(data_import)
```


```R
# Check the total of person in the dataset
    max(data_import$Iteration)
```


5994


## 3. Subsetting the Dataset


```R
# subset rows with first name, last name, and date.
    data_name <- subset(data_import, grepl("first name:", data_import$Row.Type), select = c(Row.Type, Iter.Number, Power1, Iteration))
    colnames(data_name) <- c("First.Name", "Last.Name", "Date", "Iteration")
```


```R
# Cleaning data in data_name, deleting unnecessary string like 'first name:', 'last name:' and 'date:'
    data_name$First.Name <- gsub("first name:", "", data_name$First.Name)
    data_name$Last.Name <- gsub("last name:", "", data_name$Last.Name)
    data_name$Date <- gsub("date: ", "", data_name$Date)
```


```R
# subset rows without name, column name and missing value
    no_name <- !(grepl("first name:", data_import$Row.Type)|
                   grepl('Row Type', data_import$Row.Type))
    data_no_name <- subset(data_import, no_name)
    data_no_name <- data_no_name[!(data_no_name$Row.Type == ""|
                                     is.na(data_no_name$Row.Type)),]
```

## 4. Merge the Dataset into One Object


```R
## Join data
    output_data <- merge(x = data_name, y = data_no_name, by.x = "Iteration")
```


```R
head(output_data,12)
```


<table>
<thead><tr><th scope=col>Iteration</th><th scope=col>First.Name</th><th scope=col>Last.Name</th><th scope=col>Date</th><th scope=col>Row.Type</th><th scope=col>Iter.Number</th><th scope=col>Power1</th><th scope=col>Speed1</th><th scope=col>Speed2</th><th scope=col>Electricity</th><th scope=col>Effort</th><th scope=col>Weight</th><th scope=col>Torque</th><th scope=col>Iteration.1</th></tr></thead>
<tbody>
	<tr><td>1          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Iter       </td><td>1          </td><td>360        </td><td>108        </td><td>863        </td><td>599        </td><td>680        </td><td>442        </td><td>982        </td><td>1          </td></tr>
	<tr><td>1          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Iter       </td><td>2          </td><td>684        </td><td>508        </td><td>613        </td><td>241        </td><td>249        </td><td>758        </td><td>639        </td><td>1          </td></tr>
	<tr><td>1          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Iter       </td><td>3          </td><td>365        </td><td>126        </td><td>825        </td><td>407        </td><td>855        </td><td>164        </td><td>86         </td><td>1          </td></tr>
	<tr><td>1          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Iter       </td><td>4          </td><td>764        </td><td>594        </td><td>304        </td><td>718        </td><td>278        </td><td>674        </td><td>774        </td><td>1          </td></tr>
	<tr><td>1          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Iter       </td><td>5          </td><td>487        </td><td>97         </td><td>593        </td><td>206        </td><td>779        </td><td>800        </td><td>123        </td><td>1          </td></tr>
	<tr><td>1          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Average    </td><td>182        </td><td>361        </td><td>741        </td><td>231        </td><td>731        </td><td>493        </td><td>847        </td><td>237        </td><td>1          </td></tr>
	<tr><td>1          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Maximum    </td><td>276        </td><td>33         </td><td>97         </td><td>154        </td><td>25         </td><td>922        </td><td>9          </td><td>312        </td><td>1          </td></tr>
	<tr><td>1          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Std.Dev.   </td><td>523        </td><td>1000       </td><td>34         </td><td>904        </td><td>237        </td><td>600        </td><td>170        </td><td>553        </td><td>1          </td></tr>
	<tr><td>1          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Total      </td><td>336        </td><td>-          </td><td>-          </td><td>-          </td><td>-          </td><td>977        </td><td>744        </td><td>652        </td><td>1          </td></tr>
	<tr><td>2          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Iter       </td><td>1          </td><td>702        </td><td>494        </td><td>311        </td><td>492        </td><td>456        </td><td>370        </td><td>150        </td><td>2          </td></tr>
	<tr><td>2          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Iter       </td><td>2          </td><td>929        </td><td>82         </td><td>838        </td><td>421        </td><td>154        </td><td>346        </td><td>227        </td><td>2          </td></tr>
	<tr><td>2          </td><td> Person    </td><td> Human     </td><td>end of time</td><td>Iter       </td><td>3          </td><td>763        </td><td>402        </td><td>344        </td><td>951        </td><td>139        </td><td>295        </td><td>285        </td><td>2          </td></tr>
</tbody>
</table>




```R
dim(output_data)
```


46409

14



## 5. Export the Output


```R
# Export Output Data
    writexl::write_xlsx(output_data, "cleaned.xlsx")
```

## 6. Additional Steps

Now that I reviewed my previous code and throroughly examine the dataset, I found that all numbers of this dataset are just a random number from 1-999. Although this dataset give us random numbers, this dataset can provide us a good dataset layout for solving a data cleaning problem.

Steps number 5 was the end of processing step Shashank Kalanithi showed in his video. By looking at the output that being showed in step 4, we can see that the output table isn't entirely clean and tidy. While it's true that the dataset was way a lot cleaner by looking for its `observation`, I found a lot of repetitive value in the output. So, in this **Additional Step** I want to add a little change to the output based on my own preference.


```R
# Create a duplicate of output dataset
    data2 <- output_data
```


```R
# Replace '-' value in total to NA and change column type into a correct one
    library(dplyr)

    data2 <- data2 %>% mutate(Power1 = as.numeric(gsub("-", NA, Power1)), 
                              Speed1 = as.numeric(gsub("-", NA, Speed1)), 
                              Speed2 = as.numeric(gsub("-", NA, Speed2)), 
                              Electricity = as.numeric(gsub("-", NA, Electricity)),
                              Iter.Number = as.numeric(as.character(Iter.Number)),
                              Effort = as.numeric(as.character(Effort)),
                              Weight = as.numeric(as.character(Weight)),
                              Torque = as.numeric(as.character(Torque)),
                              Row.Type = as.character(Row.Type)
                             )
```


```R
# Create a function to extract and transform the main data to a tidier table
    Exportdata <- function(data, col, row, iter){
        if(row == "Iter"){
            temp <- with(data, data.frame(paste(Power1[Row.Type == row & Iteration == iter], collapse = ", "),
                                          paste(Speed1[Row.Type == row & Iteration == iter], collapse = ", "),
                                          paste(Speed2[Row.Type == row & Iteration == iter], collapse = ", "),
                                          paste(Electricity[Row.Type == row & Iteration == iter], collapse = ", "),
                                          paste(Effort[Row.Type == row & Iteration == iter], collapse = ", "),
                                          paste(Weight[Row.Type == row & Iteration == iter], collapse = ", "),
                                          paste(Torque[Row.Type == row & Iteration == iter], collapse = ", "),
                                          stringsAsFactors = FALSE))
        } else {
            temp <- with(data, data.frame(Power1[Row.Type == row & Iteration == iter],
                                        Speed1[Row.Type == row & Iteration == iter],
                                        Speed2[Row.Type == row & Iteration == iter],
                                        Electricity[Row.Type == row & Iteration == iter],
                                        Effort[Row.Type == row & Iteration == iter],
                                        Weight[Row.Type == row & Iteration == iter],
                                        Torque[Row.Type == row & Iteration == iter],
                                        stringsAsFactors = FALSE))
        }
        colnames(temp) <- col
        return(temp)
    }

# Check if the new function is working
    Exportdata(data = data2, col = c("Power1",'Speed1','Speed2', 'Electricity', 'Effort', 'Weight', 'Torque'), row = "Iter", iter = 1)
```


<table>
<thead><tr><th scope=col>Power1</th><th scope=col>Speed1</th><th scope=col>Speed2</th><th scope=col>Electricity</th><th scope=col>Effort</th><th scope=col>Weight</th><th scope=col>Torque</th></tr></thead>
<tbody>
	<tr><td>360, 684, 365, 764, 487</td><td>108, 508, 126, 594, 97 </td><td>863, 613, 825, 304, 593</td><td>599, 241, 407, 718, 206</td><td>680, 249, 855, 278, 779</td><td>442, 758, 164, 674, 800</td><td>982, 639, 86, 774, 123 </td></tr>
</tbody>
</table>




```R
# Transform all data into a new dataset
    outputIteration <- data.frame()
    for (i in 1:max(data2$Iteration)){
        temp <- with(data2, 
                     data.frame(ID = paste0('ID#', sprintf('%04d', i)),
                                firstName = unique(First.Name[Iteration == i]),
                                lastName = unique(Last.Name[Iteration == i]),
                                Date = unique(Date[Iteration == i]),
                                totalIteration = max(Iter.Number[Row.Type == 'Iter' & Iteration == i]),
                                Exportdata(data = data2, col = c("Power1",'Speed1','Speed2', 'Electricity', 'Effort', 'Weight', 'Torque'), row = "Iter", iter = i),
                                Exportdata(data = data2, col = c("Avg_Power1",'Avg_Speed1','Avg_Speed2', 'Avg_Electricity', 'Avg_Effort', 'Avg_Weight', 'Avg_Torque'), row = "Average", iter = i),
                                Exportdata(data = data2, col = c("Max_Power1",'Max_Speed1','Max_Speed2', 'Max_Electricity', 'Max_Effort', 'Max_Weight', 'Max_Torque'), row = "Maximum", iter = i),
                                Exportdata(data = data2, col = c("sd_Power1",'sd_Speed1','sd_Speed2', 'sd_Electricity', 'sd_Effort', 'sd_Weight', 'sd_Torque'), row = "Std.Dev.", iter = i),
                                Exportdata(data = data2, col = c("Total_Power1",'Total_Speed1','Total_Speed2', 'Total_Electricity', 'Total_Effort', 'Total_Weight', 'Total_Torque'), row = "Total", iter = i),
                                stringsAsFactors = FALSE
                               )
                    )
        outputIteration <- rbind(outputIteration, temp)
        }
```


```R
# Remove column with NA
    outputIteration <- outputIteration %>% select_if(~ !any(is.na(.)))

# Show the last six rows, dimension, and class of each column in the dataset
    tail(outputIteration); dim(outputIteration); t(sapply(outputIteration, class))
```


<table>
<thead><tr><th></th><th scope=col>ID</th><th scope=col>firstName</th><th scope=col>lastName</th><th scope=col>Date</th><th scope=col>totalIteration</th><th scope=col>Power1</th><th scope=col>Speed1</th><th scope=col>Speed2</th><th scope=col>Electricity</th><th scope=col>Effort</th><th scope=col>...</th><th scope=col>sd_Power1</th><th scope=col>sd_Speed1</th><th scope=col>sd_Speed2</th><th scope=col>sd_Electricity</th><th scope=col>sd_Effort</th><th scope=col>sd_Weight</th><th scope=col>sd_Torque</th><th scope=col>Total_Effort</th><th scope=col>Total_Weight</th><th scope=col>Total_Torque</th></tr></thead>
<tbody>
	<tr><th scope=row>5989</th><td>ID#5989                     </td><td> Person                     </td><td> Human                      </td><td>end of time                 </td><td>3                           </td><td>530, 23, 448                </td><td>650, 683, 273               </td><td>146, 627, 482               </td><td>267, 160, 748               </td><td>982, 582, 186               </td><td>...                         </td><td>458                         </td><td>479                         </td><td>777                         </td><td>397                         </td><td>678                         </td><td>967                         </td><td>560                         </td><td>161                         </td><td>598                         </td><td>124                         </td></tr>
	<tr><th scope=row>5990</th><td>ID#5990                     </td><td> Person                     </td><td> Human                      </td><td>end of time                 </td><td>4                           </td><td>877, 575, 505, 139          </td><td>262, 385, 939, 45           </td><td>70, 907, 870, 345           </td><td>681, 391, 88, 256           </td><td>585, 10, 48, 95             </td><td>...                         </td><td>188                         </td><td>532                         </td><td> 54                         </td><td> 26                         </td><td> 32                         </td><td>  3                         </td><td>470                         </td><td>445                         </td><td>  7                         </td><td>945                         </td></tr>
	<tr><th scope=row>5991</th><td>ID#5991                     </td><td> Person                     </td><td> Human                      </td><td>end of time                 </td><td>5                           </td><td>401, 665, 325, 402, 213     </td><td>183, 138, 336, 181, 949     </td><td>322, 153, 454, 734, 537     </td><td>394, 812, 15, 575, 932      </td><td>994, 359, 205, 487, 405     </td><td>...                         </td><td>723                         </td><td>528                         </td><td>315                         </td><td>669                         </td><td>301                         </td><td>788                         </td><td>909                         </td><td>354                         </td><td>616                         </td><td>851                         </td></tr>
	<tr><th scope=row>5992</th><td>ID#5992                     </td><td> Person                     </td><td> Human                      </td><td>end of time                 </td><td>5                           </td><td>434, 334, 571, 303, 170     </td><td>985, 605, 187, 544, 930     </td><td>479, 535, 584, 901, 119     </td><td>299, 595, 28, 894, 455      </td><td>75, 222, 743, 824, 588      </td><td>...                         </td><td>586                         </td><td>798                         </td><td>212                         </td><td>998                         </td><td>133                         </td><td>709                         </td><td>178                         </td><td>813                         </td><td>224                         </td><td>950                         </td></tr>
	<tr><th scope=row>5993</th><td>ID#5993                     </td><td> Person                     </td><td> Human                      </td><td>end of time                 </td><td>5                           </td><td>294, 661, 505, 663, 834     </td><td>469, 750, 581, 8, 133       </td><td>91, 489, 45, 684, 805       </td><td>243, 519, 892, 597, 907     </td><td>439, 314, 808, 109, 798     </td><td>...                         </td><td>260                         </td><td>  5                         </td><td>889                         </td><td>364                         </td><td>731                         </td><td>285                         </td><td>345                         </td><td>100                         </td><td>640                         </td><td>206                         </td></tr>
	<tr><th scope=row>5994</th><td>ID#5994                     </td><td> Person                     </td><td> Human                      </td><td>end of time                 </td><td>6                           </td><td>51, 623, 624, 311, 137, 879 </td><td>210, 2, 745, 267, 717, 73   </td><td>277, 179, 663, 247, 778, 977</td><td>993, 400, 239, 277, 975, 680</td><td>894, 34, 623, 209, 207, 500 </td><td>...                         </td><td>112                         </td><td>717                         </td><td>630                         </td><td>239                         </td><td>561                         </td><td>142                         </td><td>909                         </td><td>847                         </td><td>583                         </td><td>407                         </td></tr>
</tbody>
</table>




5994

36




<table>
<thead><tr><th scope=col>ID</th><th scope=col>firstName</th><th scope=col>lastName</th><th scope=col>Date</th><th scope=col>totalIteration</th><th scope=col>Power1</th><th scope=col>Speed1</th><th scope=col>Speed2</th><th scope=col>Electricity</th><th scope=col>Effort</th><th scope=col>...</th><th scope=col>sd_Power1</th><th scope=col>sd_Speed1</th><th scope=col>sd_Speed2</th><th scope=col>sd_Electricity</th><th scope=col>sd_Effort</th><th scope=col>sd_Weight</th><th scope=col>sd_Torque</th><th scope=col>Total_Effort</th><th scope=col>Total_Weight</th><th scope=col>Total_Torque</th></tr></thead>
<tbody>
	<tr><td>character</td><td>character</td><td>character</td><td>character</td><td>numeric  </td><td>character</td><td>character</td><td>character</td><td>character</td><td>character</td><td>...      </td><td>numeric  </td><td>numeric  </td><td>numeric  </td><td>numeric  </td><td>numeric  </td><td>numeric  </td><td>numeric  </td><td>numeric  </td><td>numeric  </td><td>numeric  </td></tr>
</tbody>
</table>




```R
# Export Output Data
    writexl::write_xlsx(outputIteration, "cleaned_and_tidy.xlsx")
```

Now look at that. Nice, clean, and tidy data. One row for one person.

The down side of this output are,.. Firstly, each person has its own `totalIteration`. So instead of making many column for each iteration, I decided to merge the iteration with `paste()` function and this method can affect the output. The data created by each person are supposed to be `numeric`. By merging all iteration data into one column, all iteration data will change to `character` class separated by `comma`. If you want to analyse this column, you had to do one more step to seperate each iteration using `strsplit()` function and transform it to `numeric` class. Secondly, I used `for loop` which makes the entire code run longer. In total, I need 5 minutes to execute this entire code. This is too long for me, personally. And I didn't know the alternative yet. So it count as a down side.

Although, this final output wasn't completely perfect. I satistied with it.
