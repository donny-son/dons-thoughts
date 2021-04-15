---
title: "R Tips and Tricks"
date: 2021-04-16T20:26:25+09:00
draft: false
toc: true
images:
enableEmoji: true
tags:
  - python
---

## Intro

Over the past few years studying statistics, I've learned a few tips and tricks with `R` and `RStudio`.

## Import multiple libraries at once

Since `R` has some object-oriented goodness, importing multiple libraries all at once is possible in the following way.

```r
pkgs <- c("ggplot2", "dplyr", "sp", "sf", "reticulate", ...)
lapply(library, pkgs, character.only = TRUE)
```

Also note that importing [`tidyverse`](https://www.tidyverse.org/packages/) package can come in handy.

## Backwards assignment with pipe(`%>%`) operators

If you're familiar with the concept of `tidy` data, you already know that the `dplyr` package supports backwards(?) assignment for variables.

```r
library(dplyr)
iris %>% head -> iris.head
```

This comes in handy when you have multiple pipes.

```r
library(dplyr)
iris %>%
  some.first.operation %>%
  ... %>%
  some.last.operation -> iris.cleaned
```

## The Best IDE for R

Personally, it's hands down `RStudio`. I know other editors such as VSCode, PyCharm support `R` with various extensions. They are all okay, but none of those could not top RStudio. I just keep comming back to it. The code execution for `Rmd` files and the variable viewer, the default 2\*2 grid layout just gives me some sort of peace of mind. I'm perfectly aware that RStudio is not the **best** editor in the development world, but at least it's great just for `R`.  
I also use `Neovim` with [`Nvim-R`](https://github.com/jamespeapen/Nvim-R/wiki). This neovim package offers a similarly satisfying experience as it supports variable viewer with great autocompletion as well.

### Tips for RStudio

#### Path Settings using Snippets

For roughly 99% of my workflow, I store all my `rmd`, `r`, images, `csv` and `.RData` all under one repository. But, in `RStudio` the default path when you first send a command to the R console is not that repository. It actually points to your R kernel path. So, this means everytime you will have to manually copy the absolute link to your repository and `setwd()`.  
Luckily, for us we can use snippets and RStudioAPI library to relieve the pain. Under `RStudio Settings(Preferences) > Code` at the bottom, you can see `Edit snippets`. Click and paste the snippet in your snippet file.

```text
snippet setpath
	current_working_dir <- dirname(rstudioapi::getActiveDocumentContext()${1:$}path)
	setwd(current_working_dir)${2}
```

Then, everytime on top of your script, you can just simply type `setpath` and Rstudio will recognize this as your snippet and suggests completion. Hit enter and execute the two lines in your console. Just like that, your R console's path is aligned with the R script path you are editing.

#### Themes

RStudio provides various color themes by default, but somehow I ended up not liking them all. You can actually add colorthemes from this [theme editor website](https://tmtheme-editor.herokuapp.com/#!/editor/theme/Material%20Theme%20Darker).
{{< figure src="rstudio-theme.png" alt="Description" >}}

#### Fonts

I recommend using ligature supported fonts such as JetBrains Mono or [Fira Code](https://github.com/tonsky/FiraCode). They're all open-sourced and looks incredible when in the editor. Math ligatures can save you quite some time on debugging.
{{< figure src="fira-code.png" alt="Description" >}}

#### Python and R at the Same time

I'm sure most of you heard that jupyter can run julia, python and r kernels. But, is it easy to use different languages cell by cell? Furthermore, can you share objects between those languages?  
The [`reticulate`](https://rstudio.github.io/reticulate/) library makes this possible. In the latest version of RStudio you can also handpick your desired python environment associated with the reticulate package. I prefer using a conda environment, so created one named `ret` and use this as the default interpreter for Rstudio.

## Conclusion

More tips and tricks will follow. Thanks for taking the time to read :wave:.
