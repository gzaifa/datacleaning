# Introduction

This code book explains how the data was transformed in to a tidy data set from the source at 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

and describes the variables in the output file. You can read more about the data set here 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

# Program Description

The program starts by reading all the relevant data files.

```{r }
features<-read.table("UCI HAR Dataset/features.txt")
activity_labels<-read.table("UCI HAR Dataset/activity_labels.txt")
names(activity_labels)<-c("activity","description")
test_data<-read.table("UCI HAR Dataset/test/X_test.txt",header=F)
train_data<-read.table("UCI HAR Dataset/train/X_train.txt",header=F)
subject_test<-read.table("UCI HAR Dataset/test/subject_test.txt")
names(subject_test)<-c("subject")
subject_train<-read.table("UCI HAR Dataset/train/subject_train.txt")
names(subject_train)<-c("subject")
test_labels<-read.table("UCI HAR Dataset/test/y_test.txt")
names(test_labels)<-c("activity")
test_labels<-data.frame(activity=join(test_labels,activity_labels)[["description"]])
train_labels<-read.table("UCI HAR Dataset/train/y_train.txt")
names(train_labels)<-c("activity")
train_labels<-data.frame(activity=join(train_labels,activity_labels)[,"description"])
```

Next it transforms the filters only the columns necessary to create the tidy data set

```{r }
relevant_features<-features[features[["V2"]] %like% c("mean\\(\\)") | features[["V2"]] %like% c("std\\(\\)"),]
test_data<-test_data[,relevant_features[["V1"]]]
train_data<-train_data[,relevant_features[["V1"]]]
```

It cleans up the column names to remove dashes and brackets

```{r }
names(test_data)<-gsub("\\)","",gsub("\\(","",tolower(gsub("-","",relevant_features[["V2"]]))))
names(train_data)<-gsub("\\)","",gsub("\\(","",tolower(gsub("-","",relevant_features[["V2"]]))))
```

Next it column binds the the test data set and training data sets with their respective subject codes and activity

```{r }
test_data<-cbind(subject_test,test_labels,test_data)
train_data<-cbind(subject_train,train_labels,train_data)
```

Finally it row binds both the data sets
```{r }
final_data<-rbind(test_data,train_data)
````

For the final data set we are using ddply where we are grouping by subject and activity. Then calculating the colMeans using an inline function

```{r }
final_tidy<-ddply(final_data,.(subject,activity),function(x){
  colMeans(x[,c(3:68)])
})

```



# Variables in the Data Set
The following variables are present in the codebook

Variable Name|Desciption|
|-------------|-----------|
subject| The ID of the subject from 1 to 30|
activity| The Descriptive activity, such as walking etc |
tbodyaccmeanx| The mean value of subject/activty combination for the field
tbodyaccmeany| The mean value of subject/activty combination for the field| 
tbodyaccmeanz| The mean value of subject/activty combination for the field| 
tbodyaccstdx| The mean value of subject/activty combination for the field| 
tbodyaccstdy| The mean value of subject/activty combination for the field| 
tbodyaccstdz| The mean value of subject/activty combination for the field| 
tgravityaccmeanx| The mean value of subject/activty combination for the field| 
tgravityaccmeany| The mean value of subject/activty combination for the field| 
tgravityaccmeanz| The mean value of subject/activty combination for the field| 
tgravityaccstdx| The mean value of subject/activty combination for the field| 
tgravityaccstdy| The mean value of subject/activty combination for the field| 
tgravityaccstdz| The mean value of subject/activty combination for the field| 
tbodyaccjerkmeanx| The mean value of subject/activty combination for the field| 
tbodyaccjerkmeany| The mean value of subject/activty combination for the field| 
tbodyaccjerkmeanz| The mean value of subject/activty combination for the field| 
tbodyaccjerkstdx| The mean value of subject/activty combination for the field| 
tbodyaccjerkstdy| The mean value of subject/activty combination for the field| 
tbodyaccjerkstdz| The mean value of subject/activty combination for the field| 
tbodygyromeanx| The mean value of subject/activty combination for the field| 
tbodygyromeany| The mean value of subject/activty combination for the field| 
tbodygyromeanz| The mean value of subject/activty combination for the field| 
tbodygyrostdx| The mean value of subject/activty combination for the field| 
tbodygyrostdy| The mean value of subject/activty combination for the field| 
tbodygyrostdz| The mean value of subject/activty combination for the field| 
tbodygyrojerkmeanx| The mean value of subject/activty combination for the field| 
tbodygyrojerkmeany| The mean value of subject/activty combination for the field| 
tbodygyrojerkmeanz| The mean value of subject/activty combination for the field| 
tbodygyrojerkstdx| The mean value of subject/activty combination for the field| 
tbodygyrojerkstdy| The mean value of subject/activty combination for the field| 
tbodygyrojerkstdz| The mean value of subject/activty combination for the field| 
tbodyaccmagmean| The mean value of subject/activty combination for the field| 
tbodyaccmagstd| The mean value of subject/activty combination for the field| 
tgravityaccmagmean| The mean value of subject/activty combination for the field| 
tgravityaccmagstd| The mean value of subject/activty combination for the field| 
tbodyaccjerkmagmean| The mean value of subject/activty combination for the field| 
tbodyaccjerkmagstd| The mean value of subject/activty combination for the field| 
tbodygyromagmean| The mean value of subject/activty combination for the field| 
tbodygyromagstd| The mean value of subject/activty combination for the field| 
tbodygyrojerkmagmean| The mean value of subject/activty combination for the field| 
tbodygyrojerkmagstd| The mean value of subject/activty combination for the field| 
fbodyaccmeanx| The mean value of subject/activty combination for the field| 
fbodyaccmeany| The mean value of subject/activty combination for the field| 
fbodyaccmeanz| The mean value of subject/activty combination for the field| 
fbodyaccstdx| The mean value of subject/activty combination for the field| 
fbodyaccstdy| The mean value of subject/activty combination for the field| 
fbodyaccstdz| The mean value of subject/activty combination for the field| 
fbodyaccjerkmeanx| The mean value of subject/activty combination for the field| 
fbodyaccjerkmeany| The mean value of subject/activty combination for the field| 
fbodyaccjerkmeanz| The mean value of subject/activty combination for the field| 
fbodyaccjerkstdx| The mean value of subject/activty combination for the field| 
fbodyaccjerkstdy| The mean value of subject/activty combination for the field| 
fbodyaccjerkstdz| The mean value of subject/activty combination for the field| 
fbodygyromeanx| The mean value of subject/activty combination for the field| 
fbodygyromeany| The mean value of subject/activty combination for the field| 
fbodygyromeanz| The mean value of subject/activty combination for the field| 
fbodygyrostdx| The mean value of subject/activty combination for the field| 
fbodygyrostdy| The mean value of subject/activty combination for the field| 
fbodygyrostdz| The mean value of subject/activty combination for the field| 
fbodyaccmagmean| The mean value of subject/activty combination for the field| 
fbodyaccmagstd| The mean value of subject/activty combination for the field| 
fbodybodyaccjerkmagmean| The mean value of subject/activty combination for the field| 
fbodybodyaccjerkmagstd| The mean value of subject/activty combination for the field| 
fbodybodygyromagmean| The mean value of subject/activty combination for the field| 
fbodybodygyromagstd| The mean value of subject/activty combination for the field| 
fbodybodygyrojerkmagmean| The mean value of subject/activty combination for the field| 
fbodybodygyrojerkmagstd| The mean value of subject/activty combination for the field| 
