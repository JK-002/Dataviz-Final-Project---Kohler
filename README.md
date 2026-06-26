# Data Visualization and Reproducible Research

> Justin Kohler 

The following is a sample of products created during the _"Data Visualization and Reproducible Research"_ course.

## Project 1

In the `Project 1/` folder, a thorough report which analyzes historical birth patterns across the United States from 2000 to 2014
using the `tidyverse` suite amongst other libraries can be found. This project was motivated by demographic concerns regarding 
declining birth rates falling below the replacement rate. This project applies core data visualization principles including
reducing chart junk, deploying color strategically, and integrating the Gestalt principles of proximity and continuity to uncover
how factors like economic conditions, seasonal cycles, and standard medical practices influence national birth distributions. A
central focus of this project was executing a "Bad Chart Redesign" to resolve calendar day bias. Rather than distorting seasonal
trends through raw monthly aggregates that artificially penalize shorter months like February, the data was mutated into a
normalized average daily birth rate to ensure an accurate evaluation. The resulting visualization showed that economic conditions
could be influential when it comes to affecting national birth rates. The birth rate rose during early-2000s and peaked at over
4.3 million in 2007 before entering a sharp decline immediately following the onset of the 2008 Great Recession. It was also found
that average daily births remain elevated on weekdays and sit around 13,000 births, but plunge to roughly 7,500 on weekends. This 
could be due to hospital labor scheduling for medically assisted delivery procedures.

**Sample data visualization:** This normalized column chart resolves the calendar limitations of the original aggregated plot by 
transitioning from raw monthly sums to a true daily average distribution rate. This was done by calculating the mean number of 
births per day within each specific month, ensuring that every month is properly evaluated. February is no longer penalized for
its shorter 28-day length, and 31-day months are stripped of their artificial advantage. Now, this visualization confirms that 
even when accounting for day counts, an increase in average daily births still takes place during the late summer, clearly 
peaking in August and September.

<img src="figures/monthly_birth_distribution.png" width="80%" height="80%">

## Project 2

In this project, multiple datasets were explored and utilized to create a variety of different data visualizations consisting of
an interactive plot, a spatial visualization, and a visualization of a model (coefficient plot). Packages including `tidyverse`,
`plotly`, `sf`, `maps`, and `broom`, were all utilized to accomplish this. These three data visualizations all tell different 
stories, came with different difficulties, and utilized different data visualization principles. The interactive plot showcases 
the volatility in game-by-game performance of the 1994 NBA championship team. The data shows that over seven games, no single 
variable, whether it be venue, points scored, assists, etc. singlehandedly dictated the outcome of the game. As for the spatial
visualization, it showcased the distribution of private schools in comparison to the total schools in each county throughout the
state of Florida, averaging at about 40% per county statewide. By comparing the baseline map against the improve version with 
point-density, the data displayed that more urbanized counties have higher percentages of private schools, while the coordinates
also demonstrated that these institutions are intensely clustered within city centers and coastal areas, leaving rural interior
zones with little to no private institutions. Finally, the coefficient plot identified that out of the three variables of
`Education`, `Age`, and `Gender`, only `Education` and `Age` were identified as statistically significant with 'Education' being
twice as significant as `Age`. This indicates that education should be prioritized if seeking an improvement in income. Find the
code and report in the `Project 2/` folder.

**Sample data visualization:** This choropleth spatial visualization provides extra geographic precision as it now locates 
where all the private school facilities are located and gathered throughout the entire state of Florida and its counties.
By plotting the actual latitudinal and longitudinal location of each private school directly over its the shaded county
it resides, this choropleth now reveals that private schools are heavily clustered within specific urban city centers 
and high-density coastal areas while leaving the interior rural areas of those same counties almost entirely empty. This
helps to provide more insight into where private schooling facilities are actually found throughout the state of Florida,
not just what counties they can be found in, but what types of areas they primarily exist.

<img src="figures/florida_private_schools.png" width="80%" height="80%">

## Project 3

This project explored a two primary datasets which were looking at weather data collected for Tampa, Florida throughout the 
year of 2022 as well as concrete mixture data. Utilizing libraries such as `tidyverse`, `lubridate`, `ggridges`, and `plotly`,
this project recreated various figures with the goal of keeping them as similar as possible to the original while ensuring
accessibility in the form of color blindness accommodations and alternative figure text. Now, the figures that were recreated
consist of a faceted histogram of maximum month temperatures, a density plot of maximum temperatures, a faceted density plot 
of maximum monthly temperatures, and ridgeline plot of maximum temperatures by month. As for the original charts that were 
created with this weather dataset, an interactive chart displaying daily precipitation trends in Tampa throughout 2022 was 
created, alongside two more charts showcasing monthly temperature distributions, one that is bad, and one that is improved. 
All these charts provided insight into the 2022 weather trends for Tampa, Florida such as which months see the highest 
temperatures, have the most temperature variance, what the average temperature in Tampa, Florida is, etc. In the case of the 
concrete dataset, two unique charts were first created to look at the recorded data ranges for cement and water. These figures 
were histograms and after doing some research, the data ranges display for both cement and water did make sense. The elaboration
can be read within the full project 3 report. Then, two more data visualizations were recreated with the first being a boxplot 
which showcased the compressive strength for concrete across various age milestones. The second figure recreated was a complex 
scatterplot which displayed the multi-variable relationship influencing the compressive strength of concrete. These influencing 
variables were water content, cement content, and age.

**Sample data visualization:** This horizontal hybrid violin-boxplot resolves the data occlusion of the overlapping plot 
by separating all twelve months sequentially along the vertical axis. The horizontal layout provides a clean progression
that allows for immediate tracking of the changing temperatures throughout 2022. The violin geometries successfully
preserve the unique density distributions of each month, revealing volatile, wide temperature spans in the winter months
of January and February, shifting toward narrow distributions centered around the mid 90s during the peak summer months 
of July and August. Additionally, the nested boxplots cleanly show the median and quartile values for each month without
leading to visual clutter.

<img src="figures/tpa_monthly_violin_boxplot.png" width="80%" height="80%">

### Moving Forward

Participating in this *Data Visualization and Reproducible Research* course has provided me with a vast amount of knowledge related 
to translating and transforming complex, raw datasets into clean, accessible, datasets which can be used to create more impact
data visualization through the utilization of RStudio. Through hands-on practice in the form of lecture examples, extra resource videos,
reading exercises, homework, and projects, I learned that there is far more to data analysis and reporting data than simply plotting 
data and providing a brief description. How the data is configured, displayed, discussed, and emphasized all directly affect whether
the audience viewing the data see it as it was intended to be seen. If you tell a bad story with your data, then the data has become 
essentially worthless. Certain data visualization principles such as the Gestalt principles, minimizing chart junk, and deploying strategic
color frameworks like *viridis* and *inferno*, all help to maximize how an audience may see the data being displayed to them or how the 
data's story is told, and thus they are incredibly useful tools. I also learned in this course about the layer approach when it comes to 
making data visualizations with programs like RStudio. This approach innately comes with far more versatility than using more common 
programs like Excel to create data visualization as element can be simply added or removed individually depending on what the user desires.
This adds a lot of convenience when it comes to the creation of data visualizations, especially more complex ones that I learned about 
officially in this course like choropleths which I have often seen, but never knew how they were made. I also learned about different chart 
types and that they all have pros and cons depending on the dataset being analyzed. Pie charts for example should almost universally be avoided
as they faced the systemic issue of people have innate difficulties with perceiving and judging angles. GitHub was also something that I was
unaware of before this course. GitHub provides a massive amount of utility when it comes to actively tracking commits or anything having to do
with RStudio and though we didn't use it that much, whenever I work with RStudio in the future, I will be sure to utilize it for saving and 
uploading my projects and data. As for what I plan on using this knowledge for in the future, in one of my quizzes, I discussed how I want to
look into soft on crime policies and the respective effects of those policies throughout America and its cities. So, when I have some time, 
I would like to look for, request, and analyze data that may provide insight into that phenomenon in the United States specifically.
