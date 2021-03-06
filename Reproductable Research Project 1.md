#Introduction from Coursera 
It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the "quantified self" movement - a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.

The data for this assignment can be downloaded from the course web site:

Dataset: Activity monitoring data [52K]
The variables included in this dataset are:

steps: Number of steps taking in a 5-minute interval (missing values are coded as \color{red}{\verb|NA|}NA)
date: The date on which the measurement was taken in YYYY-MM-DD format
interval: Identifier for the 5-minute interval in which measurement was taken
The dataset is stored in a comma-separated-value (CSV) file and there are a total of 17,568 observations in this dataset.

This assignment will be described in multiple parts. You will need to write a report that answers the questions detailed below. Ultimately, you will need to complete the entire assignment in a single R markdown document that can be processed by knitr and be transformed into an HTML file.

Throughout your report make sure you always include the code that you used to generate the output you present. When writing code chunks in the R markdown document, always use \color{red}{\verb|echo = TRUE|}echo = TRUE so that someone else will be able to read the code. This assignment will be evaluated via peer assessment so it is essential that your peer evaluators be able to review the code for your analysis.

For the plotting aspects of this assignment, feel free to use any plotting system in R (i.e., base, lattice, ggplot2)

Fork/clone the GitHub repository created for this assignment. You will submit this assignment by pushing your completed files into your forked repository on GitHub. The assignment submission will consist of the URL to your GitHub repository and the SHA-1 commit ID for your repository state.

# Submission

```{r}
library(knitr)
opts_chunk$set(echo=TRUE)
```

```{r}
library(ggplot2)
library(dplyr)
```

```{r}
fileurl <- 'https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip'
if(!file.exists('activity.csv')) {
download.file(fileurl, 'zipfile.zip', method='curl')
unzip('zipfile.zip')
file.remove('zipfile.zip')
}
activity <- read.csv('activity.csv')
```

```{r}
activity <- read.csv('activity.csv')
str(activity)
activity$date <- as.Date(activity$date, '%Y-%m-%d')
activity$interval <- sprintf("%04d", activity$interval)
activity$interval <- format(strptime(activity$interval, format="%H%M"), format = "%H:%M")
```


```{r}
steps <- tapply(activity$steps, activity$date, sum, na.rm=T)
```


```{r}
steps %>% qplot(xlab='steps per day', ylab='frequency', binwidth=700)
```

```{r}
mean <- mean(daysteps)
median <- median(daysteps)
```

```{r}
avgsteps <- activity %>% filter(!is.na(steps)) %>% group_by(interval) %>% 
summarize(steps = mean(steps))
avgsteps %>% ggplot(aes(x=interval, y=steps, group=1)) + geom_line()
```

```{r}
maxsteps <- average_steps[which.max(average_steps$steps), ][[1]]
```

```{r}
missingvalues <- sapply(activity, is.na) %>% sum
```

```{r}
stepsfinal <- activity$steps
stepsfinal[is.na(stepsfinal)] <- round(mean(activity$steps, na.rm = T), digits=0)
stepsfinal <- as.numeric(stepsfinal)
activityfinal <- cbind.data.frame(stepsfinal, activity$date, activity$interval)
colnames(activityfinsl) <- colnames(activity)
```

```{r}
stepsday <- tapply(activity_finsl$steps, activity_final$date, sum)
stepsday %>% qplot(xlab='steps per day', ylab='frequency')
```


```{r}
meanfinal <- mean(stepsday)
medianfinal <- median(stepsday)
```

```{r}
activityfinal$date <- as.Date(activity_complete$date)
weekday <-c('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday')
activityfinal$weekday <- factor((weekdays(activity_complete$date) %in% weekd),
                                     levels = c(FALSE, TRUE), labels = c('weekend', 'weekday'))
```


```{r}
averagestepsfinal <- activityfinal %>% group_by(interval, weekdayz) %>%
summarise(steps = mean(steps))
averagestepsfinal %>% ggplot(aes(x=interval, y=steps, group=1)) +
        geom_line() + 
        facet_grid(weekdays~.)
        
```