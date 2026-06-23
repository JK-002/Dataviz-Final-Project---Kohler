---
title: "Data Visualization for Exploratory Data Analysis"
author: "Justin Kohler `jkohler0835@floridapoly.edu`"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
---

# Data Visualization Project 3

In this exercise you will explore methods to create different types of data visualizations (such as plotting text data, or exploring the distributions of continuous variables).

## PART 1: Density Plots

Using the dataset obtained from FSU's [Florida Climate Center](https://climatecenter.fsu.edu/climate-data-access-tools/downloadable-data), for a station at Tampa International Airport (TPA) for 2022, attempt to recreate the charts shown below which were generated using data from 2016. You can read the 2022 dataset using the code below:


``` r
library(tidyverse)
library(lubridate)
library(ggridges)
```


``` r
weather_tpa <- read_csv("../data/tpa_weather_2022.csv")
sample_n(weather_tpa, 4)
```

```
## # A tibble: 4 × 7
##    year month   day precipitation max_temp min_temp ave_temp
##   <dbl> <dbl> <dbl>         <dbl>    <dbl>    <dbl>    <dbl>
## 1  2022    10    19             0       68       53     60.5
## 2  2022     1    31             0       69       39     54  
## 3  2022    11    22             0       78       65     71.5
## 4  2022     5     5             0       90       76     83
```


``` r
tpa_clean <- weather_tpa %>% 
  filter(max_temp != -99.9, min_temp != -99.9, precipitation != -99.99) %>% 
  unite("doy", year, month, day, sep = "-") %>% 
  mutate(doy = ymd(doy), 
         max_temp = as.double(max_temp),
         min_temp = as.double(min_temp), 
         precipitation = as.double(precipitation),
         month_name = month(doy, label = TRUE, abbr = FALSE))
```

Using the 2022 data: 

(a) Create a plot like the one below:

<img src="../figures/tpa_max_temps_facet.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(tpa_clean, aes(x = max_temp)) +
  geom_histogram(binwidth = 3, color = "white", aes(fill = month_name)) +
  facet_wrap(~month_name) +
  labs(title = "Faceted Histogram of Maximum Monthly Temperatures (2022)", 
       x = "Maximum Temperature (°F)", 
       y = "Number of Days") +
  theme(plot.title = element_text(hjust = 0.5), 
        plot.background = element_rect(fill = "white"),
        panel.background = element_rect(fill = "white"), 
        panel.grid.major = element_line(color = "lightgray"), 
        panel.grid.minor = element_line(color = "lightgray"), 
        panel.border = element_rect(color = "black", fill = NA),
        strip.background = element_rect(fill = "lightgray", color = "black"), 
        legend.position = "none")
```

![](Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

(b) Create a plot like the one below:

<img src="../figures/tpa_max_temps_density.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(tpa_clean, aes(x = max_temp)) +
  geom_density(bw = 0.5, fill = "gray50", size = 0.7, kernel = "gaussian") +
  labs(title = "Density Plot of Maximum Temperatures", 
       x = "Maximum Temperature (°F)", 
       y = "Density", 
       fill = "Month") +
  theme(plot.title = element_text(hjust = 0.5),
        axis.title.y = element_text(vjust = 3),
        panel.background = element_rect(fill = "white"), 
        panel.grid.major = element_line(color = "lightgray"), 
        panel.grid.minor = element_line(color = "lightgray"))
```

![](Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

(c) Create a plot like the one below:

<img src="../figures/tpa_max_temps_density_facet.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(tpa_clean, aes(x = max_temp)) +
  geom_density(alpha = 0.7, size = 1, aes(fill = month_name)) +
  facet_wrap(~ month_name) +
  labs(title = "Faceted Density Plot of Maximum Monthly Temperatures", 
       x = "Maximum Temperature (°F)", 
       y = "Density") + 
  theme(plot.title = element_text(hjust = 0.5), 
        plot.background = element_rect(fill = "white"),
        panel.background = element_rect(fill = "white"), 
        panel.grid.major = element_line(color = "lightgray"), 
        panel.grid.minor = element_line(color = "lightgray"), 
        panel.border = element_rect(color = "black", fill = NA),
        strip.background = element_rect(fill = "lightgray", color = "black"),
        axis.title.y = element_blank(),
        legend.position = "none")
```

![](Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

(d) Generate a plot like the chart below:

<img src="../figures/tpa_max_temps_ridges_plasma.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(tpa_clean, aes(x = max_temp, y = month_name, fill = month_name)) +
  geom_density_ridges(quantile_lines = TRUE, quantiles = 2, alpha = 0.7) +
  scale_fill_viridis_d(option = "plasma") +
  labs(
    title = "Ridgeline Plot of Maximum Temperatures by Month",
    x = "Maximum Temperature (°F)",
    y = "Month",
    fill = "Month"
  ) +
  theme_ridges() + 
  theme(legend.position = "none")
```

![](Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

Hint: use the`{ggridges}` package, and the `geom_density_ridges()` function paying close attention to the `quantile_lines` and `quantiles` parameters. The plot above uses the `plasma` option (color scale) for the _viridis_ palette.

(e) Create a plot of your choice that uses the attribute for precipitation _(values of -99.9 for temperature or -99.99 for precipitation represent missing data)_.


``` r
ggplot(tpa_clean, aes(x = doy, y = precipitation)) +
  geom_line(color = "blue", alpha = 0.6) +
  geom_smooth(color = "darkblue", method = "loess") +
  labs(
    title = "Daily Precipitation Trends in Tampa (2022)",
    x = "Date",
    y = "Precipitation (inches)"
  ) +
  theme_minimal()
```

![](Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

## PART 2 

> **You can choose to work on either Option (A) or Option (B)**. Remove from this template the option you decided not to work on. 


### Option (A): Visualizing Text Data

Review the set of slides (and additional resources linked in it) for visualizing text data: Week 6 PowerPoint slides of Visualizing Text Data. 

Choose any dataset with text data, and create at least one visualization with it. For example, you can create a frequency count of most used bigrams, a sentiment analysis of the text data, a network visualization of terms commonly used together, and/or a visualization of a topic modeling approach to the problem of identifying words/documents associated to different topics in the text data you decide to use. 

Make sure to include a copy of the dataset in the `data/` folder, and reference your sources if different from the ones listed below:

- [Billboard Top 100 Lyrics](https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/data/BB_top100_2015.csv)

- [RateMyProfessors comments](https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/data/rmp_wit_comments.csv)

- [FL Poly News Articles](https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/data/flpoly_news_SP23.csv)


(to get the "raw" data from any of the links listed above, simply click on the `raw` button of the GitHub page and copy the URL to be able to read it in your computer using the `read_csv()` function)


### Option (B): Data on Concrete Strength 

Concrete is the most important material in **civil engineering**. The concrete compressive strength is a highly nonlinear function of _age_ and _ingredients_. The dataset used here is from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php), and it contains 1030 observations with 9 different attributes 9 (8 quantitative input variables, and 1 quantitative output variable). A data dictionary is included below: 


Variable                      |    Notes                
------------------------------|-------------------------------------------
Cement                        | kg in a $m^3$ mixture             
Blast Furnace Slag            | kg in a $m^3$ mixture  
Fly Ash                       | kg in a $m^3$ mixture             
Water                         | kg in a $m^3$ mixture              
Superplasticizer              | kg in a $m^3$ mixture
Coarse Aggregate              | kg in a $m^3$ mixture
Fine Aggregate                | kg in a $m^3$ mixture      
Age                           | in days                                             
Concrete compressive strength | MPa, megapascals


Below we read the `.csv` file using `readr::read_csv()` (the `readr` package is part of the `tidyverse`)


``` r
concrete <- read_csv("../data/concrete.csv", col_types = cols())
```


Let us create a new attribute for visualization purposes, `strength_range`: 


``` r
new_concrete <- concrete %>%
  mutate(strength_range = cut(Concrete_compressive_strength, 
                              breaks = quantile(Concrete_compressive_strength, 
                                                probs = seq(0, 1, 0.2))) )
```



1. Explore the distribution of 2 of the continuous variables available in the dataset. Do ranges make sense? Comment on your findings.

2. Use a _temporal_ indicator such as the one available in the variable `Age` (measured in days). Generate a plot similar to the one shown below. Comment on your results.

<img src="../figures/concrete_strength.png" alt="" width="80%" style="display: block; margin: auto;" />


3. Create a scatterplot similar to the one shown below. Pay special attention to which variables are being mapped to specific aesthetics of the plot. Comment on your results. 

<img src="../figures/cement_plot.png" alt="" width="80%" style="display: block; margin: auto;" />




