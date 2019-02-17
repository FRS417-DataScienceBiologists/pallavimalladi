---
title: "Lab 5 Homework"
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

## Mammals Life History
Let's revisit the mammal life history data to practice our `ggplot` skills. Some of the tidy steps will be a repeat from the homework, but it is good practice. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*

## 1. Load the data.

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


## 2. Use your preferred function to have a look. Do you notice any problems?

```r
life_history
```

```
## # A tibble: 1,440 x 13
##    order family Genus species   mass gestation newborn weaning `wean mass`
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>       <dbl>
##  1 Arti… Antil… Anti… americ… 4.54e4      8.13   3246.    3           8900
##  2 Arti… Bovid… Addax nasoma… 1.82e5      9.39   5480     6.5         -999
##  3 Arti… Bovid… Aepy… melamp… 4.15e4      6.35   5093     5.63       15900
##  4 Arti… Bovid… Alce… busela… 1.50e5      7.9   10167.    6.5         -999
##  5 Arti… Bovid… Ammo… clarkei 2.85e4      6.8    -999  -999           -999
##  6 Arti… Bovid… Ammo… lervia  5.55e4      5.08   3810     4           -999
##  7 Arti… Bovid… Anti… marsup… 3.00e4      5.72   3910     4.04        -999
##  8 Arti… Bovid… Anti… cervic… 3.75e4      5.5    3846     2.13        -999
##  9 Arti… Bovid… Bison bison   4.98e5      8.93  20000    10.7       157500
## 10 Arti… Bovid… Bison bonasus 5.00e5      9.14  23000.    6.6         -999
## # … with 1,430 more rows, and 4 more variables: AFR <dbl>, `max.
## #   life` <dbl>, `litter size` <dbl>, `litters/year` <dbl>
```

```r
## Looking at the data, we see that the data in the frame is currently tidy. The problem in life_history is that there were a relative large number of null values (represented by -999). We replaced -999 with NA values.
```

## 3. There are NA's. How are you going to deal with them?
## The NA's are represented by -999

```r
life_history<-
  life_history %>% 
  na_if(-999)
## In class, we learned how -999 is sometimes used to represent values where there is no data entry, so I checked for any -999 values that may be in place for NA. If there was an -999, I replacd it with an NA so that it would be easier to identify where the null value is located within the data frame. 
```

## 4. Where are the NA's? This is important to keep in mind as you build plots.

```r
number_nas= sum(is.na(life_history))
life_history %>%
  purrr::map_df(~ sum(is.na(.))) %>%  ##shows how many NA values and where they are within the data frame
  tidyr::gather(variables, number_nas) %>% ##I had to get help from GitHub to understand the last 2 lines here
  arrange(desc(number_nas))
```

```
## # A tibble: 13 x 2
##    variables    number_nas
##    <chr>             <int>
##  1 wean mass          1039
##  2 max. life           841
##  3 litters/year        689
##  4 weaning             619
##  5 AFR                 607
##  6 newborn             595
##  7 gestation           418
##  8 mass                 85
##  9 litter size          84
## 10 order                 0
## 11 family                0
## 12 Genus                 0
## 13 species               0
```

## 5. Some of the variable names will be problematic. Let's rename them here as a final tidy step.

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
##  2 Arti… Bovid… Addax nasoma… 1.82e5      9.39   5480     6.5         NA
##  3 Arti… Bovid… Aepy… melamp… 4.15e4      6.35   5093     5.63     15900
##  4 Arti… Bovid… Alce… busela… 1.50e5      7.9   10167.    6.5         NA
##  5 Arti… Bovid… Ammo… clarkei 2.85e4      6.8      NA    NA           NA
##  6 Arti… Bovid… Ammo… lervia  5.55e4      5.08   3810     4           NA
##  7 Arti… Bovid… Anti… marsup… 3.00e4      5.72   3910     4.04        NA
##  8 Arti… Bovid… Anti… cervic… 3.75e4      5.5    3846     2.13        NA
##  9 Arti… Bovid… Bison bison   4.98e5      8.93  20000    10.7     157500
## 10 Arti… Bovid… Bison bonasus 5.00e5      9.14  23000.    6.6         NA
## # … with 1,430 more rows, and 4 more variables: AFR <dbl>, max_life <dbl>,
## #   litter_size <dbl>, litters_yr <dbl>
```


```r
options(scipen=999) #cancels the use of scientific notation for the session
```

## 6. What is the relationship between newborn body mass and gestation? Make a scatterplot that shows this relationship. 

```r
names(life_history) ##shows column names for data frame so we know what to use for axes headers
```

```
##  [1] "order"       "family"      "genus"       "species"     "mass"       
##  [6] "gestation"   "newborn"     "weaning"     "wean_mass"   "AFR"        
## [11] "max_life"    "litter_size" "litters_yr"
```


```r
ggplot(data=life_history, mapping=aes(x=gestation, y=log10(newborn))) + ##wean mass=newborn body mass
  geom_jitter() +
   geom_smooth(method=lm, se=FALSE) ## method = lm means that we are adding in a smooth lm (linear model)
```

```
## Warning: Removed 673 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](Lab_5_Homework_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
##I took the log10 of wean mass because the y-axis had a range that was too large for the graph. With log, the graph has a more understandable range
##Answer: The relationship between newborn body mass and gestation is a positive correlation. The longer the gestation period, the larger the mammal would be at birth.
```


## 7. You should notice that because of the outliers in newborn mass, we need to make some changes. We didn't talk about this in lab, but you can use `scale_x_log10()` as a layer to correct for this issue. This will log transform the y-axis values.

```r
life_history %>% 
  ggplot(aes(x=newborn, y=gestation))+
  scale_x_log10()+ ## removes outliers from newborn mass data
  geom_jitter()+
   geom_smooth(method=lm, se=FALSE)
```

```
## Warning: Removed 673 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](Lab_5_Homework_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
## This is the same function as the code before, but this coded uses scale_x_log10() instead of log10().
```


## Do not do #8,9,10 for Homework 5!
## 8. Now that you have the basic plot, color the points by taxonomic order.

## 9. Lastly, make the size of the points proportional to body mass.

## 10. Make a plot that shows the range of lifespan by order.
