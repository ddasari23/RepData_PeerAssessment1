---
title: "PA1_template1"
author: "Deepika Dasari"
date: "Sunday, April 19, 2015"
output:
  html_document
---

# Reproducible Research: Peer Assessment 1

## Loading and preprocessing the data
## Task 1: mean and median total steps per day
------


```r
setwd("C://Users/Meredith/Desktop/Deepika/DataScience/Reproducible Research/Assisgnment1")
steps<-read.csv("activity.csv")
```



## What is mean total number of steps taken per day?


```r
stepdays<-tapply(steps$steps, steps$date, sum)
##png("plot1.png",width=480, height=480, units="px")
hist(stepdays, main="Steps per day", xla="steps", yla="number of days")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

#### Mean and median: 

####mean

```r
mean(stepdays, na.rm=T)
```

```
## [1] 10766.19
```
####median

```r
median_val <-median(stepdays, na.rm=T)
```


## What is the average daily activity pattern?

### Plot time series:


```r
stepmin<-aggregate(steps$steps, list(steps$interval), mean, na.rm=T)
names(stepmin)<-c("interval", "steps")
plot(stepmin$interval, stepmin$steps, type="l")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 

### Get the interval with the most steps:

```r
stepmin[order(stepmin$steps),][288,]$interval
```

```
## [1] 835
```


## Imputing missing values

## number of missing data points:

```r
length(which(is.na(steps$steps)))
```

```
## [1] 2304
```
 
### steps ~ time model:

```r
steps$hour = floor(steps$interval/100)
steps$period=floor(steps$hour/3)
steps$period<-factor(steps$period)
levels(steps$period)<-c("0-2", "3-5", "6-8", "9-11", "12-14", "15-17", "18-20", "21-23")
mod<-lm(steps ~ period, data=steps)
mod
```

```
## 
## Call:
## lm(formula = steps ~ period, data = steps)
## 
## Coefficients:
## (Intercept)    period3-5    period6-8   period9-11  period12-14  
##       0.478        4.018       84.397       49.732       47.519  
## period15-17  period18-20  period21-23  
##      56.669       47.006        5.895
```

```r
steps$stepsi<-steps$steps
steps$stepsi[is.na(steps$steps)]<-predict(mod, newdata=steps[is.na(steps$steps),])
stepdaysi<-tapply(steps$stepsi, steps$date, sum, na.rm=T)
stepdaysi
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##   10766.19     126.00   11352.00   12116.00   13294.00   15420.00 
## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
##   11015.00   10766.19   12811.00    9900.00   10304.00   17382.00 
## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
##   12426.00   15098.00   10139.00   15084.00   13452.00   10056.00 
## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
##   11829.00   10395.00    8821.00   13460.00    8918.00    8355.00 
## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
##    2492.00    6778.00   10119.00   11458.00    5018.00    9819.00 
## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
##   15414.00   10766.19   10600.00   10571.00   10766.19   10439.00 
## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
##    8334.00   12883.00    3219.00   10766.19   10766.19   12608.00 
## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
##   10765.00    7336.00   10766.19      41.00    5441.00   14339.00 
## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
##   15110.00    8841.00    4472.00   12787.00   20427.00   21194.00 
## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
##   14478.00   11834.00   11162.00   13646.00   10183.00    7047.00 
## 2012-11-30 
##   10766.19
```

```r
hist(stepdaysi, main="Steps per day (with imputed data)", xla="steps", yla="number of days", col="#ff99ff")
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-1.png) 


## creating new variable 

```r
steps$stepsi<-steps$steps
steps$stepsi[is.na(steps$steps)]<-predict(mod, newdata=steps[is.na(steps$steps),])
             
stepdaysi<-tapply(steps$stepsi, steps$date, sum, na.rm=T)
stepdaysi
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##   10766.19     126.00   11352.00   12116.00   13294.00   15420.00 
## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
##   11015.00   10766.19   12811.00    9900.00   10304.00   17382.00 
## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
##   12426.00   15098.00   10139.00   15084.00   13452.00   10056.00 
## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
##   11829.00   10395.00    8821.00   13460.00    8918.00    8355.00 
## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
##    2492.00    6778.00   10119.00   11458.00    5018.00    9819.00 
## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
##   15414.00   10766.19   10600.00   10571.00   10766.19   10439.00 
## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
##    8334.00   12883.00    3219.00   10766.19   10766.19   12608.00 
## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
##   10765.00    7336.00   10766.19      41.00    5441.00   14339.00 
## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
##   15110.00    8841.00    4472.00   12787.00   20427.00   21194.00 
## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
##   14478.00   11834.00   11162.00   13646.00   10183.00    7047.00 
## 2012-11-30 
##   10766.19
```

```r
hist(stepdaysi, main="Steps per day (with imputed data)", xla="steps", yla="number of days", col="#ff99ff")
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10-1.png) 

####mean and median after imputing the data 

####mean

```r
mean(stepdaysi, na.rm=T)
```

```
## [1] 10766.19
```
####medain

```r
median_valnew <- median(stepdaysi, na.rm=T)
```
            
              
              
              
## Are there differences in activity patterns between weekdays and weekends?
              

```r
steps$ddate<-as.character(steps$date)
steps$ddate<-as.Date(steps$ddate, format="%Y-%m-%d")
steps$weekday<-weekdays(steps$ddate)
steps$weekend<-F
steps$weekend[steps$weekday %in% c("Saturday", "Sunday")]<-T

stepmin.i.weekdays<-aggregate(steps$stepsi[!steps$weekend], list(steps$interval[!steps$weekend]), mean, na.rm=T)
stepmin.i.weekends<-aggregate(steps$stepsi[steps$weekend], list(steps$interval[steps$weekend]), mean, na.rm=T)
names(stepmin.i.weekdays)<-c("interval", "steps")
names(stepmin.i.weekends)<-c("interval", "steps")
              
              
par(mfrow = c(2,1), mar = c(4, 4, 2, 1), oma = c(0, 0, 2, 0))
plot(stepmin.i.weekends$interval, stepmin.i.weekends$steps, pch="", ylab="Steps", xlab="", main="weekend", type="l", ylim=c(0,220), col="blue")
plot(stepmin.i.weekdays$interval, stepmin.i.weekdays$steps, pch="", ylab="Steps", xlab="", main="weekday", type="l",  ylim=c(0,220), col="darkred")
```

![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13-1.png) 

