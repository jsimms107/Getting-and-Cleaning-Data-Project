Getting-and-Cleaning-Data-Project
=================================

Jean Simms: Coursera Getting and Cleaning Data class project
 ---
title: "README"
author: "Jean Simms"
date: "September 18, 2014"
---
*****************************************
************* Instructions **************
*****************************************
Running th R script below will download the file, unzip it, read in the data, and perform the transformations described in CodeBook.md. 

The data used in this analysis can be found at https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip and a complete description of the data can be found at http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones.

The R script will save the data into a text file, "HARavg.txt", in your working directory. To open this file and load the data back into R, you may use the commands
data <- read.table("./HARavg.txt", header = TRUE)
View(data)

If you would like to work with the data right away as a dataframe in R, use the following command when calling the function.
HARavg<-run_analysis()

*****************************************
*************** R script ****************
*****************************************

```{r}
run_analysis<-function(){
  ##LIBRARIES CALLED LATER ##
  library(dplyr)
  
  
  ##FIRST DOWNLOAD THE FILE ##
  print("Downloading File")
  download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip",destfile="HumanActivityRecognition.zip", method="curl")
  #Now unzip the file
  unzip("./HumanActivityRecognition.zip")
  #That creates a folder called UCI HAR Dataset
  
  
  ##LOAD IN THE VARIOUS DATA SETS AND THEIR LABELS ##
  print("Loading data")
  #Load in the X_test and X_trial data
  xtest<-read.table("./UCI HAR Dataset/test/X_test.txt",header=F, stringsAsFactors=F,fill=T)
  xtrain<-read.table("./UCI HAR Dataset/train/X_train.txt",header=F, stringsAsFactors=F,fill=T)
  
  #Load in the Y_test and Y_trial data
  ytest<-read.table("./UCI HAR Dataset/test/y_test.txt",header=F, stringsAsFactors=F,fill=T)
  ytrain<-read.table("./UCI HAR Dataset/train/y_train.txt",header=F, stringsAsFactors=F,fill=T)
  
  #Load in the subject_test and subject_trial data
  stest<-read.table("./UCI HAR Dataset/test/subject_test.txt",header=F, stringsAsFactors=F,fill=T)
  strain<-read.table("./UCI HAR Dataset/train/subject_train.txt",header=F, stringsAsFactors=F,fill=T)
  
  #Load in the names of the X_test and X_train columns from the
  #features.txt file
  featureNames<-read.table("./UCI HAR Dataset/features.txt",header=F, stringsAsFactors=F)
  
  #Load in the names of the Y_test and Y_train variables from
  #activity labels.txt
  activitylabels<-read.table("./UCI HAR Dataset/activity_labels.txt",header=F, stringsAsFactors=F)
  
  
  ##TRANSFORMATIONS TO CLEAN UP THE DATA ##
  print("Tidying up the data")
  #assign the names from features.txt to the columns of xtest and
  #xtrain
  colnames(xtest)<-featureNames[,2]
  colnames(xtrain)<-featureNames[,2]
  
  #Combine into one dataset
  #This is step 1 of instructions 
  HARdata<-rbind(xtrain,xtest) 
  
  #Extracts only the measurements on the mean and standard deviation
  #for each measurement
  #This is step 2 of instructions
  HARdata<-select(HARdata,contains("mean"),contains("std"))
  #remove the "angle" measurements because they aren't mean and std
  #measurements. They are measurements of the angle between one or
  #two means for example: angle(tBodyAccMean,gravity) is the angle
  #between the mean body acceleration and gravity
  HARdata<-HARdata[!grepl("angle",names(HARdata))] 
  
  #Rewrite ytest and ytrain assigning the correct activity label
  #based on the value
  for (i in 1:nrow(ytest)){
    ytest[i,1]=activitylabels[as.numeric(ytest[i,1]),2]
    }
  for (i in 1:nrow(ytrain)){
    ytrain[i,1]=activitylabels[as.numeric(ytrain[i,1]),2]
  }

  #Now add the descriptive activity names to the data set
  #This is step 3 of instructions
  Y<-rbind(ytrain,ytest)
  colnames(Y)<-"Activity"
  HARdata<-cbind(Y,HARdata)
  
  #also add subject numbers to the dataset
  subject<-rbind(strain,stest)
  colnames(subject)<-"Subject"
  HARdata<-cbind(subject,HARdata) 
  
  ##Appropriately labels the data set with descriptive variable
  #names
  #This is step 4 of the instructions
  x<-names(HARdata)
  x<-gsub("()","",x,fixed=TRUE) #get rid of ()
  x<-gsub("BodyBody","Body",x,fixed=TRUE) #get rid of extra Body 
  x<-gsub("-"," ",x,fixed=TRUE) #get rid of dash
  names(HARdata)<-x
  
  ##Creates a second tidy data set with the average of each column
  #(numcolwise(mean)) 
  #for each activity and each subject .(Subject,Activity)
  #This is step 5 of the instructions
  HARavg<-ddply(HARdata, .(Subject,Activity), numcolwise(mean))
  
  #Output the final tidy dataset as a text file
  print("Saving the data as HARavg.txt")
  #This will be uploaded to complete the assignment
  write.table(HARavg,file="./HARavg.txt",row.names=FALSE)
  
  #Return the final tidy data frame as the output of the 
  #run_analysis function
  HARavg
  
}
  
```
