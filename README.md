# Getting and Cleaning Data-Course Project1
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  

One of the most exciting areas in all of data science right now is wearable computing - see for example  this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: 
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: 
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

You should create one R script called run_analysis.R that does the following. 
1.Merges the training and the test sets to create one data set.
2.Extracts only the measurements on the mean and standard deviation for each measurement. 
3.Uses descriptive activity names to name the activities in the data set
4.Appropriately labels the data set with descriptive variable names.
5.From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Good luck!
################################################################################

###Run_Analysis.R

Set to working directory
setwd('C:/Users/User/Documents/My Course/Data Science/Data/Course3-Getting and Cleaning Data/')

Data URL: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

####Download zip file from given URL
if(!file.exists("./data")){dir.create("./data")} #to create new data folder if not exist yet
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl,destfile="./data/m3a1_Dataset.zip")

####Unzip the folder
unzip(zipfile="./data/m3a1_Dataset.zip",exdir="./data")

####By default the unzip folder name: UCI HAR Dataset
path_uhDataset <- file.path("./data" , "UCI HAR Dataset")
files<-list.files(path_uhDataset, recursive=TRUE)
files #to list all the files inside UCI HAR Dataset folder


####Read the training data & test data (***the files do not contain header)
####Read subject files
dataSubjectTrain <- read.table(file.path(path_uhDataset, "train", "subject_train.txt"),header = FALSE)
dataSubjectTest  <- read.table(file.path(path_uhDataset, "test" , "subject_test.txt"),header = FALSE)

####Read activity files
dataActivityTest  <- read.table(file.path(path_uhDataset, "test" , "Y_test.txt" ),header = FALSE)
dataActivityTrain <- read.table(file.path(path_uhDataset, "train", "Y_train.txt"),header = FALSE)

####Read data files.
dataFeaturesTest  <- read.table(file.path(path_uhDataset, "test" , "X_test.txt" ),header = FALSE)
dataFeaturesTrain <- read.table(file.path(path_uhDataset, "train", "X_train.txt"),header = FALSE)

####View the structure of the table to check for the data
str(dataSubjectTrain)
str(dataSubjectTest)
str(dataActivityTest)
str(dataActivityTrain)
str(dataFeaturesTest)
str(dataFeaturesTrain)

#####################################################################
####1.Merges the training and the test sets to create one data set.
#####################################################################

#####Concatenate the data tables by rows
dataSubject <- rbind(dataSubjectTrain, dataSubjectTest)
dataActivity<- rbind(dataActivityTrain, dataActivityTest)
dataFeatures<- rbind(dataFeaturesTrain, dataFeaturesTest)

#####set names to variables
names(dataSubject)<-c("subject")
names(dataActivity)<- c("activity")
dataFeaturesNames <- read.table(file.path(path_uhDataset, "features.txt"),head=FALSE)
names(dataFeatures)<- dataFeaturesNames$V2

#####Merge columns to get the data frame Data for all data
dataCombine <- cbind(dataSubject, dataActivity)
Data <- cbind(dataFeatures, dataCombine)

###########################################################################################
####2.Extracts only the measurements on the mean and standard deviation for each measurement.
###########################################################################################

#####Subset Name of Features by measurements on the mean and standard deviation
#####i.e taken Names of Features with "mean()" or "std()"
subdataFeaturesNames<-dataFeaturesNames$V2[grep("mean\\(\\)|std\\(\\)", dataFeaturesNames$V2)]

#####Subset the data frame Data by seleted names of Features
selectedNames<-c(as.character(subdataFeaturesNames), "subject", "activity" )
Data<-subset(Data,select=selectedNames)

#####Check the structures of the data frame Data
str(Data)

###########################################################################################
####3. Uses descriptive activity names to name the activities in the data set
###########################################################################################
#####Read descriptive activity names from "activity_labels.txt"
activityLabels <- read.table(file.path(path_uhDataset, "activity_labels.txt"),header = FALSE)

#####Factorize variable activity in the data frame Data using descriptive activity names
head(Data$activity,30)

###########################################################################################
####4. Appropriately labels the data set with descriptive variable names
###########################################################################################

####In the former part, variables activity and subject and names of the activities have been labelled using descriptive names.
####In this part, Names of Feteatures will labelled using descriptive variable names.
names(Data)<-gsub("^t", "time", names(Data))
names(Data)<-gsub("^f", "frequency", names(Data))
names(Data)<-gsub("Acc", "Accelerometer", names(Data))
names(Data)<-gsub("Gyro", "Gyroscope", names(Data))
names(Data)<-gsub("Mag", "Magnitude", names(Data))
names(Data)<-gsub("BodyBody", "Body", names(Data))

####Check Data naming
names(Data)

###########################################################################################
####5.From the data set in step 4, creates a second, independent tidy data set 
####with the average of each variable for each activity and each subject.
###########################################################################################

####In this part,a second, independent tidy data set will be created with the average of each variable for each activity and each subject based on the data set in step 4.

library(plyr);
Data2<-aggregate(. ~subject + activity, Data, mean)
Data2<-Data2[order(Data2$subject,Data2$activity),]
write.table(Data2, file = "./data/UCI HAR Dataset/tidydata.txt",row.name=FALSE)





