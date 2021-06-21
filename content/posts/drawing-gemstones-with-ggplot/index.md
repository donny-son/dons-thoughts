---
title: "Drawing Gemstones With ggplot"
date: 2021-06-22T06:21:04+09:00
draft: false
toc: true
images:
comment: true
enableEmoji: true
tags:
- R
- ggplot
---

# Intro

Composition of irregular and complex patterns can be simplified with ggplot; graphical package used in R.   
This project aims to recreate various crystallic shapes via random sampling from uniform and standard normal distribution.
Additional to the `ggplot2` package there are two more dependencies; `cowplot` and `RColorBrewer`.
`cowplot` is required for importing the `theme_nothing()` function(minimal theme without any legends or axis) and the `plot_grid()` function which enables plotting multiple ggplot objects in a single page. 
`RColorBrewer` package is required to import various color palettes.
The following images are gem-like patterns created from this exercise. 


{{< figure src="amber_round.png" alt="Solar" >}}

{{< figure src="emerald_square.png" alt="Emerald" >}}

```{r}
library(ggplot2)
library(cowplot)
library(RColorBrewer)
```

# Code

One global variable has to be defined prior to function definitions. 
```{r}
# levels of palette
BREWER_MAX = 8
```
This sets the number of levels of predefined color palettes from `RColorBrewer`.
In this project, the maximum number 8 is used for more depth and complexity. 


## Functions

### `set_jewels_df`

The following `set_jewels_df` function creates random sampled dataframe.
Detailed explanation of the function arguments is displayed in the following table. 

| Argument | Meaning |
| --- | ------- |
| n_polygons | Number of polygons to draw  |
| shape | If "square" samples from uniform distribution. If "round" samples from standard normal distribution |
| seed1 | Seed for setting the brewer level to the polygon |
| seed2 | Seed for sampling the x axis |
| seed3 | Seed for sampling the y axis |

```{r}
# create polygons
set_jewels_df <- function(n_polyngons, shape, seed1, seed2, seed3) {
  n <- n_polyngons*4
  ids <- factor(1:n)
  set.seed(seed1)
  values <- data.frame(
    id = ids,
    value = sample(1:n, BREWER_MAX, replace = TRUE)
  )
  positions <- data.frame(
    id = rep(ids, each=4)
  )
  if (shape == "square") {
    set.seed(seed1); positions$x <- runif(4*n)
    set.seed(seed2); positions$y <- runif(4*n)   
  } else if (shape == "round") {
    set.seed(seed1); positions$x <- rnorm(4*n)
    set.seed(seed2); positions$y <- rnorm(4*n)   
  } else {
    stop("shape argument must be 'square' or 'round'")
  }
  polydf <- merge(values, positions,  by = c("id"))
  return(polydf)
}
```



### `gg_jewels`

The following `gg_jewels` function creates random sampled dataframe by calling the `set_jewels_df` function and returns the `geom_polygon` ggplot object. 
Detailed explanation of the function arguments is displayed in the following table. 

| Argument | Meaning |
| --- | ------- |
| brew_name | color scheme name. all palattes can be seen via `display.brewer.all()` |
| n_polygons | Number of polygons to draw  |
| shape | If "square" samples from uniform distribution. If "round" samples from standard normal distribution |
| seed1 | Seed for setting the brewer level to the polygon |
| seed2 | Seed for sampling the x axis |
| seed3 | Seed for sampling the y axis |


```{r}
# create && plot polygons
gg_jewels <- function(brew_name, n_polygons, dist, seed1, seed2, seed3) {
  plte <- brewer.pal(BREWER_MAX, brew_name)
  pdf <- set_jewels_df(n_polygons,dist,seed1,seed2,seed3)
  ggplot(pdf, aes(x=x, y=y)) +
    geom_polygon(aes(fill=factor(value), group=id), alpha=0.2) + 
    scale_fill_manual(values = plte) +
    coord_equal() +
    theme_nothing() + 
    theme(
      panel.background = element_rect(fill="black", color = "black"),
      plot.background = element_rect(fill="black", color = "black")
      )-> p
  return(p) 
}
```

# Result

The following is an example of 5 square and round cut simulated jewels with 5 different color schemes. 

{{< figure src="all.png" alt="Composition of result images" >}}

The complete script and the `.Rmd` file is available at [my repository](https://github.com/donny-son/gg_jewels).