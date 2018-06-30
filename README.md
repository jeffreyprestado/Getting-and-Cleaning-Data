# Getting-and-Cleaning-Data

The data used in this course project represent data collected from the accelerometers from the Samsung Galaxy S smartphone.

More information can be found at the data source website: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

The data for this project can be downloaded here:

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

##The R run_analysis.R code in this repository primarily performs these 5 functions:

Merges the training and the test sets to create one data set.

Extracts only the measurements on the mean and standard deviation for each measurement

Uses descriptive activity names to name the activities in the data set

Appropriately labels the data set with descriptive variable names

From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject

-----------------------------------------------------------------------------------------------------------------------

##The R run_analysis.R

#run_analysis.R

install.packages("dplyr")
install.packages("data.table")
#Load the packages
library(data.table)
library(dplyr)

#Set your working directory
setwd("C:/Users/NEDA/Documents/R/Getting and Cleaning Data")

#Download UCI data files from the web, unzip them, and specify time/date settings
URL <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
destFile <- "CourseDataset.zip"
if (!file.exists(destFile)){
  download.file(URL, destfile = destFile, mode='wb')
}
if (!file.exists("./UCI HAR Dataset")){
  unzip(destFile)
}
dateDownloaded <- date()

#Start reading files
setwd("C:/Users/NEDA/Documents/R/Getting and Cleaning Data/UCI HAR Dataset")

#Reading Activity files
ActivityTest <- read.table("./test/y_test.txt", header = F)
ActivityTrain <- read.table("./train/y_train.txt", header = F)

#Read features files
FeaturesTest <- read.table("./test/X_test.txt", header = F)
FeaturesTrain <- read.table("./train/X_train.txt", header = F)

#Read subject files
SubjectTest <- read.table("./test/subject_test.txt", header = F)
SubjectTrain <- read.table("./train/subject_train.txt", header = F)

#Read Activity Labels
ActivityLabels <- read.table("./activity_labels.txt", header = F)

#Read Feature Names
FeaturesNames <- read.table("./features.txt", header = F)

#Merg dataframes: Features Test&Train,Activity Test&Train, Subject Test&Train
FeaturesData <- rbind(FeaturesTest, FeaturesTrain)
SubjectData <- rbind(SubjectTest, SubjectTrain)
ActivityData <- rbind(ActivityTest, ActivityTrain)

#Renaming colums in ActivityData & ActivityLabels dataframes
names(ActivityData) <- "ActivityN"
names(ActivityLabels) <- c("ActivityN", "Activity")

#Get factor of Activity names
Activity <- left_join(ActivityData, ActivityLabels, "ActivityN")[, 2]

#Rename SubjectData columns
names(SubjectData) <- "Subject"
#Rename FeaturesData columns using columns from FeaturesNames
names(FeaturesData) <- FeaturesNames$V2

#Create one large Dataset with only these variables: SubjectData,  Activity,  FeaturesData
DataSet <- cbind(SubjectData, Activity)
DataSet <- cbind(DataSet, FeaturesData)

#Create New datasets by extracting only the measurements on the mean and standard deviation for each measurement
subFeaturesNames <- FeaturesNames$V2[grep("mean\\(\\)|std\\(\\)", FeaturesNames$V2)]
DataNames <- c("Subject", "Activity", as.character(subFeaturesNames))
DataSet <- subset(DataSet, select=DataNames)

#Rename the columns of the large dataset using more descriptive activity names
names(DataSet)<-gsub("^t", "time", names(DataSet))
names(DataSet)<-gsub("^f", "frequency", names(DataSet))
names(DataSet)<-gsub("Acc", "Accelerometer", names(DataSet))
names(DataSet)<-gsub("Gyro", "Gyroscope", names(DataSet))
names(DataSet)<-gsub("Mag", "Magnitude", names(DataSet))
names(DataSet)<-gsub("BodyBody", "Body", names(DataSet))

#Create a second, independent tidy data set with the average of each variable for each activity and each subject
SecondDataSet<-aggregate(. ~Subject + Activity, DataSet, mean)
SecondDataSet<-SecondDataSet[order(SecondDataSet$Subject,SecondDataSet$Activity),]

#Save this tidy dataset to local file
write.table(SecondDataSet, file = "tidydata.txt",row.name=FALSE)

-----------------------------------------------------------------------------------------------------------------------
