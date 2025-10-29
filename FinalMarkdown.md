Final Markdown Report
================
Luke Hatch
2025-10-29

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
  - [Question](#question)
  - [Hypothesis](#hypothesis)
  - [Possible visualizations and test
    statistic(s)](#possible-visualizations-and-test-statistics)
  - [Prediction](#prediction)
- [METHODS](#methods)
  - [1st Analysis (Line Plot)](#1st-analysis-line-plot)
  - [2nd Analysis (Logistic/Linear
    Regressions)](#2nd-analysis-logisticlinear-regressions)
- [DISCUSSION](#discussion)
  - [Interpretation of Analysis 1](#interpretation-of-analysis-1)
  - [Interpretation of Analysis 2](#interpretation-of-analysis-2)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

# BACKGROUND

## Question

How has firefly abundance changed monthly over the last 8 years
(2016-2024)?

## Hypothesis

Due to temperature changes (global warming), we’d expect to see an
increase in abundance sooner in the year as each year passes.

## Possible visualizations and test statistic(s)

line plot (change over time) and linear regression (for peak abundance
each year)

## Prediction

# METHODS

## 1st Analysis (Line Plot)

``` r
# Libraries
library(ggplot2)
library(hrbrthemes)
read.csv("UtahFireflies - 2014-2024.csv") #insert file once data is refined

# create data
Time <- 1:10 #this will read from the dates in the data
Firefly Sightings <- cumsum(rnorm(10)) #this will pull from the counts/sightings
data <- data.frame(Time,Firefly Sightings)

# Plot
ggplot(data, aes(x=Time, y=Firefly Sightings)) +
  geom_line( color="#69b3a2", size=2, alpha=0.9, linetype=2) +
  theme_ipsum() +
  ggtitle("Monthly Firefly Count from 2018-2024")
```

## 2nd Analysis (Logistic/Linear Regressions)

``` r
# Add data
data <- data.frame(
  cond = rep(c("condition_1", "condition_2"), each=10), 
  my_x = 1:100 + rnorm(100,sd=9), 
  my_y = 1:100 + rnorm(100,sd=16) 
)

# Basic scatter plot.
p1 <- ggplot(data, aes(x=my_x, y=my_y)) + 
  geom_point( color="#69b3a2") +
  theme_ipsum()
 
# with linear trend
p2 <- ggplot(data, aes(x=my_x, y=my_y)) +
  geom_point() +
  geom_smooth(method=lm , color="red", se=FALSE) +
  theme_ipsum()

# linear trend + confidence interval
p3 <- ggplot(data, aes(x=my_x, y=my_y)) +
  geom_point() +
  geom_smooth(method=lm , color="red", fill="#69b3a2", se=TRUE) +
  theme_ipsum()
```

# DISCUSSION

## Interpretation of Analysis 1

In analysis 1, results in the bar plots show higher bloodmeal counts in
both the + and - WNV areas for the house finch, house sparrow, and
american robin. In both the + and - areas, these birds are in the same
respective order from highest to lower bloodmeal count. These results
from analysis 1 support the hypothesis (house finches are important
reservoirs in SLC) on the surface/visual level, but the second analysis
clarifies significance into the numerical role of house finches
specifically.

## Interpretation of Analysis 2

The second analysis was ran in two parts. The first section of the code
chunk was evaluating the data from the house finch for any positive test
of WNV at the locations. This produced a statistically significant
result (0.0287). After this was determined to be significant, additional
analysis was done to determine relativity of each positive test at each
location. This was also statistically significant (4.54e-05), meaning
there was a higher likelihood of WNV presence where house finches were
found. This supports our hypothesis of house finches acting as important
reservoirs. One uncertainty, however, could be based on the blood meal
collection time period, as there may be different hosts in different
seasons or different feeding patterns from the mosquitos.

# CONCLUSION

Overall, the results support the hypothesis - house finches showed
significance in acting as reservoirs for WNV. This was shown by the data
organized in the bar plot demonstrating higher bloodmeals in both
positive and negative areas for WNV in house finches. The second
analysis delved deeper into the house finch specifically and tested for
numerical significance. It can be concluded that as the number of house
finch bloodmeals increases, as does the likelihood of finding WNV.
Similarly, the more house finch meals mosquitos have, the more likely
the spread - and therefore amplification - of WNV in SLC.

# REFERENCES

1.  Komar, N., Langevin, S., Hinten, S., Nemeth, N., Edwards, E.,
    Hettler, D., Davis, B., Bowen, R., & Bunning, M. (2003).
    Experimental infection of North American birds with the New York
    1999 strain of West Nile virus. Emerging Infectious Diseases, 9(3),
    311–322. <https://doi.org/10.3201/eid0903.020628>

2.  ChatGPT. OpenAI, version Jan 2025. Used as a reference for functions
    such as plot() and to correct syntax errors. Accessed 2025-10-29.
