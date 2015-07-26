# Getting and cleaning data


Companies like *FitBit, Nike,* and *Jawbone Up* are racing to develop the most advanced algorithms to attract new users. The data linked are collected from the accelerometers from the Samsung Galaxy S smartphone. 

A full description is available at the site where the data was obtained:  

<http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones>

The data is available at:

<https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>

Broad steps
- Merges the training and the test sets to create one data set.
- Extracts only the measurements on the mean and standard deviation for each measurement. 
- Uses descriptive activity names to name the activities in the data set
- Appropriately labels the data set with descriptive variable names. 
- From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Files in the repo
- run_analysis.R : the R-code run on the data set
- Tidy.txt : the clean data extracted 
- CodeBook.md : CodeBook 
- README.md : the analysis of the code 

Read training and test data
```
subjectTrain <- read.table("UCI HAR Dataset/train/subject_train.txt", header = FALSE)
activityTrain <- read.table("UCI HAR Dataset/train/y_train.txt", header = FALSE)
featuresTrain <- read.table("UCI HAR Dataset/train/X_train.txt", header = FALSE)

subjectTest <- read.table("UCI HAR Dataset/test/subject_test.txt", header = FALSE)
activityTest <- read.table("UCI HAR Dataset/test/y_test.txt", header = FALSE)
featuresTest <- read.table("UCI HAR Dataset/test/X_test.txt", header = FALSE)

```

##Part 1 - Merge the training and the test sets to create one data set

```
subject <- rbind(subjectTrain, subjectTest)
activity <- rbind(activityTrain, activityTest)
features <- rbind(featuresTrain, featuresTest)
```


##Part 2 - Extracts only the measurements on the mean and standard deviation for each measurement

```
columnsWithMeanSTD <- grep(".*mean.*|.*std.*", names(completeData), ignore.case=TRUE)

```
##Part 3 - Uses descriptive activity names to name the activities in the data set
```
extractedData$Activity <- as.character(extractedData$Activity)
for (i in 1:6){
  extractedData$Activity[extractedData$Activity == i] <- as.character(activityLabels[i,2])
}
```

##Part 4 Appropriately labels the data set with descriptive variable names. 
```
names(extractedData)<-gsub("^t", "Time", names(extractedData))
names(extractedData)<-gsub("^f", "Frequency", names(extractedData))
names(extractedData)<-gsub("tBody", "TimeBody", names(extractedData))
```

##Part 5 - From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject
```


tidyData <- aggregate(. ~Subject + Activity, extractedData, mean)
tidyData <- tidyData[order(tidyData$Subject,tidyData$Activity),]
write.table(tidyData, file = "Tidy.txt", row.names = FALSE)
```
