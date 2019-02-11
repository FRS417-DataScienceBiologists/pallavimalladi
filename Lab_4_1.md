---
title: "Tidy Data 2 and Summarize"
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
## tidy data: every column is essentially its own vector; there is one data type; names of columns are of the same class (ex. characters rather than data)

## Load the tidyverse

```r
library("tidyverse")
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

## Tidyr
+ `gather()` and `spread()` convert data between wide and long format  
+ `separate()` and `unite()` separates or unites information in columns 

## gather()
Recall that we use `gather()` when our column names actually represent variables. A classic example would be that the column names represent observations of a variable.

```r
?USArrests
## pulls up a brief article about this data set
```


```r
arrests <- 
  USArrests %>% 
  mutate(State=rownames(USArrests)) %>% 
  select(State, Murder, Assault, Rape)
arrests ##shows arrests per 100,000 individuals per state
```

```
##             State Murder Assault Rape
## 1         Alabama   13.2     236 21.2
## 2          Alaska   10.0     263 44.5
## 3         Arizona    8.1     294 31.0
## 4        Arkansas    8.8     190 19.5
## 5      California    9.0     276 40.6
## 6        Colorado    7.9     204 38.7
## 7     Connecticut    3.3     110 11.1
## 8        Delaware    5.9     238 15.8
## 9         Florida   15.4     335 31.9
## 10        Georgia   17.4     211 25.8
## 11         Hawaii    5.3      46 20.2
## 12          Idaho    2.6     120 14.2
## 13       Illinois   10.4     249 24.0
## 14        Indiana    7.2     113 21.0
## 15           Iowa    2.2      56 11.3
## 16         Kansas    6.0     115 18.0
## 17       Kentucky    9.7     109 16.3
## 18      Louisiana   15.4     249 22.2
## 19          Maine    2.1      83  7.8
## 20       Maryland   11.3     300 27.8
## 21  Massachusetts    4.4     149 16.3
## 22       Michigan   12.1     255 35.1
## 23      Minnesota    2.7      72 14.9
## 24    Mississippi   16.1     259 17.1
## 25       Missouri    9.0     178 28.2
## 26        Montana    6.0     109 16.4
## 27       Nebraska    4.3     102 16.5
## 28         Nevada   12.2     252 46.0
## 29  New Hampshire    2.1      57  9.5
## 30     New Jersey    7.4     159 18.8
## 31     New Mexico   11.4     285 32.1
## 32       New York   11.1     254 26.1
## 33 North Carolina   13.0     337 16.1
## 34   North Dakota    0.8      45  7.3
## 35           Ohio    7.3     120 21.4
## 36       Oklahoma    6.6     151 20.0
## 37         Oregon    4.9     159 29.3
## 38   Pennsylvania    6.3     106 14.9
## 39   Rhode Island    3.4     174  8.3
## 40 South Carolina   14.4     279 22.5
## 41   South Dakota    3.8      86 12.8
## 42      Tennessee   13.2     188 26.9
## 43          Texas   12.7     201 25.5
## 44           Utah    3.2     120 22.9
## 45        Vermont    2.2      48 11.2
## 46       Virginia    8.5     156 20.7
## 47     Washington    4.0     145 26.2
## 48  West Virginia    5.7      81  9.3
## 49      Wisconsin    2.6      53 10.8
## 50        Wyoming    6.8     161 15.6
```

## Practice
1. Are these data tidy? Please use `gather()` to tidy the data.
## Data is not tidy because there is not one row that corresponds to the 3 sets of numerical data; the state has 3 columns representing affiliated with it

```r
tidy_arrests<-
  arrests %>% 
  gather(Murder, Assault, Rape, key="Crime", value="per100k")
tidy_arrests
```

```
##              State   Crime per100k
## 1          Alabama  Murder    13.2
## 2           Alaska  Murder    10.0
## 3          Arizona  Murder     8.1
## 4         Arkansas  Murder     8.8
## 5       California  Murder     9.0
## 6         Colorado  Murder     7.9
## 7      Connecticut  Murder     3.3
## 8         Delaware  Murder     5.9
## 9          Florida  Murder    15.4
## 10         Georgia  Murder    17.4
## 11          Hawaii  Murder     5.3
## 12           Idaho  Murder     2.6
## 13        Illinois  Murder    10.4
## 14         Indiana  Murder     7.2
## 15            Iowa  Murder     2.2
## 16          Kansas  Murder     6.0
## 17        Kentucky  Murder     9.7
## 18       Louisiana  Murder    15.4
## 19           Maine  Murder     2.1
## 20        Maryland  Murder    11.3
## 21   Massachusetts  Murder     4.4
## 22        Michigan  Murder    12.1
## 23       Minnesota  Murder     2.7
## 24     Mississippi  Murder    16.1
## 25        Missouri  Murder     9.0
## 26         Montana  Murder     6.0
## 27        Nebraska  Murder     4.3
## 28          Nevada  Murder    12.2
## 29   New Hampshire  Murder     2.1
## 30      New Jersey  Murder     7.4
## 31      New Mexico  Murder    11.4
## 32        New York  Murder    11.1
## 33  North Carolina  Murder    13.0
## 34    North Dakota  Murder     0.8
## 35            Ohio  Murder     7.3
## 36        Oklahoma  Murder     6.6
## 37          Oregon  Murder     4.9
## 38    Pennsylvania  Murder     6.3
## 39    Rhode Island  Murder     3.4
## 40  South Carolina  Murder    14.4
## 41    South Dakota  Murder     3.8
## 42       Tennessee  Murder    13.2
## 43           Texas  Murder    12.7
## 44            Utah  Murder     3.2
## 45         Vermont  Murder     2.2
## 46        Virginia  Murder     8.5
## 47      Washington  Murder     4.0
## 48   West Virginia  Murder     5.7
## 49       Wisconsin  Murder     2.6
## 50         Wyoming  Murder     6.8
## 51         Alabama Assault   236.0
## 52          Alaska Assault   263.0
## 53         Arizona Assault   294.0
## 54        Arkansas Assault   190.0
## 55      California Assault   276.0
## 56        Colorado Assault   204.0
## 57     Connecticut Assault   110.0
## 58        Delaware Assault   238.0
## 59         Florida Assault   335.0
## 60         Georgia Assault   211.0
## 61          Hawaii Assault    46.0
## 62           Idaho Assault   120.0
## 63        Illinois Assault   249.0
## 64         Indiana Assault   113.0
## 65            Iowa Assault    56.0
## 66          Kansas Assault   115.0
## 67        Kentucky Assault   109.0
## 68       Louisiana Assault   249.0
## 69           Maine Assault    83.0
## 70        Maryland Assault   300.0
## 71   Massachusetts Assault   149.0
## 72        Michigan Assault   255.0
## 73       Minnesota Assault    72.0
## 74     Mississippi Assault   259.0
## 75        Missouri Assault   178.0
## 76         Montana Assault   109.0
## 77        Nebraska Assault   102.0
## 78          Nevada Assault   252.0
## 79   New Hampshire Assault    57.0
## 80      New Jersey Assault   159.0
## 81      New Mexico Assault   285.0
## 82        New York Assault   254.0
## 83  North Carolina Assault   337.0
## 84    North Dakota Assault    45.0
## 85            Ohio Assault   120.0
## 86        Oklahoma Assault   151.0
## 87          Oregon Assault   159.0
## 88    Pennsylvania Assault   106.0
## 89    Rhode Island Assault   174.0
## 90  South Carolina Assault   279.0
## 91    South Dakota Assault    86.0
## 92       Tennessee Assault   188.0
## 93           Texas Assault   201.0
## 94            Utah Assault   120.0
## 95         Vermont Assault    48.0
## 96        Virginia Assault   156.0
## 97      Washington Assault   145.0
## 98   West Virginia Assault    81.0
## 99       Wisconsin Assault    53.0
## 100        Wyoming Assault   161.0
## 101        Alabama    Rape    21.2
## 102         Alaska    Rape    44.5
## 103        Arizona    Rape    31.0
## 104       Arkansas    Rape    19.5
## 105     California    Rape    40.6
## 106       Colorado    Rape    38.7
## 107    Connecticut    Rape    11.1
## 108       Delaware    Rape    15.8
## 109        Florida    Rape    31.9
## 110        Georgia    Rape    25.8
## 111         Hawaii    Rape    20.2
## 112          Idaho    Rape    14.2
## 113       Illinois    Rape    24.0
## 114        Indiana    Rape    21.0
## 115           Iowa    Rape    11.3
## 116         Kansas    Rape    18.0
## 117       Kentucky    Rape    16.3
## 118      Louisiana    Rape    22.2
## 119          Maine    Rape     7.8
## 120       Maryland    Rape    27.8
## 121  Massachusetts    Rape    16.3
## 122       Michigan    Rape    35.1
## 123      Minnesota    Rape    14.9
## 124    Mississippi    Rape    17.1
## 125       Missouri    Rape    28.2
## 126        Montana    Rape    16.4
## 127       Nebraska    Rape    16.5
## 128         Nevada    Rape    46.0
## 129  New Hampshire    Rape     9.5
## 130     New Jersey    Rape    18.8
## 131     New Mexico    Rape    32.1
## 132       New York    Rape    26.1
## 133 North Carolina    Rape    16.1
## 134   North Dakota    Rape     7.3
## 135           Ohio    Rape    21.4
## 136       Oklahoma    Rape    20.0
## 137         Oregon    Rape    29.3
## 138   Pennsylvania    Rape    14.9
## 139   Rhode Island    Rape     8.3
## 140 South Carolina    Rape    22.5
## 141   South Dakota    Rape    12.8
## 142      Tennessee    Rape    26.9
## 143          Texas    Rape    25.5
## 144           Utah    Rape    22.9
## 145        Vermont    Rape    11.2
## 146       Virginia    Rape    20.7
## 147     Washington    Rape    26.2
## 148  West Virginia    Rape     9.3
## 149      Wisconsin    Rape    10.8
## 150        Wyoming    Rape    15.6
```

2. Restrict the data to assualt only. Sort in ascending order.

```r
##filter the tidy_arrests since it is already gathered; use the filter command to find assaults
tidy_arrests %>% 
  filter(Crime=="Assault") %>% 
  arrange(desc(per100k))
```

```
##             State   Crime per100k
## 1  North Carolina Assault     337
## 2         Florida Assault     335
## 3        Maryland Assault     300
## 4         Arizona Assault     294
## 5      New Mexico Assault     285
## 6  South Carolina Assault     279
## 7      California Assault     276
## 8          Alaska Assault     263
## 9     Mississippi Assault     259
## 10       Michigan Assault     255
## 11       New York Assault     254
## 12         Nevada Assault     252
## 13       Illinois Assault     249
## 14      Louisiana Assault     249
## 15       Delaware Assault     238
## 16        Alabama Assault     236
## 17        Georgia Assault     211
## 18       Colorado Assault     204
## 19          Texas Assault     201
## 20       Arkansas Assault     190
## 21      Tennessee Assault     188
## 22       Missouri Assault     178
## 23   Rhode Island Assault     174
## 24        Wyoming Assault     161
## 25     New Jersey Assault     159
## 26         Oregon Assault     159
## 27       Virginia Assault     156
## 28       Oklahoma Assault     151
## 29  Massachusetts Assault     149
## 30     Washington Assault     145
## 31          Idaho Assault     120
## 32           Ohio Assault     120
## 33           Utah Assault     120
## 34         Kansas Assault     115
## 35        Indiana Assault     113
## 36    Connecticut Assault     110
## 37       Kentucky Assault     109
## 38        Montana Assault     109
## 39   Pennsylvania Assault     106
## 40       Nebraska Assault     102
## 41   South Dakota Assault      86
## 42          Maine Assault      83
## 43  West Virginia Assault      81
## 44      Minnesota Assault      72
## 45  New Hampshire Assault      57
## 46           Iowa Assault      56
## 47      Wisconsin Assault      53
## 48        Vermont Assault      48
## 49         Hawaii Assault      46
## 50   North Dakota Assault      45
```


## spread()
The opposite of `gather()`. You use `spread()` when you have an observation scattered across multiple rows. In the example below, `cases` and `population` represent variable names not observations.

```r
country <- c("Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan", "Brazil", "Brazil", "Brazil", "Brazil", "China", "China", "China", "China")
year <- c("1999", "1999", "2000", "2000", "1999", "1999", "2000", "2000", "1999", "1999", "2000", "2000")
key <- c("cases", "population", "cases", "population", "cases", "population", "cases", "population", "cases", "population", "cases", "population")
value <- c(745, 19987071, 2666, 20595360, 37737, 172006362, 80488, 174504898, 212258, 1272915272, 213766, 1280428583)

tb_data <- data.frame(country=country, year=year, key=key, value=value)
tb_data ##shows tuberculosis infection rates in different countries by year
```

```
##        country year        key      value
## 1  Afghanistan 1999      cases        745
## 2  Afghanistan 1999 population   19987071
## 3  Afghanistan 2000      cases       2666
## 4  Afghanistan 2000 population   20595360
## 5       Brazil 1999      cases      37737
## 6       Brazil 1999 population  172006362
## 7       Brazil 2000      cases      80488
## 8       Brazil 2000 population  174504898
## 9        China 1999      cases     212258
## 10       China 1999 population 1272915272
## 11       China 2000      cases     213766
## 12       China 2000 population 1280428583
```

When using `spread()` the `key` is the variable that you are spreading.

```r
tb_data %>% 
  spread(key=key, value=value) ##makes data that are tidy; each column represents it own vector
```

```
##       country year  cases population
## 1 Afghanistan 1999    745   19987071
## 2 Afghanistan 2000   2666   20595360
## 3      Brazil 1999  37737  172006362
## 4      Brazil 2000  80488  174504898
## 5       China 1999 212258 1272915272
## 6       China 2000 213766 1280428583
```

```r
##key takes the number of categories and creates that same number of colums (key contains cases and population, so these will become 2 columns after using the spread() function)
```

## Practice
1. Run the following to build the `gene_exp` data frame.

```r
id <- c("gene1", "gene1", "gene2", "gene2", "gene3", "gene3")
type <- c("treatment", "control", "treatment", "control","treatment", "control")
L4_values <- rnorm(6, mean = 20, sd = 3)
```


```r
gene_exp <- 
  data.frame(gene_id=id, type=type, L4_values=L4_values)
gene_exp
```

```
##   gene_id      type L4_values
## 1   gene1 treatment  22.44854
## 2   gene1   control  22.43180
## 3   gene2 treatment  24.41605
## 4   gene2   control  22.53449
## 5   gene3 treatment  21.87971
## 6   gene3   control  18.10016
```

2. Are these data tidy? Please use `spread()` to tidy the data.

```r
gene_exp %>% 
  spread(type, L4_values) ##could also write as spread(key=type, value=L4_values)
```

```
##   gene_id  control treatment
## 1   gene1 22.43180  22.44854
## 2   gene2 22.53449  24.41605
## 3   gene3 18.10016  21.87971
```

```r
##separates type into control and treatment columns
```


## summarize()
summarize() will produce summary statistics for a given variable in a data frame. For example, in homework 2 you were asked to calculate the mean of the sleep total column for large and small mammals. We did this using a combination of tidyverse and base R commands, which isn't very efficient or clean. It also took two steps.

```r
?msleep ##produces information about the msleep data frame
##name of data frame msleep is "mammals sleep"
```

From homework 2.

```r
large <- msleep %>% 
  select(name, genus, bodywt, sleep_total) %>% 
  filter(bodywt>=200) %>% 
  arrange(desc(bodywt))
large
```

```
## # A tibble: 7 x 4
##   name             genus         bodywt sleep_total
##   <chr>            <chr>          <dbl>       <dbl>
## 1 African elephant Loxodonta      6654          3.3
## 2 Asian elephant   Elephas        2547          3.9
## 3 Giraffe          Giraffa         900.         1.9
## 4 Pilot whale      Globicephalus   800          2.7
## 5 Cow              Bos             600          4  
## 6 Horse            Equus           521          2.9
## 7 Brazilian tapir  Tapirus         208.         4.4
```


```r
mean(large$sleep_total) ##find the means of the bodyweight since we are looking at "large" data frame which is filtered for bodyweight
```

```
## [1] 3.3
```


```r
names(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"       
##  [5] "conservation" "sleep_total"  "sleep_rem"    "sleep_cycle" 
##  [9] "awake"        "brainwt"      "bodywt"
```

We can accomplish the same task using the `summarize()` function in the tidyverse and make things cleaner.

```r
msleep %>% 
  filter(bodywt>=200) %>%
  summarize(mean_sleep_lg=mean(sleep_total)) ##makes a new column that is the mean of the sleep total
```

```
## # A tibble: 1 x 1
##   mean_sleep_lg
##           <dbl>
## 1           3.3
```

You can also combine functions to make useful summaries for multiple variables.

```r
msleep %>% 
    filter(bodywt>=200) %>% 
    summarize(mean_sleep_lg = mean(sleep_total), 
              min_sleep_lg = min(sleep_total),
              max_sleep_lg = max(sleep_total),
              sd_sleep_lg=sd(sleep_total), ##gives the standard deviation for total sleep (compares values to the mean total sleep)
              total = n()) ## returns the length of the vector
```

```
## # A tibble: 1 x 5
##   mean_sleep_lg min_sleep_lg max_sleep_lg sd_sleep_lg total
##           <dbl>        <dbl>        <dbl>       <dbl> <int>
## 1           3.3          1.9          4.4       0.870     7
```

```r
##shows the summary of the mean of the requested variables
```

There are many other useful summary statistics, depending on your needs: sd(), min(), max(), median(), sum(), n() (returns the length of vector), first() (returns first value in vector), last() (returns last value in vector) and n_distinct() (number of distinct values in vector).

## Practice
1. How many genera are represented in the msleep data frame?

```r
msleep %>% 
  summarize (ngenera=n_distinct(genus)) ##looking at number of genera within the genus column
```

```
## # A tibble: 1 x 1
##   ngenera
##     <int>
## 1      77
```

2. What are the min, max, and mean body weight for all of the mammals? Be sure to include the total n.

```r
msleep %>% 
  summarize(minbodywt=min(bodywt),
            maxbodywt=max(bodywt),
            meanbodywt=mean(bodywt),
            total=n()) ##returns the total number of observations within that column
```

```
## # A tibble: 1 x 4
##   minbodywt maxbodywt meanbodywt total
##       <dbl>     <dbl>      <dbl> <int>
## 1     0.005      6654       166.    83
```

## show how many types of "vores" there are within the msleep data frame

```r
msleep %>% 
  count(vore)
```

```
## # A tibble: 5 x 2
##   vore        n
##   <chr>   <int>
## 1 carni      19
## 2 herbi      32
## 3 insecti     5
## 4 omni       20
## 5 <NA>        7
```

```r
##vore is a column
```

##group_by()
The `summarize()` function is most useful when used in conjunction with `group_by()`. Although producing a summary of body weight for all of the mammals in the dataset is helpful, what if we were interested in body weight by feeding ecology?

```r
msleep %>%
  group_by(vore) %>% ##vore=trophic types; herbivore, omnivore, carnivore, etc; we are grouping by feeding ecology
  summarize(min_bodywt=min(bodywt),
            max_bodywt=max(bodywt),
            mean_bodywt=mean(bodywt),
            total=n()) #gives number of observations within each set
```

```
## # A tibble: 5 x 5
##   vore    min_bodywt max_bodywt mean_bodywt total
##   <chr>        <dbl>      <dbl>       <dbl> <int>
## 1 carni        0.028      800        90.8      19
## 2 herbi        0.022     6654       367.       32
## 3 insecti      0.01        60        12.9       5
## 4 omni         0.005       86.2      12.7      20
## 5 <NA>         0.021        3.6       0.858     7
```

```r
## in a huge data set, you can now group by however you want to within a data set; specify which data you are looking at
## use in data sets that you don't know very well; statistical summary of basic info provided within the frame
```

## Practice
1. Calculate mean brain weight by taxonomic order in the msleep data.

```r
msleep %>% 
  group_by(order) %>% ##groups by taxonomic order
  summarize(mean_brain_wt=(mean(brainwt)))
```

```
## # A tibble: 19 x 2
##    order           mean_brain_wt
##    <chr>                   <dbl>
##  1 Afrosoricida         0.0026  
##  2 Artiodactyla        NA       
##  3 Carnivora           NA       
##  4 Cetacea             NA       
##  5 Chiroptera           0.000275
##  6 Cingulata            0.0459  
##  7 Didelphimorphia     NA       
##  8 Diprotodontia       NA       
##  9 Erinaceomorpha       0.00295 
## 10 Hyracoidea           0.0152  
## 11 Lagomorpha           0.0121  
## 12 Monotremata          0.025   
## 13 Perissodactyla       0.414   
## 14 Pilosa              NA       
## 15 Primates            NA       
## 16 Proboscidea          5.16    
## 17 Rodentia            NA       
## 18 Scandentia           0.0025  
## 19 Soricomorpha         0.000592
```

```r
##There are many different Artiodactyla; if there is even 1 Ariodactyla without the brain data entered, it will return NA for all the Artiodactyla!
```


2. What does `NA` mean?
##some values return NA bc values are not present for that order

3. Try running the code again, but this time add `na.rm=TRUE`. What is the problem with Cetacea?

```r
msleep %>% 
  group_by(order) %>%
  summarize(mean_brain_wt=mean(brainwt,na.rm=TRUE)) ##return brain weight for which the values are not NA; Cratacea has a problem since it does not contain any brain weight values
```

```
## # A tibble: 19 x 2
##    order           mean_brain_wt
##    <chr>                   <dbl>
##  1 Afrosoricida         0.0026  
##  2 Artiodactyla         0.198   
##  3 Carnivora            0.0986  
##  4 Cetacea            NaN       
##  5 Chiroptera           0.000275
##  6 Cingulata            0.0459  
##  7 Didelphimorphia      0.0063  
##  8 Diprotodontia        0.0114  
##  9 Erinaceomorpha       0.00295 
## 10 Hyracoidea           0.0152  
## 11 Lagomorpha           0.0121  
## 12 Monotremata          0.025   
## 13 Perissodactyla       0.414   
## 14 Pilosa             NaN       
## 15 Primates             0.254   
## 16 Proboscidea          5.16    
## 17 Rodentia             0.00357 
## 18 Scandentia           0.0025  
## 19 Soricomorpha         0.000592
```


```r
msleep %>% 
  filter(order=="Cetacea")
```

```
## # A tibble: 3 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
## 1 Pilo… Glob… carni Ceta… cd                   2.7       0.1          NA
## 2 Comm… Phoc… carni Ceta… vu                   5.6      NA            NA
## 3 Bott… Turs… carni Ceta… <NA>                 5.2      NA            NA
## # … with 3 more variables: awake <dbl>, brainwt <dbl>, bodywt <dbl>
```

```r
##there is no brain weight for Cetacea; can't calculate the mean for something that is not there
```
