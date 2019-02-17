---
title: "Data Visualization 1"
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
##Plots are built in layers
## What makes a good chart? Its elegance and simplicity. It provides a clean, clear visual of the data without being overwhelming to the reader.
## Data Types
+  `discrete` quantitative data that only contains integers
+ `continuous` quantitative data that can take any numerical value
+ `categorical` qualitative data that can take on a limited number of values

## Basics: **plot= data + geom_ + aesthetics**  
## Load the libraries

```r
library("tidyverse")
library("skimr")
```


```r
?iris ##provides overview of iris data
```


```r
?iris
names(iris) ##gives category (column names) for iris
```

```
## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
## [5] "Species"
```


```r
iris <-
  iris ##puts iris data into a new object so that we can look at iris data quickly
```

## To make a plot, we need to first specify the data and map the aesthetics. The aesthetics include how each variable in our dataset will be used. In the example below, I am using the aes() function to identify the x and y variables in the plot.

```r
ggplot(data=iris, mapping=aes(x=Species, y=Petal.Length)) ## we named Species and Petal.length based on names(iris)
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

## Notice that we have a nice background, labeled axes, and even values of our variables- but no plot. This is because we need to tell ggplot what type of plot we want to make. This is called the geometry or `geom()`.Here we specify that we want a boxplot, indicated by `geom_boxplot()`.

```r
ggplot(data=iris, mapping=aes(x=Species, y=Petal.Length))+
  geom_boxplot() ##creates a boxplot based on the ggplot data from x and y axes
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

```r
## We add a "+" bc plots are built in layers
## First layer is the data in the plot; Second layer is the design of the plot
```

## Practice
1. Take a moment to practice. Use the iris data to build a scatterplot that compares sepal length vs. sepal width. Use the cheatsheet to find the correct `geom_` for a scatterplot.

```r
ggplot(data=iris, mapping=aes(x=Sepal.Width, y=Sepal.Length))+
  geom_point() ##plots data in iris (Species.Width, Sepal.Length) as a scatterplot diagram
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

 ##Two standard plots: 1) scatterplots and 2) barplots.


**Database of vertebrate home range sizes.Import from classfiles**


```r
homerange <- readr::read_csv("~/Desktop/FRS_417/class_files-master/Tamburelloetal_HomeRangeDatabase.csv")
```

```r
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

A little bit of cleaning to focus on the variables of interest. Good `dplyr` practice!

```r
homerange <- ##modifies homerange to select for these categories
  homerange %>%  
  select(common.name, taxon, family, genus, species, mean.mass.g, log10.mass, mean.hra.m2, log10.hra, preymass, log10.preymass, trophic.guild)
```


```r
names(homerange)
```

```
##  [1] "common.name"    "taxon"          "family"         "genus"         
##  [5] "species"        "mean.mass.g"    "log10.mass"     "mean.hra.m2"   
##  [9] "log10.hra"      "preymass"       "log10.preymass" "trophic.guild"
```

### 1. Scatter Plots
##Scatter plots are good at revealing relationships that are not readily visible in the raw data. For now, we will not add regression lines or calculate any r^2^ values. In this case, we are exploring whether or not there is a relationship between animal mass and homerange. We are using log transformed values because there is a very large difference in mass and homerange among the different  species in the data.

##Scatter plot deals with numerical data, not categorical

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_point()
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

```r
## Is there an association between size of the beast and the area it requires to live? Hopefully, the answer is yes since larger beasts require more space to live

## The scatter plot is linear--> there is a correlation between mass and homerange area
## There is a lot of overlap between some points (dense areas) --> we can use geom_jitter to clean up the plot
```

## In big data sets with lots of similar values, overplotting can be an issue. `geom_jitter()` is similar to `geom_point()` but it helps with overplotting by adding some random noise to the data and separating some of the individual points.

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_jitter()
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

##Adding in a linear regression line

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_jitter()+
  geom_smooth(method=lm, se=FALSE) 
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

```r
## method = lm means that we are adding in a smooth lm (linear model)
## se=FALSE 
```

### 2A. Bar Plot: `stat="count"` When making a bar graph, the default is to count the number of observations in the specified column. This is best for categorical data and is defined by the aesthetic `stat="count"`. Here, I want to know how many carnivores vs. herbivores are in the data.  
## Bar plots are best suited for categorical data

## Notice that we can use pipes and the `mapping=` function is implied by `aes` and so is often left out (Bc ggplot already understands it's mapping)  

#### we cannot specify a y variable for a geom_bar. By default, it creates a count on the y-axis

```r
homerange %>% 
  ggplot(aes(x=trophic.guild))+ ##trophic guild represents carnivore, herbivore, omnivore, etc
  geom_bar(stat="count") # I am identifying stat="count" but this isn't necessary since it is default
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

```r
## x axis is the trophic guild and y axis represents the number of each type of trophic guild (the count of each)
```
##we can also remove the stat="count" since the y-axis will automatically create a count-like axis

```r
homerange %>% 
  ggplot(aes(x=trophic.guild))+ ##trophic guild represents carnivore, herbivore, omnivore, etc
  geom_bar()
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


### 2B. Bar Plot: `stat="identity"`
`stat="identity"` allows us to map a variable to the y axis so that we aren't restricted to count. Let's use our dplyr skills to first filter out carnivorous mammals and see which families have the highest mean body weight.

```r
carni_mammals <- 
  homerange %>% 
  filter(taxon=="mammals", 
         trophic.guild=="carnivore") %>% 
  group_by(family) %>% ## groups by families that are within the carnivourous trophic guild
  summarize(count=n(),
            mean_body_wt=mean(log10.mass)) %>% 
  arrange(desc(mean_body_wt))
carni_mammals ## shows the carnivores and their families by decreasing body weight
```

```
## # A tibble: 18 x 3
##    family          count mean_body_wt
##    <chr>           <int>        <dbl>
##  1 ursidae             1        4.99 
##  2 felidae            19        4.16 
##  3 hyanidae            1        4    
##  4 eupleridae          1        3.98 
##  5 canidae             7        3.73 
##  6 viverridae          3        3.49 
##  7 herpestidae         5        3.16 
##  8 mustelidae         15        3.08 
##  9 peramelidae         2        2.74 
## 10 erinaceidae         2        2.69 
## 11 tachyglossidae      1        2.41 
## 12 dasyuridae          4        2.32 
## 13 macroscelididae     3        2.27 
## 14 chrysochloridae     2        2.00 
## 15 talpidae            4        1.90 
## 16 cricetidae          2        1.39 
## 17 didelphidae         2        1.38 
## 18 soricidae           6        0.882
```

##Visualize the previous code(body weight of carnivores in different families) by creating a plot
Now let's plot the data using `stat="identity"` to help visualize these differences.

```r
carni_mammals%>% 
  ggplot(aes(x=family, y=mean_body_wt))+
  geom_bar(stat="identity")
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

```r
##plots the different body weights on the y-axis instead of count
```

## This looks nice, but the x axis is a mess and needs adjustment. We make these adjustments using `theme()`. 
## Adjust the angle of the text on the x-axis

```r
carni_mammals%>% 
  ggplot(aes(x=family, y=mean_body_wt))+
  geom_bar(stat="identity")+
  theme(axis.text.x = element_text(angle = 60, hjust=1)) ##Change the angle of the text on the x-axis to 60 degrees
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

```r
## hjudt=1 adjusts the length of the text next to the bar (placement of the text on the x-axis)
```

##We can make this a little bit better by reordering.

```r
carni_mammals%>% 
  ggplot(aes(x=reorder(family, -mean_body_wt), y=mean_body_wt))+ ##reorder will label data as ascending or descending based on if mean_body_wt is - or +
  geom_bar(stat="identity")+
  theme(axis.text.x = element_text(angle = 60, hjust=1))
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

##Turn the plot on its side; flip the coordinates.

```r
carni_mammals%>% 
  ggplot(aes(x=reorder(family, -mean_body_wt), y=mean_body_wt))+
  geom_bar(stat="identity")+
  coord_flip()
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

## Practice
Filter the `homerange` data to include `mammals` only.

```r
mammals <- 
  homerange %>% 
  filter(taxon=="mammals") %>% 
  select(common.name, family, genus, species, trophic.guild, mean.mass.g, log10.mass, mean.hra.m2, log10.hra, preymass, log10.preymass)
```


```r
mammals
```

```
## # A tibble: 238 x 11
##    common.name family genus species trophic.guild mean.mass.g log10.mass
##    <chr>       <chr>  <chr> <chr>   <chr>               <dbl>      <dbl>
##  1 giant gold… chrys… chry… trevel… carnivore            437.       2.64
##  2 Grant's go… chrys… erem… granti  carnivore             23        1.36
##  3 pronghorn   antil… anti… americ… herbivore          46100.       4.66
##  4 impala      bovid… aepy… melamp… herbivore          63504.       4.80
##  5 hartebeest  bovid… alce… busela… herbivore         136000.       5.13
##  6 barbary sh… bovid… ammo… lervia  herbivore         167498.       5.22
##  7 American b… bovid… bison bison   herbivore         629999.       5.80
##  8 European b… bovid… bison bonasus herbivore         628001.       5.80
##  9 goat        bovid… capra hircus  herbivore          51000.       4.71
## 10 Spanish ib… bovid… capra pyrena… herbivore          69499.       4.84
## # … with 228 more rows, and 4 more variables: mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, preymass <dbl>, log10.preymass <dbl>
```

##1. Are there more herbivores or carnivores in mammals? Make a bar plot that shows their relative proportions.

```r
mammals %>% 
  ggplot(aes(x=trophic.guild))+ ##trophic guild represents carnivore, herbivore, omnivore, etc
  geom_bar()
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-24-1.png)<!-- -->

```r
## We can tell that there are more herbivores by looking at the bar plot of mammals and plotting the trophic guild on the x axis
```


```r
names(mammals)
```

```
##  [1] "common.name"    "family"         "genus"          "species"       
##  [5] "trophic.guild"  "mean.mass.g"    "log10.mass"     "mean.hra.m2"   
##  [9] "log10.hra"      "preymass"       "log10.preymass"
```

##2. Is there a positive or negative relationship between mass and homerange? How does this compare between herbivores and carnivores? Make two plots that illustrate these eamples below.  
## USe dyplr functions such as filter, select, etc
carnivores

```r
mammals %>% 
  filter (trophic.guild=="carnivore") %>%
  ggplot(aes(x=log10.mass, y=log10.hra)) + ##make a plot based off of the carnivore trophic guild 
  geom_point()+
  geom_smooth(method=lm, se=FALSE) 
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-26-1.png)<!-- -->

herbivores

```r
mammals %>% 
  filter (trophic.guild=="herbivore") %>%
  ggplot(aes(x=log10.mass, y=log10.hra)) + ##make a plot based off of the herbivore trophic guild 
  geom_point()+
  geom_smooth(method=lm, se=FALSE) 
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-27-1.png)<!-- -->

##3. Make a barplot that shows the masses of the top 10 smallest mammals in terms of mass. Be sure to use `stat'="identity"`.
## Hard Way

```r
homerange %>% 
  filter(taxon=="mammals") %>% 
  select(common.name, mean.mass.g) %>% 
  arrange (mean.mass.g)
```

```
## # A tibble: 238 x 2
##    common.name                    mean.mass.g
##    <chr>                                <dbl>
##  1 cinereus shrew                        4.17
##  2 slender shrew                         4.37
##  3 arctic shrew                          8.13
##  4 crowned shrew                         9.33
##  5 greater white-footed shrew           10   
##  6 salt marsh harvest mouse             11.0 
##  7 long-clawed shrew                    14.1 
##  8 Northern three-striped opossum       19.5 
##  9 wood mouse                           21.2 
## 10 southern grasshopper mouse           22   
## # … with 228 more rows
```

```r
## cinereus shrew is the mammal w smallest mass 
```

## Easy way

```r
mammals %>% 
  top_n(-10, log10.mass) %>% ##gives the top 10 smallest mammals
  ggplot(aes(x=common.name, y = log10.mass))+
  geom_bar (stat="identity")+
  coord_flip()
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-29-1.png)<!-- -->

## Make the bar plot from #3 look prettier! This will show the bar plot in increasing order!

```r
 mammals %>% 
  top_n(-10, log10.mass) %>% ##gives the top 10 smallest mammals
  ggplot(aes(x=reorder(common.name,log10.mass), y = log10.mass))+
  geom_bar (stat="identity")+
  coord_flip()
```

![](Lab5_1_rev_files/figure-html/unnamed-chunk-30-1.png)<!-- -->
