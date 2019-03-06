---
title: "Visualizing Data 3"
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
## Load the libraries

```r
library(tidyverse)
```


```r
options(scipen=999) #cancels the use of scientific notation for the session
```


```r
homerange <- 
  readr::read_csv("~/Desktop/FRS_417/class_files-master/data/Tamburelloetal_HomeRangeDatabase.csv", na = c("", " ", "NA", "#N/A", "-999", "\\")) ##Replaces the -999 values w NA immediately as the data file is read into R
homerange
```

```
## # A tibble: 569 x 24
##    taxon common.name class order family genus species primarymethod N    
##    <chr> <chr>       <chr> <chr> <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake… american e… acti… angu… angui… angu… rostra… telemetry     16   
##  2 rive… blacktail … acti… cypr… catos… moxo… poecil… mark-recaptu… <NA> 
##  3 rive… central st… acti… cypr… cypri… camp… anomal… mark-recaptu… 20   
##  4 rive… rosyside d… acti… cypr… cypri… clin… fundul… mark-recaptu… 26   
##  5 rive… longnose d… acti… cypr… cypri… rhin… catara… mark-recaptu… 17   
##  6 rive… muskellunge acti… esoc… esoci… esox  masqui… telemetry     5    
##  7 mari… pollack     acti… gadi… gadid… poll… pollac… telemetry     2    
##  8 mari… saithe      acti… gadi… gadid… poll… virens  telemetry     2    
##  9 mari… lined surg… acti… perc… acant… acan… lineat… direct obser… <NA> 
## 10 mari… orangespin… acti… perc… acant… naso  litura… telemetry     8    
## # … with 559 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>,
## #   thermoregulation <chr>, locomotion <chr>, trophic.guild <chr>,
## #   dimension <chr>, preymass <dbl>, log10.preymass <dbl>, PPMR <dbl>,
## #   prey.size.reference <chr>
```

```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```


## Barplots revisited
At this point you should be able to build a barplot that shows counts of observations in a variable using `geom_bar()`. But, you should also be able to use `stat="identity"` to specify both x and y axes.  

## Here is the plot using `geom_bar(stat="identity")`

```r
homerange %>% 
  filter(family=="salmonidae") %>% #filter for salmonid fish only
  ggplot(aes(x=common.name, y=log10.mass))+
  geom_bar(stat="identity")
```

![](Lab_6_1_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

## Practice
Although we did not use it last time, `geom_col()` is the same as specifying `stat="identity"` using `geom_bar()`.     
1. Make the same plot above, but use `geom_col()`

```r
homerange %>% 
  filter(family=="salmonidae") %>% #filter for salmonid fish only
  ggplot(aes(x=common.name, y=log10.mass, fill=common.name))+ ##fills color based on the common names
  geom_col()
```

![](Lab_6_1_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

## Barplots and multiple variables
Last time we explored the `fill` option in boxplots as a way to bring color to the plot; i.e. we filled by the same variable that we were plotting. What happens when we fill by a different categorical variable? 
Let's start by counting how many obervations we have in each taxonomic group.

```r
homerange %>% 
  count(taxon)
```

```
## # A tibble: 9 x 2
##   taxon             n
##   <chr>         <int>
## 1 birds           140
## 2 lake fishes       9
## 3 lizards          11
## 4 mammals         238
## 5 marine fishes    90
## 6 river fishes     14
## 7 snakes           41
## 8 tortoises        12
## 9 turtles          14
```

Now let's make a barplot of these data.

```r
homerange %>% 
  ggplot(aes(x=taxon, fill=taxon))+ ## fills by taxon
  geom_bar(alpha=0.9, na.rm=TRUE)+
  coord_flip()+
  labs(title = "Observations by Taxon in Homerange Data",
       x = "Taxonomic Group")
```

![](Lab_6_1_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

By specifying `fill=trophic.guild` we build a stacked bar plot that shows the proportion of a given taxonomic group that is an herbivore or carnivore.

```r
homerange %>% 
  ggplot(aes(x=taxon, fill=trophic.guild))+ ##makes a stacked bar blot with trophic guild and taxon
  geom_bar(alpha=0.9, na.rm=T)+
  coord_flip()+
  labs(title = "Observations by Taxon in Homerange Data",
       x = "Taxonomic Group",
       fill= "Trophic Guild")
```

![](Lab_6_1_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

We can also have the proportions of each trophic guild within taxonomic group shown side-by-side by specifying `position="dodge"`.

```r
homerange %>% 
  ggplot(aes(x=taxon, fill=trophic.guild))+ ##filling by a different categorical variable than x axis (taxon)
  geom_bar(positiion="dodge")+
  theme(legend.position = "right",
        axis.text.x = element_text(angle = 60, hjust=1))+
  labs(title = "Observations by Taxon in Homerange Data",
       x = "Taxonomic Group",
       fill= "Trophic Guild")+
coord_flip()
```

```
## Warning: Ignoring unknown parameters: positiion
```

![](Lab_6_1_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

## Practice
1. Make a barplot that shows locomotion type by `primarymethod`. Try both a stacked barplot and `position="dodge"`.

```r
homerange %>% 
  ggplot(aes(x=locomotion, fill=primarymethod))+
  geom_bar(position="dodge") ##without position="dodge", the bar plot would be stacked (may be harder to read)!
```

![](Lab_6_1_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


```r
homerange %>% 
  ggplot(aes(x=locomotion, fill=primarymethod)) +
  geom_bar(position=position_fill(), stat="count")+ ## stat="count" is default but we are adding it in for reference purposes
  scale_y_continuous(labels=scales::percent) ##changes scales to percent format
```

![](Lab_6_1_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

## Histograms and Density Plots

## Need to specify number of counts and bends in a histogram, but this may not always be clear based on types of data given
## Histograms are frequency distribution plots
What does the distribution of body mass look like in the homerange data?

```r
homerange %>% 
  ggplot(aes(x=log10.mass)) +
  geom_histogram(fill="steelblue", alpha=0.8, color="black")+ ##using geom_histogram and making it "pretty"; alpha control the transparency levels; color describes the outline color
  labs(title = "Distribution of Body Mass")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](Lab_6_1_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


```r
homerange %>% 
  ggplot(aes(x=log10.mass)) +
  geom_histogram()+ ##using geom_histogram
  labs(title = "Distribution of Body Mass")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](Lab_6_1_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

```r
## This is the histogram without making it more pretty!
```


`Density plots` are similar to histograms but they use a smoothing function to make the distribution more even and clean looking. They do not use bins.; ## No bins in the histogram; graph will look like a curve

```r
homerange %>% 
  ggplot(aes(x=log10.mass)) +
  geom_density(fill="steelblue", alpha=0.4)+
  labs(title = "Distribution of Body Mass")
```

![](Lab_6_1_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

##Both the histogram and the density curve so I often plot them together. Note that I assign the density plot a different color.

```r
homerange %>% 
  ggplot(aes(x=log10.mass)) +
  geom_histogram(aes(y = ..density..), binwidth = .4, fill="steelblue", alpha=0.8, color="black")+
  geom_density(color="red")+
  labs(title = "Distribution of Body Mass")
```

![](Lab_6_1_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

```r
##This will plot both the density graph and the histogram plot 
```

## Practice
1. Make a histogram of `log10.hra`. Make sure to add a title.

```r
homerange %>% 
  ggplot(aes(x=log10.hra))+
  geom_histogram(fill="yellow", alpha=0.3, color="black")+
  labs(title = "Distribution of HRA")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](Lab_6_1_files/figure-html/unnamed-chunk-17-1.png)<!-- -->


2. Now plot the same variable using `geom_density()`.

```r
homerange %>% 
  ggplot(aes(x=log10.hra))+
  geom_density(fill="orange", alpha=0.3, color="black")+
  labs(title = "Distribution of HRA")
```

![](Lab_6_1_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

3. Combine them both!

```r
homerange %>% 
  ggplot(aes(x=log10.hra))+
  geom_histogram(aes(y=..density..), binwidth = 0.5, fill="orchid", alpha=0.7, color="black")+
  geom_density(color="brown")
```

![](Lab_6_1_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

```r
  labs(title = "Distribution of HRA")
```

```
## $title
## [1] "Distribution of HRA"
## 
## attr(,"class")
## [1] "labels"
```
