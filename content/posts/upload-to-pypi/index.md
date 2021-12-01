---
title: "Upload to Pypi"
date: 2021-04-15T00:01:49+09:00
draft: false
toc: true
images:
comment: true
enableEmoji: true
tags:
  - python
  - pypi
  - packaging
---

## Intro

Few days ago, I published [my first python package](https://github.com/donny-son/tictronome) to [pypi](https://pypi.org/project/tictronome/). Here's what I've learned.

{{< figure src="sample.png" alt="Description" >}}

## Tools

`build` and `twine` were the two fundamental packages.
The `build` package literally **builds** a repository into a distributable compressed file.
This simple command `python3 -m build` will create a `dist/` directory which contains the `tar.gz` and `whl` files.  
`twine` can directly **upload** your repository to `pypi` and `testpypi`. `testpypi` is nothing more than a test version of `pypi`. This comes in handy before you publish to `pypi` in order to get an exact preview.

## Structuring

The following is the project tree structure of my `tictronome` package. Starting from the top, `LICENSE` file is a simple text file that contains an open source license. The MIT license is widely used.  
`README.md` should contain instructions on how to install and use your package. I personally recommend creating a separate readme file for `pypi` since it does not render images in my `assets` folder. For convenience I just used the same `README.md` file for my GitHub repo.  
`assests` directory contains images and resources associated with my `README.md` file.  
`build/` is automatically created by running the `build` command.  
`pyproject.toml` contains configurations of your build system, such as the version of `setuptools` required.  
`setup.cfg` is the core configuration file. This will be a roadmap to your build. The `.cfg` file is a static config file. However you can also use a dynamic configuration file(`setup.py`). I decided to use the static configfile from this [documentation](https://packaging.python.org/tutorials/packaging-projects/).  
`test/` contains simple tests for my main scripts.

```.
ticronome
├── LICENSE
├── README.md
├── assets
│   ├── color_example.png
│   ├── seconds.gif
│   └── simple-tics.gif
├── build
│   └── bdist.macosx-10.9-x86_64
├── pyproject.toml
├── setup.cfg
├── src
│   └── tictronome
│       ├── __init__.py
│       ├── colors
│       │   ├── __init__.py
│       │   ├── ansi.py
│       │   └── colored_string.py
│       └── tictronome.py
└── tests
    └── test_tic.py
```

I used the `src` directory to be my package directory. So if I do a `pip install tictronome`, then I can import code in the following way.

```python
from tictronome.tictronome import Tic
from tictronome.colors import ansi
from tictronome.colors import colored_string
```

Note that `Tic` in the first line is a class defined in `tictronome.py`.

## Todos

Even after some trial and error, I was not able to perform a direct import. In other words, I could not do this.

```python
from tictronome import Tic
```

Still trying to figure out how I could make this happen.

Also, on my next build, I'm considering to use a dynamic configuration file(`setup.py`) and not use the `build` package and only using the `setuptools` package.
