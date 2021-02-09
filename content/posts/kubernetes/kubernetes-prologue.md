---
author: "coldbottle"
title: "Kubernetes Prologue"
date: 2021-02-09
draft: true
tags: ["kubernetes", "k8s", "devops", "mlops"]
categories: ["kubernetes"]
series: ["kubernetes"]
ShowToc: true
TocOpen: true
weight: 11
---

## Docker

하나의 컴퓨터에서 docker를 사용해서 image를 만들고 container를 실행하는 작업이 있었음
하지만 docker command line을 개발 당시에는 외웠지만 어떤 volume mapping 하고 network어쩌구
관리가 어려움
물론 shell script으로 관리하면 된다.?
여러개 container를 동시에 띄우려면?
shell script?


## Docker-compose

하나의 컴퓨터에서 여러 container를 한번에 띄우기 위한 도구


## kubernetes

여러대의 컴퓨터에서 container를 관리할 수는 없을까?
