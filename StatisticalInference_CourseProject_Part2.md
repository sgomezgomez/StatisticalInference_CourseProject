---
title: "Inferential Data Analysis for Tooth Growth in Guinea Pigs with different Treatments and Doses"
author: "Sebastian Gomez Gomez"
output: 
  pdf_document: 
    keep_md: yes
---


# Report - Part II: Basic Inferential Data Analysis

## Overview

This report includes loading a preparing the ToothGrowth data set. Also demonstrates a basic exploratory data analysis showing summary statistics and boxplots of the different Treatment and Dose levels. Finally, it performs two sided T-tests to validate whether Treatment and Dose levels have a significant impact on the tooth growth registered in Guinea Pigs.

## Loading and Preparing Data

Below is the code to load the ToothGrowth dataset from the datasets R library.


```r
data('ToothGrowth'); ToothGrowth$dose <- as.factor(ToothGrowth$dose)
```

## Basic Data Summary

Below are the aggregated summary (min, 25th, median, mean, 75th and max values) statistics for each of the Treatment and Dose combinations:


```r
tg_agg_summary <- aggregate(ToothGrowth$len, by = list(ToothGrowth$supp, ToothGrowth$dose), FUN = 'summary')
print(tg_agg_summary)
```

```
##   Group.1 Group.2 x.Min. x.1st Qu. x.Median x.Mean x.3rd Qu. x.Max.
## 1      OJ     0.5  8.200     9.700   12.250 13.230    16.175 21.500
## 2      VC     0.5  4.200     5.950    7.150  7.980    10.900 11.500
## 3      OJ       1 14.500    20.300   23.450 22.700    25.650 27.300
## 4      VC       1 13.600    15.275   16.500 16.770    17.300 22.500
## 5      OJ       2 22.400    24.575   25.950 26.060    27.075 30.900
## 6      VC       2 18.500    23.375   25.950 26.140    28.800 33.900
```

Additionally, here's the box plot for each of the Treatment and Dose combinations:


```r
print({g <- ggplot(ToothGrowth, aes(x = dose, y = len))
        g + geom_boxplot() + ggtitle("Boxplot per Dosage level") + 
        xlab('Dose') + ylab('Tooth length')})
print({g <- ggplot(ToothGrowth, aes(x = supp, y = len))
        g + geom_boxplot() + ggtitle("Boxplot per Treatment level") + 
        xlab('Treatment') + ylab('Tooth length')})
```


\includegraphics[width=0.5\linewidth]{StatisticalInference_CourseProject_Part2_files/figure-latex/unnamed-chunk-4-1} \includegraphics[width=0.5\linewidth]{StatisticalInference_CourseProject_Part2_files/figure-latex/unnamed-chunk-4-2} 

## Key Assumptions

For the purpose of this exercise, the distributions of the observations will be assumed to follow a normal distribution. However, since the number of observations ranges from 30 (for each treatment level) to 20 (for each dose level), a T-student distribution will be used for comparison.

Additionally, following the advise of the course lectures, when in doubt variances for each group will be considered different.

## Comparing Tooth Growth by Treatment (supp) and Dose

Below is the code and results for the two sided T-test comparing tooth growth for both OJ and VC treatments in the data set:


```r
tOJ <- ToothGrowth[ToothGrowth$supp == 'OJ', ]
tVC <- ToothGrowth[ToothGrowth$supp == 'VC', ]
t.test(tOJ$len, tVC$len, alternative = 'two.sided', var.equal = FALSE)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  tOJ$len and tVC$len
## t = 1.9153, df = 55.309, p-value = 0.06063
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -0.1710156  7.5710156
## sample estimates:
## mean of x mean of y 
##  20.66333  16.96333
```

Below is the code and results for the two sided T-test comparing tooth growth for the lowest (0.5) and highest (2) dosage values in the data set:


```r
d05 <- ToothGrowth[ToothGrowth$dose == '0.5', ]
d2 <- ToothGrowth[ToothGrowth$dose == '2', ]
t.test(d05$len, d2$len, alternative = 'two.sided', var.equal = FALSE)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  d05$len and d2$len
## t = -11.799, df = 36.883, p-value = 4.398e-14
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -18.15617 -12.83383
## sample estimates:
## mean of x mean of y 
##    10.605    26.100
```

## Conclusions - Part II

As per the T-test results:

1. With a P-value of approximately 0.061, the null hypothesis of both treatment length means being equal cannot be rejected. Hence, there isn't enough evidence to conclude tooth growth is affected differently overall between the two studied treatments applied to Guinea Pigs.

2. With a P-value of 4.398e-14, the null hypothesis of both dosage tooth length means being equal is rejected. Thus, there is significant evidence to conclude the tooth growth is affected differently from dosage values of 0.5 to 2 applied to Guinea Pigs.

