Climate Exercise
================
Joslyn Fu & Kelly Yuan

**This is a climate change excerise by Joslyn and Jiaming to dive into
how the main indicators of climate change, including C02 trends, global
temperature change.**

-----

# CO2 Trends

First, we exmaine the CO2 trends. This is from the [NASA Climate Change
Section - Carbon
Dioxide](http://climate.nasa.gov/vital-signs/carbon-dioxide/), and the
original data is obtained from the National Oceanic and Atmospheric
Administration, specifically the Earth System Research Laboratories.

The Data is shown below:

``` r
library(tidyverse)
```

``` r
co2 <- 
read_table("https://raw.githubusercontent.com/espm-157/climate-template/master/assignment/co2_mm_mlo.txt", 
                  comment="#",
                  col_names = c("year", "month", "decimal_date", "average",
                                "interpolated", "trend", "days"),
                  na = c("-1", "-99.99"))
co2
```

    ## # A tibble: 749 x 7
    ##     year month decimal_date average interpolated trend  days
    ##    <dbl> <dbl>        <dbl>   <dbl>        <dbl> <dbl> <dbl>
    ##  1  1958     3        1958.    316.         316.  315.    NA
    ##  2  1958     4        1958.    317.         317.  315.    NA
    ##  3  1958     5        1958.    318.         318.  315.    NA
    ##  4  1958     6        1958.     NA          317.  315.    NA
    ##  5  1958     7        1959.    316.         316.  315.    NA
    ##  6  1958     8        1959.    315.         315.  316.    NA
    ##  7  1958     9        1959.    313.         313.  316.    NA
    ##  8  1958    10        1959.     NA          313.  316.    NA
    ##  9  1958    11        1959.    313.         313.  315.    NA
    ## 10  1958    12        1959.    315.         315.  316.    NA
    ## # … with 739 more rows

We then visualize the data.

``` r
ggplot(co2, aes(x = decimal_date, y = average)) + geom_line() 
```

![](climate_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## Trend from the Graph

According to the dataset, C02 level usually reaches its annual maximum
around May and June, and its minimum around September and Octobor. Based
on our research, much of this variation happens because of the role of
plants in the carbon cycle. Plants extract CO2 from the atmosphere,
along with sunlight and water, to make food and other substances that
they need to grow —— this is the process of **photosysthesis**.
Therefore, CO2 in the atmosphere decreases during the growing season and
increases during the rest of the year, which leads to maximum buildup in
May before photosynthesis begins to take over in the summer time.
Photosynthesis, in which plants take up CO2 from the atmosphere and
release oxygen, dominates during the growing season (the warmer part of
the year). Respiration, by which plants and animals take up oxygen and
release CO2, occurs all the time but dominates during the colder months
of the year.

What rolling average is used in computing the “trend” line? How does the
trend depend on the rolling average?

-----

# Global Temperatre

After examining CO2 trend, we look into the global temperature data from
[NASA Global Climate Change Section - Global
Temperature](http://climate.nasa.gov/vital-signs/global-temperature).

## Dataset Description

The temperature dataset contains information about gobal annual mean
temperature from 1880 to 2019 relative to 1951-1980 average
temperatures. This graph illustrates the change in global surface
temperature relative to 1951-1980 average temperatures.

Please see the below dataset table.

``` r
temperature <- read_table(
  "http://climate.nasa.gov/system/internal_resources/details/original/647_Global_Temperature_Data_File.txt",
  skip = 5,
  col_names = c("year","temp","lowess5"))
```

    ## Parsed with column specification:
    ## cols(
    ##   year = col_double(),
    ##   temp = col_double(),
    ##   lowess5 = col_double()
    ## )

``` r
temperature
```

    ## # A tibble: 140 x 3
    ##     year  temp lowess5
    ##    <dbl> <dbl>   <dbl>
    ##  1  1880 -0.16   -0.08
    ##  2  1881 -0.07   -0.12
    ##  3  1882 -0.1    -0.16
    ##  4  1883 -0.17   -0.19
    ##  5  1884 -0.28   -0.23
    ##  6  1885 -0.32   -0.25
    ##  7  1886 -0.3    -0.26
    ##  8  1887 -0.35   -0.26
    ##  9  1888 -0.16   -0.26
    ## 10  1889 -0.09   -0.25
    ## # … with 130 more rows

This is the trend in global mean temperatures over time.

``` r
temperature %>%
  ggplot(aes(x = year, y = temp)) + geom_line()
```

![](climate_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

According to the graph, the overall trend in global temperature is
increasing. Starting from 2012, the increase in global temperature is
surging at a higher rate.

## Question 4: Evaluating the evidence for a “Pause” in warming?

The [2013 IPCC
Report](https://www.ipcc.ch/pdf/assessment-report/ar5/wg1/WG1AR5_SummaryVolume_FINAL.pdf)
included a tentative observation of a “much smaller increasing trend” in
global mean temperatures since 1998 than was observed previously. This
led to much discussion in the media about the existence of a “Pause” or
“Hiatus” in global warming rates, as well as much research looking
into where the extra heat could have gone. (Examples discussing this
question include articles in [The
Guardian](http://www.theguardian.com/environment/2015/jun/04/global-warming-hasnt-paused-study-finds),
[BBC News](http://www.bbc.com/news/science-environment-28870988), and
[Wikipedia](https://en.wikipedia.org/wiki/Global_warming_hiatus)).

By examining the data here, what evidence do you find or not find for
such a pause? Present an analysis of this data (using the tools &
methods we have covered in Foundation course so far) to argue your case.

What additional analyses or data sources would better help you refine
your arguments?

## Question 5: Rolling averages

  - What is the meaning of “5 year average” vs “annual average”?
  - Construct 5 year averages from the annual data. Construct 10 &
    20-year averages.
  - Plot the different averages and describe what differences you see
    and why.

-----

# Melting Ice Sheets

The ice loss has been extremely massive over the past years. In order to
investigate this problem, we look into both **Antarctica** (green line)
and **Greenland** (blue line) mass data from 2002 [NASA Global Climate
Change Section - Ice
Sheets](https://climate.nasa.gov/vital-signs/ice-sheets/).

## Dataset Description

``` r
ice <- read_csv("http://climate.nasa.gov/system/internal_resources/details/original/499_GRN_ANT_mass_changes.csv",skip = 10,
                col_names = c("time","greenland_mass","antarctica_mass"))
```

    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   greenland_mass = col_double(),
    ##   antarctica_mass = col_double()
    ## )

``` r
ice
```

    ## # A tibble: 140 x 3
    ##     time greenland_mass antarctica_mass
    ##    <dbl>          <dbl>           <dbl>
    ##  1 2002.          1491.            967.
    ##  2 2002.          1486.            979.
    ##  3 2003.          1287.            512.
    ##  4 2003.          1258.            859.
    ##  5 2003.          1257.            694.
    ##  6 2003.          1288.            592.
    ##  7 2003.          1337.            658.
    ##  8 2003.          1354.            477.
    ##  9 2003.          1363.            546.
    ## 10 2003.          1427.            494.
    ## # … with 130 more rows

There are three columns in the table. i) ‘Time’ in units of ‘year-day’
ii) ‘greenland\_mass’ in units of ‘Gt’ iii) ‘antarctica\_mass’ in units
of ‘Gt’

The numbers are from Ice mass measurement by NASA’s GRACE satellites.
The uncertainty depends on the accuracy of the satellites. The missing
values indicate a gap between missions during that time (from June 2017
to June 2018).

## Graph

``` r
ice %>%
  ggplot(aes(x = time)) + 
  geom_line(aes(y = greenland_mass), col="blue") +
  geom_line(aes(y = antarctica_mass), col = "green")
```

![](climate_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

The trend is that both ice mass are decreasing. In each year, there is a
wave-like shape, suggesting that there is a decrease in summer and an
increase in winter. This is due to the natural temperature variation in
different reasons. However, greenland mass is decreasing at a faster
rate than antarctica mass. While greenland mass was initially heavier
than antarctica mass, greenland mass is now less than the antarctica
mass.

# Rising Sea Level

Rising sea level is another vital indicator of climate change. According
to NASA, sea level rise is caused primarily by two factors: the added
water from melting ice sheets and glaciers and the expansion of seawater
as it warms. We analyze rising sea level from obtain from
[Na](https://climate.nasa.gov/vital-signs/sea-level/)

  - Data description: <http://climate.nasa.gov/vital-signs/sea-level/>
  - Raw data file:
    <http://climate.nasa.gov/system/internal_resources/details/original/121_Global_Sea_Level_Data_File.txt>

## Question 1:

  - Describe the data set: what are the columns and units?
  - Where do these data come from?
  - What is the uncertainty in measurment? Resolution of the data?
    Interpretation of missing values?

## Question 2:

Construct the necessary R code to import this data set as a tidy `Table`
object.

``` r
sea_level <- read_table("http://climate.nasa.gov/system/internal_resources/details/original/121_Global_Sea_Level_Data_File.txt",
                        skip = 50,
                        col_names = c("altimeter_type","merged_file_cycle","number_of_observations","number_of_weighted_observations",
                                      "gmsl_variation_adjustment_not_applied","standard_deviation_of_gmsl_variation_no_adjustment",
                                      "gmsl_variation_adjustment_applied","standard_deviation_of_gmsl_adjustment",
                                      "smoothed_gmsl_variation","smoothed_gmsl_variation_removed_signal"))
```

    ## Parsed with column specification:
    ## cols(
    ##   altimeter_type = col_double(),
    ##   merged_file_cycle = col_double(),
    ##   number_of_observations = col_double(),
    ##   number_of_weighted_observations = col_double(),
    ##   gmsl_variation_adjustment_not_applied = col_double(),
    ##   standard_deviation_of_gmsl_variation_no_adjustment = col_double(),
    ##   gmsl_variation_adjustment_applied = col_double(),
    ##   standard_deviation_of_gmsl_adjustment = col_double(),
    ##   smoothed_gmsl_variation = col_double(),
    ##   smoothed_gmsl_variation_removed_signal = col_double()
    ## )

``` r
sea_level
```

    ## # A tibble: 844 x 10
    ##    altimeter_type merged_file_cyc… number_of_obser… number_of_weigh…
    ##             <dbl>            <dbl>            <dbl>            <dbl>
    ##  1              0               14            1993.           419112
    ##  2              0               15            1993.           456793
    ##  3              0               16            1993.           414055
    ##  4              0               17            1993.           465235
    ##  5              0               18            1993.           463257
    ##  6              0               19            1993.           458542
    ##  7            999               20            1993.           464921
    ##  8              0               21            1993.           459017
    ##  9              0               22            1993.           467041
    ## 10              0               23            1993.           464740
    ## # … with 834 more rows, and 6 more variables:
    ## #   gmsl_variation_adjustment_not_applied <dbl>,
    ## #   standard_deviation_of_gmsl_variation_no_adjustment <dbl>,
    ## #   gmsl_variation_adjustment_applied <dbl>,
    ## #   standard_deviation_of_gmsl_adjustment <dbl>, smoothed_gmsl_variation <dbl>,
    ## #   smoothed_gmsl_variation_removed_signal <dbl>

## Question 3:

Plot the data and describe the trends you observe.

``` r
sea_level %>%
  ggplot(aes(x = number_of_observations)) +
  geom_line(aes(y = smoothed_gmsl_variation),col= "blue") +
  geom_line(aes(y = smoothed_gmsl_variation_removed_signal),col= "red")
```

![](climate_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

# Exercise IV: Arctic Sea Ice?

  - <http://nsidc.org/data/G02135>
  - <ftp://sidads.colorado.edu/DATASETS/NOAA/G02135/north/daily/data/N_seaice_extent_daily_v3.0.csv>

## Question 1:

  - Describe the data set: what are the columns and units?
  - Where do these data come from?
  - What is the uncertainty in measurement? Resolution of the data?
    Interpretation of missing values?

## Question 2:

Construct the necessary R code to import this data set as a tidy `Table`
object.

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
sea_ice <- read_csv("https://github.com/espm-157/climate-template/releases/download/data/N_seaice_extent_daily_v3.0.csv",
                    skip = 2, col_names = c("year","month","day","extent","missing"))
```

    ## Parsed with column specification:
    ## cols(
    ##   year = col_double(),
    ##   month = col_character(),
    ##   day = col_character(),
    ##   extent = col_double(),
    ##   missing = col_double()
    ## )

    ## Warning: 13648 parsing failures.
    ## row col  expected    actual                                                                                                 file
    ##   1  -- 5 columns 6 columns 'https://github.com/espm-157/climate-template/releases/download/data/N_seaice_extent_daily_v3.0.csv'
    ##   2  -- 5 columns 6 columns 'https://github.com/espm-157/climate-template/releases/download/data/N_seaice_extent_daily_v3.0.csv'
    ##   3  -- 5 columns 6 columns 'https://github.com/espm-157/climate-template/releases/download/data/N_seaice_extent_daily_v3.0.csv'
    ##   4  -- 5 columns 6 columns 'https://github.com/espm-157/climate-template/releases/download/data/N_seaice_extent_daily_v3.0.csv'
    ##   5  -- 5 columns 6 columns 'https://github.com/espm-157/climate-template/releases/download/data/N_seaice_extent_daily_v3.0.csv'
    ## ... ... ......... ......... ....................................................................................................
    ## See problems(...) for more details.

``` r
sea_ice %>%
  mutate(date = make_date(year, month, day))
```

    ## # A tibble: 13,648 x 6
    ##     year month day   extent missing date      
    ##    <dbl> <chr> <chr>  <dbl>   <dbl> <date>    
    ##  1  1978 10    26      10.2       0 1978-10-26
    ##  2  1978 10    28      10.4       0 1978-10-28
    ##  3  1978 10    30      10.6       0 1978-10-30
    ##  4  1978 11    01      10.7       0 1978-11-01
    ##  5  1978 11    03      10.8       0 1978-11-03
    ##  6  1978 11    05      11.0       0 1978-11-05
    ##  7  1978 11    07      11.1       0 1978-11-07
    ##  8  1978 11    09      11.2       0 1978-11-09
    ##  9  1978 11    11      11.3       0 1978-11-11
    ## 10  1978 11    13      11.5       0 1978-11-13
    ## # … with 13,638 more rows

## Question 3:

Plot the data and describe the trends you observe.

``` r
sea_ice %>%
  ggplot(aes(x = year)) + 
  geom_point(aes(y = extent))
```

![](climate_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

# Exercise V: Longer term trends in CO2 Records

The data we analyzed in the unit introduction included CO2 records
dating back only as far as the measurements at the Manua Loa
observatory. To put these values into geological perspective requires
looking back much farther than humans have been monitoring atmosopheric
CO2 levels. To do this, we need another approach.

[Ice core data](http://cdiac.ornl.gov/trends/co2/ice_core_co2.html):

Vostok Core, back to 400,000 yrs before present day

  - Description of data set:
    <http://cdiac.esd.ornl.gov/trends/co2/vostok.html>
  - Data source:
    <http://cdiac.ornl.gov/ftp/trends/co2/vostok.icecore.co2>

## Questions / Tasks:

  - Describe the data set: what are the columns and units? Where do the
    numbers come from?
  - What is the uncertainty in measurment? Resolution of the data?
    Interpretation of missing values?
  - Read in and prepare data for analysis.
  - Reverse the ordering to create a chronological record.  
  - Plot data
  - Consider various smoothing windowed averages of the data.
  - Join this series to Mauna Loa data
  - Plot joined data
  - Describe your
conclusions

<!-- end list -->

``` r
table <- read_table("https://cdiac.ess-dive.lbl.gov/ftp/trends/co2/vostok.icecore.co2")
```

    ## Parsed with column specification:
    ## cols(
    ##   `*******************************************************************************` = col_character()
    ## )

``` r
table
```

    ## # A tibble: 382 x 1
    ##    `***************************************************************************…
    ##    <chr>                                                                        
    ##  1 *** Historical CO2 Record from the Vostok Ice Core                          …
    ##  2 ***                                                                         …
    ##  3 *** Source: J.M. Barnola                                                    …
    ##  4 ***         D. Raynaud                                                      …
    ##  5 ***         C. Lorius                                                       …
    ##  6 ***         Laboratoire de Glaciologie et de Geophysique de l'Environnement …
    ##  7 ***         38402 Saint Martin d'Heres Cedex, France                        …
    ##  8 ***                                                                         …
    ##  9 ***         N. I. Barkov                                                    …
    ## 10 ***         Arctic and Antarctic Research Institute                         …
    ## # … with 372 more rows
