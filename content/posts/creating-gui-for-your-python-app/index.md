---
title: "Creating Gui for Your Python App"
date: 2021-04-15T08:05:55+09:00
draft: false
toc: true
images:
enableEmoji: true
tags:
  - python
  - gui
---

## Intro

Sometimes it is just nice to make a simple GUI for your python application. There could be a lot of reasons why you want to, or perhaps need to do this.

{{< youtube "i1wqQoHG6xU" >}}

For personal use, you might have several machines that you would like to run the application on. For sharing purposes, there might be fellow colleagues who need to use the python application wihtout wanting to interact with the terminal. It's also easier to distribute your program to multiple users. As shown in the attached video, launching the application is simple as double-clicking the custom-made icon. There's no need for individual users to create a virtual python environment, install required packages and so on...

## Journey

It didn't took too long creating my `main.py`, which consists of one function and one class. The `m3u8Downloader` class is the core element of this app. It inputs a filename and the url that ends with `.m3u8` for each video content. The `.m3u8` file acts as a meta data for loading short clips of an entire video. In other words, this is a instruction manual for reconstructing the entire video with short sample videos. I guess this is one way to stop people from easily downloading a video content to their local machine.  
Note that this downloader might not work with other platforms since they could all have different instruction manuals for their content.

### Core Libraries

Two python packages were used in this app, `requests` and `m3u8`. As many of you know, `requests` package enables us to simply sends HTTP requests via python. The `m3u8` package handles the meta data file and grabs all the child elements for you. So, my task was fairly simple.

1. Make a custom parser for the `m3u8` python package so it can successfully grab short clips.
2. Send GET requests for the short clips.
3. Merge the downloaded files into a whole video.
4. Miscellaneous tasks such as creating a download directory and deleting all short clips after the merge.

### GUI framework

The last time I used a python GUI framework was to create an automated PDF extractor for medical reports. I used `PyQt5`, but it wasn't easy. Also, since it was merely a python wrapper library for the enormous `Qt5` framework, the documentation was lacking certain features I value the most during development, docstrings. After some research, I ended up using [`pysimplegui`](https://pysimplegui.readthedocs.io/en/latest/), and it was absolutely amazing. The documentation was extensive, autocompletion was good, and it was so simple and easy to use.  
My `gui.py` contains the GUI layout and the while loop for listening to events and the values of the user input. This was also very straightforward to implement by the `PySimpleGui.Window().read()` function. If a `START` event is initiated by the user clicking the START button, the `m3u8Downloader` from `main.py` is initialized with values from the `PySimpleGui.InputText()` objects.

### Building the app

The building process was very tedious and surprisingly difficult. The `pyinstaller` command that worked for me was,

```shell
pyinstaller --icon -icons.icns --windowed --name LearnusPy gui.py
```

The first obstacle for me was the icon. Initially, I just wanted my app to be an UNIX executable app without a GUI interface, since my app only asks for two string inputs and takes care of the rest. Also, this app was one of the reasons why I uploaded my [`tictronome`](https://pypi.org/project/tictronome/) package to `pypi` mentioned in my [previous post]({{< ref "posts/upload-to-pypi" >}}). However, it turns out I could not overwrite the executable icon with my `icon.icns`. Also, Mac users should use `.icns` format as their app icon. `.ico` is compatible with Windows.

So that is basically why I made a `gui.py`. Just to use my weird little custom-made icon. Well we all have our obsessions for somethings...right?  
Since, my app had a GUI now, the `--windowed` option was given, to tell the installer that we literally need a window for running our app.

The other obstacle was that my app, once built, had an unusually long starting time. This was because I was giving the `--onefile` option to the `pyinstaller` command. After removing that option, it loaded way faster. This was very interesting to me, considering that several stackoverflow articles regarding `pyinstaller` used the `--onfile` option almost like a default setting.

## Conclusion

The satisfaction after my first successful build was insurmounatble. It started with a small critical thinking like,

> "Hey, if we're seeing some content on the browser, why can't we download it? After all it is literally receiving data from the server which has no idea what we're doing on our side...".

The entire process of actually figuring out that the intial hypothesis was true and building a solution to overcome the obstacle, it is pure satistcation and a bliss.
