run_analysis.R file , Getting and Cleaning Data, Project1
library(“plyr”, lib.loc=“~/R/win-library/3.1”) library(knitr) knit2html(“Codebook.md”);

if(!file.exists(“getdata_projectfiles_UCI HAR Dataset.zip”)) fileUrl <- “https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip” download.file(fileUrl,destfile=“getdata_projectfiles_UCI HAR Dataset.zip”,method=“curl”)

Unzip the file##run_analysis.R file , Getting and Cleaning Data, Project1
library(“plyr”, lib.loc=“~/R/win-library/3.1”) library(knitr) knit2html(“Codebook.md”);

if(!file.exists(“getdata_projectfiles_UCI HAR Dataset.zip”)) fileUrl <- “https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip” download.file(fileUrl,destfile=“getdata_projectfiles_UCI HAR Dataset.zip”,method=“curl”)

Unzip the file
unzip(zipfile=“getdata_projectfiles_UCI HAR Dataset.zip”)

unzipped files are in the folder UCI HAR Dataset. Get the list of the files
path_rf <- file.path(“./” , “UCI HAR Dataset”) files<-list.files(path_rf, recursive=TRUE) files

1.Merges the training and the test sets to create one data set.
trainDataset = read.csv(“UCI HAR Dataset/train/X_train.txt”, sep=“”, header=FALSE) trainDataset[,562] = read.csv(“UCI HAR Dataset/train/Y_train.txt”, sep=“”, header=FALSE) trainDataset[,563] = read.csv(“UCI HAR Dataset/train/subject_train.txt”, sep=“”, header=FALSE)

testDataset = read.csv(“UCI HAR Dataset/test/X_test.txt”, sep=“”, header=FALSE) testDataset[,562] = read.csv(“UCI HAR Dataset/test/Y_test.txt”, sep=“”, header=FALSE) testDataset[,563] = read.csv(“UCI HAR Dataset/test/subject_test.txt”, sep=“”, header=FALSE)

activityLabelsDataset = read.csv(“UCI HAR Dataset/activity_labels.txt”, sep=“”, header=FALSE)

2.Extracts only the measurements on the mean and standard deviation for each measurement.
Read features and calculate Mean
featuresDataset = read.csv(“UCI HAR Dataset/features.txt”, sep=“”, header=FALSE) featuresDataset[,2] = gsub(‘-mean’, ‘Mean’, features[,2]) featuresDataset[,2] = gsub(‘-std’, ‘Std’, features[,2]) featuresDataset[,2] = gsub(‘[-()]’, ’’, features[,2])

Merge datasets of training and test sets.
allData = rbind(trainDataset, testDataset)

Read only required Columns we need from mean and std. dev.
requiredColumns <- grep(“.Mean.|.Std.”, featuresDataset[,2])

Tidy data by reducing the features table with only requied column names
featuresDataset <- featuresDataset[requiredColumns,]

Now add the last two columns (subject and activity)
requiredColumns <- c(requiredColumns, 562, 563)

And remove the unwanted columns from allData
allData <- allData[,requiredColumns]

3.Uses descriptive activity names to name the activities in the data set
4.Appropriately labels the data set with descriptive variable names.
Add the required column names (features) to allData
colnames(allData) <- c(featuresDataset$V2, “Activity”, “Subject”) colnames(allData) <- tolower(colnames(allData))

currentActivity = 1 for (currentActivityLabel in activityLabelsV2) {   allDataactivity <- gsub(currentActivity, currentActivityLabel, allData$activity) currentActivity <- currentActivity + 1 }

5.From the data set in step 4, creates a second, independent tidy data set
with the average of each variable for each activity and each subject
allDataactivity<−as.factor(allDataactivity) allDatasubject<−as.factor(allDatasubject)

tidydata = aggregate(allData, by=list(activity = allDataactivity,subject=allDatasubject), mean)

Now, remove the subject and activity column
tidydata[,90] = NULL tidydata[,89] = NULL write.table(tidydata, “tidy.txt”, row.name=FALSE, sep=“”) unzip(zipfile=“getdata_projectfiles_UCI HAR Dataset.zip”)

unzipped files are in the folder UCI HAR Dataset. Get the list of the files
path_rf <- file.path(“./” , “UCI HAR Dataset”) files<-list.files(path_rf, recursive=TRUE) files

1.Merges the training and the test sets to create one data set.
trainDataset = read.csv(“UCI HAR Dataset/train/X_train.txt”, sep=“”, header=FALSE) trainDataset[,562] = read.csv(“UCI HAR Dataset/train/Y_train.txt”, sep=“”, header=FALSE) trainDataset[,563] = read.csv(“UCI HAR Dataset/train/subject_train.txt”, sep=“”, header=FALSE)

testDataset = read.csv(“UCI HAR Dataset/test/X_test.txt”, sep=“”, header=FALSE) testDataset[,562] = read.csv(“UCI HAR Dataset/test/Y_test.txt”, sep=“”, header=FALSE) testDataset[,563] = read.csv(“UCI HAR Dataset/test/subject_test.txt”, sep=“”, header=FALSE)

activityLabelsDataset = read.csv(“UCI HAR Dataset/activity_labels.txt”, sep=“”, header=FALSE)

2.Extracts only the measurements on the mean and standard deviation for each measurement.
Read features and calculate Mean
featuresDataset = read.csv(“UCI HAR Dataset/features.txt”, sep=“”, header=FALSE) featuresDataset[,2] = gsub(‘-mean’, ‘Mean’, features[,2]) featuresDataset[,2] = gsub(‘-std’, ‘Std’, features[,2]) featuresDataset[,2] = gsub(‘[-()]’, ’’, features[,2])

Merge datasets of training and test sets.
allData = rbind(trainDataset, testDataset)

Read only required Columns we need from mean and std. dev.
requiredColumns <- grep(“.Mean.|.Std.”, featuresDataset[,2])

Tidy data by reducing the features table with only requied column names
featuresDataset <- featuresDataset[requiredColumns,]

Now add the last two columns (subject and activity)
requiredColumns <- c(requiredColumns, 562, 563)

And remove the unwanted columns from allData
allData <- allData[,requiredColumns]

3.Uses descriptive activity names to name the activities in the data set
4.Appropriately labels the data set with descriptive variable names.
Add the required column names (features) to allData
colnames(allData) <- c(featuresDataset$V2, “Activity”, “Subject”) colnames(allData) <- tolower(colnames(allData))

currentActivity = 1 for (currentActivityLabel in activityLabelsV2) {   allDataactivity <- gsub(currentActivity, currentActivityLabel, allData$activity) currentActivity <- currentActivity + 1 }

5.From the data set in step 4, creates a second, independent tidy data set
with the average of each variable for each activity and each subject
allDataactivity<−as.factor(allDataactivity) allDatasubject<−as.factor(allDatasubject)

tidydata = aggregate(allData, by=list(activity = allDataactivity,subject=allDatasubject), mean)

Now, remove the subject and activity column
tidydata[,90] = NULL tidydata[,89] = NULL write.table(tidydata, “tidy.txt”, row.name=FALSE, sep=“”)
