---
title: "Lab 4 Homework"
author: "Pallavi Malladi"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---
## Load the tidyverse

```r
library(tidyverse)
```

Aren't mammals fun? Let's load up some more mammal data. This will be life history data for mammals. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*  


```r
life_history <- readr::read_csv("~/Desktop/FRS_417/class_files-master/mammal_lifehistories_v2.csv")
```

```
## Parsed with column specification:
## cols(
##   order = col_character(),
##   family = col_character(),
##   Genus = col_character(),
##   species = col_character(),
##   mass = col_double(),
##   gestation = col_double(),
##   newborn = col_double(),
##   weaning = col_double(),
##   `wean mass` = col_double(),
##   AFR = col_double(),
##   `max. life` = col_double(),
##   `litter size` = col_double(),
##   `litters/year` = col_double()
## )
```


```r
names(life_history)
```

```
##  [1] "order"        "family"       "Genus"        "species"     
##  [5] "mass"         "gestation"    "newborn"      "weaning"     
##  [9] "wean mass"    "AFR"          "max. life"    "litter size" 
## [13] "litters/year"
```

Rename some of the variables. Notice that I am replacing the old `life_history` data.

```r
life_history <- 
  life_history %>% 
  dplyr::rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year`
          )
life_history
```

```
## # A tibble: 1,440 x 13
##    order family genus species   mass gestation newborn weaning wean_mass
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>
##  1 Arti… Antil… Anti… americ… 4.54e4      8.13   3246.    3         8900
##  2 Arti… Bovid… Addax nasoma… 1.82e5      9.39   5480     6.5       -999
##  3 Arti… Bovid… Aepy… melamp… 4.15e4      6.35   5093     5.63     15900
##  4 Arti… Bovid… Alce… busela… 1.50e5      7.9   10167.    6.5       -999
##  5 Arti… Bovid… Ammo… clarkei 2.85e4      6.8    -999  -999         -999
##  6 Arti… Bovid… Ammo… lervia  5.55e4      5.08   3810     4         -999
##  7 Arti… Bovid… Anti… marsup… 3.00e4      5.72   3910     4.04      -999
##  8 Arti… Bovid… Anti… cervic… 3.75e4      5.5    3846     2.13      -999
##  9 Arti… Bovid… Bison bison   4.98e5      8.93  20000    10.7     157500
## 10 Arti… Bovid… Bison bonasus 5.00e5      9.14  23000.    6.6       -999
## # … with 1,430 more rows, and 4 more variables: AFR <dbl>, max_life <dbl>,
## #   litter_size <dbl>, litters_yr <dbl>
```

## 1. Explore the data using the method that you prefer. Below, I am using a new package called `skimr`. If you want to use this, make sure that it is installed.

```r
##install.packages("skimr")
##installs a new package (not the tidyverse!)
```


```r
library("skimr")
```

```
## Warning: package 'skimr' was built under R version 3.5.2
```


```r
life_history %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## ── Variable type:character ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## ── Variable type:numeric ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
##     variable missing complete    n      mean         sd   p0  p25     p50
##          AFR       0     1440 1440   -408.12     504.97 -999 -999    2.5 
##    gestation       0     1440 1440   -287.25     455.36 -999 -999    1.05
##  litter_size       0     1440 1440    -55.63     234.88 -999    1    2.27
##   litters_yr       0     1440 1440   -477.14     500.03 -999 -999    0.38
##         mass       0     1440 1440 383576.72 5055162.92 -999   50  403.02
##     max_life       0     1440 1440   -490.26     615.3  -999 -999 -999   
##      newborn       0     1440 1440   6703.15   90912.52 -999 -999    2.65
##    wean_mass       0     1440 1440  16048.93   5e+05    -999 -999 -999   
##      weaning       0     1440 1440   -427.17     496.71 -999 -999    0.73
##      p75          p100     hist
##    15.61     210       ▆▁▁▁▁▁▇▁
##     4.5       21.46    ▃▁▁▁▁▁▁▇
##     3.83      14.18    ▁▁▁▁▁▁▁▇
##     1.15       7.5     ▇▁▁▁▁▁▁▇
##  7009.17       1.5e+08 ▇▁▁▁▁▁▁▁
##   147.25    1368       ▇▁▁▃▂▁▁▁
##    98    2250000       ▇▁▁▁▁▁▁▁
##    10          1.9e+07 ▇▁▁▁▁▁▁▁
##     2         48       ▆▁▁▁▁▁▁▇
```

```r
##skimr is like another summary function
```

## 2. Run the code below. Are there any NA's in the data? Does this seem likely?

```r
msleep %>% ## we used msleep instead of life_history because life_history is found within msleep
  summarize(number_nas= sum(is.na(life_history)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```

```r
## Technically, there are no NA's, but it is possible that there is another value such as -999 that represents a null value
```

## 3. Are there any missing data (NA's) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?

```r
life_history<-
  life_history %>% 
  na_if(-999)
## In class, we learned how -999 is sometimes used to represent values where there is no data entry, so I checked for any -999 values that may be in place for NA. If there was an -999, I replacd it with an NA so that it would be easier to identify where the null value is located within the data frame. 
```


```r
number_nas= sum(is.na(life_history))
life_history %>%
  purrr::map_df(~ sum(is.na(.))) %>%  ##shows how many NA values and where they are within the data frame
  tidyr::gather(variables, number_nas) %>% ##I had to get help from GitHub to understand the last 2 lines here
  arrange(desc(number_nas))
```

```
## # A tibble: 13 x 2
##    variables   number_nas
##    <chr>            <int>
##  1 wean_mass         1039
##  2 max_life           841
##  3 litters_yr         689
##  4 weaning            619
##  5 AFR                607
##  6 newborn            595
##  7 gestation          418
##  8 mass                85
##  9 litter_size         84
## 10 order                0
## 11 family               0
## 12 genus                0
## 13 species              0
```

```r
##There might be many missing wean and newborn values missing since not all are measured exactly at birth
```

## 4. Compared to the `msleep` data, we have better representation among taxa. Produce a summary that shows the number of observations by taxonomic order.

```r
names(life_history) ##I'm using this to get a general idea of the column headers so I know which notation to use within my code
```

```
##  [1] "order"       "family"      "genus"       "species"     "mass"       
##  [6] "gestation"   "newborn"     "weaning"     "wean_mass"   "AFR"        
## [11] "max_life"    "litter_size" "litters_yr"
```


```r
life_history %>% 
  group_by(order) %>% ##groups by taxonomic order
  summarize(n())   ## n represents the number of observations; each order has a certain number of observations
```

```
## # A tibble: 17 x 2
##    order          `n()`
##    <chr>          <int>
##  1 Artiodactyla     161
##  2 Carnivora        197
##  3 Cetacea           55
##  4 Dermoptera         2
##  5 Hyracoidea         4
##  6 Insectivora       91
##  7 Lagomorpha        42
##  8 Macroscelidea     10
##  9 Perissodactyla    15
## 10 Pholidota          7
## 11 Primates         156
## 12 Proboscidea        2
## 13 Rodentia         665
## 14 Scandentia         7
## 15 Sirenia            5
## 16 Tubulidentata      1
## 17 Xenarthra         20
```

## 5. Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.

```r
life_history %>%
  mutate(lifespan=max_life/12) %>% ##This changes lifespan values from months into years  
  group_by(order) %>%
  summarize(min_lifespan=min(lifespan, na.rm=TRUE), ##return lifespan values for which the values are not NA
            max_lifespan=max(lifespan, na.rm=TRUE),
            mean_lifespan=mean(lifespan, na.rm=TRUE),
            sd_lifespan=sd(lifespan, na.rm=TRUE),
            total_lifespan=n()
  )
```

```
## # A tibble: 17 x 6
##    order min_lifespan max_lifespan mean_lifespan sd_lifespan total_lifespan
##    <chr>        <dbl>        <dbl>         <dbl>       <dbl>          <int>
##  1 Arti…         6.75        61            20.7        7.70             161
##  2 Carn…         5           56            21.1        9.42             197
##  3 Ceta…        13          114            48.8       27.7               55
##  4 Derm…        17.5         17.5          17.5       NA                  2
##  5 Hyra…        11           12.2          11.6        0.884              4
##  6 Inse…         1.17        13             3.85       2.90              91
##  7 Lago…         6           18             9.02       3.85              42
##  8 Macr…         3            8.75          5.69       2.39              10
##  9 Peri…        21           50            35.5       10.2               15
## 10 Phol…        20           20            20         NA                  7
## 11 Prim…         8.83        60.5          25.7       11.0              156
## 12 Prob…        70           80            75          7.07               2
## 13 Rode…         1           35             6.99       5.30             665
## 14 Scan…         2.67        12.4           8.86       5.38               7
## 15 Sire…        12.5         73            43.2       30.3                5
## 16 Tubu…        24           24            24         NA                  1
## 17 Xena…         9           40            21.3        9.93              20
```

## 6. Let's look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?

```r
life_history %>% 
  group_by(order) %>% 
  summarize(mean_gestation=mean(gestation, na.rm=TRUE), ##summarizes mean for specified values
            mean_newborn_mass=mean(newborn, na.rm=TRUE),
            mean_wean_mass=mean(wean_mass, na.rm=TRUE)) %>% 
  arrange(desc(mean_gestation)) 
```

```
## # A tibble: 17 x 4
##    order          mean_gestation mean_newborn_mass mean_wean_mass
##    <chr>                   <dbl>             <dbl>          <dbl>
##  1 Proboscidea             21.3           99523.         600000  
##  2 Perissodactyla          13.0           27015.         382191. 
##  3 Cetacea                 11.8          343077.        4796125  
##  4 Sirenia                 10.8           22878.          67500  
##  5 Hyracoidea               7.4             231.            500  
##  6 Artiodactyla             7.26           7082.          51025. 
##  7 Tubulidentata            7.08           1734            6250  
##  8 Primates                 5.47            287.           2115. 
##  9 Xenarthra                4.95            314.            420  
## 10 Carnivora                3.69           3657.          21020. 
## 11 Pholidota                3.63            276.           2006. 
## 12 Dermoptera               2.75             35.9           NaN  
## 13 Macroscelidea            1.91             24.5           104. 
## 14 Scandentia               1.63             12.8           102. 
## 15 Rodentia                 1.31             35.5           135. 
## 16 Lagomorpha               1.18             57.0           715. 
## 17 Insectivora              1.15              6.06           33.1
```
## Proboscidea has longest mean gestation; "The Proboscidea (from the Greek προβοσκίς and the Latin proboscis) are a taxonomic order of afrotherian mammals containing one living family (Elephantidae) and several extinct families"
