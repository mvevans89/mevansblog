---
title: Advent of Code 2021
author: Michelle
date: '2021-12-05'
slug: []
categories: []
tags:
  - R
description: ''
---

This post will follow my solutions to the [2021 Advent of Code](https://adventofcode.com/2021). Each "day" is it's own self-contained chunk of code, so all of the packages, functions, data, etc. needed for that day is found under that sub-title.

# Day 1

## Part I

[Here](https://adventofcode.com/2021/day/1) is a link to the full problem. Essentially, we need to find a way to count instances where `x+1 > x`.  A nice way to do this is via a function.

First, we can create some example data to test our function on:


```r
test.data <- floor(runif(10,100,200))
test.data
```

```
##  [1] 108 131 157 119 130 111 100 101 103 182
```

Now we write a function to return the count of how many instances of increase over one step there are:


```r
count_increase <- function(vec){
  count <- 0
  for(i in 2:length(vec)){
    if(vec[i]>vec[i-1]) count <- count +1
  }
  return(count)
}
```

Does it work?


```r
count_increase(test.data)
```

```
## [1] 6
```

Now we try it on the true data:


```r
#load here package for easier data loading
library(here)
```

```
## here() starts at /Users/mvevans/Dropbox/git/mevansblog
```

```r
input.data <- read.table(here("static", "data", "advent-code", "day01.txt"))

#test our function
count_increase(input.data$V1)
```

```
## [1] 1477
```

## Part II

For the second part, we want to compare moving window statistics to see the difference in depth across groups of three measurements. For more info on moving window statistics, you can look at an [old post of mine on the topic](https://ditheringdata.netlify.app/2020/02/11/rolling-functions-along-columns/).

For extra challenge, we can this without the help of packages like `zoo` or `dplyr` that already have built in functions for this.

We can make a new function that compares the averages across a rolling window, and make that window itself an argument for the function, so we could apply it to a range of window sizes.


```r
count_increase_window <- function(window_size, vec){
  count <- 0
  for(i in (window_size+1):length(vec)){
    if(sum(vec[(i-(window_size-1)):i]) > sum(vec[(i-1):(i-window_size)])) count <- count + 1
  }
  return(count)
}
```

Note that we start the for-loop at `window_size+1` so we our first comparison can be with a full window from `1:window_size`


```r
count_increase_window(window_size = 3, vec = input.data$V1)
```

```
## [1] 1523
```
