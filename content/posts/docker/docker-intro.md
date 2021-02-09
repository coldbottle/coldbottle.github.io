---
author: "coldbottle"
title: "도커(Docker) Intro"
date: 2021-02-09
draft: false
tags: ["docker", "devops", "mlops"]
categories: ["docker"]
series: ["docker"]
ShowToc: true
TocOpen: true
weight: 2
---

이번 글에서는 [지난글](../docker-prologue)에 이어서 docker를 사용할 때 꼭 알고 있어야 하는 개념을 정리해보았습니다.
## docker vs dockerd

docker를 설치하게 되면 2가지 Software를 얻게 됩니다.

dockerd가 docker daemon의 줄인 말로 Server의 역할을 하고 사용자의 입력을 받아서 Container를 제어합니다.

다른 하나인 docker는 CLI(CommandLine Interface)형태로 제공된 Clinet으로 dockerd에게 명령을 전달할 때 사용됩니다.

CLI 형태의 Client도 있지만, GUI(Graphic User Inferace)형태의 Client가 [Docker Desktop for Window and Mac](https://www.docker.com/products/docker-desktop)도 존재합니다.

## Image vs Container

CS(Computer Science)전공인 독자들을 프로그램과 프로세스의 차이를 알고있을 것입니다.

프로그램은 실행되지 않는 상태를 뜻합니다. 예를들어 `test.py`는 프로그램입니다.

프로세스는 프로그램이 실행중인 상태를 뜻합니다. 예를들어 `python test.py`가 종료되지 않았다면 프로세스입니다.

docker에서도 Image와 Container는 프로그램과 프로세스의 관계랑 같습니다.

Image는 미리 만들어 진것을 그대로 사용하기도 하고, 미리 만들어진 Image에서 자신에 입맛에 맞게 수정해서 사용하기도 합니다.

자신만의 Image를 만들기 위해서는 Dockerfile을 작성해야 하고, `docker build`라는 명령어를 통해서 Image를 만들게 됩니다.

이렇게 자신만의 Image또는 미리 만들어진 Image를 `docker run`라는 명령어를 통해서 Container가 됩니다.

* Dockerfile -> Image -> Container

## Container Registry

위에서 언급한 미리 만들어진 Image를 그대로 사용하기 위해서는 Container Registry라는 개념을 알고있어야 합니다.

Container Registry는 원격 Image 저장소 입니다. Public으로 사용할 수 있는 저장소 중 가장 유명한 [Dockerhub](https://hub.docker.com)라는 것이 있습니다.

다른 사람의 Image를 사용하기만 한다면 Dockerhub에 가입할 필요는 없습니다. 하지만 Dockerhub에 Image를 Upload하기 위해서는 가입해야 합니다.

이러한 Public 저장소가 많이 있지만, 그 중에 GitHub Container Registry를 추천합니다. 최근에 Dockerhub에서 pull 횟수 제한 정책이 발표되면서 찾아보다가 알게되었습니다.

Image를 정의한 Dockerfile을 관리하는 코드 저장소와 Dockerfile을 통해 만들어진 Image를 하나의 도메인에서 관리할 수 있다는 것이 매력적입니다.

사용하는 방법은 [공식 Guide](https://docs.github.com/en/packages/guides/container-guides-for-github-packages)를 보셔도 되고, GitHub Container Registry라는 글을 참조하시면 됩니다.
