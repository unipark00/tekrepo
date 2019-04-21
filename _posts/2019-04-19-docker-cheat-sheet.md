---
title: "Docker Cheat Sheet"
search: false
categories:
  - Docker
last_modified_at: 2019-04-18T21::00+09:00
toc: false
---

This post shows the cheat sheets for quick docker operation.

## 1) docker ps: formatting
```console
docker ps -a --format "table {{.ID}}\t{{.Status}}\t{{.Ports}}\t{{.Image}}\t{{.Names}}"
```
