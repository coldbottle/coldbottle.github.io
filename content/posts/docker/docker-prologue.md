---
author: "coldbottle"
title: "도커(Docker) 프롤로그"
date: 2021-01-26
draft: false
tags: ["docker", "devops", "mlops"]
categories: ["docker"]
series: ["docker"]
ShowToc: true
TocOpen: true
weight: 1
---

도커를 설명할 때 실행환경 가상화라고 한다.

Computer Science에서 있었던 가상화/추상화를 위한 흐름을 살펴보면, 도커(Docker)가 무엇인지와 각광받는 이유를 알 수 있다.

Computer Science가 전공이 아닌 독자들도 이해할 수 있게 설명하기 위해서 단편화된 내용들로 구성 하였습니다.

## Virtual Memory

질문으로 시작해보자! `OS가 없는 상황에서` 프로그램을 개발할 때 고려해야 하는 것은 무엇일까?

이 질문에 대답하기에 앞서 OS가 하는 역할에 대해서 알면 위의 질문에 답을 할수 있을 것 같다.

OS는 Computer의 자원(CPU, Memory, Storage등)을 관리하는 역할을 하는 S/W이다.

그러므로 OS가 없는 상황에서 프로그램을 개발할 때는 프로그램의 Logic뿐만 아니라 Computer의 자원을 관리하는 Code도 구현해야 한다.

그리고 OS에는 수 많은 프로세스가 동작하게 된다. 이 프로세스들은 서로 다른 S/W Engineer들의 의해서 개발된 프로그램들이 동작하고 있는 상태이다.

전제 조건이 틀릴 수도 있지만, `OS가 없는 상황`에서 여러 다른 프로그램들이 사용하는 자원을 고려하면서 프로그램을 개발하는 것은 매우 어려운 일이다.

그렇다면 OS는 어떻게 여러 프로세스(프로그램이 실행중인 상태)가 사용하는 Memory를 어떻게 관리하는 것일까?

핵심은 2개의 문장으로 요약할 수 있다.

* 물리적 Memory가 얼마나 있든 상관없이 각 프로세스에게 똑같은 4GB(32bit OS기준)의 메모리가 있는 것처럼 보여준다.
* 프로세스가 사용하는 가상의 Memory주소를 물리적 Memory주소로 바꾸는 작업을 하고, 이 Mapping을 관리한다.

OS는 프로세스에게 Memory라는 자원을 추상화한 것이다. 이것을 Virtual Memory라고 한다.

Virtual Memory의 핵심은 프로그램을 개발할 때 Memory관리하는 Code가 필요없게 만든 것이다.

## Virtual Machine

질문으로 시작해보자! `여러 OS에서 배포되는` 프로그램을 개발할 때 고려해야 하는 것은 무엇일까?

여러 OS에서 배포되기 위해서는 특정 OS와 관련된 것을 사용해서 프로그램을 개발하면 다른 OS에서는 동작하지 않게된다.

즉, 특정 OS와 관련된 것을 사용하면 안된다.

하지만 꼭 사용해야 한다면 이것을 어떻게 해결 할 수 있을까?

기술은 간단하지 않지만 개념적으로 배포 환경의 OS가 무엇이든 그 OS위에서 개발할 때 사용했던 OS를 설치하고 개발한 프로그램을 실행하면 된다.

Virtual Machine은 Machine을 추상화 하는 것으로 기존 Computer에 설치된 OS(Host)위에 OS(Guest)를 설치할 수 있게 만든다.

즉, Virtual Machine의 핵심은 프로그램을 개발하고 배포할 때 생기는 OS의존성 문제를 해결해 주는 것이다.

이렇게 하면 특정 OS에서 개발한 프로그램이 동작하는 데는 문제가 없다.

하지만, 큰 단점이 하나 발생하게 된다.

예를들어서 문자열을 적고 저장하는 프로그램이 아래와 같이 있다고 가정하자.
```python
with open("/path/to/file", "w") as f:
    f.write("Hello World!")
```

이 프로그램이 Host OS에서 동작하면

Python Code -> OS(Host) Code -> Physical Disk

위와 같은 경로로 물리적인 Disk에 저장될 것이다.

하지만 이 프로그램이 Guest OS에서 동작한다면?

Python Code -> OS(Guest) Code -> Virtual Disk -> OS(Host) Code -> Physical Disk

즉, OS Code가 두번 실행 되게 된다. (Guest, Host)

단편적인 예시지만 OS Code가 두번 실행되는 것이 계속되면 CPU Resource가 낭비되게 된다. 그리고 느려진다.

## Docker

질문으로 시작해보자! Virtual Machine의 단점인 `OS Code가 두번 실행` 되는 것을 없앨 수는 없을까?

간단하게 답하면, OS를 공유하면 된다.

그래서 나온 기술이 Container라는 기술이다.

Container라는 기술은 OS를 Share하면서 프로세스가 동작해야하는 실행 환경까지만 가상화 한것이다.

그래서 실제로 같은 프로그램을 Host OS에서 실행했을 때의 성능과 Container에서 실행했을 때의 성능은 차이가 거의 없다.

그렇다면 Docker는 무엇일까?

Docker는 Container기술을 사용할 수 있게 만든 도구중 가장 유명한 도구이다.

그래서 대부분 Docker하면 Container기술을 지칭하는 대명사가 되었다.

## 결론

필자가 생각하는 Docker(Container)가 각광받는 이유는 배포하는 환경이 개발환경과 다르지 않게 하면서도 성능은 떨어지지 않게 만드는 기술이기 때문이다.

Docker의 각종 명령어 및 Dockerfile 문법들을 배우기 전에 Docker가 나왔던 배경에 대해서 간단하게 적어보면 배우는데 도움이 되지 않을까 생각해본다.
