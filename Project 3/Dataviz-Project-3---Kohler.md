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
library(plotly)
```


``` r
weather_tpa <- read_csv("../data/tpa_weather_2022.csv")
sample_n(weather_tpa, 4)
```

```
## # A tibble: 4 × 7
##    year month   day precipitation max_temp min_temp ave_temp
##   <dbl> <dbl> <dbl>         <dbl>    <dbl>    <dbl>    <dbl>
## 1  2022     2    18          0          81       68     74.5
## 2  2022     8    31          0.01       93       80     86.5
## 3  2022    10    28          0.71       87       72     79.5
## 4  2022     1    29          0          55       41     48
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

<img src="../figures/tpa_max_temps_facet.png" alt="" width="80%" style="display: block; margin: auto auto auto 0;" />


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
        panel.grid.major = element_line(color = "lightgray", size = 0.7), 
        panel.grid.minor = element_line(color = "lightgray", size = 0.3), 
        panel.border = element_rect(color = "black", fill = NA),
        strip.background = element_rect(fill = "lightgray", color = "black"), 
        legend.position = "none")
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-5-1.png" alt="Faceted histogram showing maximum monthly temperatures (°F) for 2022. Summer months show a tight distribution clustered around the mid 90s, while winter months show a much wider variation from the low 40s to the mid 80s."  />

>*This faceted histogram displays the maximum monthly temperatures (°F) experienced throughout 2022 as well as the number of days each temperature was reached. The winter months of December, January, and February display a broader range of temperatures ranging from the yearly lows of the mid 40s to the mid 80s. Conversely, the summer months of June, July, and August, unlike the winter months, exhibit a much tighter range of temperatures and also feature yearly temperature highs in the mid to high 90s.*

(b) Create a plot like the one below:

<img src="../figures/tpa_max_temps_density.png" alt="" width="80%" style="display: block; margin: auto auto auto 0;" />


``` r
ggplot(tpa_clean, aes(x = max_temp)) +
  geom_density(bw = 0.5, fill = "gray50", size = 0.7, kernel = "gaussian") +
  labs(title = "Density Plot of Maximum Temperatures", 
       x = "Maximum Temperature (°F)", 
       y = "Density", 
       fill = "Month") +
  theme(plot.title = element_text(hjust = 0.5), 
        panel.background = element_rect(fill = "white"), 
        panel.grid.major = element_line(color = "lightgray", size = 0.7), 
        panel.grid.minor = element_line(color = "lightgray", size = 0.3),
        axis.title.y = element_text(vjust = 3))
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-7-1.png" alt="An aggregate density plot of maximum temperatures (°F) in Tampa for 2022. Though the range of temperatures experienced throughout the year is rather broad with the low being around the mid 40s and the high being around the high 90s, during the majority of the year, the temperatures in Tampa range from the low 80s to the mid 90s."  />

>*This aggregate density plot showcases Tampa's maximum temperature (°F) profile across the entirety of 2022. Though the range of temperatures experienced throughout the year is rather broad with the low being around the mid 40s and the high being around the high 90s, it can be clearly seen that the majority of the data lies on the right side of the figure. This would indicate that during most of the year, the temperatures in Tampa range from the low 80s to the mid 90s.*

(c) Create a plot like the one below:

<img src="../figures/tpa_max_temps_density_facet.png" alt="" width="80%" style="display: block; margin: auto auto auto 0;" />


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
        panel.grid.major = element_line(color = "lightgray", size = 0.7), 
        panel.grid.minor = element_line(color = "lightgray", size = 0.3), 
        panel.border = element_rect(color = "black", fill = NA),
        strip.background = element_rect(fill = "lightgray", color = "black"),
        axis.title.y = element_blank(),
        legend.position = "none")
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-9-1.png" alt="Faceted density curves of maximum monthly temperatures (°F). It shows wide, flat curves indicating high temperature variability in winter, and tall, narrow spikes indicating hot weather in July and August."  />

>*In this faceted density plot, a clear contrast between monthly temperature (°F) volatility can be seen. In the summer months of July and August, tall and narrow density curves can be seen which indicates low temperature variance. However, for the winter months of December, January, and February, wider and flatter curves can be seen showcasing the day-to-day temperature variability that is seen during the cooler months.*

(d) Generate a plot like the chart below:

<img src="../figures/tpa_max_temps_ridges_plasma.png" alt="" width="80%" style="display: block; margin: auto auto auto 0;" />


``` r
ggplot(tpa_clean, aes(x = max_temp, y = month_name, fill = stat(x))) +
  geom_density_ridges_gradient(quantile_lines = TRUE, quantiles = 2, alpha = 0.7) +
  scale_fill_viridis_c(option = "plasma", breaks = c(40, 60, 80, 100),
                       labels = c("40", "60", "80", "100")) +
  guides(fill = guide_colorbar(
    barwidth = unit(0.8, "cm"),
    barheight = unit(3.5, "cm"))) + 
  labs(title = "Ridgeline Plot of Maximum Temperatures by Month", 
       x = "Maximum Temperature (°F)",
       fill = NULL) +
  theme(plot.title = element_text(hjust = 0.5), 
        panel.background = element_rect(fill = "white"), 
        panel.grid.major = element_line(color = "lightgray", size = 0.7), 
        panel.grid.minor = element_line(color = "lightgray", size = 0.3), 
        axis.title.x = element_text(size = 12),
        axis.title.y = element_blank(),
        axis.text = element_text(size = 10))
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-11-1.png" alt="A ridgeline plot organizing maximum temperatures (°F) chronologically by month. The ridges physically shift to the right and transition from deep blue to bright yellow as the temperatures increase into the summer months. They then begin to move back left as temperatures begin to decrease towards the end of fall and into winter."  />

>*The above ridgeline plot displays the chronological view of the shifting temperatures (°F) throughout 2022 in Tampa, Florida. In the winter months of January and February, the ridges begin to shift to the right as temperatures increase into the summer months where the ridges peak at their highest temperature values. Then, shortly after the summer season has ended, the ridges start to broaden and move to the left as cooler temperatures become present towards the end of fall months in October and November, then reaching its left most position during the winter month of December, concluding the year.*

(e) Create a plot of your choice that uses the attribute for precipitation _(values of -99.9 for temperature or -99.99 for precipitation represent missing data)_.


``` r
plot <- ggplot(tpa_clean, aes(x = doy, y = precipitation)) +
  geom_line(color = "blue", alpha = 0.6) +
  geom_smooth(color = "darkblue", method = "loess", name = "Trend") +
  labs(title = "Daily Precipitation Trends in Tampa From January 1 2022 to January 1 2023",
       y = "Precipitation (inches)") +
  theme(plot.background = element_rect(fill = "white"),
        panel.background = element_rect(fill = "white"), 
        panel.grid.major = element_line(color = "lightgray"), 
        panel.grid.minor = element_line(color = "lightgray"), 
        axis.title.y = element_text(vjust = 3),
        axis.title.x = element_blank())

interactive_plot <- ggplotly(plot, tooltip = c("x", "y"))
interactive_plot$x$data[[2]]$hoverinfo <- "none"
interactive_plot$x$data[[3]]$hoverinfo <- "none"

interactive_plot <- interactive_plot %>%
  layout(
    xaxis = list(
      rangeslider = list(type = "date"), 
      rangeselector = list(buttons = list(
        list(count = 1, label = "1m", step = "month", stepmode = "backward"), 
        list(count = 3, label = "3m", step = "month", stepmode = "backward"), 
        list(step = "all", label = "All")))))

interactive_plot
```

```{=html}
<div class="plotly html-widget html-fill-item" id="htmlwidget-7986b1db4dab382cb4c7" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-7986b1db4dab382cb4c7">{"x":{"data":[{"x":[18993,18994,18995,18996,18997,18998,18999,19000,19001,19002,19003,19004,19005,19006,19007,19008,19009,19010,19011,19012,19013,19014,19015,19016,19017,19018,19019,19020,19021,19022,19023,19024,19025,19026,19027,19028,19029,19030,19031,19032,19033,19034,19035,19036,19037,19038,19039,19040,19041,19042,19043,19044,19045,19046,19047,19048,19049,19050,19051,19052,19053,19054,19055,19056,19057,19058,19059,19060,19061,19062,19063,19064,19065,19066,19067,19068,19069,19070,19071,19072,19073,19074,19075,19076,19077,19078,19079,19080,19081,19082,19083,19084,19085,19086,19087,19088,19089,19090,19091,19092,19093,19094,19095,19096,19097,19098,19099,19100,19101,19102,19103,19104,19105,19106,19107,19108,19109,19110,19111,19112,19113,19114,19115,19116,19117,19118,19119,19120,19121,19122,19123,19124,19125,19126,19127,19128,19129,19130,19131,19132,19133,19134,19135,19136,19137,19138,19139,19140,19141,19142,19143,19144,19145,19146,19147,19148,19149,19150,19151,19152,19153,19154,19155,19156,19157,19158,19159,19160,19161,19162,19163,19164,19165,19166,19167,19168,19169,19170,19171,19172,19173,19174,19175,19176,19177,19178,19179,19180,19181,19182,19183,19184,19185,19186,19187,19188,19189,19190,19191,19192,19193,19194,19195,19196,19197,19198,19199,19200,19201,19202,19203,19204,19205,19206,19207,19208,19209,19210,19211,19212,19213,19214,19215,19216,19217,19218,19219,19220,19221,19222,19223,19224,19225,19226,19227,19228,19229,19230,19231,19232,19233,19234,19235,19236,19237,19238,19239,19240,19241,19242,19243,19244,19245,19246,19247,19248,19249,19250,19251,19252,19253,19254,19255,19256,19257,19258,19259,19260,19261,19262,19263,19264,19265,19266,19267,19268,19269,19270,19271,19272,19273,19274,19275,19276,19277,19278,19279,19280,19281,19282,19283,19284,19285,19286,19287,19288,19289,19290,19291,19292,19293,19294,19295,19296,19297,19298,19299,19300,19301,19302,19303,19304,19305,19306,19307,19308,19309,19310,19311,19312,19313,19314,19315,19316,19317,19318,19319,19320,19321,19322,19323,19324,19325,19326,19327,19328,19329,19330,19331,19332,19333,19334,19335,19336,19337,19338,19339,19340,19341,19342,19343,19344,19345,19346,19347,19348,19349,19350,19351,19352,19353,19354,19355,19356,19357],"y":[0,0,0.02,0,0,1.0000000000000001e-05,1.0000000000000001e-05,0,0,0,0,0,0,0,0,0.68000000000000005,0,0,0,0,0,0.01,1.0000000000000001e-05,0,0.67000000000000004,0.080000000000000002,0,1.0000000000000001e-05,0,0,0,0,0,0,0,0.02,0.02,0,0.34999999999999998,1.0000000000000001e-05,0,0,1.0000000000000001e-05,0.10000000000000001,0,0,0,0,0,0.13,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1.0000000000000001e-05,0,0.40999999999999998,0,0,1.04,1.0000000000000001e-05,0,0,0,0,0,0,0,1.4199999999999999,0,0,0,0,0,0,0.040000000000000001,1.3700000000000001,1.1100000000000001,0,0,0,0,1.3400000000000001,0,0,0,0,0,0,0,0.02,0,0,0,0,0,0,0,0,0,0,0,0,0.14000000000000001,1.22,1.5600000000000001,0.029999999999999999,0,0.62,0,0,0,0.14999999999999999,0,0,0,0,0,0,0,0,0,0,0,0,0.17000000000000001,1.0000000000000001e-05,0,1.0000000000000001e-05,0.01,0,0,0,0,0,1.6499999999999999,0.080000000000000002,1.0000000000000001e-05,1.6000000000000001,1.0000000000000001e-05,0.46999999999999997,0,0,0.16,0,0.10000000000000001,0.01,2.8100000000000001,0,0,0,0.11,1.0000000000000001e-05,0,0,0.040000000000000001,0.10000000000000001,0,0,0,0.19,0,0,1.28,0.089999999999999997,0.040000000000000001,1.0700000000000001,1.0000000000000001e-05,1.0000000000000001e-05,0,1.0000000000000001e-05,0.01,0.050000000000000003,0.029999999999999999,0.19,0,0.87,0.63,0,0,2.0800000000000001,0,2.8599999999999999,0.78000000000000003,0,0.02,0,0,1.6399999999999999,1.6899999999999999,0,0.02,1.1200000000000001,0,0,0,0,0,0,1.0000000000000001e-05,0.97999999999999998,1.24,1.0000000000000001e-05,0.040000000000000001,0.23999999999999999,0.059999999999999998,1.28,0,0,0,0,1.0000000000000001e-05,1.0000000000000001e-05,0,0.64000000000000001,0,0,0.46999999999999997,0.45000000000000001,0,0,0.60999999999999999,0.31,1.0000000000000001e-05,0.02,0.16,0,0,0.01,2.7400000000000002,1.4099999999999999,0.40000000000000002,0,0,0,0.10000000000000001,1.6399999999999999,0.13,1.0900000000000001,0.02,0.029999999999999999,0.029999999999999999,0.059999999999999998,0,0.87,0.059999999999999998,0,0.02,0.070000000000000007,1.0000000000000001e-05,0,1.0700000000000001,0,0,1.0000000000000001e-05,0.080000000000000002,2.4700000000000002,0,0,0,0,0,0,0,0,0,0,0,1.0000000000000001e-05,1.0000000000000001e-05,0.02,0.29999999999999999,0,0,0,1.0000000000000001e-05,0.01,0,0,0,0,0,0,0,0,0.059999999999999998,0.70999999999999996,0,0,0,0,0,0.90000000000000002,0,1.0000000000000001e-05,1.0000000000000001e-05,0,0.059999999999999998,0.089999999999999997,2.46,0.76000000000000001,0.01,0.01,0,0,0.14000000000000001,0,0,0,0.39000000000000001,1.0000000000000001e-05,0,0.050000000000000003,0,1.0000000000000001e-05,0,0.27000000000000002,0,0,0.040000000000000001,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1.0900000000000001,0.050000000000000003,0.02,0.040000000000000001,0,0.11,0.59999999999999998,0.14999999999999999,1.0000000000000001e-05,0,0,0,0,0,0,0,0.28999999999999998],"text":["doy: 2022-01-01<br />precipitation: 0.00000","doy: 2022-01-02<br />precipitation: 0.00000","doy: 2022-01-03<br />precipitation: 0.02000","doy: 2022-01-04<br />precipitation: 0.00000","doy: 2022-01-05<br />precipitation: 0.00000","doy: 2022-01-06<br />precipitation: 0.00001","doy: 2022-01-07<br />precipitation: 0.00001","doy: 2022-01-08<br />precipitation: 0.00000","doy: 2022-01-09<br />precipitation: 0.00000","doy: 2022-01-10<br />precipitation: 0.00000","doy: 2022-01-11<br />precipitation: 0.00000","doy: 2022-01-12<br />precipitation: 0.00000","doy: 2022-01-13<br />precipitation: 0.00000","doy: 2022-01-14<br />precipitation: 0.00000","doy: 2022-01-15<br />precipitation: 0.00000","doy: 2022-01-16<br />precipitation: 0.68000","doy: 2022-01-17<br />precipitation: 0.00000","doy: 2022-01-18<br />precipitation: 0.00000","doy: 2022-01-19<br />precipitation: 0.00000","doy: 2022-01-20<br />precipitation: 0.00000","doy: 2022-01-21<br />precipitation: 0.00000","doy: 2022-01-22<br />precipitation: 0.01000","doy: 2022-01-23<br />precipitation: 0.00001","doy: 2022-01-24<br />precipitation: 0.00000","doy: 2022-01-25<br />precipitation: 0.67000","doy: 2022-01-26<br />precipitation: 0.08000","doy: 2022-01-27<br />precipitation: 0.00000","doy: 2022-01-28<br />precipitation: 0.00001","doy: 2022-01-29<br />precipitation: 0.00000","doy: 2022-01-30<br />precipitation: 0.00000","doy: 2022-01-31<br />precipitation: 0.00000","doy: 2022-02-01<br />precipitation: 0.00000","doy: 2022-02-02<br />precipitation: 0.00000","doy: 2022-02-03<br />precipitation: 0.00000","doy: 2022-02-04<br />precipitation: 0.00000","doy: 2022-02-05<br />precipitation: 0.02000","doy: 2022-02-06<br />precipitation: 0.02000","doy: 2022-02-07<br />precipitation: 0.00000","doy: 2022-02-08<br />precipitation: 0.35000","doy: 2022-02-09<br />precipitation: 0.00001","doy: 2022-02-10<br />precipitation: 0.00000","doy: 2022-02-11<br />precipitation: 0.00000","doy: 2022-02-12<br />precipitation: 0.00001","doy: 2022-02-13<br />precipitation: 0.10000","doy: 2022-02-14<br />precipitation: 0.00000","doy: 2022-02-15<br />precipitation: 0.00000","doy: 2022-02-16<br />precipitation: 0.00000","doy: 2022-02-17<br />precipitation: 0.00000","doy: 2022-02-18<br />precipitation: 0.00000","doy: 2022-02-19<br />precipitation: 0.13000","doy: 2022-02-20<br />precipitation: 0.00000","doy: 2022-02-21<br />precipitation: 0.00000","doy: 2022-02-22<br />precipitation: 0.00000","doy: 2022-02-23<br />precipitation: 0.00000","doy: 2022-02-24<br />precipitation: 0.00000","doy: 2022-02-25<br />precipitation: 0.00000","doy: 2022-02-26<br />precipitation: 0.00000","doy: 2022-02-27<br />precipitation: 0.00000","doy: 2022-02-28<br />precipitation: 0.00000","doy: 2022-03-01<br />precipitation: 0.00000","doy: 2022-03-02<br />precipitation: 0.00000","doy: 2022-03-03<br />precipitation: 0.00000","doy: 2022-03-04<br />precipitation: 0.00000","doy: 2022-03-05<br />precipitation: 0.00000","doy: 2022-03-06<br />precipitation: 0.00000","doy: 2022-03-07<br />precipitation: 0.00000","doy: 2022-03-08<br />precipitation: 0.00000","doy: 2022-03-09<br />precipitation: 0.00000","doy: 2022-03-10<br />precipitation: 0.00001","doy: 2022-03-11<br />precipitation: 0.00000","doy: 2022-03-12<br />precipitation: 0.41000","doy: 2022-03-13<br />precipitation: 0.00000","doy: 2022-03-14<br />precipitation: 0.00000","doy: 2022-03-15<br />precipitation: 1.04000","doy: 2022-03-16<br />precipitation: 0.00001","doy: 2022-03-17<br />precipitation: 0.00000","doy: 2022-03-18<br />precipitation: 0.00000","doy: 2022-03-19<br />precipitation: 0.00000","doy: 2022-03-20<br />precipitation: 0.00000","doy: 2022-03-21<br />precipitation: 0.00000","doy: 2022-03-22<br />precipitation: 0.00000","doy: 2022-03-23<br />precipitation: 0.00000","doy: 2022-03-24<br />precipitation: 1.42000","doy: 2022-03-25<br />precipitation: 0.00000","doy: 2022-03-26<br />precipitation: 0.00000","doy: 2022-03-27<br />precipitation: 0.00000","doy: 2022-03-28<br />precipitation: 0.00000","doy: 2022-03-29<br />precipitation: 0.00000","doy: 2022-03-30<br />precipitation: 0.00000","doy: 2022-03-31<br />precipitation: 0.04000","doy: 2022-04-01<br />precipitation: 1.37000","doy: 2022-04-02<br />precipitation: 1.11000","doy: 2022-04-03<br />precipitation: 0.00000","doy: 2022-04-04<br />precipitation: 0.00000","doy: 2022-04-05<br />precipitation: 0.00000","doy: 2022-04-06<br />precipitation: 0.00000","doy: 2022-04-07<br />precipitation: 1.34000","doy: 2022-04-08<br />precipitation: 0.00000","doy: 2022-04-09<br />precipitation: 0.00000","doy: 2022-04-10<br />precipitation: 0.00000","doy: 2022-04-11<br />precipitation: 0.00000","doy: 2022-04-12<br />precipitation: 0.00000","doy: 2022-04-13<br />precipitation: 0.00000","doy: 2022-04-14<br />precipitation: 0.00000","doy: 2022-04-15<br />precipitation: 0.02000","doy: 2022-04-16<br />precipitation: 0.00000","doy: 2022-04-17<br />precipitation: 0.00000","doy: 2022-04-18<br />precipitation: 0.00000","doy: 2022-04-19<br />precipitation: 0.00000","doy: 2022-04-20<br />precipitation: 0.00000","doy: 2022-04-21<br />precipitation: 0.00000","doy: 2022-04-22<br />precipitation: 0.00000","doy: 2022-04-23<br />precipitation: 0.00000","doy: 2022-04-24<br />precipitation: 0.00000","doy: 2022-04-25<br />precipitation: 0.00000","doy: 2022-04-26<br />precipitation: 0.00000","doy: 2022-04-27<br />precipitation: 0.00000","doy: 2022-04-28<br />precipitation: 0.14000","doy: 2022-04-29<br />precipitation: 1.22000","doy: 2022-04-30<br />precipitation: 1.56000","doy: 2022-05-01<br />precipitation: 0.03000","doy: 2022-05-02<br />precipitation: 0.00000","doy: 2022-05-03<br />precipitation: 0.62000","doy: 2022-05-04<br />precipitation: 0.00000","doy: 2022-05-05<br />precipitation: 0.00000","doy: 2022-05-06<br />precipitation: 0.00000","doy: 2022-05-07<br />precipitation: 0.15000","doy: 2022-05-08<br />precipitation: 0.00000","doy: 2022-05-09<br />precipitation: 0.00000","doy: 2022-05-10<br />precipitation: 0.00000","doy: 2022-05-11<br />precipitation: 0.00000","doy: 2022-05-12<br />precipitation: 0.00000","doy: 2022-05-13<br />precipitation: 0.00000","doy: 2022-05-14<br />precipitation: 0.00000","doy: 2022-05-15<br />precipitation: 0.00000","doy: 2022-05-16<br />precipitation: 0.00000","doy: 2022-05-17<br />precipitation: 0.00000","doy: 2022-05-18<br />precipitation: 0.00000","doy: 2022-05-19<br />precipitation: 0.00000","doy: 2022-05-20<br />precipitation: 0.17000","doy: 2022-05-21<br />precipitation: 0.00001","doy: 2022-05-22<br />precipitation: 0.00000","doy: 2022-05-23<br />precipitation: 0.00001","doy: 2022-05-24<br />precipitation: 0.01000","doy: 2022-05-25<br />precipitation: 0.00000","doy: 2022-05-26<br />precipitation: 0.00000","doy: 2022-05-27<br />precipitation: 0.00000","doy: 2022-05-28<br />precipitation: 0.00000","doy: 2022-05-29<br />precipitation: 0.00000","doy: 2022-05-30<br />precipitation: 1.65000","doy: 2022-05-31<br />precipitation: 0.08000","doy: 2022-06-01<br />precipitation: 0.00001","doy: 2022-06-02<br />precipitation: 1.60000","doy: 2022-06-03<br />precipitation: 0.00001","doy: 2022-06-04<br />precipitation: 0.47000","doy: 2022-06-05<br />precipitation: 0.00000","doy: 2022-06-06<br />precipitation: 0.00000","doy: 2022-06-07<br />precipitation: 0.16000","doy: 2022-06-08<br />precipitation: 0.00000","doy: 2022-06-09<br />precipitation: 0.10000","doy: 2022-06-10<br />precipitation: 0.01000","doy: 2022-06-11<br />precipitation: 2.81000","doy: 2022-06-12<br />precipitation: 0.00000","doy: 2022-06-13<br />precipitation: 0.00000","doy: 2022-06-14<br />precipitation: 0.00000","doy: 2022-06-15<br />precipitation: 0.11000","doy: 2022-06-16<br />precipitation: 0.00001","doy: 2022-06-17<br />precipitation: 0.00000","doy: 2022-06-18<br />precipitation: 0.00000","doy: 2022-06-19<br />precipitation: 0.04000","doy: 2022-06-20<br />precipitation: 0.10000","doy: 2022-06-21<br />precipitation: 0.00000","doy: 2022-06-22<br />precipitation: 0.00000","doy: 2022-06-23<br />precipitation: 0.00000","doy: 2022-06-24<br />precipitation: 0.19000","doy: 2022-06-25<br />precipitation: 0.00000","doy: 2022-06-26<br />precipitation: 0.00000","doy: 2022-06-27<br />precipitation: 1.28000","doy: 2022-06-28<br />precipitation: 0.09000","doy: 2022-06-29<br />precipitation: 0.04000","doy: 2022-06-30<br />precipitation: 1.07000","doy: 2022-07-01<br />precipitation: 0.00001","doy: 2022-07-02<br />precipitation: 0.00001","doy: 2022-07-03<br />precipitation: 0.00000","doy: 2022-07-04<br />precipitation: 0.00001","doy: 2022-07-05<br />precipitation: 0.01000","doy: 2022-07-06<br />precipitation: 0.05000","doy: 2022-07-07<br />precipitation: 0.03000","doy: 2022-07-08<br />precipitation: 0.19000","doy: 2022-07-09<br />precipitation: 0.00000","doy: 2022-07-10<br />precipitation: 0.87000","doy: 2022-07-11<br />precipitation: 0.63000","doy: 2022-07-12<br />precipitation: 0.00000","doy: 2022-07-13<br />precipitation: 0.00000","doy: 2022-07-14<br />precipitation: 2.08000","doy: 2022-07-15<br />precipitation: 0.00000","doy: 2022-07-16<br />precipitation: 2.86000","doy: 2022-07-17<br />precipitation: 0.78000","doy: 2022-07-18<br />precipitation: 0.00000","doy: 2022-07-19<br />precipitation: 0.02000","doy: 2022-07-20<br />precipitation: 0.00000","doy: 2022-07-21<br />precipitation: 0.00000","doy: 2022-07-22<br />precipitation: 1.64000","doy: 2022-07-23<br />precipitation: 1.69000","doy: 2022-07-24<br />precipitation: 0.00000","doy: 2022-07-25<br />precipitation: 0.02000","doy: 2022-07-26<br />precipitation: 1.12000","doy: 2022-07-27<br />precipitation: 0.00000","doy: 2022-07-28<br />precipitation: 0.00000","doy: 2022-07-29<br />precipitation: 0.00000","doy: 2022-07-30<br />precipitation: 0.00000","doy: 2022-07-31<br />precipitation: 0.00000","doy: 2022-08-01<br />precipitation: 0.00000","doy: 2022-08-02<br />precipitation: 0.00001","doy: 2022-08-03<br />precipitation: 0.98000","doy: 2022-08-04<br />precipitation: 1.24000","doy: 2022-08-05<br />precipitation: 0.00001","doy: 2022-08-06<br />precipitation: 0.04000","doy: 2022-08-07<br />precipitation: 0.24000","doy: 2022-08-08<br />precipitation: 0.06000","doy: 2022-08-09<br />precipitation: 1.28000","doy: 2022-08-10<br />precipitation: 0.00000","doy: 2022-08-11<br />precipitation: 0.00000","doy: 2022-08-12<br />precipitation: 0.00000","doy: 2022-08-13<br />precipitation: 0.00000","doy: 2022-08-14<br />precipitation: 0.00001","doy: 2022-08-15<br />precipitation: 0.00001","doy: 2022-08-16<br />precipitation: 0.00000","doy: 2022-08-17<br />precipitation: 0.64000","doy: 2022-08-18<br />precipitation: 0.00000","doy: 2022-08-19<br />precipitation: 0.00000","doy: 2022-08-20<br />precipitation: 0.47000","doy: 2022-08-21<br />precipitation: 0.45000","doy: 2022-08-22<br />precipitation: 0.00000","doy: 2022-08-23<br />precipitation: 0.00000","doy: 2022-08-24<br />precipitation: 0.61000","doy: 2022-08-25<br />precipitation: 0.31000","doy: 2022-08-26<br />precipitation: 0.00001","doy: 2022-08-27<br />precipitation: 0.02000","doy: 2022-08-28<br />precipitation: 0.16000","doy: 2022-08-29<br />precipitation: 0.00000","doy: 2022-08-30<br />precipitation: 0.00000","doy: 2022-08-31<br />precipitation: 0.01000","doy: 2022-09-01<br />precipitation: 2.74000","doy: 2022-09-02<br />precipitation: 1.41000","doy: 2022-09-03<br />precipitation: 0.40000","doy: 2022-09-04<br />precipitation: 0.00000","doy: 2022-09-05<br />precipitation: 0.00000","doy: 2022-09-06<br />precipitation: 0.00000","doy: 2022-09-07<br />precipitation: 0.10000","doy: 2022-09-08<br />precipitation: 1.64000","doy: 2022-09-09<br />precipitation: 0.13000","doy: 2022-09-10<br />precipitation: 1.09000","doy: 2022-09-11<br />precipitation: 0.02000","doy: 2022-09-12<br />precipitation: 0.03000","doy: 2022-09-13<br />precipitation: 0.03000","doy: 2022-09-14<br />precipitation: 0.06000","doy: 2022-09-15<br />precipitation: 0.00000","doy: 2022-09-16<br />precipitation: 0.87000","doy: 2022-09-17<br />precipitation: 0.06000","doy: 2022-09-18<br />precipitation: 0.00000","doy: 2022-09-19<br />precipitation: 0.02000","doy: 2022-09-20<br />precipitation: 0.07000","doy: 2022-09-21<br />precipitation: 0.00001","doy: 2022-09-22<br />precipitation: 0.00000","doy: 2022-09-23<br />precipitation: 1.07000","doy: 2022-09-24<br />precipitation: 0.00000","doy: 2022-09-25<br />precipitation: 0.00000","doy: 2022-09-26<br />precipitation: 0.00001","doy: 2022-09-27<br />precipitation: 0.08000","doy: 2022-09-28<br />precipitation: 2.47000","doy: 2022-09-29<br />precipitation: 0.00000","doy: 2022-09-30<br />precipitation: 0.00000","doy: 2022-10-01<br />precipitation: 0.00000","doy: 2022-10-02<br />precipitation: 0.00000","doy: 2022-10-03<br />precipitation: 0.00000","doy: 2022-10-04<br />precipitation: 0.00000","doy: 2022-10-05<br />precipitation: 0.00000","doy: 2022-10-06<br />precipitation: 0.00000","doy: 2022-10-07<br />precipitation: 0.00000","doy: 2022-10-08<br />precipitation: 0.00000","doy: 2022-10-09<br />precipitation: 0.00000","doy: 2022-10-10<br />precipitation: 0.00001","doy: 2022-10-11<br />precipitation: 0.00001","doy: 2022-10-12<br />precipitation: 0.02000","doy: 2022-10-13<br />precipitation: 0.30000","doy: 2022-10-14<br />precipitation: 0.00000","doy: 2022-10-15<br />precipitation: 0.00000","doy: 2022-10-16<br />precipitation: 0.00000","doy: 2022-10-17<br />precipitation: 0.00001","doy: 2022-10-18<br />precipitation: 0.01000","doy: 2022-10-19<br />precipitation: 0.00000","doy: 2022-10-20<br />precipitation: 0.00000","doy: 2022-10-21<br />precipitation: 0.00000","doy: 2022-10-22<br />precipitation: 0.00000","doy: 2022-10-23<br />precipitation: 0.00000","doy: 2022-10-24<br />precipitation: 0.00000","doy: 2022-10-25<br />precipitation: 0.00000","doy: 2022-10-26<br />precipitation: 0.00000","doy: 2022-10-27<br />precipitation: 0.06000","doy: 2022-10-28<br />precipitation: 0.71000","doy: 2022-10-29<br />precipitation: 0.00000","doy: 2022-10-30<br />precipitation: 0.00000","doy: 2022-10-31<br />precipitation: 0.00000","doy: 2022-11-01<br />precipitation: 0.00000","doy: 2022-11-02<br />precipitation: 0.00000","doy: 2022-11-03<br />precipitation: 0.90000","doy: 2022-11-04<br />precipitation: 0.00000","doy: 2022-11-05<br />precipitation: 0.00001","doy: 2022-11-06<br />precipitation: 0.00001","doy: 2022-11-07<br />precipitation: 0.00000","doy: 2022-11-08<br />precipitation: 0.06000","doy: 2022-11-09<br />precipitation: 0.09000","doy: 2022-11-10<br />precipitation: 2.46000","doy: 2022-11-11<br />precipitation: 0.76000","doy: 2022-11-12<br />precipitation: 0.01000","doy: 2022-11-13<br />precipitation: 0.01000","doy: 2022-11-14<br />precipitation: 0.00000","doy: 2022-11-15<br />precipitation: 0.00000","doy: 2022-11-16<br />precipitation: 0.14000","doy: 2022-11-17<br />precipitation: 0.00000","doy: 2022-11-18<br />precipitation: 0.00000","doy: 2022-11-19<br />precipitation: 0.00000","doy: 2022-11-20<br />precipitation: 0.39000","doy: 2022-11-21<br />precipitation: 0.00001","doy: 2022-11-22<br />precipitation: 0.00000","doy: 2022-11-23<br />precipitation: 0.05000","doy: 2022-11-24<br />precipitation: 0.00000","doy: 2022-11-25<br />precipitation: 0.00001","doy: 2022-11-26<br />precipitation: 0.00000","doy: 2022-11-27<br />precipitation: 0.27000","doy: 2022-11-28<br />precipitation: 0.00000","doy: 2022-11-29<br />precipitation: 0.00000","doy: 2022-11-30<br />precipitation: 0.04000","doy: 2022-12-01<br />precipitation: 0.00000","doy: 2022-12-02<br />precipitation: 0.00000","doy: 2022-12-03<br />precipitation: 0.00000","doy: 2022-12-04<br />precipitation: 0.00000","doy: 2022-12-05<br />precipitation: 0.00000","doy: 2022-12-06<br />precipitation: 0.00000","doy: 2022-12-07<br />precipitation: 0.00000","doy: 2022-12-08<br />precipitation: 0.00000","doy: 2022-12-09<br />precipitation: 0.00000","doy: 2022-12-10<br />precipitation: 0.00000","doy: 2022-12-11<br />precipitation: 0.00000","doy: 2022-12-12<br />precipitation: 0.00000","doy: 2022-12-13<br />precipitation: 0.00000","doy: 2022-12-14<br />precipitation: 0.00000","doy: 2022-12-15<br />precipitation: 1.09000","doy: 2022-12-16<br />precipitation: 0.05000","doy: 2022-12-17<br />precipitation: 0.02000","doy: 2022-12-18<br />precipitation: 0.04000","doy: 2022-12-19<br />precipitation: 0.00000","doy: 2022-12-20<br />precipitation: 0.11000","doy: 2022-12-21<br />precipitation: 0.60000","doy: 2022-12-22<br />precipitation: 0.15000","doy: 2022-12-23<br />precipitation: 0.00001","doy: 2022-12-24<br />precipitation: 0.00000","doy: 2022-12-25<br />precipitation: 0.00000","doy: 2022-12-26<br />precipitation: 0.00000","doy: 2022-12-27<br />precipitation: 0.00000","doy: 2022-12-28<br />precipitation: 0.00000","doy: 2022-12-29<br />precipitation: 0.00000","doy: 2022-12-30<br />precipitation: 0.00000","doy: 2022-12-31<br />precipitation: 0.29000"],"type":"scatter","mode":"lines","line":{"width":1.8897637795275593,"color":"rgba(0,0,255,0.6)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18993,18997.607594936708,19002.215189873419,19006.822784810127,19011.430379746835,19016.037974683546,19020.645569620254,19025.253164556962,19029.860759493669,19034.468354430381,19039.075949367088,19043.683544303796,19048.291139240508,19052.898734177215,19057.506329113923,19062.113924050635,19066.721518987342,19071.32911392405,19075.936708860758,19080.544303797469,19085.151898734177,19089.759493670885,19094.367088607596,19098.974683544304,19103.582278481012,19108.189873417723,19112.797468354431,19117.405063291139,19122.012658227846,19126.620253164558,19131.227848101265,19135.835443037973,19140.443037974685,19145.050632911392,19149.6582278481,19154.265822784811,19158.873417721519,19163.481012658227,19168.088607594938,19172.696202531646,19177.303797468354,19181.911392405062,19186.518987341773,19191.126582278481,19195.734177215189,19200.3417721519,19204.949367088608,19209.556962025315,19214.164556962027,19218.772151898735,19223.379746835442,19227.98734177215,19232.594936708861,19237.202531645569,19241.810126582277,19246.417721518988,19251.025316455696,19255.632911392404,19260.240506329115,19264.848101265823,19269.455696202531,19274.063291139242,19278.67088607595,19283.278481012658,19287.886075949365,19292.493670886077,19297.101265822785,19301.708860759492,19306.316455696204,19310.924050632912,19315.531645569619,19320.139240506331,19324.746835443038,19329.354430379746,19333.962025316454,19338.569620253165,19343.177215189873,19347.784810126581,19352.392405063292,19357],"y":[0.020702764414242736,0.024120676367449551,0.027703417062082059,0.031449156618119124,0.035356065155548011,0.039422312794356498,0.043646069654522689,0.048025505856034212,0.052558791518875589,0.057244096763035088,0.062080135214554973,0.067079167391985925,0.072247194481873792,0.077584576443587891,0.083091673236509678,0.088768844820021148,0.094616451153490766,0.10063485219630039,0.1068244079078275,0.1131854782474546,0.11972947015100945,0.12667925906489541,0.13406322511366789,0.14180293915548312,0.1498199720485146,0.15803589465093695,0.16637227782090491,0.17475069241659291,0.18309270929616886,0.19131989931780705,0.19943656494673448,0.20887061952335564,0.21978185527695315,0.23166495218067035,0.2440145902076761,0.25632544933114104,0.26809220952420632,0.27880955076004249,0.28797215301181711,0.29507469625267618,0.29999828172454268,0.30466812212116062,0.30925155396059778,0.31354193750416326,0.31733263301317738,0.32041700074895879,0.32258840097281927,0.32364019394607801,0.32336573993005063,0.32155839918605433,0.31796744154218404,0.31236035543238039,0.30504840313925824,0.29640405322816477,0.2867997742644301,0.27660803481338148,0.26620130344037052,0.25595204871072458,0.24623273918977187,0.23741584344286318,0.22926599307848805,0.22052059014247855,0.21119521738326094,0.20136994921649712,0.19112486005785564,0.18054002432299676,0.16969551642760586,0.15867141078734329,0.14754778181786887,0.13640470393486884,0.12528454081264803,0.11412101531947683,0.10287793581427204,0.0915215766170423,0.080018212047804987,0.068334116426568267,0.056435564073367944,0.044288829308212319,0.03186018645110885,0.01911590982209458],"text":["doy: 18993.00<br />precipitation: 0.02070276","doy: 18997.61<br />precipitation: 0.02412068","doy: 19002.22<br />precipitation: 0.02770342","doy: 19006.82<br />precipitation: 0.03144916","doy: 19011.43<br />precipitation: 0.03535607","doy: 19016.04<br />precipitation: 0.03942231","doy: 19020.65<br />precipitation: 0.04364607","doy: 19025.25<br />precipitation: 0.04802551","doy: 19029.86<br />precipitation: 0.05255879","doy: 19034.47<br />precipitation: 0.05724410","doy: 19039.08<br />precipitation: 0.06208014","doy: 19043.68<br />precipitation: 0.06707917","doy: 19048.29<br />precipitation: 0.07224719","doy: 19052.90<br />precipitation: 0.07758458","doy: 19057.51<br />precipitation: 0.08309167","doy: 19062.11<br />precipitation: 0.08876884","doy: 19066.72<br />precipitation: 0.09461645","doy: 19071.33<br />precipitation: 0.10063485","doy: 19075.94<br />precipitation: 0.10682441","doy: 19080.54<br />precipitation: 0.11318548","doy: 19085.15<br />precipitation: 0.11972947","doy: 19089.76<br />precipitation: 0.12667926","doy: 19094.37<br />precipitation: 0.13406323","doy: 19098.97<br />precipitation: 0.14180294","doy: 19103.58<br />precipitation: 0.14981997","doy: 19108.19<br />precipitation: 0.15803589","doy: 19112.80<br />precipitation: 0.16637228","doy: 19117.41<br />precipitation: 0.17475069","doy: 19122.01<br />precipitation: 0.18309271","doy: 19126.62<br />precipitation: 0.19131990","doy: 19131.23<br />precipitation: 0.19943656","doy: 19135.84<br />precipitation: 0.20887062","doy: 19140.44<br />precipitation: 0.21978186","doy: 19145.05<br />precipitation: 0.23166495","doy: 19149.66<br />precipitation: 0.24401459","doy: 19154.27<br />precipitation: 0.25632545","doy: 19158.87<br />precipitation: 0.26809221","doy: 19163.48<br />precipitation: 0.27880955","doy: 19168.09<br />precipitation: 0.28797215","doy: 19172.70<br />precipitation: 0.29507470","doy: 19177.30<br />precipitation: 0.29999828","doy: 19181.91<br />precipitation: 0.30466812","doy: 19186.52<br />precipitation: 0.30925155","doy: 19191.13<br />precipitation: 0.31354194","doy: 19195.73<br />precipitation: 0.31733263","doy: 19200.34<br />precipitation: 0.32041700","doy: 19204.95<br />precipitation: 0.32258840","doy: 19209.56<br />precipitation: 0.32364019","doy: 19214.16<br />precipitation: 0.32336574","doy: 19218.77<br />precipitation: 0.32155840","doy: 19223.38<br />precipitation: 0.31796744","doy: 19227.99<br />precipitation: 0.31236036","doy: 19232.59<br />precipitation: 0.30504840","doy: 19237.20<br />precipitation: 0.29640405","doy: 19241.81<br />precipitation: 0.28679977","doy: 19246.42<br />precipitation: 0.27660803","doy: 19251.03<br />precipitation: 0.26620130","doy: 19255.63<br />precipitation: 0.25595205","doy: 19260.24<br />precipitation: 0.24623274","doy: 19264.85<br />precipitation: 0.23741584","doy: 19269.46<br />precipitation: 0.22926599","doy: 19274.06<br />precipitation: 0.22052059","doy: 19278.67<br />precipitation: 0.21119522","doy: 19283.28<br />precipitation: 0.20136995","doy: 19287.89<br />precipitation: 0.19112486","doy: 19292.49<br />precipitation: 0.18054002","doy: 19297.10<br />precipitation: 0.16969552","doy: 19301.71<br />precipitation: 0.15867141","doy: 19306.32<br />precipitation: 0.14754778","doy: 19310.92<br />precipitation: 0.13640470","doy: 19315.53<br />precipitation: 0.12528454","doy: 19320.14<br />precipitation: 0.11412102","doy: 19324.75<br />precipitation: 0.10287794","doy: 19329.35<br />precipitation: 0.09152158","doy: 19333.96<br />precipitation: 0.08001821","doy: 19338.57<br />precipitation: 0.06833412","doy: 19343.18<br />precipitation: 0.05643556","doy: 19347.78<br />precipitation: 0.04428883","doy: 19352.39<br />precipitation: 0.03186019","doy: 19357.00<br />precipitation: 0.01911591"],"type":"scatter","mode":"lines","name":"fitted values","line":{"width":3.7795275590551185,"color":"rgba(0,0,139,1)","dash":"solid"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"none","frame":null},{"x":[18993,18997.607594936708,19002.215189873419,19006.822784810127,19011.430379746835,19016.037974683546,19020.645569620254,19025.253164556962,19029.860759493669,19034.468354430381,19039.075949367088,19043.683544303796,19048.291139240508,19052.898734177215,19057.506329113923,19062.113924050635,19066.721518987342,19071.32911392405,19075.936708860758,19080.544303797469,19085.151898734177,19089.759493670885,19094.367088607596,19098.974683544304,19103.582278481012,19108.189873417723,19112.797468354431,19117.405063291139,19122.012658227846,19126.620253164558,19131.227848101265,19135.835443037973,19140.443037974685,19145.050632911392,19149.6582278481,19154.265822784811,19158.873417721519,19163.481012658227,19168.088607594938,19172.696202531646,19177.303797468354,19181.911392405062,19186.518987341773,19191.126582278481,19195.734177215189,19200.3417721519,19204.949367088608,19209.556962025315,19214.164556962027,19218.772151898735,19223.379746835442,19227.98734177215,19232.594936708861,19237.202531645569,19241.810126582277,19246.417721518988,19251.025316455696,19255.632911392404,19260.240506329115,19264.848101265823,19269.455696202531,19274.063291139242,19278.67088607595,19283.278481012658,19287.886075949365,19292.493670886077,19297.101265822785,19301.708860759492,19306.316455696204,19310.924050632912,19315.531645569619,19320.139240506331,19324.746835443038,19329.354430379746,19333.962025316454,19338.569620253165,19343.177215189873,19347.784810126581,19352.392405063292,19357,19357,19357,19352.392405063292,19347.784810126581,19343.177215189873,19338.569620253165,19333.962025316454,19329.354430379746,19324.746835443038,19320.139240506331,19315.531645569619,19310.924050632912,19306.316455696204,19301.708860759492,19297.101265822785,19292.493670886077,19287.886075949365,19283.278481012658,19278.67088607595,19274.063291139242,19269.455696202531,19264.848101265823,19260.240506329115,19255.632911392404,19251.025316455696,19246.417721518988,19241.810126582277,19237.202531645569,19232.594936708861,19227.98734177215,19223.379746835442,19218.772151898735,19214.164556962027,19209.556962025315,19204.949367088608,19200.3417721519,19195.734177215189,19191.126582278481,19186.518987341773,19181.911392405062,19177.303797468354,19172.696202531646,19168.088607594938,19163.481012658227,19158.873417721519,19154.265822784811,19149.6582278481,19145.050632911392,19140.443037974685,19135.835443037973,19131.227848101265,19126.620253164558,19122.012658227846,19117.405063291139,19112.797468354431,19108.189873417723,19103.582278481012,19098.974683544304,19094.367088607596,19089.759493670885,19085.151898734177,19080.544303797469,19075.936708860758,19071.32911392405,19066.721518987342,19062.113924050635,19057.506329113923,19052.898734177215,19048.291139240508,19043.683544303796,19039.075949367088,19034.468354430381,19029.860759493669,19025.253164556962,19020.645569620254,19016.037974683546,19011.430379746835,19006.822784810127,19002.215189873419,18997.607594936708,18993,18993],"y":[-0.15766573173030271,-0.13984843922697501,-0.12283640948129813,-0.1066928489368521,-0.091484023410496551,-0.077276220449003188,-0.064130445589904272,-0.052094391830394979,-0.04119196356482123,-0.031412005578086974,-0.022698567127890278,-0.014927929374325175,-0.0079566588993695009,-0.0016382925836992274,0.0041766693658719656,0.0096327033835583026,0.014863178476113043,0.019986814122483793,0.025106392101276132,0.030309001720553869,0.03569206535937039,0.041801051227626035,0.048647869264393601,0.055945472809689908,0.063422658459003106,0.070878448766521995,0.078215834078217847,0.085451565204893956,0.092702482188320764,0.10015392102512818,0.10811708968493156,0.1184147912354634,0.13081783824252549,0.14419899719767232,0.15751677237562373,0.16993642056148714,0.18091605265259622,0.19021683214705654,0.19783162809941246,0.20386816755343617,0.20879065238627523,0.21452512955234665,0.22067299772864102,0.22642965117150834,0.23109164084070749,0.23416835843998035,0.23545563725206237,0.23503507664720547,0.23319946246964285,0.23034081026392966,0.22670376737314885,0.22176890167412719,0.21550569738220335,0.20801109161790438,0.19944588963363405,0.19006976812648541,0.18025830389155117,0.1704957440555952,0.16134216141172922,0.15337795260108067,0.1463895165515873,0.13880257433592716,0.13054717930944434,0.12161667653911941,0.11198871862139284,0.10162502045235912,0.090472647400318809,0.07846755740610005,0.06554068505155787,0.05162600159242367,0.036628438471526026,0.020370260235780022,0.0027580381278428101,-0.016254938627384766,-0.036680321195554907,-0.0585059721394766,-0.081706441481603692,-0.1062509972351684,-0.13210892914331632,-0.15925258632245154,-0.15925258632245154,0.19748440596664069,0.19582930204553403,0.19482865585159304,0.19457756962833958,0.19517420499261312,0.1967167452911649,0.19929809186146935,0.20299783350070127,0.20787177040317362,0.21394064315377004,0.221183406277314,0.22955487858417989,0.23887526416858651,0.24891838545489292,0.25945502819363442,0.27026100149431842,0.28112322189387484,0.29184325545707757,0.30223860594902996,0.31214246960538883,0.32145373428464569,0.3311233169678145,0.34140835336585396,0.35214430298918986,0.36314630150027755,0.37415365889522612,0.38479701483842516,0.3945911088963131,0.40295180919063356,0.40923111571121923,0.412775988108179,0.41353201739045842,0.41224531124495056,0.40972116469357617,0.40666564305793723,0.40357362518564727,0.40065422383681815,0.39783011019255454,0.3948111146899746,0.39120591106281011,0.38628122495191619,0.37811267792422176,0.36740226937302845,0.35526836639581638,0.34271447810079492,0.33051240803972848,0.31913090716366838,0.30874587231138084,0.2993264478112479,0.2907560402085374,0.28248587761048594,0.27348293640401694,0.26404981962829188,0.25452872156359196,0.24519334053535191,0.2362172856380261,0.22766040550127631,0.21947858096294218,0.21155746690216479,0.2037668749426485,0.19606195477435534,0.18854242371437888,0.18128289027011699,0.17436972383086849,0.16790498625648398,0.16200667710714739,0.156807445470875,0.15245104786311708,0.14908626415829701,0.14685883755700022,0.14590019910415714,0.14630954660257239,0.14814540354246342,0.15142258489894964,0.1561208460377162,0.16219615372159257,0.16959116217309034,0.17824324360546226,0.18808979196187411,0.19907126055878818,-0.15766573173030271],"text":["doy: 18993.00<br />precipitation: 0.02070276","doy: 18997.61<br />precipitation: 0.02412068","doy: 19002.22<br />precipitation: 0.02770342","doy: 19006.82<br />precipitation: 0.03144916","doy: 19011.43<br />precipitation: 0.03535607","doy: 19016.04<br />precipitation: 0.03942231","doy: 19020.65<br />precipitation: 0.04364607","doy: 19025.25<br />precipitation: 0.04802551","doy: 19029.86<br />precipitation: 0.05255879","doy: 19034.47<br />precipitation: 0.05724410","doy: 19039.08<br />precipitation: 0.06208014","doy: 19043.68<br />precipitation: 0.06707917","doy: 19048.29<br />precipitation: 0.07224719","doy: 19052.90<br />precipitation: 0.07758458","doy: 19057.51<br />precipitation: 0.08309167","doy: 19062.11<br />precipitation: 0.08876884","doy: 19066.72<br />precipitation: 0.09461645","doy: 19071.33<br />precipitation: 0.10063485","doy: 19075.94<br />precipitation: 0.10682441","doy: 19080.54<br />precipitation: 0.11318548","doy: 19085.15<br />precipitation: 0.11972947","doy: 19089.76<br />precipitation: 0.12667926","doy: 19094.37<br />precipitation: 0.13406323","doy: 19098.97<br />precipitation: 0.14180294","doy: 19103.58<br />precipitation: 0.14981997","doy: 19108.19<br />precipitation: 0.15803589","doy: 19112.80<br />precipitation: 0.16637228","doy: 19117.41<br />precipitation: 0.17475069","doy: 19122.01<br />precipitation: 0.18309271","doy: 19126.62<br />precipitation: 0.19131990","doy: 19131.23<br />precipitation: 0.19943656","doy: 19135.84<br />precipitation: 0.20887062","doy: 19140.44<br />precipitation: 0.21978186","doy: 19145.05<br />precipitation: 0.23166495","doy: 19149.66<br />precipitation: 0.24401459","doy: 19154.27<br />precipitation: 0.25632545","doy: 19158.87<br />precipitation: 0.26809221","doy: 19163.48<br />precipitation: 0.27880955","doy: 19168.09<br />precipitation: 0.28797215","doy: 19172.70<br />precipitation: 0.29507470","doy: 19177.30<br />precipitation: 0.29999828","doy: 19181.91<br />precipitation: 0.30466812","doy: 19186.52<br />precipitation: 0.30925155","doy: 19191.13<br />precipitation: 0.31354194","doy: 19195.73<br />precipitation: 0.31733263","doy: 19200.34<br />precipitation: 0.32041700","doy: 19204.95<br />precipitation: 0.32258840","doy: 19209.56<br />precipitation: 0.32364019","doy: 19214.16<br />precipitation: 0.32336574","doy: 19218.77<br />precipitation: 0.32155840","doy: 19223.38<br />precipitation: 0.31796744","doy: 19227.99<br />precipitation: 0.31236036","doy: 19232.59<br />precipitation: 0.30504840","doy: 19237.20<br />precipitation: 0.29640405","doy: 19241.81<br />precipitation: 0.28679977","doy: 19246.42<br />precipitation: 0.27660803","doy: 19251.03<br />precipitation: 0.26620130","doy: 19255.63<br />precipitation: 0.25595205","doy: 19260.24<br />precipitation: 0.24623274","doy: 19264.85<br />precipitation: 0.23741584","doy: 19269.46<br />precipitation: 0.22926599","doy: 19274.06<br />precipitation: 0.22052059","doy: 19278.67<br />precipitation: 0.21119522","doy: 19283.28<br />precipitation: 0.20136995","doy: 19287.89<br />precipitation: 0.19112486","doy: 19292.49<br />precipitation: 0.18054002","doy: 19297.10<br />precipitation: 0.16969552","doy: 19301.71<br />precipitation: 0.15867141","doy: 19306.32<br />precipitation: 0.14754778","doy: 19310.92<br />precipitation: 0.13640470","doy: 19315.53<br />precipitation: 0.12528454","doy: 19320.14<br />precipitation: 0.11412102","doy: 19324.75<br />precipitation: 0.10287794","doy: 19329.35<br />precipitation: 0.09152158","doy: 19333.96<br />precipitation: 0.08001821","doy: 19338.57<br />precipitation: 0.06833412","doy: 19343.18<br />precipitation: 0.05643556","doy: 19347.78<br />precipitation: 0.04428883","doy: 19352.39<br />precipitation: 0.03186019","doy: 19357.00<br />precipitation: 0.01911591","doy: 19357.00<br />precipitation: 0.01911591","doy: 19357.00<br />precipitation: 0.01911591","doy: 19352.39<br />precipitation: 0.03186019","doy: 19347.78<br />precipitation: 0.04428883","doy: 19343.18<br />precipitation: 0.05643556","doy: 19338.57<br />precipitation: 0.06833412","doy: 19333.96<br />precipitation: 0.08001821","doy: 19329.35<br />precipitation: 0.09152158","doy: 19324.75<br />precipitation: 0.10287794","doy: 19320.14<br />precipitation: 0.11412102","doy: 19315.53<br />precipitation: 0.12528454","doy: 19310.92<br />precipitation: 0.13640470","doy: 19306.32<br />precipitation: 0.14754778","doy: 19301.71<br />precipitation: 0.15867141","doy: 19297.10<br />precipitation: 0.16969552","doy: 19292.49<br />precipitation: 0.18054002","doy: 19287.89<br />precipitation: 0.19112486","doy: 19283.28<br />precipitation: 0.20136995","doy: 19278.67<br />precipitation: 0.21119522","doy: 19274.06<br />precipitation: 0.22052059","doy: 19269.46<br />precipitation: 0.22926599","doy: 19264.85<br />precipitation: 0.23741584","doy: 19260.24<br />precipitation: 0.24623274","doy: 19255.63<br />precipitation: 0.25595205","doy: 19251.03<br />precipitation: 0.26620130","doy: 19246.42<br />precipitation: 0.27660803","doy: 19241.81<br />precipitation: 0.28679977","doy: 19237.20<br />precipitation: 0.29640405","doy: 19232.59<br />precipitation: 0.30504840","doy: 19227.99<br />precipitation: 0.31236036","doy: 19223.38<br />precipitation: 0.31796744","doy: 19218.77<br />precipitation: 0.32155840","doy: 19214.16<br />precipitation: 0.32336574","doy: 19209.56<br />precipitation: 0.32364019","doy: 19204.95<br />precipitation: 0.32258840","doy: 19200.34<br />precipitation: 0.32041700","doy: 19195.73<br />precipitation: 0.31733263","doy: 19191.13<br />precipitation: 0.31354194","doy: 19186.52<br />precipitation: 0.30925155","doy: 19181.91<br />precipitation: 0.30466812","doy: 19177.30<br />precipitation: 0.29999828","doy: 19172.70<br />precipitation: 0.29507470","doy: 19168.09<br />precipitation: 0.28797215","doy: 19163.48<br />precipitation: 0.27880955","doy: 19158.87<br />precipitation: 0.26809221","doy: 19154.27<br />precipitation: 0.25632545","doy: 19149.66<br />precipitation: 0.24401459","doy: 19145.05<br />precipitation: 0.23166495","doy: 19140.44<br />precipitation: 0.21978186","doy: 19135.84<br />precipitation: 0.20887062","doy: 19131.23<br />precipitation: 0.19943656","doy: 19126.62<br />precipitation: 0.19131990","doy: 19122.01<br />precipitation: 0.18309271","doy: 19117.41<br />precipitation: 0.17475069","doy: 19112.80<br />precipitation: 0.16637228","doy: 19108.19<br />precipitation: 0.15803589","doy: 19103.58<br />precipitation: 0.14981997","doy: 19098.97<br />precipitation: 0.14180294","doy: 19094.37<br />precipitation: 0.13406323","doy: 19089.76<br />precipitation: 0.12667926","doy: 19085.15<br />precipitation: 0.11972947","doy: 19080.54<br />precipitation: 0.11318548","doy: 19075.94<br />precipitation: 0.10682441","doy: 19071.33<br />precipitation: 0.10063485","doy: 19066.72<br />precipitation: 0.09461645","doy: 19062.11<br />precipitation: 0.08876884","doy: 19057.51<br />precipitation: 0.08309167","doy: 19052.90<br />precipitation: 0.07758458","doy: 19048.29<br />precipitation: 0.07224719","doy: 19043.68<br />precipitation: 0.06707917","doy: 19039.08<br />precipitation: 0.06208014","doy: 19034.47<br />precipitation: 0.05724410","doy: 19029.86<br />precipitation: 0.05255879","doy: 19025.25<br />precipitation: 0.04802551","doy: 19020.65<br />precipitation: 0.04364607","doy: 19016.04<br />precipitation: 0.03942231","doy: 19011.43<br />precipitation: 0.03535607","doy: 19006.82<br />precipitation: 0.03144916","doy: 19002.22<br />precipitation: 0.02770342","doy: 18997.61<br />precipitation: 0.02412068","doy: 18993.00<br />precipitation: 0.02070276","doy: 18993.00<br />precipitation: 0.02070276"],"type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(0,0,139,0.4)","dash":"solid"},"fill":"toself","fillcolor":"rgba(153,153,153,0.4)","hoveron":"points","hoverinfo":"none","showlegend":false,"xaxis":"x","yaxis":"y","frame":null}],"layout":{"margin":{"t":40.840182648401829,"r":7.3059360730593621,"b":22.648401826484022,"l":31.415525114155255},"plot_bgcolor":"rgba(255,255,255,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724},"title":{"text":"Daily Precipitation Trends in Tampa From January 1 2022 to January 1 2023","font":{"color":"rgba(0,0,0,1)","family":"","size":17.534246575342465},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[18974.799999999999,19375.200000000001],"tickmode":"array","ticktext":["Jan 2022","Apr 2022","Jul 2022","Oct 2022","Jan 2023"],"tickvals":[18993,19083,19174,19266,19358],"categoryorder":"array","categoryarray":["Jan 2022","Apr 2022","Jul 2022","Oct 2022","Jan 2023"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(211,211,211,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":null,"family":null,"size":0}},"hoverformat":".2f","rangeslider":{"type":"date"},"rangeselector":{"buttons":[{"count":1,"label":"1m","step":"month","stepmode":"backward"},{"count":3,"label":"3m","step":"month","stepmode":"backward"},{"step":"all","label":"All"}]}},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.31021521563857413,3.0109626293161225],"tickmode":"array","ticktext":["0","1","2","3"],"tickvals":[0,0.99999999999999989,2,3],"categoryorder":"array","categoryarray":["0","1","2","3"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.6529680365296811,"tickwidth":0.66417600664176002,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.68949771689498},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(211,211,211,1)","gridwidth":0.66417600664176002,"zeroline":false,"anchor":"x","title":{"text":"Precipitation (inches)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.611872146118724}},"hoverformat":".2f"},"shapes":[],"showlegend":false,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.8897637795275593,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.68949771689498}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"5bdc51f243a2":{"x":{},"y":{},"type":"scatter"},"5bdc219ccc":{"x":{},"y":{}}},"cur_data":"5bdc51f243a2","visdat":{"5bdc51f243a2":["function (y) ","x"],"5bdc219ccc":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```

>*This figure displays the volatile trends of rainfall in Tampa, Florida over the course of 2022. It can be seen that the rainy season takes place during the summer and fall months, peaking around late July both in intensity and volume. This trend is also reflected in the dark blue LOESS smoothing trend line which displays that rainfall increased from the spring months into the summer and fall months, then decreases again into the winter months. This plot is interactive. By clicking and dragging the cursor on the plot, a specific range of data can be viewed in more detail. Double-clicking on the plot will return to normal viewing when zoomed in.*

**Interactive vs. Static Visualizations:** A static version of this chart suffers heavily from visual crowding due to the nature of this highly erratic precipitation data, making individual dates difficult to isolate and view. However, when incorporating interactivity into this chart, it solves this by letting the reader hover over any single peak to extract precise local values and exact dates instantly. Furthermore, the inclusion of a range slider allows any user to dynamically isolate specific stormy weeks or dry spells, providing more accessibility for inspection of the data that a static data visualization simple could not.

## Bad Chart Redesign: Monthly Temperature Distributions

### Original Version (Before Redesign)


``` r
ggplot(tpa_clean, aes(x = max_temp, fill = month_name)) +
  geom_density(alpha = 0.3) +
  labs(title = "Maximum Temperatures by Month", 
       x = "Maximum Temperature (°F)", 
       y = "Density") +
  theme(plot.title = element_text(hjust = 0.5), 
        panel.background = element_rect(fill = "white"), 
        panel.grid.major = element_line(color = "lightgray", size = 0.7), 
        panel.grid.minor = element_line(color = "lightgray", size = 0.3),
        legend.title = element_blank())
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-13-1.png" alt="An aggregate density plot overlaying twelve distinct monthly temperature (°F) curves on a single set of axes. Due to severe overlapping, the individual curves, especially for the summer months, blend into somewhat unreadable spikes, completely obscuring the unique shape and range of each month's distribution."  />

**Discussion:** This overlapping density plot attempts to compare the maximum temperature (°F) profiles of all twelve months simultaneously on a single axis. While intended to provide a direct comparison, it is visually cluttered and the density shapes blend into one another masking the data. This makes it impossible to isolate individual trends or accurately trace where one month ends and another begins, especially in the summer months.

### Improved Version (After Redesign)


``` r
violin_boxplot <- ggplot(tpa_clean, aes(x = max_temp, y = month_name, fill = month_name)) +
  geom_violin(alpha = 0.5, color = "black", scale = "width") +
  geom_boxplot(width = 0.15, fill = "white", color = "black", outlier.shape = NA, alpha = 0.8) +
  scale_fill_viridis_d(option = "viridis") +
  labs(title = "Monthly Maximum Temperature Distributions (2022)",
       x = "Maximum Temperature (°F)",
       y = "Month") +
 theme(plot.title = element_text(hjust = 0.5, size = 14), 
       panel.background = element_rect(fill = "white"), 
       panel.grid.major = element_line(color = "lightgray", size = 0.7), 
       panel.grid.minor = element_line(color = "lightgray", size = 0.3), 
       axis.title.x = element_text(size = 12),
       axis.text = element_text(size = 10),
       axis.title.y = element_blank(),
       legend.position = "none")

violin_boxplot
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-14-1.png" alt="A horizontal hybrid visualization displaying monthly maximum temperature (°F) distributions for 2022. Twelve sequentially stacked violin plots illustrate the density curves for each month horizontally, while nested white boxplots mark the median and IQR. The layout captures wide, flat winter distributions transitioning into tight, high-temperature summer distributions near the mid 90s."  />

``` r
ggsave(filename = "../figures/tpa_monthly_violin_boxplot.png", 
       plot = violin_boxplot, 
       width = 6, 
       height = 7, 
       dpi = 300)
```

**Discussion:** This horizontal hybrid violin-boxplot resolves the data occlusion of the overlapping plot by separating all twelve months sequentially along the vertical axis. The horizontal layout provides a clean progression that allows for immediate tracking of the changing temperatures throughout 2022. The violin geometries successfully preserve the unique density distributions of each month, revealing volatile, wide temperature spans in the winter months of January and February, shifting toward narrow distributions centered around the mid 90s during the peak summer months of July and August. Additionally, the nested boxplots cleanly show the median and quartile values for each month without leading to visual clutter.

## PART 2 

> **You can choose to work on either Option (A) or Option (B)**. Remove from this template the option you decided not to work on.

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
                                                probs = seq(0, 1, 0.2))))
```

1. Explore the distribution of 2 of the continuous variables available in the dataset. Do ranges make sense? Comment on your findings.


``` r
ggplot(new_concrete, aes(x = Cement)) +
  geom_histogram(binwidth = 30, fill = "darkgrey", color = "white", alpha = 0.8) +
  labs(title = "Distribution of Cement Content",
       x = "Cement (kg in a m³ mixture)",
       y = "Count") +
  theme(plot.title = element_text(hjust = 0.5),
        panel.background = element_rect(fill = "white"), 
        panel.grid.major = element_line(color = "lightgray"), 
        panel.grid.minor = element_line(color = "lightgray"))
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-17-1.png" alt="A histogram showing the distribution of cement content across the concrete samples. The values range from roughly 100 kg/m³ to over 500 kg/m³, with a heavy concentration of data points falling below 400 kg/m³, representing ordinary and standard concrete categories and grades."  />

**Cement Content Ranges & Distribution:** The cement content shown in the above histogram ranges roughly from a little less than $100 \text{ kg/m}^3$ to over $500 \text{ kg/m}^3$, and after some [research](https://pennarrmc.com/grades-of-concrete/), this range does of values does indeed make sense. Concrete mixtures depending on the grade, were found to range roughly from $170 \text{ kg/m}^3$ on the lower end up to $510 \text{ kg/m}^3$on the high end. Now, there are three main categories of concrete mixtures which exist, those being *ordinary concrete*, *standard concrete*, and *high-strength concrete*. Each of these three main categories has different grades which fall into their respective categories depending on their mechanical properties. *Ordinary concrete* and *standard concrete* are the most common and they tend to see mixtures with cement content under $400 \text{ kg/m}^3$, whereas *high-strength concrete* is less common and typically sees mixtures with cement content over $400 \text{ kg/m}^3$. When taking this into account, the range of values found above does make complete sense as most of the data points are found to be under $400 \text{ kg/m}^3$ falling into the categories of either *ordinary concrete* or *standard concrete* which are more commonly used, while less data points are over $400 \text{ kg/m}^3$ and fall into the category of *high-strength concrete* which is less commonly utilized.


``` r
ggplot(new_concrete, aes(x = Water)) +
  geom_histogram(binwidth = 10, fill = "lightblue", color = "white", alpha = 0.8) +
  labs(title = "Distribution of Water Content",
       x = "Water (kg in a m³ mixture)",
       y = "Count") +
  theme(plot.title = element_text(hjust = 0.5),
        panel.background = element_rect(fill = "white"), 
        panel.grid.major = element_line(color = "lightgray"), 
        panel.grid.minor = element_line(color = "lightgray"))
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-18-1.png" alt="A histogram displaying the distribution of water content in the concrete mixtures. The distribution spans from approximately 120 kg/m³ to 250 kg/m³, with the vast majority of the samples clustering tightly between 160 kg/m³ and 200 kg/m³."  />

**Water Content Ranges & Distribution:** The water content shown in this histogram ranges from approximately $120 \text{ kg/m}^3$ to nearly $250 \text{ kg/m}^3$. This range is realistic as the typical water to cement ratio typically lies between [0.3 to 0.8](https://concretecountertopinstitute.com/free-training/the-importance-of-water-cement-ratio-in-concrete-countertop-mix-design/). Now, having a higher water to cement ratio will lead to a weaker and more brittle concrete mixture, while having a lower water to cement ratio will lead to a stronger mixture, with a ratio of 0.6 - 0.7 being the average. When taking into account that the majority of data points for cement fell between $160 \text{ kg/m}^3$ and $400 \text{ kg/m}^3$, the respective range for water should fall between $115 \text{ kg/m}^3$ and $240 \text{ kg/m}^3$. This is almost what the data exactly reflects, therefore, verifying that the range is indeed realistic.

2. Use a _temporal_ indicator such as the one available in the variable `Age` (measured in days). Generate a plot similar to the one shown below. Comment on your results.

<img src="../figures/concrete_strength.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(new_concrete, aes(x = as.factor(Age), y = Concrete_compressive_strength, fill = strength_range)) +
  geom_boxplot(outlier.size = 1) +
  scale_fill_viridis_d(option = "viridis") +
  labs(x = "Age (in days)",
       y = "Compressive Strength (in MPa)",
       fill = "Strength Range") +
  theme(panel.background = element_rect(fill = "white"),
        panel.grid.major = element_line(color = "lightgray", size = 0.3),
        panel.grid.minor = element_line(color = "lightgray", size = 0.1),
        legend.position = "right")
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-20-1.png" alt="Side-by-side boxplots mapping concrete compressive strength across discrete age milestones, colored by strength range quantiles. The visualization shows an trending upward shift in strength over time, the complete disappearance of the lowest strength category after 28 days, and a peak concentration of high-strength concrete around 91 days of aging."  />

**Discussion:** This boxplot data visualization showcases `Concrete_compressive_strength` across various `age` milestones while being segmented by the calculated `strength_range` quantiles. Early on, a trend showing an increase in the compressive strength can be seen as the strength range transitions from just including the (dark purple, 2.33 – 21 MPa) range to include the (dark blue, 21 – 30.9 MPa) range, then the (teal, 30.9 – 39 MPa) range, etc. as the aging process goes on. However, at 28 days, this trend seems to end alongside the presence of the lowest strength range (dark purple, 2.33 – 21 MPa) which completely disappears as the aging process continues. As for the highest peak compressive strengths, they are heavily concentrated around the 91-day mark. Though, once past 90 days, primarily from 180 to 365 days, the distribution stabilizes, and fewer high-strength outliers are observed.

3. Create a scatterplot similar to the one shown below. Pay special attention to which variables are being mapped to specific aesthetics of the plot. Comment on your results. 

<img src="../figures/cement_plot.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(concrete, aes(x = Cement, y = Concrete_compressive_strength, color = Water, size = Age)) +
  geom_point(alpha = 0.6) +
  scale_color_viridis_c(option = "viridis") +
  scale_size_continuous(breaks = c(100, 200, 300)) +
  labs(title = "Exploring Strength versus (Cement, Age, and Water)",
       x = "Cement (kg in a m³ mixture)",
       y = "Compressive Strength (in MPa)",
       color = "Water",
       size = "Age",
       caption = "Age is measured in days") +
  theme(plot.title = element_text(size = 14), 
        plot.caption = element_text(hjust = 1, size = 10),
        panel.background = element_rect(fill = "white"),
        panel.grid.major = element_line(color = "lightgray", size = 0.3),
        panel.grid.minor = element_line(color = "lightgray", size = 0.1),
        axis.title = element_text(size = 12),
        legend.position = "right")
```

<img src="Dataviz-Project-3---Kohler_files/figure-html/unnamed-chunk-22-1.png" alt="A comprehensive scatterplot exploring concrete compressive strength against cement content on the axes, with water content mapped to a viridis color scale and age mapped to point size. It highlights that the highest compressive strengths are achieved by mixtures that have higher cement volumes and lower water content while having been aged for longer."  />

**Discussion:** This scatterplot visualizes the multi-variable relationship influencing the compressive strength of concrete. A notable positive correlation between `Cement` content and `Compressive Strength` can be seen as when the cement content increases from $100 \text{ kg/m}^3$ to over $500 \text{ kg/m}^3$, the baseline strength floor noticeably rises. Another positive correlation can be seen between `Age` and `Compressive Strength` as the highest strength values are overwhelmingly populated by points that are approaching or exceeding 100 days in age. On the other hand, the opposite type of correlation is seen between `Water` and `Compressive Strength` with the strength of the concrete mixture increasing with less water content ($125 \text{ kg/m}^3$ to $175 \text{ kg/m}^3$). Conversely, the bright yellow-green clusters (`Water` content > $200 \text{ kg/m}^3$) are heavily concentrated at lower strength. This shows that in order to optimize the compressive strength of concrete, one must maximize cement volume, minimize water volume, and allow over 90 - 100 days of aging to take place.
