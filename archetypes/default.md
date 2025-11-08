---
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
date: {{ .Date }}
categories: []
description: >
  Replace this copy with a short description of the album. Each bundle should
  include at least one image resource so the gallery can render its cover.
resources:
  - src: example.png
    params:
      cover: true
---
