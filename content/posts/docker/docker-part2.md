---
author: "coldbottle"
title: "Docker Part2"
date: 2021-02-21
draft: true
tags: []
categories: []
series: []
ShowToc: true
TocOpen: true
weight: 99999
---

Part2에서는 Docker Image를 만드는데 필요한 Dockerfile, 명령어 docker build에 대해서 알아보는 것을 목표로 합니다.

## Dockerfile

Docker Image를 만드는데 사용하는 파일입니다. 파일명을 Dockerfile로 하는 것이 관례적이지만, 다른 파일 명을 사용해도 됩니다.
대신 `docker build`시에 `-f` option으로 파일명을 넘겨주어야 합니다.

Dockerfile의 내용은 다음과 같은 Format을 따릅니다.

```dockerfile
# Comment
INSTRUCTION arguments
```

### FROM

어떤 Base Image에서부터 시작하는지 명시합니다. Official Image를 Base으로 하는 경우가 보통이지만, 다른 개발자나 본인이 만든 Image를 Base으로 할 수 있습니다.

다음과 같은 구조를 가지고 있습니다.
좀더 자세한 정보는 [링크](https://docs.docker.com/engine/reference/builder/#from)를 확인하면 됩니다.

```dockerfile
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
```

#### FROM example

ubuntu:18.04 Image를 Base으로 하는 경우에는 다음과 같이 Dockerfile에 추가합니다.

```dockerfile
FROM ubuntu:18.04
...
```

python:3.8 Image를 Base으로 하는 경우에는 다음과 같이 Dockerfile에 추가합니다.

```dockerfile
FROM python:3.8
...
```

### RUN

어떤 명령어을 실행하는 명령입니다.

다음과 같은 형태를 가지고 있습니다.
좀더 자세한 정보는 [링크](https://docs.docker.com/engine/reference/builder/#run)를 확인하면 됩니다.

```dockerfile
RUN <command>
```

#### RUN example

ubuntu:18.04 Base Image에 git을 설치하는 Dockerfile입니다.

```dockerfile
FROM ubuntu:18.04
RUN apt update && apt install -y git
```

### CMD, ENTRYPOINT

[링크](https://docs.docker.com/engine/reference/builder/#understand-how-cmd-and-entrypoint-interact)

### COPY, ADD

두개 모두 Docker Image에 file을 복사/추가하는 명령이다.

기본적으로는 host에 있는 파일(src)를 Docker Image에 파일(dest)을 전달할 때 사용한다.

하지만, ADD의 경우에는 src가 압축파일 경우에는 압축을 풀고,
src가 url인 경우에는 다운 받는 기능을 포함하고 있다.

좀더 자세한 정보는 [ADD 링크](https://docs.docker.com/engine/reference/builder/#add), [COPY 링크](https://docs.docker.com/engine/reference/builder/#copy)를 확인하면 됩니다.

### ARG

#### ARG example

## docker build

위에서 설명한 Dockerfile에 기술된 명령어를 실행하며 Docker Image를 만드는 명령어 입니다.



part2
Dockerfile
Image layer
docker build

part 3
docker exec
이미지를 만들때 사용하면 유용함
docker logs
docker system prune 

## login

[dockerhub](https://hub.docker.com)에서 가입한 후에 진행해야 합니다.

가입하지 않아도 

로그인 명령은 아래와 같습니다.

```shell
docker login
```

docker push

docker run 실습