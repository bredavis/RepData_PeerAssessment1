---
output: 
  html_document: 
    keep_md: yes
---
Loading and preprocessing the data
===================================
First, load the data using read.csv()

```r
data <- read.csv("activity.csv")
library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 3.2.2
```

```r
library(plyr)
```

What is mean total number of steps taken per day?
==================================================
Calculate the total number of steps taken per day

```r
sum(data$steps, na.rm = TRUE)
```

```
## [1] 570608
```

Make a histogram of the total number of steps taken each day

```r
hist(data$steps)
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

Calculate and report the mean and median of the total number of steps taken per day
Mean:

```r
avgstep <- mean(data$steps, na.rm = TRUE)
avgstep
```

```
## [1] 37.3826
```

Median:

```r
median(data$steps, na.rm = TRUE)
```

```
## [1] 0
```

What is the average daily activity pattern?
============================================
Make a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
a <- ddply(data, "interval", summarize, fun = mean(steps, na.rm=TRUE))
g <- qplot(interval, fun, data = a)
g + geom_area()
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png) 

Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
b <- a[order(-a$fun),]
b[1,1]
```

```
## [1] 835
```

Imputing missing values
===========================
Calculate and report the total number of missing values in the dataset 

```r
c <- is.na(data[,1])
sum(c)
```

```
## [1] 2304
```

Devise a strategy for filling in all of the missing values in the dataset. Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
data2 <- data
data2[is.na(data2)] <- avgstep
```

Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
hist(data2$steps)
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10-1.png) 

Are there differences in activity patterns between weekdays and weekends?
======================================================
Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day


Make a panel plot containing a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis) 

