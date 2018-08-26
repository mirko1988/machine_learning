---
title: "Final_Project_Machine_Learning"
author: "Mirko Brogioli"
date: "26 agosto 2018"
output: 
  html_document: 
    keep_md: yes
---



## Final Project Machine Learning

One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. So, in this project, my goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants to determine in what way a particular exercise, that is burbell lift, has been done. 
To do this, I used a dataset composed by 19622 observation and 160 variables. The variable that identifies the quality of the exercise is the variable "classe". To the participants have been asked to perform barbell lifts correctly and incorrectly in 5 different ways classified by the letters A, B, C, D, E. 
After having built an adequate model to predict the quality of the exercise with the available variables, my goal will be that to test my model on a testing set formed by 20 observation.

## Cross Validation and out of sample error

The data are supplied in two different datasets. The first, the training dataset, is that used to build the model. The second, the testing dataset, is that used to make a prediction on 20 observation for which are not available the values of the "classe" variable.
So, in terms of cross validation, I didn't perform any particular activities because the datasets have been provided already ready to be used. In an another case, I should have divided the entire dataset in two specific datasets (training on which estimate the prediction model and testing on which to verify the correctness and the adequacy of the model) using different techniques (for instance k-fold, leave one out etc.)

The out of sample error will be of course bigger than the in of sample error due to the fact that the model is parameterized on a specific dataset that differ from any other dataset. In addition, there could be a phenomenon of overfitting that could increase this aspect.

## Getting and cleaning data

Observing the datasets, I noticed that a huge number of variables were completely formed by NA values. So, I have proceeded excluding these variables and, subsequently, I focused on my attention on same particular predictors (gyros, accelerator and magnet).


```r
setwd("F:/COURSERA/Practical Machine Learning/4° week")
fileUrl<-"https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"
download.file(fileUrl,destfile = "F:/COURSERA/Practical Machine Learning/4° week/training.csv",method = "curl")
training<-read.csv("training.csv")

fileUrl<-"https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv"
download.file(fileUrl,destfile = "F:/COURSERA/Practical Machine Learning/4° week/testing.csv",method = "curl")
testing<-read.csv("testing.csv")

library(dplyr)
library(caret)

not_any_na <- function(x) all(!is.na(x))
a<-training %>% select_if(not_any_na)
b<-select(a,starts_with("gyros"))
c<-select(a,starts_with("accel"))
d<-select(a,starts_with("magnet"))
training_2<-cbind(classe=a$classe,b,c,d)

a_1<-testing %>% select_if(not_any_na)
b_1<-select(a_1,starts_with("gyros"))
c_1<-select(a_1,starts_with("accel"))
d_1<-select(a_1,starts_with("magnet"))
testing_2<-cbind(b_1,c_1,d_1)
```

## Model

I tried to estimate three different models. The first model is a regression tree, the second is a linear discriminant analysis model and the third is a quadratic discriminat analysis model. As we can see from the result tables below, the model with an higher accuracy is the quadratic model which I have choosen to describe the underlying phenomenon.

Regression Tree - accuracy 37%

```
   
       A    B    C    D    E
  A 3711    0    0 1863    6
  B 1436    0    0 2360    1
  C 1560    0    0 1862    0
  D  640    0    0 2576    0
  E  560    0    0 2019 1028
```

Linear discriminant analysis - accuracy 63%

```
   
       A    B    C    D    E
  A 4259  254  546  455   66
  B  702 2304  424  157  210
  C  609  285 2067  387   74
  D  244  252  443 1953  324
  E  246  677  294  506 1884
```

Quadratic discriminant analysis - accuracy 81%

```
   
       A    B    C    D    E
  A 4187  453  724  159   57
  B  130 3182  406   11   68
  C   20  268 3112   10   12
  D   40   34  720 2355   67
  E   10  172  137  179 3109
```

## Prediction

Here is reported the prediction on the testing dataset on the 20 observation requested.


```
 [1] C A B C A E D B A A C C B A E E A B B B
Levels: A B C D E
```
