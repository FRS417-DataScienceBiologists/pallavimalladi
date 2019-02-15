---
title: "Dealing with NA's"
author: "Pallavi Malladi"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc: yes
    toc_float: yes
  pdf_document:
    toc: yes
---

*At the end of this exercise, you will be able to:*    
1. Produce summaries of the number of NA's in a data set.  
2. Replace values with `NA` in a data set as appropriate. 
3. Be able to deal with NA values

## Load the tidyverse

```r
library("tidyverse")
```

```
## ── Attaching packages ─────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
## ✔ tibble  2.0.0     ✔ dplyr   0.7.8
## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
## ✔ readr   1.3.1     ✔ forcats 0.3.0
```

```
## Warning: package 'tibble' was built under R version 3.5.2
```

```
## ── Conflicts ────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

## Dealing with NA's
In almost all scientific data, there are missing observations. These can be tricky to deal with, partly because you first need to determine how missing values were treated in the original study. Scientists use different conventions in showing missing data; some use blank spaces, others use "-", etc. Worse yet, some scientists indicate **missing data with numerics like -999.0!**  

## Practice
1. What are some possible problems if missing data are indicated by "-999.0"?

## Load the `msleep` data into a new object

```r
msleep <- msleep
```

## Are there any NA's?
Let's first check to see if our data has any NA's. is.na() is a function that determines whether a value in a data frame is or is not an NA. This is evaluated logically as TRUE or FALSE.

```r
is.na(msleep) ##TRUE means there are NA in the data set
```

```
##        name genus  vore order conservation sleep_total sleep_rem
##  [1,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
##  [2,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
##  [3,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [4,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [5,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [6,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
##  [7,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [8,] FALSE FALSE  TRUE FALSE         TRUE       FALSE      TRUE
##  [9,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [10,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [11,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [12,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [13,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [14,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [15,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [16,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [17,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [18,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [19,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [20,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [21,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [22,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [23,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [24,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [25,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [26,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [27,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [28,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [29,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [30,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [31,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [32,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [33,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [34,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [35,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [36,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [37,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [38,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [39,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [40,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [41,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [42,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [43,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [44,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [45,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [46,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [47,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [48,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [49,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [50,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [51,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [52,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [53,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [54,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [55,] FALSE FALSE  TRUE FALSE        FALSE       FALSE     FALSE
## [56,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [57,] FALSE FALSE  TRUE FALSE         TRUE       FALSE      TRUE
## [58,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE
## [59,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [60,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [61,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [62,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [63,] FALSE FALSE  TRUE FALSE        FALSE       FALSE     FALSE
## [64,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [65,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [66,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [67,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [68,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [69,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE
## [70,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [71,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [72,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [73,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE
## [74,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [75,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [76,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [77,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [78,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [79,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [80,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [81,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [82,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [83,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
##       sleep_cycle awake brainwt bodywt
##  [1,]        TRUE FALSE    TRUE  FALSE
##  [2,]        TRUE FALSE   FALSE  FALSE
##  [3,]        TRUE FALSE    TRUE  FALSE
##  [4,]       FALSE FALSE   FALSE  FALSE
##  [5,]       FALSE FALSE   FALSE  FALSE
##  [6,]       FALSE FALSE    TRUE  FALSE
##  [7,]       FALSE FALSE    TRUE  FALSE
##  [8,]        TRUE FALSE    TRUE  FALSE
##  [9,]       FALSE FALSE   FALSE  FALSE
## [10,]        TRUE FALSE   FALSE  FALSE
## [11,]        TRUE FALSE   FALSE  FALSE
## [12,]       FALSE FALSE   FALSE  FALSE
## [13,]        TRUE FALSE    TRUE  FALSE
## [14,]       FALSE FALSE   FALSE  FALSE
## [15,]        TRUE FALSE   FALSE  FALSE
## [16,]        TRUE FALSE   FALSE  FALSE
## [17,]       FALSE FALSE   FALSE  FALSE
## [18,]       FALSE FALSE   FALSE  FALSE
## [19,]        TRUE FALSE   FALSE  FALSE
## [20,]       FALSE FALSE   FALSE  FALSE
## [21,]        TRUE FALSE   FALSE  FALSE
## [22,]       FALSE FALSE   FALSE  FALSE
## [23,]       FALSE FALSE   FALSE  FALSE
## [24,]        TRUE FALSE   FALSE  FALSE
## [25,]       FALSE FALSE   FALSE  FALSE
## [26,]        TRUE FALSE   FALSE  FALSE
## [27,]        TRUE FALSE    TRUE  FALSE
## [28,]       FALSE FALSE   FALSE  FALSE
## [29,]       FALSE FALSE   FALSE  FALSE
## [30,]        TRUE FALSE    TRUE  FALSE
## [31,]        TRUE FALSE    TRUE  FALSE
## [32,]        TRUE FALSE   FALSE  FALSE
## [33,]        TRUE FALSE   FALSE  FALSE
## [34,]       FALSE FALSE   FALSE  FALSE
## [35,]        TRUE FALSE    TRUE  FALSE
## [36,]        TRUE FALSE   FALSE  FALSE
## [37,]        TRUE FALSE    TRUE  FALSE
## [38,]       FALSE FALSE   FALSE  FALSE
## [39,]        TRUE FALSE    TRUE  FALSE
## [40,]       FALSE FALSE   FALSE  FALSE
## [41,]        TRUE FALSE    TRUE  FALSE
## [42,]       FALSE FALSE   FALSE  FALSE
## [43,]       FALSE FALSE   FALSE  FALSE
## [44,]        TRUE FALSE    TRUE  FALSE
## [45,]        TRUE FALSE   FALSE  FALSE
## [46,]        TRUE FALSE    TRUE  FALSE
## [47,]        TRUE FALSE    TRUE  FALSE
## [48,]       FALSE FALSE   FALSE  FALSE
## [49,]        TRUE FALSE   FALSE  FALSE
## [50,]       FALSE FALSE   FALSE  FALSE
## [51,]        TRUE FALSE    TRUE  FALSE
## [52,]        TRUE FALSE   FALSE  FALSE
## [53,]        TRUE FALSE    TRUE  FALSE
## [54,]       FALSE FALSE   FALSE  FALSE
## [55,]        TRUE FALSE   FALSE  FALSE
## [56,]        TRUE FALSE    TRUE  FALSE
## [57,]        TRUE FALSE    TRUE  FALSE
## [58,]        TRUE FALSE   FALSE  FALSE
## [59,]        TRUE FALSE    TRUE  FALSE
## [60,]        TRUE FALSE    TRUE  FALSE
## [61,]        TRUE FALSE    TRUE  FALSE
## [62,]        TRUE FALSE   FALSE  FALSE
## [63,]        TRUE FALSE   FALSE  FALSE
## [64,]       FALSE FALSE   FALSE  FALSE
## [65,]        TRUE FALSE    TRUE  FALSE
## [66,]        TRUE FALSE   FALSE  FALSE
## [67,]       FALSE FALSE   FALSE  FALSE
## [68,]       FALSE FALSE   FALSE  FALSE
## [69,]        TRUE FALSE   FALSE  FALSE
## [70,]        TRUE FALSE   FALSE  FALSE
## [71,]       FALSE FALSE   FALSE  FALSE
## [72,]        TRUE FALSE    TRUE  FALSE
## [73,]       FALSE FALSE   FALSE  FALSE
## [74,]       FALSE FALSE   FALSE  FALSE
## [75,]        TRUE FALSE   FALSE  FALSE
## [76,]        TRUE FALSE    TRUE  FALSE
## [77,]       FALSE FALSE   FALSE  FALSE
## [78,]        TRUE FALSE   FALSE  FALSE
## [79,]       FALSE FALSE   FALSE  FALSE
## [80,]        TRUE FALSE    TRUE  FALSE
## [81,]        TRUE FALSE   FALSE  FALSE
## [82,]        TRUE FALSE   FALSE  FALSE
## [83,]       FALSE FALSE   FALSE  FALSE
```

OK, what are we supposed to do with that? Unless you have a small data frame, applying the is.na function universally is not helpful but we can use the code in another way. Let's incorporate it into the `summarize()` function.

```r
msleep %>% 
  summarize(number_nas= sum(is.na(msleep))) ##shows the number of NA's within the entire data frame; does not tell us where the NA's are actually located!
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1        136
```

This is better, but we still don't have any idea of where those NA's are in our data frame. If there were a systemic problem in the data it would be hard to determine. In order to do this, we need to apply `is.na` to each variable of interest.

```r
msleep %>% 
  summarize(number_nas= sum(is.na(conservation)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1         29
```

What if we are working with hundreds or thousands (or more!) variables?! In order to deal with this problem efficiently we can use another package in the tidyverse called `purrr`.

```r
msleep_na <- 
  msleep %>%
  purrr::map_df(~ sum(is.na(.))) #map to a new data frame the sum results of the is.na function for all columns
  ##purrr takes a command and repeats it across the data
msleep_na
```

```
## # A tibble: 1 x 11
##    name genus  vore order conservation sleep_total sleep_rem sleep_cycle
##   <int> <int> <int> <int>        <int>       <int>     <int>       <int>
## 1     0     0     7     0           29           0        22          51
## # … with 3 more variables: awake <int>, brainwt <int>, bodywt <int>
```

```r
##quickly tells us everywhere where there is an NA in the various data frame columns
```

Don't forget that we can use `gather()` to make viewing this output easier.

```r
msleep %>% 
  purrr::map_df(~ sum(is.na(.))) %>% 
  tidyr::gather(key="variables", value="num_nas") %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 11 x 2
##    variables    num_nas
##    <chr>          <int>
##  1 sleep_cycle       51
##  2 conservation      29
##  3 brainwt           27
##  4 sleep_rem         22
##  5 vore               7
##  6 name               0
##  7 genus              0
##  8 order              0
##  9 sleep_total        0
## 10 awake              0
## 11 bodywt             0
```

This is much better, but we need to be careful. R can have difficulty interpreting missing data. This is especially true for categorical variables. Always do a reality check if the output doesn't make sense to you. A quick check never hurts.  

You can explore a specific variable more intently using `count()`. This operates similarly to `group_by()`.

```r
msleep %>% 
  count(conservation) ##tells the number of NA's within a given column (the conservation column in this example)
```

```
## # A tibble: 7 x 2
##   conservation     n
##   <chr>        <int>
## 1 cd               2
## 2 domesticated    10
## 3 en               4
## 4 lc              27
## 5 nt               4
## 6 vu               7
## 7 <NA>            29
```

Adding the `sort=TRUE` option automatically makes a descending list.

```r
msleep %>% 
  count(conservation, sort=TRUE) 
```

```
## # A tibble: 7 x 2
##   conservation     n
##   <chr>        <int>
## 1 <NA>            29
## 2 lc              27
## 3 domesticated    10
## 4 vu               7
## 5 en               4
## 6 nt               4
## 7 cd               2
```

```r
## 29 in the first row since there are 29 total NA's within conservation
```

It is true that all of this is redundant, but you want to be able to run multiple checks on the data. Remember, just because your code runs without errors doesn't mean it is doing what you intended.  

## Replacing NA's
Once you have an idea of how NA's are represented in the data, you can replace them with `NA` so that R can better deal with them. The bit of code below is very handy, especially if the data has NA's represented as **actual values that you want replaced** or if you want to make sure any blanks are treated as NA.

```r
msleep_na2 <- 
  msleep %>% 
  na_if("") #replace blank data with NA; replace something with NA
##similar to replacing -999 (some people use this but it can interfere with data calulations) with NA
msleep_na2
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
##  1 Chee… Acin… carni Carn… lc                  12.1      NA        NA    
##  2 Owl … Aotus omni  Prim… <NA>                17         1.8      NA    
##  3 Moun… Aplo… herbi Rode… nt                  14.4       2.4      NA    
##  4 Grea… Blar… omni  Sori… lc                  14.9       2.3       0.133
##  5 Cow   Bos   herbi Arti… domesticated         4         0.7       0.667
##  6 Thre… Brad… herbi Pilo… <NA>                14.4       2.2       0.767
##  7 Nort… Call… carni Carn… vu                   8.7       1.4       0.383
##  8 Vesp… Calo… <NA>  Rode… <NA>                 7        NA        NA    
##  9 Dog   Canis carni Carn… domesticated        10.1       2.9       0.333
## 10 Roe … Capr… herbi Arti… lc                   3        NA        NA    
## # … with 73 more rows, and 3 more variables: awake <dbl>, brainwt <dbl>,
## #   bodywt <dbl>
```

## Practice
1. Did replacing blanks with NA have any effect on the msleep data? Demonstrate this using some code.
## No, NA should not have any effect on the data

## Practice and Homework
We will work together on the next part (time permitting) and this will end up being your homework. Make sure that you save your work and copy and paste your responses into the RMarkdown homework file.

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
msleep %>% 
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
