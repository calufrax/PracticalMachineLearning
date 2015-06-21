# PracticalMachineLearning

##Introduction
The purpose of this project is to analyse fitness tracker data as supplied, and use the training data set to build a predictor for the test data set.

##Analysis
The primary work was done in the cleaning of the data. After importing, a quick overview revealed many data columns unnecessary for analysis; these included user name, time and date stamps, and many columns unused, or containing NaN values. Additionally, given the number of entries, a random sample of 25% of the set was taken and used, so as to limit memory and computation time, taking the training set from 19622 entries x 160 variables, down to 4907 observations x 52 variables.

```pmltrain<-pmltrain[,7:160]
pmltest<-pmltest[,7:160]
cleanTrain<-apply(!is.na(pmltrain),2,sum)>19621
pmltrain<-pmltrain[,cleanTrain]

pmlSub<-createDataPartition(y=pmltrain$classe,p=0.25,list=FALSE)
pmlSubTrain <- pmltrain[pmlSub,]
pmlSubTrain <- pmlSubTrain[,-c(1,5,6,7,8,9,10,11,12,13,14,37,38,39,40,41,42,46,47,48,49,50,51,52,53,54,68,69,70,71,72,73,74,75,76)]
```

The reduced data set was then analysed using a basic random forest with 5-fold cross validation.

```pmlmodel<-train(classe~.,data=pmlSubTrain,method="rf",trControl=trainControl(method="cv",number=5))
```

The accuracy returned by the training subset, as given below, was determined to be sufficiently high that a greater proportion of the subset was not required.
 
    Resampling results across tuning parameters:

  mtry  Accuracy   Kappa      Accuracy SD  Kappa SD   
   2    0.9694320  0.9613064  0.007452919  0.009440061
  26    0.9708558  0.9631199  0.003131292  0.003964243
  51    0.9671861  0.9584797  0.005239389  0.006641565

  Accuracy was used to select the optimal model using  the largest value.
  The final value used for the model was mtry = 26.


  Cross-Validated (5 fold) Confusion Matrix 

  (entries are percentages of table totals)
 
            Reference
  Prediction    A    B    C    D    E
           A 28.2  0.7  0.0  0.0  0.0
           B  0.1 18.2  0.4  0.0  0.1
           C  0.1  0.4 16.8  0.4  0.2
           D  0.1  0.0  0.3 15.9  0.1
           E  0.0  0.0  0.0  0.0 18.0

The test set was then run using the model, and submitted. As all predicted results were correct, no further analysis was undertaken.

##Conclusion
This is a basic analysis, and contains a fraction of the complete training set; a more complete analysis would use a larger set, as well as the use of a validation set.

