# week4-peergradedassingmnet-
coursera Cleaning and Collecting Data

library(dplyr)
library(data.table)
library(tidyr)


#Reading

train_subject<-tbl_df(read.table("train/subject_train.txt"))
test_subject<-tbl_df(read.table("test/subject_test.txt"))

train_x_train <- tbl_df(read.table("train/X_train.txt"))
test_x_test <- tbl_df(read.table("test/X_test.txt"))

train_y_train <- tbl_df(read.table("train/y_train.txt"))
test_y_test <- tbl_df(read.table("test/y_test.txt"))

activity_lables<-tbl_df(read.table("activity_labels.txt"))
features <- tbl_df(read.table("features.txt"))

#Merging

colnames(activity_lables)<- c("V1","Activity")
setnames(features, names(features), c("featureNum", "featureName"))


number <- rename(train_subject, number=V1)
number_exercise<- cbind(number, train_y_train)
train <- merge(number_exercise, activity_lables, by=("V1"))
colnames(train_x_train) <- features$featureName
train1 <- cbind(train, train_x_train)

number_test <- rename(test_subject, number=V1)
number_exercise_test <- cbind(number_test, test_y_test)
test <- merge(number_exercise_test, activity_lables, by=("V1"))
colnames(test_x_test) <- features$featureName
test1 <- cbind(test, test_x_test)


mergedData = rbind(train1, test1)

mergedData1<- mergedData[,-1]

mergedData2<-select(mergedData1, contains("number"), contains("Activity"), contains("mean"), contains("std"))

str(mergedData2)    

#Find means for all variables
        
TidyData<-  mergedData2%>%
        group_by(number, Activity)%>%
        summarise_each(funs( mean))
print(TidyData)
       

