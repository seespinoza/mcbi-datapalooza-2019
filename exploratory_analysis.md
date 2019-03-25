Exploratory Analysis
================

Now that we've aggregated potentially relevant data we want to do an exploratory analysis to look at our features. For this competition we are tasked with creating a scoring metric without any data to evaluate against. Thus, we only have guesses as to characteristics constitute a high scoring ZIP code. It also means we are not able to incorporate any kind of supervised learning.

First, we want to look for correlations among variables. If two or more variables are highly correlated there may not be any use in trying to include all the correlated variables in a scoring model.

``` r
library(ggplot2)
# Read in
zip_data <- read.csv("zip_data_final.csv")

# Subset data for zip codes with population greater than 0. These zip codes are not important for our scoring model
subset <- zip_data[which(zip_data$population > 0 & zip_data$median_indiv_income > 2000),]
```

Let's look at density for zip codes with population greater than 0.

``` r
ggplot(subset, aes(population)) + geom_density(color="red", fill="red", alpha=0.4) + theme(legend.position="none") + coord_cartesian(xlim=c(0,80000))
```

![](exploratory_analysis_files/figure-markdown_github/unnamed-chunk-2-1.png)

Not surpisingly, this is highly skewed. Zip codes are designed to aid in postal delivery logistics, not to meaningfully represent geographical areas for data science.

Our main concern is that some variables are highly correlated and thus including them individually in our simple scoring model is needlessly complex. This data may be more useful in its entirety with known response data, where features could be evaluated for importance.

``` r
ggplot(subset, aes(rpp, median_indiv_income)) + geom_point(color="firebrick3", fill="firebrick3", alpha=0.4) + coord_cartesian(xlim=c(75,130), ylim=c(0,150000))
```

![](exploratory_analysis_files/figure-markdown_github/unnamed-chunk-3-1.png)

This is median invididual income by zip code as a function of regional price parity, a measure to compare the costs of goods, services, and rents in different regions of the US. There are lots of RPP "bins", because we had to use lots of state RPP values for zip codes and also because the higher resolution RPP values were often very close, or the same, for zip codes in major metropolitan areas.

RPP and median individual income seem to be only vaguely correlated here. There seems to be a positive correlation, but RPP may still be a valuable variable for our pricing model in that it may allow us to slide the income-weighting formula.

``` r
ggplot(subset, aes(median_indiv_income, life_expectancy)) + geom_point(color="springgreen3", fill="springgreen3", alpha=0.3) + coord_cartesian(xlim=c(0,150000), ylim=c(55,95))
```

![](exploratory_analysis_files/figure-markdown_github/unnamed-chunk-4-1.png)

There is a very clear relationship between the median individual income for a zip code and the floor of life expectancy. This is fairly easy to believe. We were thinking that the life expectancy for a zip code may potentially impact the target customer age for that zip code.

Is there any relationship between longitude and a the relative frequency of individuals ages 45-59 in a zip code?

``` r
ggplot(subset, aes(longitude, freq_pop_45.49 + freq_pop_50.54 + freq_pop_55.59)) + geom_point(color="royalblue3", fill="royalblue3", alpha=0.3) + coord_cartesian(xlim=c(-130,-70), ylim=c(0,.5))
```

![](exploratory_analysis_files/figure-markdown_github/unnamed-chunk-5-1.png)

Probably not.

####################### 

testing stuffs

``` r
ggplot(subset, aes(freq_pop_45.49 + freq_pop_50.54 + freq_pop_55.59)) + geom_density(color="red", fill="red", alpha=0.4) + theme(legend.position="none")
```

![](exploratory_analysis_files/figure-markdown_github/unnamed-chunk-6-1.png)