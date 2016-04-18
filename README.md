# machine-learning-project using pml training and testing data

####Summary and goal of the project:

The project use weight lifting data set from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. 
They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. 

The goal of the project is to predict the manner in which participant did the exercise using "classe" variable in the training set.
Create a report describing how model is build, how it is cross validated and report on out of sample error.
Use this prediction model to predict 20 different test cases in test data set.


####Training and test data set
The training data used for the project is:
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv
The test data are available here:
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv
The data for this project come from this source: http://groupware.les.inf.puc-rio.br/har

####Summary of raw training and testing data set and installing libraries for the project 
library(caret)
dftrain<-read.csv("pml-training.csv")
dftest<-read.csv("pml-testing.csv")






