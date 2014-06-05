# Reproducible Research: Peer Assessment 1

## Loading and preprocessing the data

Load data from CSV file:

```r
data <- read.csv("activity.csv", header=TRUE, stringsAsFactors=FALSE)
```

Convert variable "date" to proper R date format:

```r
data$date <- as.Date(strptime(data$date, format="%Y-%m-%d"))
```

## What is mean total number of steps taken per day?

Analysis of mean total number of steps taken per day, for this we omit
incomplete measurements:

```r
comp.data <- data[complete.cases(data),]
```

Calculate number of steps per day and assign meaning variables names:

```r
daily.steps <- aggregate(comp.data$steps,list(comp.data$date),FUN=sum)
col.names <- c("date","total.steps")
colnames(daily.steps) <- col.names
```

Plot total number of steps per day:

```r
hist(daily.steps$total.steps,breaks=10,col="red",main="total number of steps per day",xlab="number of steps per day",ylab="frequency")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1.png) 

Calculate mean and median of total number of steps per day:

```r
paste("Mean value of total number of steps per day:",format(mean(daily.steps$total.steps),digits=5))
```

```
## [1] "Mean value of total number of steps per day: 10766"
```

```r
paste("Median value of total number of steps per day:",median(daily.steps$total.steps))
```

```
## [1] "Median value of total number of steps per day: 10765"
```

## What is the average daily activity pattern?

Calculate average number of steps taken per 5-minute interval:

```r
interval.avg <- aggregate(comp.data$steps,list(comp.data$interval),FUN=mean,na.omit=TRUE)
col.names <- c("interval.no","avg.steps")
colnames(interval.avg) <- col.names
```

Plot average no of steps per interval:

```r
plot(interval.avg,type="l",main="average no of steps per 5-minute interval",xlab="interval number","average no of steps")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

Find interval with maximum number of steps:

```r
max.int <- sort(interval.avg$avg.steps,decreasing=TRUE,index.return=TRUE)
paste("Index of interval with maximum steps (daily average):",max.int$ix[1])
```

```
## [1] "Index of interval with maximum steps (daily average): 104"
```

## Imputing missing values

Calculate total number of rows with missing values:

```r
paste("Number of rows with missing values:",nrow(data)-nrow(comp.data))
```

```
## [1] "Number of rows with missing values: 2304"
```


## Are there differences in activity patterns between weekdays and weekends?

