# week4-peergradedassingmnet-
coursera Cleaning and Collecting Data
---
title: "R Notebook"
output: html_notebook
---

##Description of run_Analyis.R
The following chapters are describing how the script run_analysis.R is processing the data files to output the tidyData.txt.



##Loading activity labels and feature names

Both data set from activity_labels.txt and features.txt are loaded and stored in a separate data frame to provide later the activity levels and the names for the variable in the data sets

##Loading train and test data sets
Six appropriate files was loaded in R and saved  in to the tables with tbl_df for more convenient use. I do that for all necesarry files as subjects (train and test), x and y-s files for train and test. Also, I do the work with uploading activities  and features files in advance.


##Merging files
Next step is to merge the files one by one.
Fisrt sub-step is to work with activities and features, renaming the columns.

Second sib-step is to merge  the files: subject (number) and exercies (train_y_train/test_y_test) -> activities lables and than -> add the measures of the all activities, adding the namus of columns beforehand (train_x_train/test_x_test).

Third sub-step  is to merge the entire datasets for test and train. Because they have the samename columns, it's the smart move to go through row binding function.

As the last sub-step is to cut off the extra information and leave the task's one - mean and sd.

#Finding means of the columns
The dataset was grouped by number and activity and  mean was calculated. The final result is stored in tidyData.txt.


