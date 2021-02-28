## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax
for authoring HTML, PDF, and MS Word documents. For more details on
using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that
includes both content as well as the output of any embedded R code
chunks within the document. You can embed an R code chunk like this:

    #read the file into dataframe

    #set working directory

    setwd("C:/Users/rp303/OneDrive/Documents/coursera data science/ReproducibleResearchProject1")

    #create file

    filename <- "repdata_data_activity.zip"

    ## Check if the file exists already in the directory and if not, download and unzip the files in a folder
    ##named UCI HAR Dataset

    if (!file.exists(filename)){
      fileURL <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip "
      download.file(fileURL, filename, method="curl")
    }  
    if (!file.exists("repdata_data_activity")) { 
      unzip(filename) 
    }

    # load activity excel file into a data table
    activitydata = read.csv("activity.csv")                                                                            
    summary(activitydata)

    ##      steps            date              interval     
    ##  Min.   :  0.00   Length:17568       Min.   :   0.0  
    ##  1st Qu.:  0.00   Class :character   1st Qu.: 588.8  
    ##  Median :  0.00   Mode  :character   Median :1177.5  
    ##  Mean   : 37.38                      Mean   :1177.5  
    ##  3rd Qu.: 12.00                      3rd Qu.:1766.2  
    ##  Max.   :806.00                      Max.   :2355.0  
    ##  NA's   :2304

    head(activitydata)

    ##   steps       date interval
    ## 1    NA 2012-10-01        0
    ## 2    NA 2012-10-01        5
    ## 3    NA 2012-10-01       10
    ## 4    NA 2012-10-01       15
    ## 5    NA 2012-10-01       20
    ## 6    NA 2012-10-01       25

## What is the mean total number of steps taken per day?

\#ignore the missing values

    #calculate the total number of steps by day
    data2sum<-aggregate(x=activitydata$steps, by =list(activitydata$date), FUN=sum)
    data2sum

    ##       Group.1     x
    ## 1  2012-10-01    NA
    ## 2  2012-10-02   126
    ## 3  2012-10-03 11352
    ## 4  2012-10-04 12116
    ## 5  2012-10-05 13294
    ## 6  2012-10-06 15420
    ## 7  2012-10-07 11015
    ## 8  2012-10-08    NA
    ## 9  2012-10-09 12811
    ## 10 2012-10-10  9900
    ## 11 2012-10-11 10304
    ## 12 2012-10-12 17382
    ## 13 2012-10-13 12426
    ## 14 2012-10-14 15098
    ## 15 2012-10-15 10139
    ## 16 2012-10-16 15084
    ## 17 2012-10-17 13452
    ## 18 2012-10-18 10056
    ## 19 2012-10-19 11829
    ## 20 2012-10-20 10395
    ## 21 2012-10-21  8821
    ## 22 2012-10-22 13460
    ## 23 2012-10-23  8918
    ## 24 2012-10-24  8355
    ## 25 2012-10-25  2492
    ## 26 2012-10-26  6778
    ## 27 2012-10-27 10119
    ## 28 2012-10-28 11458
    ## 29 2012-10-29  5018
    ## 30 2012-10-30  9819
    ## 31 2012-10-31 15414
    ## 32 2012-11-01    NA
    ## 33 2012-11-02 10600
    ## 34 2012-11-03 10571
    ## 35 2012-11-04    NA
    ## 36 2012-11-05 10439
    ## 37 2012-11-06  8334
    ## 38 2012-11-07 12883
    ## 39 2012-11-08  3219
    ## 40 2012-11-09    NA
    ## 41 2012-11-10    NA
    ## 42 2012-11-11 12608
    ## 43 2012-11-12 10765
    ## 44 2012-11-13  7336
    ## 45 2012-11-14    NA
    ## 46 2012-11-15    41
    ## 47 2012-11-16  5441
    ## 48 2012-11-17 14339
    ## 49 2012-11-18 15110
    ## 50 2012-11-19  8841
    ## 51 2012-11-20  4472
    ## 52 2012-11-21 12787
    ## 53 2012-11-22 20427
    ## 54 2012-11-23 21194
    ## 55 2012-11-24 14478
    ## 56 2012-11-25 11834
    ## 57 2012-11-26 11162
    ## 58 2012-11-27 13646
    ## 59 2012-11-28 10183
    ## 60 2012-11-29  7047
    ## 61 2012-11-30    NA

    #create a histogram of the steps each day
    plot1<-hist(data2sum$x, main="Total Number of Steps Each Day")

![](PA1_template_files/figure-markdown_strict/Mean%20Total%20Steps-1.png)

    png(filename="plot1.png")
    plot(plot1)
    dev.off()

    ## png 
    ##   2

    #calculate the mean and ignore the NAs
    data2summean<-mean(data2sum$x, na.rm = TRUE)
    cat("The mean total of the total number of steps = ", data2summean)

    ## The mean total of the total number of steps =  10766.19

    #calculate the median and ignore the NAs
    data2summedian<-median(data2sum$x, na.rm = TRUE)
    cat("The median total of the total number of steps = ", data2summedian)

    ## The median total of the total number of steps =  10765

\#What is average daily activity pattern? Make a time series plot
(i.e. type = “l”) of the 5-minute interval (x-axis) and the average
number of steps taken, averaged across all days (y-axis) Which 5-minute
interval, on average across all the days in the dataset, contains the
maximum number of steps?

    #calculate the average steps by interval
    #calculate the avarage number of steps by interval

    avgactivitydata<-aggregate(steps~interval,data=activitydata, FUN=mean, na.rm=TRUE)

    #look at avgactivitydata
    head(avgactivitydata)

    ##   interval     steps
    ## 1        0 1.7169811
    ## 2        5 0.3396226
    ## 3       10 0.1320755
    ## 4       15 0.1509434
    ## 5       20 0.0754717
    ## 6       25 2.0943396

    #make a time series plot of interval vs. mean steps
    cbPalette <- c("#999999", "#E69F00", "#56B4E9", "#009E73", "#F0E442", "#0072B2", "#D55E00", "#CC79A7")

    plot2<-ggplot(avgactivitydata, aes(x = interval, y = steps, color = steps)) +
      geom_line()  + labs(title = "Average daily activity pattern", 
           x = "Interval", y = "Steps") +scale_colour_gradientn(colours = c("darkred", "orange", "green", "black"))


    plot2

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-1-1.png)

    ggsave("plot2.png",plot=plot2)

    ## Saving 7 x 5 in image

    #which 5 min. interval contains max num. steps
    maxsteps<-avgactivitydata[which.max(avgactivitydata$steps),]

    cat("The maximum average steps were",maxsteps$steps, "and occurred in interval", maxsteps$interval)

    ## The maximum average steps were 206.1698 and occurred in interval 835

\#Imputing missing values Note that there are a number of days/intervals
where there are missing values (coded as NA). The presence of missing
days may introduce bias into some calculations or summaries of the data.

Calculate and report the total number of missing values in the dataset
(i.e. the total number of rows with NAs) Devise a strategy for filling
in all of the missing values in the dataset. The strategy does not need
to be sophisticated. For example, you could use the mean/median for that
day, or the mean for that 5-minute interval, etc. Create a new dataset
that is equal to the original dataset but with the missing data filled
in. Make a histogram of the total number of steps taken each day and
Calculate and report the mean and median total number of steps taken per
day. Do these values differ from the estimates from the first part of
the assignment? What is the impact of imputing missing data on the
estimates of the total daily number of steps?

    #Calc the number of missing values in the dataset

    nummissing<-sum(is.na(activitydata$steps))

    cat("The number of missing values is:", nummissing)

    ## The number of missing values is: 2304

    #Use mean for missing data
    activitydata2<-activitydata
    sapply(activitydata2,class)

    ##       steps        date    interval 
    ##   "integer" "character"   "integer"

    activitydata2$steps[is.na(activitydata2$steps)]<-mean(na.omit(activitydata$steps))
    activitydata2$date<-as.Date(activitydata2$date, format="%Y-%m-%d")

    #total steps by day
    totalstepsday2<-aggregate(steps ~ date, rm.na=TRUE,data=activitydata2,FUN = sum)

    plot3<- ggplot(totalstepsday2, aes(x = steps)) + 
      geom_histogram(binwidth = 1000) + 
      labs(title = "Histogram of the total number of steps", 
           x = "Number of steps per day", y = "Interval")

    plot3

![](PA1_template_files/figure-markdown_strict/imputting%20missing%20values-1.png)

    ggsave("plot3.png",plot=plot3)

    ## Saving 7 x 5 in image

    #calc mean, median for both activitydata2 
    meanactivitydata2<-mean(totalstepsday2$steps)
    medianactivitydata2<-median(totalstepsday2$steps)

    cat("The mean steps is:",meanactivitydata2)

    ## The mean steps is: 10767.19

    cat("The median steps is:",medianactivitydata2)

    ## The median steps is: 10767.19

\#Are there differences in activity patterns between weekdays and
weekends? For this part the weekdays() function may be of some help
here. Use the dataset with the filled-in missing values for this part.

Create a new factor variable in the dataset with two levels – “weekday”
and “weekend” indicating whether a given date is a weekday or weekend
day. Make a panel plot containing a time series plot (i.e. type = “l”)
of the 5-minute interval (x-axis) and the average number of steps taken,
averaged across all weekday days or weekend days (y-axis). See the
README file in the GitHub repository to see an example of what this plot
should look like using simulated data.

    #use the data set where nas are filled; activitydata2 and determine what is a weekday or weekend
    activitydata3<-activitydata2

    activitydata3$weekday<-weekdays(activitydata3$date)

    #differentiate between weekend and weekday with Saturday and Sunday as weekends
    activitydata3$daytype<-ifelse(activitydata3$weekday=='Saturday'|activitydata3$weekday=='Sunday', 'weekend', 'weekday')

    #graph weekdays versus weekends
    stepsperday<-aggregate(steps ~ interval+daytype, data=activitydata3, FUN=mean, action=na.omit)

    plot4<-ggplot(stepsperday, aes(interval, steps))
    plot5<-plot4+geom_line(col="darkred")+ggtitle("Average steps per time interval: weekdays vs. weekends")+xlab("Time")+ylab("Steps")+theme(plot.title = element_text(face="bold", size=12))+facet_grid(daytype ~ .)

      
    ggsave("plot4.png",plot=plot5)

    ## Saving 7 x 5 in image

Comparing the two graphs it does look like there is a difference between
the daily average activity levels on the weekends versus weekdays
probably due to people having more free time and daytime hours to get
outside and away from their desks.
