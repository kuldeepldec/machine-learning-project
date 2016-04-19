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
library(randomForest)

In data there were lot of columns with NA and Div/0! showing missing values. So removing these columns and rows are important before we perform any analysis.

train <- read.csv('pml-training.csv',header=TRUE,na.strings=c('#DIV/0!', '', 'NA')) 
test <- read.csv('pml-testing.csv',header=TRUE,na.strings=c('#DIV/0!', '', 'NA')) 

summary(train)
summary(test)

We can check if 'NA' and '#Div/0! values are removed by
sum(train=='#DIV/0!', na.rm=TRUE)
sum(test=='#DIV/0!',na.rm=TRUE)

Next we trim our columns, as columns with removed NA's are blank and removing timestamp and user name data.
dftrain<-train[, colSums(is.na(train))==0]
dftest<-test[, colSums(is.na(test))==0]

dftrain<-subset(dftrain,select=c(8:60))
dftest<-subset(dftest,select=c(8:60))


#### We now split our clean training data into two parts training data and cross validation data.
inTrain<-createDataPartition(y = dftrain$classe, p = 0.7, list = FALSE)
training<-dftrain[inTrain,]
validation<-dftrain[-inTrain,]

#### pre process data with PCA and visualizing correlation in training data. The reason for doing pca was it help in reducing predictor variables and getting same accuracy as training data with more variable.

To visualize correlation in data we have to find corelation between predictor variables 
M<-abs(cor(training[sapply(training, function(x) !is.factor(x))]))

To remove corelation with itself we use below function to put all the diagnol value to 0
diag(M)<-0

To visualize the correlation between different variable we use below function. I have added my corrplot with readme.md as file name corrplot.jpeg. Blue line show the variables with highest correlation and red show least correlation.

corrplot(M,type="lower")

##### We pre-process our data using a principal component analysis in caret package, leaving out last column classe. After pre-processing we use 'predict' function to apply pca on test and training data.

preProc<- preProcess(training[, -53], method = "pca", thresh = 0.99)
trainPC<-predict(preProc,training[,-53])
validPC<-predict(preProc,validation[,-53])

Now use random forest to train the model on training data using classe variable. We have used randomForest instead of train function as train function takes more time.

modelFit<-randomForest(training$classe ~.,data=trainPC,importance=TRUE)

####We can visualize the confusion matrix and variable importance from below functions. We have attached principal component importance plot in (prinComp.jpeg) file. The plot show principal componenets on Y-axis and accuracy in X-axis. Points with the highest accuracy are at the top.

print(modelFit)
importance(modelFit)
varImpPlot(modelFit$finalModel,sort=TRUE,main="principal components")

#### For validating model on validation data and how the model actually fit we predict our result on validation data.
pred_valid<-predict(modelFit,validPC)
confusion<-confusionMatrix(validation$classe,pred_valid)
confusion$table

####To find how much accurate our prediction is , we check accuracy and out of sample error of our model on vaildation data. 
accur <- postResample(validation$classe, pred_valid)
outSamperror<-1-accur[[1]]

The estimated accuracy of the model is 98.07% and the estimated out-of-sample error based on fitted model applied to the cross validation data is 1.92%.

##### final test on test data set, so first we pre-process our data set and remove last column of problem_id. We ran our result on test data set and predict result
testPC <- predict(preProc, dftest[,-53])
predFinal <- predict(modelFit, testPC)

I have shown final result in a final_result.jpeg, showing my confusion matrix and final predicted result
























