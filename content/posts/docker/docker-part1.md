---
author: "coldbottle"
title: "Docker Part1"
date: 2021-02-21
draft: false
tags: ["docker", "devops", "mlops"]
categories: ["docker"]
series: ["docker"]
ShowToc: true
TocOpen: true
weight: 3
---

Part1에서는 Docker의 Image를 만들지 않고 사용해 보는 것을 목표로 합니다.

## Installation

자세한 설치 방법은 운영체제에 따라 달라서 링크로 대체합니다.

* [ubuntu](https://docs.docker.com/engine/install/ubuntu/)
* [mac](https://docs.docker.com/docker-for-mac/install/)
* [window](https://docs.docker.com/docker-for-windows/install/)

## docker pull

Docker Image를 다운 받는 명령입니다.

다음과 같은 구조를 가지고 있습니다. 좀더 자세한 정보는 [링크](https://docs.docker.com/engine/reference/commandline/pull/)를 확인하면 됩니다.

```shell
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

* NAME
  * Dockerhub의 경우에 NAME에는 이름만 적어줍니다.
  * 만약, 다른 Container Registry를 사용할 경우에는 이곳에 주소를 적어줍니다.
* TAG
  * TAG가 주어지지 않으면 `latest`가 default입니다.
  * TAG는 버전 처럼 사용하기 때문에 deploy시에는 꼭 특정 TAG를 사용해야 합니다.

### docker pull example

* [Ubuntu Official Image](https://hub.docker.com/_/ubuntu)를 다운 받는 명령은 다음과 같습니다.

```shell
docker pull ubuntu:18.04
```

* [Python Official Image](https://hub.docker.com/_/python)를 다운 받는 명령은 다음과 같습니다.

```shell
docker pull python:3.8-slim
```

## docker images (docker image ls)

Local에 있는 Docker Image를 확인하는 명령어 입니다.

Local에 있는 경우는 크게 2가지 입니다.

* Docker Image를 다운 받은 경우
* Docker Image를 Build한 경우

다음과 같은 구조를 가지고 있습니다. 좀더 자세한 정보는 [링크](https://docs.docker.com/engine/reference/commandline/pull/)를 확인하면 됩니다.

```shell
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

### docker images example

``` shell
docker images
```

출력되는 정보입니다.

```shell
REPOSITORY   TAG        IMAGE ID       CREATED        SIZE
python       3.8-slim   13172ea67a56   36 hours ago   118MB
ubuntu       18.04      c090eaba6b94   4 weeks ago    63.3MB
```

## docker run

Docker Image를 실행하여 Container를 만드는 명령입니다.

다음과 같은 구조를 가지고 있습니다. 좀더 자세한 정보는 [링크](https://docs.docker.com/engine/reference/commandline/run/)를 확인하면 됩니다.

```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

OPTIONS은 하나씩 설명하기에 너무 많기 때문에 example에서 사용할 때 마다 설명하는 것으로 대체합니다.

### docker run example

example에서 사용하는 run OPTION은 `--tty, -t`, `--interactive, -i`, `--rm` 입니다.

* `-it`
  * `-i`, `-t` option을 한번에 적용시키기 위함입니다.
* `--rm`
  * container 종료 후에 container를 지워 주기 위함입니다.

ubuntu:18.04 Image를 Container로 만든 후, Container에 접근하는 명렵니다.

```shell
docker run -it --rm ubuntu:18.04 /bin/bash
```

현재 Container에 내부에서 Bash Shell이 실행 된 상태입니다.

Shell의 Prompt가 바뀐 것을 확인 할 수 있습니다.

```shell
❯ docker run -it --rm ubuntu:18.04 /bin/bash
root@b80db95f4d49:/#
```

## docker ps

실행 중인 Container들의 현황을 확인 할 수 있는 명령입니다.

다음과 같은 구조를 가지고 있습니다. 좀더 자세한 정보는 [링크](https://docs.docker.com/engine/reference/commandline/ps/)를 확인하면 됩니다.

```shell
docker ps [OPTIONS]
```

### docker ps example

example에서 사용하는 run OPTION은 `--detach , -d`입니다.

* `-d`
  * backgroud에서 container를 실행하기 위함입니다.

example에서 사용하는 ps Option은 `--all, -a`입니다.

* `-a`
  * 모든 상태의 container를 확인하기 위함입니다.

ubuntu:18.04 Image를 Container로 만들면서 `sleep 100`이라는 명령을 주었습니다.

```shell
❯ docker run -d ubuntu:18.04 sleep 100
9f14f870eead796ad78b9a948e7fa8b18452710f71e485693835c166f9664ac8
❯ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
9f14f870eead   ubuntu:18.04   "sleep 100"   8 seconds ago   Up 7 seconds             agitated_mestorf
```

100초 이후에 Container의 상태를 확인하면 다음과 같습니다.

Option이 없는 경우에는 Container가 실행중인 상태가 아니기 때문에 보이지 않습니다.

`-a` option을 주면 멈춘 상태의 Container도 확인 할 수 있습니다.

``` shell
❯ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
❯ docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS                          PORTS     NAMES
9f14f870eead   ubuntu:18.04   "sleep 100"   3 minutes ago   Exited (0) About a minute ago             agitated_mestorf
```

## 정리

다른 개발자들이 만들어 놓은 Image를 그대로 사용하는 경우는 DB와 같이 추가 변경 없이 환경변수를 넘겨주는 것으로 사용할 수 있을 때 입니다.

보통은 다른 개발자들이 만들어 놓은 Image를 Base으로 Image에 추가적인 binary, package를 설치하여 Image를 만들어서 사용합니다.

그래서 Part2에서는 Docker Image를 만드는 데 필수적인 Dockerfile에 대해서 알아보겠습니다.
