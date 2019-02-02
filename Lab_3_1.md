---
title: "dplyr::select(), mutate(), and pipes"
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
## Load the tidyverse library

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

## Load the fish scale data by getting the working directory

```r
getwd
```

```
## function () 
## .Internal(getwd())
## <bytecode: 0x7f8f4dbdd0a8>
## <environment: namespace:base>
```

## copy and paste working directory 

```r
fish <- readr::read_csv("~/Desktop/FRS_417/class_files-master/Gaeta_etal_CLC_data.csv")
```

```
## Parsed with column specification:
## cols(
##   lakeid = col_character(),
##   fish_id = col_double(),
##   annnumber = col_character(),
##   length = col_double(),
##   radii_length_mm = col_double(),
##   scalelength = col_double()
## )
```

```r
## load .csv file with data on fish scales
## Open the fule using read_csv()
```

##make sure we are in the correct working directory by creating a temporary frame

```r
temp_fish<-
  readr::read_csv("~/Desktop/FRS_417/class_files-master/Gaeta_etal_CLC_data.csv")
```

```
## Parsed with column specification:
## cols(
##   lakeid = col_character(),
##   fish_id = col_double(),
##   annnumber = col_character(),
##   length = col_double(),
##   radii_length_mm = col_double(),
##   scalelength = col_double()
## )
```

## get information from data frame

```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


## filter()

```r
## will cr
fish %>%
  filter(lakeid=="AL",
         length==167)
```

```
## # A tibble: 3 x 6
##   lakeid fish_id annnumber length radii_length_mm scalelength
##   <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
## 1 AL         299 EDGE         167            2.70        2.70
## 2 AL         299 2            167            2.04        2.70
## 3 AL         299 1            167            1.31        2.70
```

## Dplyr: select()
Select allows you to build a new data frame by selecting your columns (variables) of interest. Our fish data only has six columns, but this should give you some ideas especially when you have large data frames with lots of columns.  

## We are only interested in lakeid and scalelength.

```r
fish%>%
  select(lakeid, scalelength)
```

```
## # A tibble: 4,033 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 4,023 more rows
```

## For very large data frames with lots of variables, `select()` uses many different operators to make things easier. Let's say we are only interested in the variables that deal with length.

```r
fish %>%
  select(contains("length"))
```

```
## # A tibble: 4,033 x 3
##    length radii_length_mm scalelength
##     <dbl>           <dbl>       <dbl>
##  1    167            2.70        2.70
##  2    167            2.04        2.70
##  3    167            1.31        2.70
##  4    175            3.02        3.02
##  5    175            2.67        3.02
##  6    175            2.14        3.02
##  7    175            1.23        3.02
##  8    194            3.34        3.34
##  9    194            2.97        3.34
## 10    194            2.29        3.34
## # … with 4,023 more rows
```


```r
fish %>%
  select(ends_with("gth"))
```

```
## # A tibble: 4,033 x 2
##    length scalelength
##     <dbl>       <dbl>
##  1    167        2.70
##  2    167        2.70
##  3    167        2.70
##  4    175        3.02
##  5    175        3.02
##  6    175        3.02
##  7    175        3.02
##  8    194        3.34
##  9    194        3.34
## 10    194        3.34
## # … with 4,023 more rows
```

```r
## gth is the last 3 letters of length
## based on character string
```

## The `-` operator is useful in select. It allows us to select everything except the specified variables.

```r
select(fish, -fish_id, -annnumber, -length, -radii_length_mm)
```

```
## # A tibble: 4,033 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 4,023 more rows
```

## Practice
## 1. Build a dataframe focused on the lakes `AL` and `AR` and looking at radii length between 2 and 4 only. Use pipes!
## filter fish to include AL and AR lakeid with specifiec radii length

```r
fish %>% 
  filter (lakeid=="AL" | lakeid=="AR") %>% 
  filter(radii_length_mm>=2) %>% 
  filter(radii_length_mm <=4)
```

```
## # A tibble: 253 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         300 EDGE         175            3.02        3.02
##  4 AL         300 3            175            2.67        3.02
##  5 AL         300 2            175            2.14        3.02
##  6 AL         301 EDGE         194            3.34        3.34
##  7 AL         301 3            194            2.97        3.34
##  8 AL         301 2            194            2.29        3.34
##  9 AL         302 4            324            3.72        6.07
## 10 AL         302 3            324            3.23        6.07
## # … with 243 more rows
```

##arrange length in increasing order

```r
fish %>% 
  arrange (lakeid)
```

```
## # A tibble: 4,033 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 4,023 more rows
```

## shows count: how many different types of lakes there are

```r
fish %>% 
  count (lakeid)
```

```
## # A tibble: 16 x 2
##    lakeid     n
##    <chr>  <int>
##  1 AL       383
##  2 AR       262
##  3 BO       197
##  4 BR       291
##  5 CR       343
##  6 DY       355
##  7 FD       302
##  8 JN       238
##  9 LC       173
## 10 LJ       181
## 11 LR       292
## 12 LSG      143
## 13 MN       293
## 14 RD       135
## 15 UB       191
## 16 WS       254
```

```r
## will tell how many lakes there are associated with each lakeid
```

## Practice
Some additional options to select columns based on a specific criteria include:  
1. ends_with() = Select columns that end with a character string  
2. contains() = Select columns that contain a character string  
3. matches() = Select columns that match a regular expression  
4. one_of() = Select columns names that are from a group of names  

## 1. What are the names of the columns in the `fish` data?

```r
colnames(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

```r
## produces the name of the column titles
```

## 2. We are only interested in the variables `lakeid`, `length`, and `scalelength`. Use `select()` to build a new dataframe focused on these variables.

```r
fish %>%
  select(lakeid, length, scalelength)
```

```
## # A tibble: 4,033 x 3
##    lakeid length scalelength
##    <chr>   <dbl>       <dbl>
##  1 AL        167        2.70
##  2 AL        167        2.70
##  3 AL        167        2.70
##  4 AL        175        3.02
##  5 AL        175        3.02
##  6 AL        175        3.02
##  7 AL        175        3.02
##  8 AL        194        3.34
##  9 AL        194        3.34
## 10 AL        194        3.34
## # … with 4,023 more rows
```

## Dplyr: Can we combine filter() and select()?
Absolutely. This is one of the strengths of the tidyverse, it uses the same grammar to specify commands.


```r
fish2 <- select(fish, lakeid, scalelength)
## creates a new data frame with selected columns from original fish data
```


```r
filter(fish2, lakeid=="AL")
```

```
## # A tibble: 383 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 373 more rows
```

```r
## filters fish2 data frame to include fish from AL only. Still prints the scalength as well
```

## Dplyr: Pipes %>% 
##The code above works fine but there is a more efficient way. We need to learn pipes `%>%`. Pipes allow you to feed the output from one function to the input of another function. We are going to use pipes from here on to keep things cleaner. (command+shift+m)

```r
fish%>%
  select(lakeid, scalelength)%>%
  filter(lakeid=="AL")
```

```
## # A tibble: 383 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 373 more rows
```

```r
## filter
```

## Dplyr: arrange()
The `arrange()` command allows us to sort values in a column.

```r
fish %>% 
  arrange(desc(scalelength))
```

```
## # A tibble: 4,033 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 WS         180 EDGE         403           11.0         11.0
##  2 WS         180 16           403           10.6         11.0
##  3 WS         180 15           403           10.3         11.0
##  4 WS         180 14           403            9.93        11.0
##  5 WS         180 13           403            9.56        11.0
##  6 WS         180 12           403            9.17        11.0
##  7 WS         180 11           403            8.62        11.0
##  8 WS         180 10           403            8.15        11.0
##  9 WS         180 9            403            7.49        11.0
## 10 WS         180 8            403            6.97        11.0
## # … with 4,023 more rows
```

It can be very helpful in combination with the other functions.

```r
fish %>% 
  select(lakeid, length, fish_id, scalelength) %>% 
  filter(lakeid=="AL") %>% 
  arrange(fish_id)
```

```
## # A tibble: 383 x 4
##    lakeid length fish_id scalelength
##    <chr>   <dbl>   <dbl>       <dbl>
##  1 AL        167     299        2.70
##  2 AL        167     299        2.70
##  3 AL        167     299        2.70
##  4 AL        175     300        3.02
##  5 AL        175     300        3.02
##  6 AL        175     300        3.02
##  7 AL        175     300        3.02
##  8 AL        194     301        3.34
##  9 AL        194     301        3.34
## 10 AL        194     301        3.34
## # … with 373 more rows
```

## 2. Build a dataframe focused on the scalelengths of `fish_id` 300 and 301. Use `arrange()` to sort from smallest to largest scalelength. Use pipes!

```r
fish %>% 
  filter(fish_id=="300" | fish_id=="301") %>% 
  arrange (scalelength)
```

```
## # A tibble: 8 x 6
##   lakeid fish_id annnumber length radii_length_mm scalelength
##   <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
## 1 AL         300 EDGE         175            3.02        3.02
## 2 AL         300 3            175            2.67        3.02
## 3 AL         300 2            175            2.14        3.02
## 4 AL         300 1            175            1.23        3.02
## 5 AL         301 EDGE         194            3.34        3.34
## 6 AL         301 3            194            2.97        3.34
## 7 AL         301 2            194            2.29        3.34
## 8 AL         301 1            194            1.55        3.34
```


## Dplyr: mutate(): mutate() allows us to create new columns in a data frame
## When you use mutate() the original data used are preserved

```r
fish %>% 
  select(lakeid, fish_id, scalelength, length) %>% ## filter this into..
  filter(lakeid=="AL") %>% ## filter this further into...
  arrange (fish_id) %>% ## via increasing order
  mutate(scale_ratio=(length/scalelength)) ## new column name will be scale_ratio
```

```
## # A tibble: 383 x 5
##    lakeid fish_id scalelength length scale_ratio
##    <chr>    <dbl>       <dbl>  <dbl>       <dbl>
##  1 AL         299        2.70    167        61.9
##  2 AL         299        2.70    167        61.9
##  3 AL         299        2.70    167        61.9
##  4 AL         300        3.02    175        58.0
##  5 AL         300        3.02    175        58.0
##  6 AL         300        3.02    175        58.0
##  7 AL         300        3.02    175        58.0
##  8 AL         301        3.34    194        58.0
##  9 AL         301        3.34    194        58.0
## 10 AL         301        3.34    194        58.0
## # … with 373 more rows
```

```r
  ## values in scale_ration specified by equation length/scalelength
```

## Practice
## 1. Add a new column to the fish data that is radii_length_mm divided by scalelength. Add another column that scales this number to a percentage.

```r
fish %>% 
  mutate(radii_ratio=(radii_length_mm/scalelength)) %>% ## creates new equation
  mutate(percent_ratio= radii_ratio * 100 ) ##converts answer to %
```

```
## # A tibble: 4,033 x 8
##    lakeid fish_id annnumber length radii_length_mm scalelength radii_ratio
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70       1    
##  2 AL         299 2            167            2.04        2.70       0.755
##  3 AL         299 1            167            1.31        2.70       0.486
##  4 AL         300 EDGE         175            3.02        3.02       1    
##  5 AL         300 3            175            2.67        3.02       0.886
##  6 AL         300 2            175            2.14        3.02       0.709
##  7 AL         300 1            175            1.23        3.02       0.408
##  8 AL         301 EDGE         194            3.34        3.34       1    
##  9 AL         301 3            194            2.97        3.34       0.888
## 10 AL         301 2            194            2.29        3.34       0.686
## # … with 4,023 more rows, and 1 more variable: percent_ratio <dbl>
```

