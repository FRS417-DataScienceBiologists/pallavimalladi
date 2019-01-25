---
title: "Homework 1"
author: "Pallavi Malladi"
date: "1/19/2019"
output: 
  html_document: 
    keep_md: yes
---

## 1. Calculate the following expressions

```r
5-3*2
```

```
## [1] -1
```

```r
8/2 **2
```

```
## [1] 2
```

## 2. Did any of the results in #4 surprise you? Write two programs that calculate each expression such that the result for the first example is 4 and the second example is 16.

```r
## The answers did not surprise me since the calculations used PEMDAS to solve. 
(5-3)*2
```

```
## [1] 4
```

```r
(8/2)**2
```

```
## [1] 16
```

## 3. You have decided to use your new analytical powers in R to become a professional gambler. Here are your winnings and losses this week.

```r
blackjack <- c(140, -20, 70, -120, 240)
roulette <- c(60, 50, 120, -300, 10)
```
## 3a. Build a new vector called days for the days of the week (Monday through Friday).

```r
days <- c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday")
days
```

```
## [1] "Monday"    "Tuesday"   "Wednesday" "Thursday"  "Friday"
```
 
##We will use days to name the elements in the poker and roulette vectors.

```r
names(blackjack) <- days
names(roulette) <- days
```

## 3b. Calculate how much you won or lost in blackjack over the week

```r
total_blackjack <- sum(blackjack)
total_blackjack
```

```
## [1] 310
```

## 3c. Calculate how much you won or lost in roulette over the week

```r
total_rouletee <- sum(roulette)
total_rouletee
```

```
## [1] -60
```

## 3d. Build a total_week vector to show how much you lost or won on each day over the week. Which days seem lucky or unlucky for you?

```r
total_week <-c(blackjack + roulette) 
total_week 
```

```
##    Monday   Tuesday Wednesday  Thursday    Friday 
##       200        30       190      -420       250
```

```r
## prints the net total for each day (blackjack game + roulette game)
## Monday and Friday are the most lucky days, and Thursday has the worst luck.
```

## Should you stick to blackjack or roulette? Write a program that verifies this below

```r
total_blackjack > total_rouletee
```

```
## [1] TRUE
```

```r
## Since the statement returns true that the total blackjack sum is greater than the roulette sum for the week, the player should stick to blackjack.
```

