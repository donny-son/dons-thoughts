---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: false
---

{{ template "_internal/disqus.html" . }}
