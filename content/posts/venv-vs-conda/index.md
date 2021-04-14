---
title: "venv vs conda"
date: 2021-04-14T22:14:21+09:00
draft: false
toc: true
description: "Comparison between `conda` and `venv`"
images:
enableEmoji: true
tags:
- python
---
## Intro
I used to prefer `venv` over `conda`. This was before I had to simultaneously manange multiple projects. For every repository I would easily setup a `venv` folder in my local machine. Now I completely switched to `conda` because it's easier to work with jupyter and most of my projects share same packages(numpy, scipy, etc.) 

## Summary
`venv` : setting up individual python interpreters(fresh and new) for each project.  
`conda` : creating and using conda environments for each individual project. 

## Personal Thoughts

### `venv`:

#### pro: 
- native python. Able to use the latest python versions 3.9, 3.10, ...
- easy to use

#### con: 

- take up lots of memory if working simultaneously on multiple projects
- `pip install -r requirements.txt` doesn’t always work seamlessly when recreating environments on a different system.

### `conda`:

#### pro:

- less compatibility issues with ML frameworks(tensorflow, pytorch).
- can use both conda install, pip install.
- saves time installing commonly used packages.
- works like a charm with jupyter notebooks. No need for fiddling around with ipykernel.

#### con:

- cannot use with latest versions of python.

The following is a handwritten note. 

{{< figure src="venv-vs-conda.jpeg" alt="Description" >}}
