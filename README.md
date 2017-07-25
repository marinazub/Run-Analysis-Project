Let's begin with set ups of necesarry libraries

```{r}
library(dplyr)
library(data.table)
library(tidyr)
```
##Reading

The first step is to read the files into the R. I save them in to the tables with tbl_df for more convenient use. I do that for all necesarry files as subjects (train and test), x and y-s files for train and test. Also, I do the work with uploading activities  and features files in advance.
```{r}

train_subject<-tbl_df(read.table("train/subject_train.txt"))
test_subject<-tbl_df(read.table("test/subject_test.txt"))

train_x_train <- tbl_df(read.table("train/X_train.txt"))
test_x_test <- tbl_df(read.table("test/X_test.txt"))

train_y_train <- tbl_df(read.table("train/y_train.txt"))
test_y_test <- tbl_df(read.table("test/y_test.txt"))

activity_lables<-tbl_df(read.table("activity_labels.txt"))
features <- tbl_df(read.table("features.txt"))
```

##Merging
Next step is to merge the files one by one.
Fisrt step is to work with activities and features, renaming the columns.
```{r}

colnames(activity_lables)<- c("V1","Activity")
setnames(features, names(features), c("featureNum", "featureName"))
```
Next point is to merge  the files: subject (number) and exercies (train_y_train/test_y_test) -> activities lables and than -> add the measures of the all activities, adding the namus of columns beforehand (train_x_train/test_x_test).

```{r}
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
```


The last step is to merge the entire datasets for test and train. Because they have the samename columns, it's the smart move to go through row binding function.

```{r}
mergedData = rbind(train1, test1)
```
As the final round is to cut off the extra information and leave the task's one - mean and sd
```{r}
mergedData1<- mergedData[,-1]

mergedData2<-select(mergedData1, contains("number"), contains("Activity"), contains("mean"), contains("std"))

str(mergedData2)  

```

##FInd means for all variables
```{r}

        
TidyData<-  mergedData2%>%
        group_by(number, Activity)%>%
        summarise_each(funs( mean))
print(TidyData)
       


```

