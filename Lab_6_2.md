---
title: "Visualizing Data 4"
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
## Load the tidyverse

```r
library(tidyverse)
options(scipen=999)
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

## Review color and fill options
We have learned that there are a variety of color and fill options in `ggplot`. These are helpful in making plots more informative and visually appealing; i.e., in this scatterplot we show the relationship between mass and homerange while coloring the points by a different variable.

```r
homerange %>% 
  ggplot(aes(x=log10.mass, y=log10.hra, color=locomotion))+
  geom_point()
```

![](Lab_6_2_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

We can change the color of the points universally instead of using `fill`, but we need to put this into a different layer and specify the exact color. Notice the different options! Experiment by adjusting them.

```r
homerange %>% 
  ggplot(aes(x=log10.mass, y=log10.hra))+
  geom_point()+ ##color=black will add a black edge around each point
  scale_colour_brewer(palette="Dark2") ##points will look more interesting if we add differnet color
```

![](Lab_6_2_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
##use size to control point size; # shape controls circularity
```

## Practice
1. Make a barplot that shows counts of animals by taxonomic class. Fill by thermoregulation type.

```r
homerange %>% 
  ggplot(aes(x=class, fill=thermoregulation))+
  geom_bar()
```

![](Lab_6_2_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
## if we fill we fill by thermoregulation, it adds color based on ectotherm/endotherm
```

2. Make a box plot that shows the range of log10.mass by taxonomic class. Use `group` to cluster animals together by taxon and `fill` to color each box by taxon.

```r
homerange %>% 
  ggplot(aes(x=class, y=log10.mass, group=taxon, fill=taxon))+ ## "fill" shows breakdown of taxa in each class
  geom_boxplot()
```

![](Lab_6_2_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

```r
## Graphing taxa my mass!!
##There are many different taxa associated with class
## Ex. within actinoterygii class, there are 3 taxa, but there is only 1 taxa within aves class
## Black line between box represents median' points outside the box represent outliers
```

## Adjusting the x and y limits
There are many options for adjusting the x and y axes. To adjust limits, we can use the `xlim` and `ylim` commands. When you do this, any data outside the specified ranges are not plotted. ##This will remove the values of points that are outside the range

```r
homerange %>% 
  ggplot(aes(x=log10.mass, y=log10.hra, color=locomotion))+
  geom_point()+
  xlim(0, 4)+
  ylim(1, 6)
```

```
## Warning: Removed 175 rows containing missing values (geom_point).
```

![](Lab_6_2_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

## Faceting: `facet_wrap()`
Faceting allows you to make multi-panel plots for easy comparison. Make histograms of log10.mass for every taxon. We read the `~` in the `facet_wrap()` layer as `by`.

```r
homerange %>% 
  ggplot(aes(x = log10.mass)) +
  geom_histogram() +
  facet_wrap(~taxon) ##"facet wrap by tilda taxon"- provides individual histograns for each of the taxa
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](Lab_6_2_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

## Faceting: `facet_grid()` 
##tends to work only if you are faceting by groups with only few categories (ex. 2 categories in trophic guild=good for facet_grid())
## `facet_grid`. This can be helpful when you facet by only a few variables.

```r
homerange %>% 
  ggplot(aes(x = log10.mass)) +
  geom_histogram() +
  facet_grid(~thermoregulation)+
  scale_colour_brewer(palette = "Dark2") ##points will look more interesting if we add different color
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](Lab_6_2_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

## Practice
1. Use faceting to produce density distributions of log10.mass by taxonomic class.
##faceting will work on any type of plot (bar, point, histogram, etc)

```r
homerange %>% 
  ggplot(aes(x=log10.mass))+
  geom_density(fill="orchid", alpha=0.8)+
  facet_grid(~class)
```

![](Lab_6_2_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
facet_wrap(~thermoregulation)
```

```
## <ggproto object: Class FacetWrap, Facet, gg>
##     compute_layout: function
##     draw_back: function
##     draw_front: function
##     draw_labels: function
##     draw_panels: function
##     finish_data: function
##     init_scales: function
##     map_data: function
##     params: list
##     setup_data: function
##     setup_params: function
##     shrink: TRUE
##     train_scales: function
##     vars: function
##     super:  <ggproto object: Class FacetWrap, Facet, gg>
```

```r
## Makes 4 separate graphs for the 4 classes available
```

## RColorBrewer; library with various coor options (making a customized color palette)

```r
#install.packages("RColorBrewer")
library("RColorBrewer")
```

Access the help for RColor Brewer.

```r
?RColorBrewer
```

##Three different palettes: 1) sequential, 2) diverging, and 3) qualitative. Within each of these there are several selections. You can bring up the colors by using `display.brewer.pal()`. Specify the number of colors that you want and the palette name.

```r
display.brewer.pal(8,"BrBG")
```

![](Lab_6_2_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

```r
## Shows 8 colors of a specific palette; we can see different number of colors if we want; change 8-->4, etc
```

+`scale_colour_brewer()` is for points  
+`scale_fill_brewer()` is for fills  

```r
homerange %>% 
  ggplot(aes(x=log10.mass, y=log10.hra, color=locomotion))+ ##mass by homerange; coloring by locomotion
  geom_point(size=1.5, alpha=0.8)
```

![](Lab_6_2_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

```r
##regular graph
```


```r
homerange %>% 
  ggplot(aes(x=log10.mass, y=log10.hra, color=locomotion))+ ##mass by homerange; coloring by locomotion
  geom_point(size=1.5, alpha=0.8)+
scale_colour_brewer(palette = "Dark2") ##points will look more interesting if we add different color
```

![](Lab_6_2_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

```r
##fancier graph
```


```r
homerange %>% 
  ggplot(aes(x=thermoregulation, fill=thermoregulation))+
  geom_bar()+
  labs(title = "Thermoregulation Counts",
       x = "Thermoregulation Type",
       y = "# Individuals")+ 
  theme(plot.title = element_text(size = rel(1.25)))+
  scale_fill_brewer(palette = "Dark2")
```

![](Lab_6_2_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

```r
##install.packages("paletteer")
```



```r
library("paletteer")
```

```
## Warning: package 'paletteer' was built under R version 3.5.2
```

```r
colors<-
  paletteer::palettes_d_names
```


```r
simpsons<-
  paletteer_d(package="ggsci", palette="springfield_simpsons")
barplot(rep(1,14),axes = FALSE, col=simpsons) ## we can pull up a color palette
```

![](Lab_6_2_files/figure-html/unnamed-chunk-20-1.png)<!-- -->


```r
homerange %>% 
  ggplot(aes(x=thermoregulation, fill=thermoregulation))+
  geom_bar()+
  scale_fill_manual(values=simpsons) ##points will look more interesting if we add differnet color
```

![](Lab_6_2_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

```r
##use size to control point size; # shape contr
```

## Practice
1. Build a dodged bar plot that shows the number of carnivores and herbivores filled by taxonomic class. Use a custom color theme.

```r
homerange %>%
  ggplot(aes(x=trophic.guild, fill=class)) +
  geom_bar(alpha=0.6, position="dodge")+ ##dodge will keep bars side by side instead of stacked
  scale_fill_brewer(palette = "Dark4")
```

```
## Warning in pal_name(palette, type): Unknown palette Dark4
```

![](Lab_6_2_files/figure-html/unnamed-chunk-22-1.png)<!-- -->
