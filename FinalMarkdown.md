Final Markdown Report - Fireflies
================
Luke Hatch
2025-11-20

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
  - [Question](#question)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [METHODS](#methods)
  - [1st Analysis (Bar Plot)](#1st-analysis-bar-plot)
  - [2nd Analysis (Scatter Plot)](#2nd-analysis-scatter-plot)
  - [3rd Analysis (ANOVA)](#3rd-analysis-anova)
- [DISCUSSION](#discussion)
  - [Interpretation of Analysis 1 (Bar
    Plot)](#interpretation-of-analysis-1-bar-plot)
  - [Interpretation of Analysis 2 (Scatter
    Plot)](#interpretation-of-analysis-2-scatter-plot)
  - [Interpretation of Analysis 3
    (ANOVA)](#interpretation-of-analysis-3-anova)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

Firefly emergence is influenced by many environmental factors, including
temperature. This raises the question as to whether warming temperatures
(e.g. from climate change) could affect at what times of year adults are
observed. From citizen science data collected in Utah from 2016-2024, we
analyzed peak firefly abundance monthly from year to year by
consolidating the observations into counts. We then displayed the data
patterns using a bar plot and a scatter plot. This displayed a
consistent peak firefly count in June each year with a slight downward
trend in earlier emergence. An ANOVA analysis was also conducted in
order to test for significance between the peak emergences in 2016-2024,
yielding insignificant results (p = 0.309). These results did not
support our hypothesis of warmer temperatures affecting the timing of
firefly emergences. However, citizen science data is limiting. Long-term
observational studies in various locations could yield other results.

# BACKGROUND

Fireflies are among the most mesmerizing insects in the world due to
their unique glow in the night sky. Most people would run to avoid
insects, but for these they stop and watch. In some cases, they even
record what they saw, providing firefly citizen scientist data that
supports further research on their behavior. Before they’re in the sky,
however, they spend the majority of their lives underground developing.
Duration of their lives underground is influenced by a variety of
factors (Evans et al., 2019, p. 266). Depending primarily on climate and
soil conditions, the stages of the firefly life cycles can be altered -
taking or adding time (Evans et al., 2019, p. 266). Because these
conditions change each year, the timing and abundance of emerged
fireflies can also fluctuate. This report uses citizen science data
collected in Utah to examine firefly abundance over the past 8 years.

## Question

How has firefly peak abundance changed monthly over the last 8 years
(2016-2024)?

## Hypothesis

Due to temperature changes (e.g. global warming), fireflies are expected
to emerge sooner in the year as each year passes, as warmer temperatures
can accelerate their development and affect the timing of emergence
(Evans et al., 2019, p. 266).

## Prediction

If temperature increases have influenced firefly emergence, then peak
abundance would be observed earlier in years where the temperature is
higher earlier in that year (such as emerging in May vs. June).

# METHODS

Over the years, starting in 1964, citizen science data was collected
throughout Utah on firefly sightings. Observations included information
like date, time, location, habitat, and count was recorded. Citizen
science data is notoriously messy and unreliable, so we took the data
and simplified it by only using the dates observed and using that date
as a count, rather than the numbers of fireflies reported. We also added
a column that converted the day of the year to a Julian calendar day.
This revised data was then uploaded to R Studio Cloud and code was
written to: 1) display the data by counts per month each year in a
multi-plot 2) display the data year-to-year by peak abundance and
observe the trend and 3) perform an ANOVA to determine if the trend is
significant.

## 1st Analysis (Bar Plot)

``` r
data <-read.csv("UtahFirefly.csv")

#create a barplot that displays counts in each month, each year
ggplot(data, aes(x= Month))+
  geom_bar(fill = "steelblue3") +
  facet_grid(~ Year) +
  labs(title = "Number of Instances by Month and Year", 
       x = "Month", 
       y = "Number of Instances") +
  theme_minimal()
```

![](FinalMarkdown_files/figure-gfm/multiplot-1.png)<!-- -->

## 2nd Analysis (Scatter Plot)

``` r
# make plot of Max Julian day
table_Julian <- table(data$Year, data$Julian)
# find the max count each year
max_col <- apply(table_Julian, 1, which.max)
# create a data frame with year and max Julian day
Julian <- as.numeric(colnames(table_Julian)[max_col])
Year <- c(2016, 2017, 2018, 2019, 2020, 2021, 2022, 2023, 2024)
# Create a color gradient
color_gradient <- colorRampPalette(c("steelblue1", "steelblue4"))
# Generate a vector (color gradient) based on Julian values
cols <- color_gradient(100)[as.numeric(cut(Julian, breaks = 100))]
#plot as scatter plot
plot(Year, Julian, main = "Scatter Plot of Peak Counts Each Year",
     col = cols,
     pch = 19,       # filled circles
     cex = 1.5,      # point size
     ylab = "Julian Day")
# add a linear regression trendline
abline(lm(Julian ~ Year), col = "blue", lwd = 2, lty = 2)
```

![](FinalMarkdown_files/figure-gfm/second-1.png)<!-- -->

## 3rd Analysis (ANOVA)

``` r
#create data frame 
Julian_dataframe <- data.frame(ColumnA = Julian, ColumnB = Year)
m1 <- lm( ColumnA ~ ColumnB, data=Julian_dataframe)
# run anova test for p value
anova(m1)
```

    ## Analysis of Variance Table
    ## 
    ## Response: ColumnA
    ##           Df Sum Sq Mean Sq F value Pr(>F)
    ## ColumnB    1 135.00  135.00  1.2014 0.3093
    ## Residuals  7 786.56  112.36

``` r
summary(m1)
```

    ## 
    ## Call:
    ## lm(formula = ColumnA ~ ColumnB, data = Julian_dataframe)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -18.278  -2.778  -1.778   7.222  16.222 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)
    ## (Intercept) 3192.778   2764.342   1.155    0.286
    ## ColumnB       -1.500      1.368  -1.096    0.309
    ## 
    ## Residual standard error: 10.6 on 7 degrees of freedom
    ## Multiple R-squared:  0.1465, Adjusted R-squared:  0.02456 
    ## F-statistic: 1.201 on 1 and 7 DF,  p-value: 0.3093

# DISCUSSION

Results of citizen science firefly observations did not show a
significant change in peak abundance each year. Analyses showed each
observation on a timeline from 2016 to 2024. It was clear that firefly
abundance was highest around June each year. In taking a closer look at
the peak abundance each year, there was a slight trend downward as the
years became more current. After the ANOVA was ran, the slope of this
line was determined to be -1.5 and the significance of the peak
abundances was 0.3093. This meant the change year to year was not
significant. This did not support our hypothesis.

## Interpretation of Analysis 1 (Bar Plot)

The bar plot clearly shows a common trend of firefly abundance each
year. Abundance starts lowest in the beginning of the year, and around
June (Julian days 145-190) peaks in numbers. This is followed by a
decline as the year completes and the cycle repeats.

## Interpretation of Analysis 2 (Scatter Plot)

This scatter plot is a visual of the peak abundance from years
2018-2024. There is a large amount of fluctuation in the time of year
peak abundance is reached each year. The line helps indicate a slight
downward trend as the years go by, but does not indicate the slope of
that line.

## Interpretation of Analysis 3 (ANOVA)

The results of the ANOVA display both the slope of the Scatter Plot from
analysis 2 as well as the test for significance. The slope was -1.5,
indicating a slight downward trend. However the p-value was 0.309,
indicating the change over time was not significant.

# CONCLUSION

In determining whether the timing of firefly abundance was changing
year-to-year, the results were not significant. Overall, the data did
not support our hypothesis that warmer temperatures each year contribute
to an earlier firefly emergence. This suggests that firefly abundance is
currently remaining relatively consistent from year to year. A major
limitation of this study is the source of the data - citizen science
contributions. Although usable, citizen science data can be unreliable
in consistency and accuracy. This also contributed to the timescale for
the observations. Due to the inconsistency of observations, we limited
the analysis to only the years 2016-2024 as they appeared the most
complete and consistent. With more consistent data (i.e. more data from
the 1900s), the observed trend in the 8 years examined could be more
significant. Further research could involve a long-term study with more
reliable data to test for significance. Evans et. al (2019) has already
demonstrated the connection of firefly emergence with temperature
changes, thus further study in temperature changes year to year would
also be important in determining temperature as the most likely concern
for potential future abundance timing changes. Studying firefly
populations in other locations outside of Utah may show a similar trend
or no trend at all, furthering research on whether this is a world-wide
possibility.

# REFERENCES

1.  Citizen Science Libraries. (n.d.). UtahFireflies. Google Docs.
    Retrieved October 30, 2025, from
    <https://docs.google.com/spreadsheets/d/1XLklQd6P3d9wYnmh-3KgzNvP8GfQHL2Y2IyaZMwIOzk/edit?usp=drive_web&ouid=106039106225729317949&usp=embed_facebook>

2.  Evans, D. M., Boyer, A. G., Phillips, M. L., & Jepsen, S. J. (2019).
    Firefly (Coleoptera: Lampyridae) phenology, life history, and
    habitat associations in the western United States. Environmental
    Entomology, 48(2), 265–273. <https://doi.org/10.1111/een.12702>

3.  ChatGPT. OpenAI, version Nov 2025. Used as a reference for functions
    such as plot() and to correct syntax errors. Accessed 2025-11-20.
