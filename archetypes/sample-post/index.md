---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: false
toc: true
images:
comment: true
enableEmoji: true
tags:
- python
---

## Intro

Intro sentence goes here.

Sample Image Below.
{{< figure src="sample.jpeg" alt="Description" >}}

{{ template "_internal/disqus.html" . }}
