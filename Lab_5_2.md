---
title: "Data Visualization 2"
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
library(skimr)
```


```r
homerange <- 
  readr::read_csv("~/Desktop/FRS_417/class_files-master/data/Tamburelloetal_HomeRangeDatabase.csv", na = c("", " ", "NA", "#N/A", "-999"))
## if we have knowledge of how NA's are represented in the data, we can add the tag line while reading in data, so R can interpret all NAs. Since NA is represented by blanks (""), -999, NA of "#N/A", we can understand that these represent null values. AUTOMATICALLY READ IN DATA CORRECTLY!
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
##If there are NA's, R wil not plot the data
Notice that by including common options for NA's, we improve the way data are read into R.

```r
glimpse(homerange) ##Notice how there are dif types of data
```

```
## Observations: 569
## Variables: 24
## $ taxon                      <chr> "lake fishes", "river fishes", "river…
## $ common.name                <chr> "american eel", "blacktail redhorse",…
## $ class                      <chr> "actinopterygii", "actinopterygii", "…
## $ order                      <chr> "anguilliformes", "cypriniformes", "c…
## $ family                     <chr> "anguillidae", "catostomidae", "cypri…
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma"…
## $ species                    <chr> "rostrata", "poecilura", "anomalum", …
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-…
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2",…
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 35…
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.60…
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87…
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.09…
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home…
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aqu…
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm"…
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "…
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore"…
## $ dimension                  <chr> "3D", "2D", "2D", "2D", "2D", "2D", "…
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA,…
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA…
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, …
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et …
```


```r
names(homerange) ##gives list of column names for data
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

## Box Plots
Boxplots help us visualize a range of values. So, on the x-axis we typically have something categorical and the y-axis is the range. In the case below, we are plotting `log10.mass` by taxonomic class in the homerange data. `geom_boxplot()` is the geom type for a standard boxplot. The center line in each box represents the median, not the mean.

```r
homerange %>% 
  ggplot(aes(x=class, y=log10.mass))+
  geom_boxplot()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
## dark black line is median
## data is arranged in quartiles
## box plot will only plot a range of values
```

## Practice
1. There are more herbivores than carnivores in the homerange data, but how do their masses compare? Make a boxplot that compares their masses. Use `log10.mass`.

```r
homerange %>% 
  ggplot(aes(x=trophic.guild,y=log10.mass))+
  geom_boxplot()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

2. Practice your dplyr skills and filter out carnivorous mammals. 

```r
carni_mammals <- 
  homerange %>% 
  filter(taxon=="mammals", trophic.guild=="carnivore") ##acrni_mammals will contains mammal taxon within carnivorous trophic guild
carni_mammals
```

```
## # A tibble: 80 x 24
##    taxon common.name class order family genus species primarymethod N    
##    <chr> <chr>       <chr> <chr> <chr>  <chr> <chr>   <chr>         <chr>
##  1 mamm… giant gold… mamm… afro… chrys… chry… trevel… telemetry*    <NA> 
##  2 mamm… Grant's go… mamm… afro… chrys… erem… granti  telemetry*    <NA> 
##  3 mamm… arctic fox  mamm… carn… canid… alop… lagopus telemetry*    <NA> 
##  4 mamm… Ethiopian … mamm… carn… canid… canis simens… telemetry*    <NA> 
##  5 mamm… culpeo      mamm… carn… canid… pseu… culpae… telemetry*    <NA> 
##  6 mamm… South Amer… mamm… carn… canid… pseu… griseus telemetry*    <NA> 
##  7 mamm… kit fox     mamm… carn… canid… vulp… macroti telemetry*    <NA> 
##  8 mamm… Ruppel's f… mamm… carn… canid… vulp… ruppel… telemetry*    <NA> 
##  9 mamm… swift fox   mamm… carn… canid… vulp… velox   telemetry*    <NA> 
## 10 mamm… fossa       mamm… carn… euple… cryp… ferox   telemetry*    <NA> 
## # … with 70 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>,
## #   thermoregulation <chr>, locomotion <chr>, trophic.guild <chr>,
## #   dimension <chr>, preymass <dbl>, log10.preymass <dbl>, PPMR <dbl>,
## #   prey.size.reference <chr>
```

3. Now use a boxplot to visualize the range of body mass by family of carnivore.

```r
carni_mammals %>% 
  ggplot(aes(x=family, y=log10.mass))+
  geom_boxplot()+
  coord_flip()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

## Aesthetics: Labels
Now that we have practiced scatterplots, barplots, and boxplots we need to learn how to adjust their appearance to suit our needs. Let's start with labelling x and y axes.  

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_point()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
## shows the relationship between body mass and homerange.
```

## The plot looks clean, but it is incomplete. A reader unfamiliar with the data might have a difficult time interpreting the current labels (ex. log.10.hra). To add custom labels, we use the `labs` command.

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_point()+
  labs(title = "Mass vs. Homerange", 
       x = "Mass (log10)",
       y = "Homerange (log10)")
```

![](Lab_5_2_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
##Adds title and labels x and y axes
```

## We can improve the plot further by adjusting the size and face of the text. We do this using `theme()`.

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_point()+
  labs(title = "Mass vs. Homerange",
       x = "Mass (log10)",
       y = "Homerange (log10)")+
  theme(plot.title=element_text(size=16, face="bold"),## makes title text larger and bold 
        axis.text=element_text(size=8), ## adjusts size of the numbers of the side
        axis.title=element_text(size=14)) ## adjusts the size of the axes labels
```

![](Lab_5_2_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

The `rel()` option changes the relative size of the title to keep things consistent. Adding `hjust` allows control of title position.

```r
ggplot(data=homerange, mapping=aes(x=log10.mass, y=log10.hra)) +
  geom_point()+
  labs(title = "Mass vs. Homerange",
       x = "Mass (log10)",
       y = "Homerange (log10)")+ 
  theme(plot.title = element_text(size = rel(2), hjust = 0.25))
```

![](Lab_5_2_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

```r
## rel(2) picks relative sizes of text so that we do not have to individually tweak it
```

## Practice
1. Make a barplot that shows the number of individuals per locomotion type. Be sure to provide a title and label the axes appropriately.

```r
homerange %>% 
  ggplot(aes(x=locomotion))+ ## No need to code for y axis since bar plot automatically adds numeric to the y-axis
  geom_bar()+
  labs(title="Number of Individuals per Locomotion Type",
       x = "Locomotion Type",
       y = "Number Individuals")+ 
  theme(plot.title = element_text(size = rel(1.5)))+
  coord_flip()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

```r
##The graph shows that walking is the most common type of locomotion
```

## Other Aesthetics
There are lots of options for aesthtics. An aesthetic can be assigned to either numeric or categorical data. `color` is a common option; notice that an appropriate key is displayed when you use one of these options.

```r
homerange %>% 
  ggplot(aes(x=log10.mass, y=log10.hra, color=locomotion))+ 
  geom_point()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

```r
##color=locomotion will add color based on the locomotion type; coloring by another variable
```

##Color code by trophic guild

```r
homerange %>% 
  ggplot(aes(x=log10.mass, y=log10.hra, color=trophic.guild))+ 
  geom_point()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

```r
##color=trophic.guilf will add color based on carnivore or herbivoree; coloring by another variable
```

## Change the color 

```r
homerange %>% 
  ggplot(aes(x=log10.mass, y=log10.hra))+
           geom_point(color="red") ## Can do orange, blue, purple, etc
```

![](Lab_5_2_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

```r
## This will NOT color by variable! It will only change all the points to the specified color on the plot
```

## `size` adjusts the size of points relative to a continuous variable.

```r
options(scipen = 999) #disable scientific notation

homerange %>% 
  ggplot(aes(x=log10.mass, y=log10.hra, size=mean.mass.g))+ ##changes size of point based on animal mass
  geom_point()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

```r
##Animals that have the largest mass are located at the top with the largest point!
```

## Here I am plotting `class` on the x-axis and `log10.mass` on the y-axis. I use `group` to make individual box plots for each taxon. I also use `fill` so I can associate the different taxa with a color coded key.

```r
homerange %>% 
  ggplot(aes(x=class, y=log10.mass, group=taxon, fill=class))+
  geom_boxplot()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

## Show different classes within the taxa

```r
homerange %>% 
  ggplot(aes(x=class, y=log10.mass, group=taxon, fill=taxon))+ ## ##looking for all the taxa and organizing by class
  ## fills in box plots based on their taxons and their specific classes
  geom_boxplot()
```

![](Lab_5_2_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

## Practice
1. Make a barplot that shows counts of ectotherms and endotherms. Label the axes, provide a title, and fill by thermoregulation type.

```r
homerange %>% 
  ggplot(aes(x=thermoregulation, fill=thermoregulation))+
  geom_bar()+
  labs(title = "Thermoregulation Counts",
       x = "Thermoregulation Type",
       y = "# Individuals")+ 
  theme(plot.title = element_text(size = rel(1.25)))
```

![](Lab_5_2_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

2. Make a boxplot that compares thermoregulation type by log10.mass. Group and fill by class. Label the axes and provide a title.

```r
homerange %>% 
  ggplot(aes(x=thermoregulation, y=log10.mass, group=class, fill=class))+
  geom_boxplot()+
  labs(title = "Thermoregulation vs. Log10 Mass by Taxonomic Class",
       x = "Thermoregulation Type",
       y = "log10.mass")+ 
  theme(plot.title = element_text(size = rel(1.25)))
```

![](Lab_5_2_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

## Wrap-up
Please review the learning goals and be sure to use the code here as a reference when completing the homework.

-->[Home](https://jmledford3115.github.io/datascibiol/)
