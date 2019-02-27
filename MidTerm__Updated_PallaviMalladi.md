---
title: "Midterm Exam"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc_float: no
  pdf_document:
    toc: yes
---

## Instructions
This exam is designed to show me what you have learned and where there are problems. You may use your notes and anything from the `class_files` folder, but please no internet searches. You have 35 minutes to complete as many of these exercises as possible on your own, and 10 minutes to work with a partner.  

At the end of the exam, upload the complete .Rmd file to your GitHub repository.  

1. Load the tidyverse.

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.2.1 ──
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
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```


2. For these questions, we will use data about California colleges. Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges<-readr::read_csv("~/Desktop/FRS_417/class_files-master/data/ca_college_data.csv")
```

```
## Parsed with column specification:
## cols(
##   INSTNM = col_character(),
##   CITY = col_character(),
##   STABBR = col_character(),
##   ZIP = col_character(),
##   ADM_RATE = col_double(),
##   SAT_AVG = col_double(),
##   PCIP26 = col_double(),
##   COSTT4_A = col_double(),
##   C150_4_POOLED = col_double(),
##   PFTFTUG1_EF = col_double()
## )
```


3. Use your preferred function to have a look at the data and get an idea of its structure.

```r
glimpse(colleges) ##Shares number of observations, number of variavles, and column names
```

```
## Observations: 341
## Variables: 10
## $ INSTNM        <chr> "Grossmont College", "College of the Sequoias", "C…
## $ CITY          <chr> "El Cajon", "Visalia", "San Mateo", "Ventura", "Ox…
## $ STABBR        <chr> "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "C…
## $ ZIP           <chr> "92020-1799", "93277-2214", "94402-3784", "93003-3…
## $ ADM_RATE      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ SAT_AVG       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ PCIP26        <dbl> 0.0016, 0.0066, 0.0038, 0.0035, 0.0085, 0.0151, 0.…
## $ COSTT4_A      <dbl> 7956, 8109, 8278, 8407, 8516, 8577, 8580, 9181, 92…
## $ C150_4_POOLED <dbl> NA, NA, NA, NA, NA, NA, 0.2334, NA, NA, NA, NA, 0.…
## $ PFTFTUG1_EF   <dbl> 0.3546, 0.5413, 0.3567, 0.3824, 0.2753, 0.4286, 0.…
```


4. What are the column names?

```r
names(colleges)
```

```
##  [1] "INSTNM"        "CITY"          "STABBR"        "ZIP"          
##  [5] "ADM_RATE"      "SAT_AVG"       "PCIP26"        "COSTT4_A"     
##  [9] "C150_4_POOLED" "PFTFTUG1_EF"
```

```r
## Shows 10 column names specified within "college" data frame
```


5. Are there any NA's in the data? If so, how many are present and in which variables?

```r
colleges %>% 
  summarize(number_nas= sum(is.na(colleges)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1        949
```

```r
## There are 949 NA values within the data
```


```r
number_nas= sum(is.na(colleges))
colleges %>%
  purrr::map_df(~ sum(is.na(.))) %>%  ## where NAs are located within the data frame (also shows how many are in each variable)
  tidyr::gather(Variables, number_nas) %>% ## displays variable name and number of NA's within that column (there should be 10 rows for 10 vaiables)
  arrange(desc(number_nas))
```

```
## # A tibble: 10 x 2
##    Variables     number_nas
##    <chr>              <int>
##  1 SAT_AVG              276
##  2 ADM_RATE             240
##  3 C150_4_POOLED        221
##  4 COSTT4_A             124
##  5 PFTFTUG1_EF           53
##  6 PCIP26                35
##  7 INSTNM                 0
##  8 CITY                   0
##  9 STABBR                 0
## 10 ZIP                    0
```


6. Which cities in California have the highest number of colleges?

```r
colleges %>% 
  count(CITY) %>% ## counts the number of colleges within each CITY
  arrange(desc(n))
```

```
## # A tibble: 161 x 2
##    CITY              n
##    <chr>         <int>
##  1 Los Angeles      24
##  2 San Diego        18
##  3 San Francisco    15
##  4 Sacramento       10
##  5 Berkeley          9
##  6 Oakland           9
##  7 Claremont         7
##  8 Pasadena          6
##  9 Fresno            5
## 10 Irvine            5
## # … with 151 more rows
```


7. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest cost?

```r
colleges %>% 
  arrange(desc(COSTT4_A))
```

```
## # A tibble: 341 x 10
##    INSTNM CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A C150_4_POOLED
##    <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 Harve… Clar… CA     91711   0.129     1496 0.0674    69355         0.925
##  2 South… Los … CA     9001…  NA           NA 0         67225         0.533
##  3 Unive… Los … CA     90089   0.166     1395 0.0461    67064         0.922
##  4 Occid… Los … CA     9004…   0.458     1315 0.137     67046         0.842
##  5 Clare… Clar… CA     9171…   0.0944    1413 0.0681    66325         0.924
##  6 Peppe… Mali… CA     90263   0.369     1251 0.0276    66152         0.854
##  7 Scrip… Clar… CA     9171…   0.299     1353 0.152     66060         0.871
##  8 Pitze… Clar… CA     9171…   0.137       NA 0.0888    65880         0.888
##  9 San F… San … CA     9413…   0.926       NA 0         65453         0.388
## 10 Pomon… Clar… CA     9171…   0.0944    1442 0.171     64870         0.957
## # … with 331 more rows, and 1 more variable: PFTFTUG1_EF <dbl>
```

```r
## Harvey Mudd College has the highest annual cost
```

8. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What does this mean?

```r
colleges %>% 
  ggplot(aes(x=ADM_RATE, y=C150_4_POOLED))+
  geom_point() +
  geom_smooth(method=lm)
```

```
## Warning: Removed 251 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 251 rows containing missing values (geom_point).
```

![](MidTerm__Updated_PallaviMalladi_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
## There is a negative relationship between admissions rate and 4 year completion rate. This means that the higher the admissions rate, the lower the 4-year completion rate
```


9. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Run the code below and look at the output. Are all of the columns tidy? Why or why not?

```r
univ_calif<-colleges %>% 
  filter_all(any_vars(str_detect(.,pattern= "University of California")))
univ_calif
```

```
## # A tibble: 10 x 10
##    INSTNM CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A C150_4_POOLED
##    <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 Unive… La J… CA     92093    0.357    1324  0.216    31043         0.872
##  2 Unive… Irvi… CA     92697    0.406    1206  0.107    31198         0.876
##  3 Unive… Rive… CA     92521    0.663    1078  0.149    31494         0.73 
##  4 Unive… Los … CA     9009…    0.180    1334  0.155    33078         0.911
##  5 Unive… Davis CA     9561…    0.423    1218  0.198    33904         0.850
##  6 Unive… Sant… CA     9506…    0.578    1201  0.193    34608         0.776
##  7 Unive… Berk… CA     94720    0.169    1422  0.105    34924         0.916
##  8 Unive… Sant… CA     93106    0.358    1281  0.108    34998         0.816
##  9 Unive… San … CA     9410…   NA          NA NA           NA        NA    
## 10 Unive… San … CA     9414…   NA          NA NA           NA        NA    
## # … with 1 more variable: PFTFTUG1_EF <dbl>
```

```r
## The data is  tidy since each college has 9 corresponding columns with it. Each row represents each individual college and its provided data.
```


10. Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
 colleges<- colleges %>% 
  separate(INSTNM, into= c("UNIV", "CAMPUS"), sep = " ")
```

```
## Warning: Expected 2 pieces. Additional pieces discarded in 264 rows [2,
## 3, 8, 10, 16, 17, 18, 20, 21, 22, 23, 24, 26, 29, 30, 31, 33, 36, 37,
## 40, ...].
```

```
## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 3 rows [243,
## 244, 245].
```

```r
colleges
```

```
## # A tibble: 341 x 11
##    UNIV  CAMPUS CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A
##    <chr> <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>
##  1 Gros… Colle… El C… CA     9202…       NA      NA 0.0016     7956
##  2 Coll… of     Visa… CA     9327…       NA      NA 0.0066     8109
##  3 Coll… of     San … CA     9440…       NA      NA 0.0038     8278
##  4 Vent… Colle… Vent… CA     9300…       NA      NA 0.0035     8407
##  5 Oxna… Colle… Oxna… CA     9303…       NA      NA 0.0085     8516
##  6 Moor… Colle… Moor… CA     9302…       NA      NA 0.0151     8577
##  7 Skyl… Colle… San … CA     9406…       NA      NA 0          8580
##  8 Glen… Commu… Glen… CA     9120…       NA      NA 0.002      9181
##  9 Citr… Colle… Glen… CA     9174…       NA      NA 0.0021     9281
## 10 Fres… City   Fres… CA     93741       NA      NA 0.0324     9370
## # … with 331 more rows, and 2 more variables: C150_4_POOLED <dbl>,
## #   PFTFTUG1_EF <dbl>
```


11. As a final step, remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```r
univ_calif_final<-colleges %>% 
  filter(CAMPUS !="University of California-Hastings College of Law" ,
         CAMPUS !="University of California-San Francisco")
univ_calif_final
```

```
## # A tibble: 338 x 11
##    UNIV  CAMPUS CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A
##    <chr> <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>
##  1 Gros… Colle… El C… CA     9202…       NA      NA 0.0016     7956
##  2 Coll… of     Visa… CA     9327…       NA      NA 0.0066     8109
##  3 Coll… of     San … CA     9440…       NA      NA 0.0038     8278
##  4 Vent… Colle… Vent… CA     9300…       NA      NA 0.0035     8407
##  5 Oxna… Colle… Oxna… CA     9303…       NA      NA 0.0085     8516
##  6 Moor… Colle… Moor… CA     9302…       NA      NA 0.0151     8577
##  7 Skyl… Colle… San … CA     9406…       NA      NA 0          8580
##  8 Glen… Commu… Glen… CA     9120…       NA      NA 0.002      9181
##  9 Citr… Colle… Glen… CA     9174…       NA      NA 0.0021     9281
## 10 Fres… City   Fres… CA     93741       NA      NA 0.0324     9370
## # … with 328 more rows, and 2 more variables: C150_4_POOLED <dbl>,
## #   PFTFTUG1_EF <dbl>
```

12. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Please use a barplot.

```r
univ_calif %>% 
  ggplot(aes(x=ADM_RATE, y=INSTNM))+
  geom_bar(stat="identity")
```

```
## Warning: Removed 2 rows containing missing values (position_stack).
```

![](MidTerm__Updated_PallaviMalladi_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

