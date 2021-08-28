---
title: "Drawing Gemstones With ggplot"
date: 2021-08-28T06:21:04+09:00
draft: false
toc: true
images:
comment: true
enableEmoji: true
tags:
- R
- ggplot
---


Composition of irregular and complex patterns can be simplified with ggplot; graphical package used in R.   
This project aims to recreate various crystallic shapes via random sampling from uniform and standard normal distribution.
Additional to the `ggplot2` package there are two more dependencies; `cowplot` and `RColorBrewer`.
`cowplot` is required for importing the `theme_nothing()` function(minimal theme without any legends or axis) and the `plot_grid()` function which enables plotting multiple ggplot objects in a single page. 
`RColorBrewer` package is required to import various color palettes.
The following images are gem-like patterns created from this exercise. 


{{< figure src="amber_round.png" alt="Solar" >}}

{{< figure src="emerald_square.png" alt="Emerald" >}}

The following is an example of 5 square and round cut simulated jewels with 5 different color schemes. 

{{< figure src="all.png" alt="Composition of result images" >}}

Variations of prints can be purchased from my [OpenSea NFT store](https://opensea.io/DNYSN).