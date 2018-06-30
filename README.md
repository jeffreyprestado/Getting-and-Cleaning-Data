# Getting-and-Cleaning-Data

The data used in this course project represent data collected from the accelerometers from the Samsung Galaxy S smartphone.

More information can be found at the data source website: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

The data for this project can be downloaded here:

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

# The R run_analysis.R code in this repository primarily performs these 5 functions:

* Merges the training and the test sets to create one data set.

* Extracts only the measurements on the mean and standard deviation for each measurement

* Uses descriptive activity names to name the activities in the data set

* Appropriately labels the data set with descriptive variable names

* From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject

# File descriptions

* README.md - which basically gives an overview of the repo and brief description of the files within it.
* run_analysis.R - the R script that was used to extract the required dataset.
* tidydata.txt - which is the submitted or the result data of the run_analysis.R
* CodeBook.md - which gives a description on the variables and the calculations made.
