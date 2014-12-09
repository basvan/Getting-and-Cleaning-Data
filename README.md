Getting-and-Cleaning-Data
=========================

Getting and Cleaning Data Course Project - Peer Assignment

---
title: "Readme"
author: "Bas"
date: "Tuesday, December 09, 2014"
output: html_document
---

## SEE THE ENTIRE R SCRIPT AT THE BOTTOM OF THIS FILE!

Before starting on assignment steps the data needs to be downloaded and read into R. This is done using the following R code:

```{r}
        library(dplyr)
        library(tidyr)
        
        temp <- tempfile()
        download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip", temp)
        
        #test data
        subject_test <- read.csv(unz(temp, "UCI HAR Dataset/test/subject_test.txt"), sep="", header=FALSE)
        X_test <- read.csv(unz(temp, "UCI HAR Dataset/test/X_test.txt"), sep="", header=FALSE)
        y_test <- read.csv(unz(temp, "UCI HAR Dataset/test/y_test.txt"), sep="", header=FALSE)
        #training data
        subject_train <- read.csv(unz(temp, "UCI HAR Dataset/train/subject_train.txt"), sep="", header=FALSE)
        X_train <- read.csv(unz(temp, "UCI HAR Dataset/train/X_train.txt"), sep="", header=FALSE)
        y_train <- read.csv(unz(temp, "UCI HAR Dataset/train/y_train.txt"), sep="", header=FALSE)
        #activity labels
        activity_labels <-   read.csv(unz(temp, "UCI HAR Dataset/activity_labels.txt"), sep="", header=FALSE)
        #features        
        features <- read.csv(unz(temp, "UCI HAR Dataset/features.txt"), sep="", header=FALSE)
        
        unlink(temp)
```

Here the data is explained:
subject_train/test = Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30.
y_train/test = Training labels
X_train/test = Training set

features = List of all features
activity_labels = Links the class labels with their activity name.

-------------------------------------------------------------------------------------------------

Step 1: 

Gave names to all the variables in the data frame. Names are based on the feature list (features.txt) and "labels" and "subject" for the activity labelnr. and subjectnr.

Merge the training and testing set into one. 

First create a complete set of testing and training using cbind. Then combine the testing and training into a complete dataset using rbind.

```{r}
        names(X_test) = features[,2]
        names(X_train) = features[,2]
        names(y_test) = "labels"
        names(y_train) = "labels"
        names(subject_test) = "subject"
        names(subject_train) = "subject"
        
        testing <- cbind(X_test, y_test, subject_test)
        training <- cbind(X_train, y_train, subject_train) 
        
        dataset <- rbind(testing,training)
```

------------------------------------------------------------------------------------------------

Step 2:

Create a dataset with just the mean and standard deviation measurement variables from the data. The grep searches for variables with mean or std in the name. All are included (also for exampel meanFreq) because it still presents an average.

Variables included:
mean(): Mean value
std(): Standard deviation
meanFreq(): Weighted average of the frequency components to obtain a mean frequency
gravityMean
tBodyAccMean
tBodyAccJerkMean
tBodyGyroMean
tBodyGyroJerkMean

```{r}
        features_extract <- c(grep("mean|std", features[,2] , ignore.case = TRUE), 562,563)
        dataset_extracts <- dataset[features_extract]
```


-----------------------------------------------------------------------------------------------

Step 3:

ifelse makes it possible to adapt the variables content to a certain value. The range 1:6 is linked to the activity_label vector that contains the names extracted from the activity_labels dataframe. 
This way the labels change from 1 to WALKING and from 6 to STANDING. 

```{r}
        activity_label <- as.character(activity_labels[,2])
        
        dataset_extracts$labels <- ifelse(dataset_extracts$labels == 1, activity_label[1],
                                   ifelse(dataset_extracts$labels == 2, activity_label[2],
                                   ifelse(dataset_extracts$labels == 3, activity_label[3],
                                   ifelse(dataset_extracts$labels == 4, activity_label[4],      
                                   ifelse(dataset_extracts$labels == 5, activity_label[5],
                                   ifelse(dataset_extracts$labels == 6, activity_label[6], NA))))))
```

----------------------------------------------------------------------------------------------

Step 4: already done in step 1.

----------------------------------------------------------------------------------------------

Step 5:

Aggregrate splits the data into subsets, computes summary statistics for each, and returns the results in grouped form.

Removed the labels & subject column because a mean of those has no use.

txt file created with write.table() using row.name=FALSE

```{r}
        dataset_final = aggregate(dataset_extracts, by=list(activity = dataset_extracts$labels, subject = dataset_extracts$subject), mean)
        
        dataset_final[,90] = NULL
        dataset_final[,89] = NULL
        
        write.table(dataset_final, "dataset_final.txt", row.name=FALSE)
```

----------------------------
ENTIRE R SCRIPT:
----------------------------
```{r}
## 0. Download and read the data
        
        library(dplyr)
        library(tidyr)
        
        temp <- tempfile()
        download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip", temp)
        
        #test data
        subject_test <- read.csv(unz(temp, "UCI HAR Dataset/test/subject_test.txt"), sep="", header=FALSE)
        X_test <- read.csv(unz(temp, "UCI HAR Dataset/test/X_test.txt"), sep="", header=FALSE)
        y_test <- read.csv(unz(temp, "UCI HAR Dataset/test/y_test.txt"), sep="", header=FALSE)
        #training data
        subject_train <- read.csv(unz(temp, "UCI HAR Dataset/train/subject_train.txt"), sep="", header=FALSE)
        X_train <- read.csv(unz(temp, "UCI HAR Dataset/train/X_train.txt"), sep="", header=FALSE)
        y_train <- read.csv(unz(temp, "UCI HAR Dataset/train/y_train.txt"), sep="", header=FALSE)
        #activity labels
        activity_labels <-   read.csv(unz(temp, "UCI HAR Dataset/activity_labels.txt"), sep="", header=FALSE)
        #features        
        features <- read.csv(unz(temp, "UCI HAR Dataset/features.txt"), sep="", header=FALSE)
        
        unlink(temp)


## 1. Merges the training and the test sets to create one data set.
        
        names(X_test) = features[,2]
        names(X_train) = features[,2]
        names(y_test) = "labels"
        names(y_train) = "labels"
        names(subject_test) = "subject"
        names(subject_train) = "subject"
        
        testing <- cbind(X_test, y_test, subject_test)
        training <- cbind(X_train, y_train, subject_train) 
        
        dataset <- rbind(testing,training)

## 2. Extracts only the measurements on the mean and standard deviation for each measurement. 

        features_extract <- c(grep("mean|std", features[,2] , ignore.case = TRUE), 562,563)
        dataset_extracts <- dataset[features_extract]

## 3. Uses descriptive activity names to name the activities in the data set
        
        activity_label <- as.character(activity_labels[,2])
        
        dataset_extracts$labels <- ifelse(dataset_extracts$labels == 1, activity_label[1],
                                   ifelse(dataset_extracts$labels == 2, activity_label[2],
                                   ifelse(dataset_extracts$labels == 3, activity_label[3],
                                   ifelse(dataset_extracts$labels == 4, activity_label[4],      
                                   ifelse(dataset_extracts$labels == 5, activity_label[5],
                                   ifelse(dataset_extracts$labels == 6, activity_label[6], NA))))))
        
        
## 4. Appropriately labels the data set with descriptive variable names. 

        #Already done in step 1. The names are based on the variable names from the features.txt file.

## 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
        
        dataset_final = aggregate(dataset_extracts, by=list(activity = dataset_extracts$labels, subject = dataset_extracts$subject), mean)
        # Removing labels & subject column because a mean of those has no use
        dataset_final[,90] = NULL
        dataset_final[,89] = NULL
        
        write.table(dataset_final, "dataset_final.txt", row.name=FALSE)
       
```
