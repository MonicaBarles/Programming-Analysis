# Programming-Analysis
Getting and Cleaning Data Programming Assignment

#download the data
#running in Windows

>fileURL="https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"

>download.file(fileURL,destfile="./Dataset.zip")

>unzip(zipfile="./Dataset.zip",exdir="./Data")

#PART 1_MERGE THE TRAINING TEST AND TEST TEST TO CREATE ONE DATA SET
#read the data sets then combine

#read train data sets
>trainset <- read.table("./Data/UCI HAR Dataset/train/X_train.txt")

>trainactivity <- read.table("./Data/UCI HAR Dataset/train/y_train.txt")

>trainsubject <- read.table('./Data/UCI HAR Dataset/train/subject_train.txt')

#read train data sets
>testset <- read.table("./Data/UCI HAR Dataset/test/X_test.txt")

>testactivity <- read.table("./Data/UCI HAR Dataset/test/y_test.txt")

>testsubject <- read.table("./Data/UCI HAR Dataset/test/subject_test.txt")

#readfeatures
>features<-read.table("./Data/UCI HAR Dataset/features.txt")

#combine training and test tables
>activity<-rbind(trainactivity,testactivity)

>subject<-rbind(trainsubject,testsubject)

>set<-rbind(trainset,testset)

#set names to variables
>names(activity)<-c("activity")

>names(subject)<-c("subject")

>names(set)<-features[,2]

#merge all data
>mergeddata<-cbind(subject,activity,set)


#PART 2_EXTRACTS ONLY THE MEASUREMENTS ON THE MEAN AND STANDARD DEVIATION FOR EACH MEASUREMENT

#find values of mean or standard deviation
>meanstd<-grep("mean\\(\\)|std\\(\\)", features$V2,value=TRUE)

>meanstd<-union(c("subject","activity"),meanstd)

>mergeddata<-subset(mergeddata,select=meanstd)


#PART 3_USES DESCRIPTIVE ACTIVITY NAMES TO NAME THE ACTIVITIES IN THE DATA SET

#read activity file
>activitylabels<-read.table("./Data/UCI HAR Dataset/activity_labels.txt")

>activitylabels <- as.character(activitylabels[,2])

>mergeddata$activity <- activitylabels[mergeddata$activity]



#PART 4_APPROPRIATELY LABELS THE DATA SET WITH DESCRIPTIVE VARIABLE NAMES
>names(mergeddata)<-gsub("std()", "standarddev", names(mergeddata))

>names(mergeddata)<-gsub("mean()", "mean", names(mergeddata))

>names(mergeddata)<-gsub("^t", "time", names(mergeddata))

>names(mergeddata)<-gsub("tBody", "TimeBody", names(mergeddata))

>names(mergeddata)<-gsub("^f", "frequency", names(mergeddata))

>names(mergeddata)<-gsub("Acc", "Accelerometer", names(mergeddata))

>names(mergeddata)<-gsub("Gyro", "Gyroscope", names(mergeddata))

>names(mergeddata)<-gsub("Mag", "Magnitude", names(mergeddata))

>names(mergeddata)<-gsub("BodyBody", "Body", names(mergeddata))

#PART 5_FROM THE DATA SET IN STEP 4, CREATES A SECOND, INDEPENDENT TIDY DATA SET WITH THE AVERAGE OF EACH VARIABLE FOR EACH ACTIVITY AND EACH SUBJECT.

>tidydata<-aggregate(. ~subject + activity, mergeddata, mean)

>tidydata<-tidydata[order(tidydata$subject,tidydata$activity),]

>write.table(tidydata, file = "./Data/tidydata.txt",row.name=FALSE)

## Code Book
Human Activity Recognition Using Smartphones Dataset
Version 1.0

Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio, Luca Oneto.
Smartlab - Non Linear Complex Systems Laboratory
DITEN - Università degli Studi di Genova.
Via Opera Pia 11A, I-16145, Genoa, Italy.
activityrecognition@smartlab.ws
www.smartlab.ws


The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

For each record it is provided:

- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

Feature Selection 

The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

These signals were used to estimate variables of the feature vector for each pattern:  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

- timeBodyAccelerator-XYZ
- timeGravityAccelerator-XYZ
- timeBodyAcceleratorJerk-XYZ
- timeBodyGyroscope-XYZ
- timeBodyGyroscopeJerk-XYZ
- timeBodyAcceleratorMagnitude
- timeGravityAcceleratorMagnitude
- timeBodyAcceleratorJerkMagnitude
- timeBodyGyroscopeMagnitude
- timeBodyGyroscopeJerkMagnitude
- timeBodyAccelerator-XYZ
- timeBodyAcceleratorJerk-XYZ
- timeBodyGyroscope-XYZ
- timeBodyAcceleratorMagnitude
- timeBodyAcceleratorJerkMagnitude
- timeBodyGyroscopeMagnitude
- timeBodyGyroscopeJerkMagnitude

The set of variables that were estimated from these signals are: 

- mean(): Mean value
- standarddev(): Standard deviation
- mad(): Median absolute deviation 
- max(): Largest value in array
- min(): Smallest value in array
- sma(): Signal magnitude area
- energy(): Energy measure. Sum of the squares divided by the number of values. 
- iqr(): Interquartile range 
- entropy(): Signal entropy
- arCoeff(): Autorregresion coefficients with Burg order equal to 4
- correlation(): correlation coefficient between two signals
- maxInds(): index of the frequency component with largest magnitude
- meanFreq(): Weighted average of the frequency components to obtain a mean frequency
- skewness(): skewness of the frequency domain signal 
- kurtosis(): kurtosis of the frequency domain signal 
- bandsEnergy(): Energy of a frequency interval within the 64 bins of the FFT of each window.
- angle(): Angle between to vectors.

Additional vectors obtained by averaging the signals in a signal window sample. These are used on the angle() variable:

- gravityMean
- timeBodyAcceleratorMean
- timeBodyAcceleratorJerkMean
- timeBodyGyroscopeMean
- timeBodyGyroscopeJerkMean
