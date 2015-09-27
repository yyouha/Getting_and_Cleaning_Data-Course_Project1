----------------------------------------
Code book describing the variables
----------------------------------------

Here are the data for the project: 
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

We can say there are 3 important part from this unzip files:
1.subject files
train/subject_train.txt
test/subject_test.txt

2. activity files
test/Y_test.txt
train/Y_train.txt

3. data files.
test/X_test.txt
train/X_train.txt

Variables:
-----------

dataSubjectTrain : to read the train/subject_train.txt files
dataSubjectTest : to read the test/subject_test.txt files
dataActivityTest : to read the test/Y_test.txt files
dataActivityTrain : to read the train/Y_train.txt files
dataFeaturesTest : to read the test/X_test.txt files
dataFeaturesTrain : to read the train/X_train.txt files

dataSubject: to bind dataSubjectTrain and dataSubjectTest
dataActivity: to bind dataActivityTrain and dataActivityTest
dataFeatures: to bind dataFeaturesTrain and dataFeaturesTest

dataCombine: to merge dataSubject and dataActivity

subdataFeaturesNames: Subset Name of Features by measurements
selectedNames: Subset the data frame Data by seleted names of Features

activityLabels: Read descriptive activity names from "activity_labels.txt
