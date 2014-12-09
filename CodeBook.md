---
title: "CodeBook"
author: "Bas"
date: "Tuesday, December 09, 2014"
output: html_document
---

CodeBook that describes the variables, the data, and any transformations or work that you performed to clean up the data.

To start the data is delivered in a zip file that contains the following files:

- activity_labels.txt
- features.txt
- features_info.txt
- README.txt
test
        - subject_test.txt
        - X_test.txt
        - y_test.txt
        Inertial Signals
train
        - subject_train.txt
        - X_train.txt
        - y_train.txt 
        Inertial Signals
        
I have read the files: activity_labels.txt, features.txt, subject_test.txt, X_test.txt, y_test.txt, subject_train.txt, X_train.txt, y_train.txt into R. And the activity_labels.txt & features.txt.  

The rest is not used, the Inertial Signals contain raw data.



Below you can find information about the different variables:
----------------
Measurements:
----------------
Feature Selection 
=================

The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

These signals were used to estimate variables of the feature vector for each pattern:  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

tBodyAcc-XYZ
tGravityAcc-XYZ
tBodyAccJerk-XYZ
tBodyGyro-XYZ
tBodyGyroJerk-XYZ
tBodyAccMag
tGravityAccMag
tBodyAccJerkMag
tBodyGyroMag
tBodyGyroJerkMag
fBodyAcc-XYZ
fBodyAccJerk-XYZ
fBodyGyro-XYZ
fBodyAccMag
fBodyAccJerkMag
fBodyGyroMag
fBodyGyroJerkMag

The set of variables that were estimated from these signals are: 

mean(): Mean value
std(): Standard deviation
mad(): Median absolute deviation 
max(): Largest value in array
min(): Smallest value in array
sma(): Signal magnitude area
energy(): Energy measure. Sum of the squares divided by the number of values. 
iqr(): Interquartile range 
entropy(): Signal entropy
arCoeff(): Autorregresion coefficients with Burg order equal to 4
correlation(): correlation coefficient between two signals
maxInds(): index of the frequency component with largest magnitude
meanFreq(): Weighted average of the frequency components to obtain a mean frequency
skewness(): skewness of the frequency domain signal 
kurtosis(): kurtosis of the frequency domain signal 
bandsEnergy(): Energy of a frequency interval within the 64 bins of the FFT of each window.
angle(): Angle between to vectors.

Additional vectors obtained by averaging the signals in a signal window sample. These are used on the angle() variable:

gravityMean
tBodyAccMean
tBodyAccJerkMean
tBodyGyroMean
tBodyGyroJerkMean
----------------
Activity labels:
----------------
1 WALKING
2 WALKING_UPSTAIRS
3 WALKING_DOWNSTAIRS
4 SITTING
5 STANDING
6 LAYING

----------------
Subject numbers:
----------------
Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30.



Below you can find information about TRANSFORMATIONS TO THE DATA:

Step 1: combine all data into complete dataset that includes all data. The data from: subject_test.txt, X_test.txt and y_test.txt are combined by binding their columns because they contain different data about the same occurance, respectively the subjectnumber (participant), the activity they are performing (e.g. walking) and the actual measurements. The same is done for the train data files. Then both the training and testing data is combined by binding their rows together. This results in an entire data set.

Also the variables are given names based on their value (names of measurements taken from feature text file)

Step 2: The variables (columns) that do not show the mean or standard deviation are taken out of the data frame. 

Step 3: The activity labels are transformed from numeric (1:6) into actual name of the label (walking, standing, etc.)

Step 4: Names are already given in step 1.

Step 5: Grouped together by label and subject number. Column of label & subject removed because the mean of that has no value (or is not there because it is categorical).  

Created a txt file of final dataset.

