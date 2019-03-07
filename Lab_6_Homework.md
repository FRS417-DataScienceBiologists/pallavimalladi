---
title: "Lab 6 Homework"
author: "Pallavi Malladi"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---



## Load the libraries

```r
library(tidyverse)
library(skimr)
library("RColorBrewer")
```

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use.

```r
##install.packages("gapminder")
```


```r
library("gapminder")
```

Please load the data into a new object called gapminder.

```r
gapminder <- 
  gapminder::gapminder
gapminder
```

```
## # A tibble: 1,704 x 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
## # … with 1,694 more rows
```

1. Explore the data using the various function you have learned. Is it tidy, are there any NA's, what are its dimensions, what are the column names, etc.
## The data is tidy because each different observation of the variables are in a different row. In addition, the data is easy to read because each of the 6 variables corresponds to one column

```r
colnames(gapminder) ##shows column names of gapminder
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```

```r
glimpse(gapminder) 
```

```
## Observations: 1,704
## Variables: 6
## $ country   <fct> Afghanistan, Afghanistan, Afghanistan, Afghanistan, Af…
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, …
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, …
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854…
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 148803…
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.…
```

```r
##1,704 observations with 6 variables (columns)
##Dimensions: 1,704 x 6 (rows x columns)
```

```r
gapminder<-
 gapminder %>% 
  na_if(-999)
##Replaces all -999 values with NA so that it is easier to track where there is an NA with the next line of code
```


```r
number_nas= sum(is.na(gapminder))
gapminder %>%
  purrr::map_df(~ sum(is.na(.))) %>%  ##shows how many NA values and where they are within the data frame
  tidyr::gather(variables, number_nas) %>% ##I had to get help from GitHub to understand the last 2 lines here
  arrange(desc(number_nas))
```

```
## # A tibble: 6 x 2
##   variables number_nas
##   <chr>          <int>
## 1 country            0
## 2 continent          0
## 3 year               0
## 4 lifeExp            0
## 5 pop                0
## 6 gdpPercap          0
```

```r
## This shows that there are no NA values in gapminder
```

2. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer on average. Make a quick plot below to visualize this relationship.

```r
gapminder %>% 
  ggplot(aes(x=gdpPercap, y=lifeExp))+
  geom_point()+
  labs(title = "Life Expectancy vs GDP",
       x = "Per Capita GDP",
       y= "Life Expectancy")
```

![](Lab_6_Homework_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
##There seems to be a positive relationship between life expectancy and gdp. The longer a person lives, the higher their per capita GDP
```

3. There is extreme disparity in per capita GDP. Rescale the x axis to make this easier to interpret. How would you characterize the relationship?

```r
gapminder %>% 
  ggplot(aes(x=log10(gdpPercap), y=lifeExp))+
  geom_point()+
   labs(title = "Life Expectancy vs GDP",
       x = "Per Capita GDP",
       y= "Life Expectancy")
```

![](Lab_6_Homework_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
##There seems to be a positive relationship between life expectancy and gdp. The longer a person lives, the higher their per capita GDP
```

4. This should look pretty dense to you with significant overplotting. Try using a faceting approach to break this relationship down by year.

```r
gapminder %>% 
  ggplot(aes(x = log10(gdpPercap), y=lifeExp)) +
  geom_point() +
   labs(title = "Life Expectancy vs GDP",
       x = "Per Capita GDP",
       y= "Life Expectancy")+
  facet_wrap(~year) ##"facet wrap by tilda taxon"- provides individual histograns for each of the taxa
```

![](Lab_6_Homework_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

```r
##Shows the positive linear relationship between life expectancy and gdp by year
```

5. Simplify the comparison by comparing only 1952 and 2007. Can you come to any conclusions?

```r
gapminder %>% 
  filter(year==1952 | year==2007) %>%
  ggplot(aes(x=log10(gdpPercap), y=lifeExp))+
  geom_point(color="orchid")+
  facet_wrap(~year)+
  theme_light()
```

![](Lab_6_Homework_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

6. Let's stick with the 1952 and 2007 comparison but make some aesthetic adjustments. First try to color by continent and adjust the size of the points by population. Add `+ scale_size(range = c(0.1, 10), guide = "none")` as a layer to clean things up a bit.

```r
gapminder %>%
  filter(year==1952 | year==2007) %>% 
  ggplot(aes(x=log10(gdpPercap), y=lifeExp, color=continent, size=pop))+
  geom_point()+
  facet_grid(~year)+
  scale_size(range = c(0.1, 10), guide = "none")
```

![](Lab_6_Homework_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


7. Although we did not introduce them in lab, ggplot has a number of built-in themes that make things easier. I like the light theme for these data, but you can see lots of options. Apply one of these to your plot above.

```r
?theme_light
```

8. What is the population for all countries on the Asian continent in 2007? Show this as a barplot.

```r
gapminder %>% 
  filter(continent=="Asia", year=="2007") %>% 
  ggplot(aes(x=country, y=pop, fill=country))+
  geom_bar(stat="identity")+
   labs(title = "China Population over the Years",
       x = "Popualtion",
       y= "Country")+
  coord_flip()
```

![](Lab_6_Homework_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

9. You should see that China's population is the largest with India a close second. Let's focus on China only. Make a plot that shows how population has changed over the years.

```r
gapminder %>% 
  filter(country=="China") %>% 
   ggplot(aes(x=year, y=pop))+
  geom_bar(stat = "identity")+
  labs(title = "China Population over the Years",
       x = "Year",
       y= "Population")+
  ylim(0, 1.5E+09)
```

![](Lab_6_Homework_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

```r
## The barplot shows the continuous increase in the China Population over the years; Increasing trendline
```


10. Let's compare China and India. Make a barplot that shows population growth by year using `position=dodge`. Apply a custom color theme using RColorBrewer.

```r
gapminder %>% 
  filter(country=="China" | country=="India") %>% 
  ggplot(aes(x=year, y=pop, fill=country))+
    geom_bar(stat="identity", position="dodge")+
  scale_fill_brewer(palette = "Dark4")
```

```
## Warning in pal_name(palette, type): Unknown palette Dark4
```

![](Lab_6_Homework_files/figure-html/unnamed-chunk-17-1.png)<!-- -->
