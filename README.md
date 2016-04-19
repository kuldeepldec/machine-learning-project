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
library(corrplot)

In data there were lot of columns with NA and Div/0! showing missing values. So removing these columns and rows are important before we perform any analysis.

train <- read.csv('pml-training.csv',header=TRUE,na.strings=c('#DIV/0!', '', 'NA')) 
test <- read.csv('pml-testing.csv',header=TRUE,na.strings=c('#DIV/0!', '', 'NA')) 

summary(train)
summary(test)

We can check if 'NA' and '#Div/0! values are removed by
sum(train=='#DIV/0!', na.rm=TRUE)
sum(test=='#DIV/0!',na.rm=TRUE)

Next we trim our columns, as columns with removed NA's are blank.
dftrain<-train[, colSums(is.na(train))==0]
dftest<-train[, colSums(is.na(train))==0]


#### We now split our clean training data into two parts training data and cross validation data.
inTrain<-createDataPartition(y = dftrain$classe, p = 0.7, list = FALSE)
training<-dftrain[inTrain,]
validation<-dftrain[-inTrain,]

#### pre process data with PCA and visualizing correlation in training data

To visualize correlation in data we have to remove user name, timestamp and classe data. So we created new data with predictors using subset function. 
sf1<-subset(train,select=c(8:92))
M<-abs(cor(sf1[sapply(sf1, function(x) !is.factor(x))]))

To remove corelation with itself we use below function 
diag(M)<-0

to visaulize the data 
















