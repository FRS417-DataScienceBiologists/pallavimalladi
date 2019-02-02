---
title: "Tidy data"
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
## Load the library

```r
library(tidyverse)
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

## The following data are results from a drug trial that shows the effect of four different treatments on six patients. The values represent resting heart rate.

```r
non_tidy1 <- data.frame(
  patient= c("Margaret", "Frank", "Hawkeye", "Trapper", "Radar", "Henry"),
  a= c(72, 84, 64, 60, 74, 88),
  b= c(74, 84, 66, 58, 72, 87),
  c= c(80, 88, 68, 64, 78, 88),
  d= c(68, 76, 64, 58, 70, 72)
)
## a,b,c,d are variables- not the column names! The column names here represents variables
## drugA, drugB, drugC, drugB are the actual column names
non_tidy1
```

```
##    patient  a  b  c  d
## 1 Margaret 72 74 80 68
## 2    Frank 84 84 88 76
## 3  Hawkeye 64 66 68 64
## 4  Trapper 60 58 64 58
## 5    Radar 74 72 78 70
## 6    Henry 88 87 88 72
```


```r
plot(non_tidy1)
```

![](Lab_3_2_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
## R does not know what to plot since the data is not tidy
## we need to make the data tidy
```

## make the code easier to plot by assigning column and value names. name this code as tidy1

```r
tidy1 <- non_tidy1 %>% 
  gather (a, b, c, d, key = "drug", value ="heartrate") ##assemble these into key-value pairs; key is the new column name (called 'drug').enter value into drug ('heartrate')
tidy1
```

```
##     patient drug heartrate
## 1  Margaret    a        72
## 2     Frank    a        84
## 3   Hawkeye    a        64
## 4   Trapper    a        60
## 5     Radar    a        74
## 6     Henry    a        88
## 7  Margaret    b        74
## 8     Frank    b        84
## 9   Hawkeye    b        66
## 10  Trapper    b        58
## 11    Radar    b        72
## 12    Henry    b        87
## 13 Margaret    c        80
## 14    Frank    c        88
## 15  Hawkeye    c        68
## 16  Trapper    c        64
## 17    Radar    c        78
## 18    Henry    c        88
## 19 Margaret    d        68
## 20    Frank    d        76
## 21  Hawkeye    d        64
## 22  Trapper    d        58
## 23    Radar    d        70
## 24    Henry    d        72
```

```r
## this is one kind of untidy data
```

## Final check:try the plot command again. Need to specify x and y axes

```r
plot(tidy1$patient, tidy1$heartrate) ##just used for proof of concept to show that R understands that it is working with a vector
```

![](Lab_3_2_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

## Practice
The data below track tuberculosis infection rates by year and country.

```r
country <- c("Afghanistan", "Brazil", "China")
`1999` <- c(745, 37737, 212258)
`2000` <- c(2666, 80488, 213766)
tb_data <- data.frame(country=country, `1999`=`1999`, `2000`=`2000`)
tb_data
```

```
##       country  X1999  X2000
## 1 Afghanistan    745   2666
## 2      Brazil  37737  80488
## 3       China 212258 213766
```

## 1. Are these data tidy? Why not? Identify the specific problem(s).
  ##  The data is not tidy since there are 2 rows for 1 column. We can consolidate 1999 and 2000 into the column "year".
  ##  Even though column names and row names are both strings of data (which match), the data cannot be plotted in this format

## 2. Use gather() to tidy the data.

```r
tidy <- tb_data %>% 
  gather ('X1999','X2000', key= "year", value = "rate") ##assemble these into key-value pairs; key is the new column name (called 'year'). Assign value to "rate"
tidy
```

```
##       country  year   rate
## 1 Afghanistan X1999    745
## 2      Brazil X1999  37737
## 3       China X1999 212258
## 4 Afghanistan X2000   2666
## 5      Brazil X2000  80488
## 6       China X2000 213766
```

## separate()
In our next example, we have  the sex of each patient included with their name. Are these data tidy? No, there is more than one value per cell in the patient column and the columns a, b, c, d once again represent values.

```r
non_tidy2 <- data.frame(
  patient= c("Margaret_f", "Frank_m", "Hawkeye_m", "Trapper_m", "Radar_m", "Henry_m"),
  a= c(72, 84, 64, 60, 74, 88),
  b= c(74, 84, 66, 58, 72, 87),
  c= c(80, 88, 68, 64, 78, 88),
  d= c(68, 76, 64, 58, 70, 72)
)
non_tidy2
```

```
##      patient  a  b  c  d
## 1 Margaret_f 72 74 80 68
## 2    Frank_m 84 84 88 76
## 3  Hawkeye_m 64 66 68 64
## 4  Trapper_m 60 58 64 58
## 5    Radar_m 74 72 78 70
## 6    Henry_m 88 87 88 72
```

## Separate name and sex in order to make the data tidy

```r
non_tidy2 %>% 
  separate (patient, into= c("patient_nm", "sex"), sep="_") ##make sure to specify column name so it is not like the original data frame
```

```
##   patient_nm sex  a  b  c  d
## 1   Margaret   f 72 74 80 68
## 2      Frank   m 84 84 88 76
## 3    Hawkeye   m 64 66 68 64
## 4    Trapper   m 60 58 64 58
## 5      Radar   m 74 72 78 70
## 6      Henry   m 88 87 88 72
```

```r
  ## look at patient column and separate patient name and sex by an underscore (_)
```

## data is still not tidy because of a, b, c, d. Gather the data
This is great; we have separated sex from patient. Are the data tidy? Not yet. We still need to use `gather()`.

```r
tidy2<- non_tidy2 %>% 
  separate(patient, into= c("patient_nm", "sex"), sep = "_") %>% 
  gather(a, b, c, d, key="drug", value="heartrate")
tidy2
```

```
##    patient_nm sex drug heartrate
## 1    Margaret   f    a        72
## 2       Frank   m    a        84
## 3     Hawkeye   m    a        64
## 4     Trapper   m    a        60
## 5       Radar   m    a        74
## 6       Henry   m    a        88
## 7    Margaret   f    b        74
## 8       Frank   m    b        84
## 9     Hawkeye   m    b        66
## 10    Trapper   m    b        58
## 11      Radar   m    b        72
## 12      Henry   m    b        87
## 13   Margaret   f    c        80
## 14      Frank   m    c        88
## 15    Hawkeye   m    c        68
## 16    Trapper   m    c        64
## 17      Radar   m    c        78
## 18      Henry   m    c        88
## 19   Margaret   f    d        68
## 20      Frank   m    d        76
## 21    Hawkeye   m    d        64
## 22    Trapper   m    d        58
## 23      Radar   m    d        70
## 24      Henry   m    d        72
```

## use rename () function: replace column names

```r
tidy3<- tidy2 %>% 
  dplyr::rename(
    MASH_character = patient_nm,
    Sex= sex,
    Drug= drug,
    Heartrate_bpm=heartrate)
tidy3 ## rename this new name data frame as tidy3
```

```
##    MASH_character Sex Drug Heartrate_bpm
## 1        Margaret   f    a            72
## 2           Frank   m    a            84
## 3         Hawkeye   m    a            64
## 4         Trapper   m    a            60
## 5           Radar   m    a            74
## 6           Henry   m    a            88
## 7        Margaret   f    b            74
## 8           Frank   m    b            84
## 9         Hawkeye   m    b            66
## 10        Trapper   m    b            58
## 11          Radar   m    b            72
## 12          Henry   m    b            87
## 13       Margaret   f    c            80
## 14          Frank   m    c            88
## 15        Hawkeye   m    c            68
## 16        Trapper   m    c            64
## 17          Radar   m    c            78
## 18          Henry   m    c            88
## 19       Margaret   f    d            68
## 20          Frank   m    d            76
## 21        Hawkeye   m    d            64
## 22        Trapper   m    d            58
## 23          Radar   m    d            70
## 24          Henry   m    d            72
```

## Practice
In this example study, ten participants were asked to categorize three face styles by clicking various buttons that represent three different categories (face 1, face 2, face 3). The time it took to click a button is in milliseconds.

```r
faces <- data.frame(
  ParticipantID_sex = c("001_m", "002_f", "003_f", "004_f", "005_m", "006_f", "007_m", "008_m", "009_m", "010_f"),
  Face_1 = c(411,723,325,456,579,612,709,513,527,379),
  Face_2 = c(123,300,400,500,600,654,789,906,413,567),
  Face_3 = c(1457,1000,569,896,956,2345,780,599,1023,678)
)
faces
```

```
##    ParticipantID_sex Face_1 Face_2 Face_3
## 1              001_m    411    123   1457
## 2              002_f    723    300   1000
## 3              003_f    325    400    569
## 4              004_f    456    500    896
## 5              005_m    579    600    956
## 6              006_f    612    654   2345
## 7              007_m    709    789    780
## 8              008_m    513    906    599
## 9              009_m    527    413   1023
## 10             010_f    379    567    678
```

## 1. Are these data tidy? Why or why not?  
  ## The data is not tidy since there are 3 columns for the data set

## 2. Tidy the data and place them into a new dataframe.

```r
## start by splitting ParticipantID_sex into 2 parts: ID and Sex
face <- 
  faces %>% 
  separate(ParticipantID_sex, into= c("patient_id", "sex"), sep = "_") %>% ## separates by the _
  gather(Face_1, Face_2, Face_3, key="face_style", value="time")
face
```

```
##    patient_id sex face_style time
## 1         001   m     Face_1  411
## 2         002   f     Face_1  723
## 3         003   f     Face_1  325
## 4         004   f     Face_1  456
## 5         005   m     Face_1  579
## 6         006   f     Face_1  612
## 7         007   m     Face_1  709
## 8         008   m     Face_1  513
## 9         009   m     Face_1  527
## 10        010   f     Face_1  379
## 11        001   m     Face_2  123
## 12        002   f     Face_2  300
## 13        003   f     Face_2  400
## 14        004   f     Face_2  500
## 15        005   m     Face_2  600
## 16        006   f     Face_2  654
## 17        007   m     Face_2  789
## 18        008   m     Face_2  906
## 19        009   m     Face_2  413
## 20        010   f     Face_2  567
## 21        001   m     Face_3 1457
## 22        002   f     Face_3 1000
## 23        003   f     Face_3  569
## 24        004   f     Face_3  896
## 25        005   m     Face_3  956
## 26        006   f     Face_3 2345
## 27        007   m     Face_3  780
## 28        008   m     Face_3  599
## 29        009   m     Face_3 1023
## 30        010   f     Face_3  678
```

3. Use `rename()` to rename a few columns for practice.

```r
 face1<-face %>% 
  dplyr::rename(
    Patient_Number = patient_id,
    Sex= sex,
    FaceType=face_style,
    Time_millisecond= time
    )
face1
```

```
##    Patient_Number Sex FaceType Time_millisecond
## 1             001   m   Face_1              411
## 2             002   f   Face_1              723
## 3             003   f   Face_1              325
## 4             004   f   Face_1              456
## 5             005   m   Face_1              579
## 6             006   f   Face_1              612
## 7             007   m   Face_1              709
## 8             008   m   Face_1              513
## 9             009   m   Face_1              527
## 10            010   f   Face_1              379
## 11            001   m   Face_2              123
## 12            002   f   Face_2              300
## 13            003   f   Face_2              400
## 14            004   f   Face_2              500
## 15            005   m   Face_2              600
## 16            006   f   Face_2              654
## 17            007   m   Face_2              789
## 18            008   m   Face_2              906
## 19            009   m   Face_2              413
## 20            010   f   Face_2              567
## 21            001   m   Face_3             1457
## 22            002   f   Face_3             1000
## 23            003   f   Face_3              569
## 24            004   f   Face_3              896
## 25            005   m   Face_3              956
## 26            006   f   Face_3             2345
## 27            007   m   Face_3              780
## 28            008   m   Face_3              599
## 29            009   m   Face_3             1023
## 30            010   f   Face_3              678
```

```r
## renaming the data table columns
```

