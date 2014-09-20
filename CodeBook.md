---
title: "CodeBook"
author: "Jean Simms"
date: "September 18, 2014"
output: html_document
---

CODE BOOK

*****************************************
*************** Variables ***************
*****************************************
Units: All variables are normalized (bounded within [-1,1]). As such they are unitless

List of abbreviations and their meanings:
- t = this variable is in the time domain
- f = this variable is in the frequency domain determined using a FFT
- Body = measurements attributed to body movement of the subject
- Gravity = measurements attributed to gravitational force
- Acc = acceleration measurement from accelerometer
- Gyro = orientation measurement from gyroscope
- Mag = magnitude
- Jerk = jerk calculation which is the rate of change of acceleration
- std = standard deviation
- mean = mathematical mean value
- Subject = the number assigned to the human test subject (1-30)
- Activity = the activity being performed by the test subject

Example: "tBodyAcc mean X" is the mean acceleration in the x direction that is attributed to body movement rather than gravity. This variable is in the time domain.

*****************************************
*********** Transformations *************
*****************************************
The R script contained in the README.md file was used to 
1) Download the data from https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
2) Unzip the file and load the data into R objects 
3) Merge the training and the test sets to create a single dataset.
4) Extract only the measurements on the mean and standard deviation for each measurement. All "angle" measurements were excluded because they they aren't actual mean and standard deviation measurements, even though the word "mean" is sometimes present in the variable name. Rather, the "angle" variables are measurements of the angle between two variables. For example: angle(tBodyAccMean,gravity) is the angle between the mean body acceleration and gravity
5) Uses descriptive activity names from activity_labels.txt to name the activities and add them to the dataset
6) Test subject identification data was added to the dataset
7) Variable column names were taken from features.txt and modified for easier understanding.
8) Full tidy dataset is named HARdata
9) A second tidy data set was created with the average of each column for each activity and each subject. This dataset was named HARavg and saved as HARavg.txt in your working directory

*****************************************
*********** About the Data **************
*****************************************
Taken from README.txt file in https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

Written by Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio, Luca Oneto.
Smartlab - Non Linear Complex Systems Laboratory
DITEN - Universit‡ degli Studi di Genova.
Via Opera Pia 11A, I-16145, Genoa, Italy.
activityrecognition@smartlab.ws
www.smartlab.ws

Human Activity Recognition Using Smartphones Dataset
Version 1.0

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain.  