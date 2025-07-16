
---
title: "activity_LDA"
output: html_document
---

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:


``` r
summary(cars)
```

```
##      speed           dist       
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
```

## Including Plots

You can also embed plots, for example:

![plot of chunk pressure](figure/pressure-1.png)

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.


---
title: "Module2_Assignment_LDA"
author: "Lale Dinc Asarcikli"
date: "10-07-2025"
output: html_document
---



# Loading the data


``` r
library(readr)
activity <- read_csv("activity.csv")
```

```
## Error: 'activity.csv' does not exist in current working directory ('\\vf-lumc-home.lumcnet.prod.intern/lumc-home$/ldincasarcikli/MyDocs/ReproducibleResearch LDA').
```


# What is mean total number of steps taken per day?


## Make a histogram of the total number of steps taken each day

``` r
library(dplyr)
library(ggplot2)

ggplot(total_steps_per_date, aes(x = date, y = total_steps)) +
  geom_bar(stat = "identity", fill = "gray", color = "black", alpha = 0.7) +
  labs(title = "Total Steps Per Day", x = "Date", y = "Total Steps") +
  theme_minimal()
```

```
## Error: object 'total_steps_per_date' not found
```
## Step 1: Calculate the total number of steps taken per day


``` r
library(tidyverse)
total_steps_per_date <- activity %>% 
  group_by(date) %>% 
  summarize(total_steps = sum(steps, na.rm = TRUE))
```

```
## Error: object 'activity' not found
```

``` r
print(total_steps_per_date)
```

```
## Error: object 'total_steps_per_date' not found
```


## Step 2: Mean of total steps per day

``` r
mean_total_steps <- mean(total_steps_per_date$total_steps)
```

```
## Error: object 'total_steps_per_date' not found
```

``` r
median_total_steps <- median(total_steps_per_date$total_steps)
```

```
## Error: object 'total_steps_per_date' not found
```

``` r
mean_total_steps
```

```
## Error: object 'mean_total_steps' not found
```

``` r
median_total_steps
```

```
## Error: object 'median_total_steps' not found
```
## Calculate and report the mean and median of the total number of steps taken per day


``` r
activity %>% 
  group_by(date) %>% 
  summarize(total_steps_mean = mean(steps, na.rm = TRUE),
            total_steps_median = median(steps, na.rm = TRUE))
```

```
## Error: object 'activity' not found
```

# What is the average daily activity pattern?

## Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


``` r
activity_time_series <- activity %>%
  group_by(interval) %>%
  summarize(average_steps = mean(steps, na.rm = TRUE))
```

```
## Error: object 'activity' not found
```

``` r
ggplot(activity_time_series, aes(x = interval, y = average_steps)) +
  geom_line() +  
  labs(title = "Average Daily Activity Pattern",
       x = "5-Minute Interval", 
       y = "Average Number of Steps") +
  theme_minimal()
```

```
## Error: object 'activity_time_series' not found
```



## Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


``` r
activity_time_series %>%
  filter(average_steps == max(average_steps, na.rm = TRUE))
```

```
## Error: object 'activity_time_series' not found
```

The answer is 835.

# Imputing missing values

## Calculate and report the total number of missing values in the dataset


``` r
missing_rows <- activity %>% filter(is.na(steps))
```

```
## Error: object 'activity' not found
```

``` r
nrow(missing_rows)
```

```
## Error: object 'missing_rows' not found
```


## Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated.
impute missing `steps`value with the **mean for that 5-minute interval**, averaged across all days.


``` r
### Calculate the mean steps for each 5-minute interval (across all days)
interval_means <- activity %>%
  group_by(interval) %>%
  summarize(mean_steps = mean(steps, na.rm = TRUE))
```

```
## Error: object 'activity' not found
```
### 3. Create New Dataset with Missing Data Filled In


``` r
# Step 2: Join and impute missing values
activity_imputed <- activity %>%
  left_join(interval_means, by = "interval") %>%
  mutate(steps = ifelse(is.na(steps), mean_steps, steps)) %>%
  select(date, interval, steps)
```

```
## Error: object 'activity' not found
```


### 4. Histogram of Total Steps per Day (After Imputation)


``` r
# Summarize total steps per day
daily_steps_imputed <- activity_imputed %>%
  group_by(date) %>%
  summarize(total_steps = sum(steps))
```

```
## Error: object 'activity_imputed' not found
```

``` r
# Plot histogram
hist(daily_steps_imputed$total_steps,
     main = "Histogram of Total Steps Per Day (Imputed Data)",
     xlab = "Total Steps",
     col = "darkgreen",
     border = "white")
```

```
## Error: object 'daily_steps_imputed' not found
```

---

### Mean and Median After Imputation


``` r
mean_imputed <- mean(daily_steps_imputed$total_steps)
```

```
## Error: object 'daily_steps_imputed' not found
```

``` r
median_imputed <- median(daily_steps_imputed$total_steps)
```

```
## Error: object 'daily_steps_imputed' not found
```

``` r
mean_imputed
```

```
## Error: object 'mean_imputed' not found
```

``` r
median_imputed
```

```
## Error: object 'median_imputed' not found
```

---

### 🔍 Comparison and Impact


``` r
# Original summary (without imputation)
daily_steps_original <- activity %>%
  group_by(date) %>%
  summarize(total_steps = sum(steps, na.rm = TRUE))
```

```
## Error: object 'activity' not found
```

``` r
mean_original <- mean(daily_steps_original$total_steps, na.rm = TRUE)
```

```
## Error: object 'daily_steps_original' not found
```

``` r
median_original <- median(daily_steps_original$total_steps, na.rm = TRUE)
```

```
## Error: object 'daily_steps_original' not found
```

``` r
comparison <- data.frame(
  Version = c("Original", "Imputed"),
  Mean = c(mean_original, mean_imputed),
  Median = c(median_original, median_imputed)
)
```

```
## Error: object 'mean_original' not found
```

``` r
comparison
```

```
## Error: object 'comparison' not found
```

---

### Interpretation


Before imputation:
Some days have missing values, so sum(steps) for those days is lower or zero, which pulls the mean and median down.

After imputation:
All missing steps values are filled using the mean for each interval. That means those days that were previously incomplete (or 0) now have a realistic total step count based on averages.
This increases both the mean and the median because we're no longer underestimating.





# Are there differences in activity patterns between weekdays and weekends?

## Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.


``` r
activity_weekday <- activity_imputed %>% 
  mutate(weekday_or_end = ifelse(weekdays(date) %in% c("Saturday", "Sunday"), "weekend", "weekday")) %>%
  mutate(weekday_or_end = factor(weekday_or_end))
```

```
## Error: object 'activity_imputed' not found
```

``` r
print(activity_weekday)
```

```
## Error: object 'activity_weekday' not found
```

## Make a panel plot containing a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis).



``` r
# Calculate average steps for each interval and day type
activity_daytype_summary <- activity_weekday %>%
  group_by(interval, weekday_or_end) %>%
  summarize(average_steps = mean(steps), .groups = "drop")
```

```
## Error: object 'activity_weekday' not found
```

``` r
# Panel time series plot using facet_wrap
ggplot(activity_daytype_summary, aes(x = interval, y = average_steps)) +
  geom_line(color = "steelblue") +
  facet_wrap(~ weekday_or_end, ncol = 1, scales = "free_y") +
  labs(title = "Average Daily Activity Patterns: Weekday vs Weekend",
       x = "5-Minute Interval",
       y = "Average Number of Steps") +
  theme_minimal()
```

```
## Error: object 'activity_daytype_summary' not found
```




In **weekends**, step activity tends to start later in the day and shows **increased movement in the afternoon**, suggesting more flexible or leisure-driven activity patterns.  
In contrast, **weekday** activity has an earlier peak — likely related to commuting or scheduled routines.


install.packages("knitr")
library (knitr)  
knit2html("LDA_assignment2.Rmd")


