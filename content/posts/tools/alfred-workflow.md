---
author: "coldbottle"
title: "Alfred Workflow"
date: 2021-02-15
draft: true
tags: ["tool", "automation", "workflow"]
categories: ["tool"]
series: ["tool"]
ShowToc: true
TocOpen: true
weight: 903
---

Mac을 사용하면서 [Alfred](https://www.alfredapp.com)는 매우 중요한 tool로 자리잡았습니다.

이미 많은 블로그에서 "Mac을 사용하면 필수적으로 설치해야 하는 Software N선"에 많이 언급이 되었기 때문에

이 글에서는 Alfred의 workflow라는 것을 만드는 과정에서 겪었던 시행착오를 적어보았습니다.

Alfred workflow를 사용하기 위해서는 **Power Pack**을 구매해야합니다.

### Alfred Workflow

Google에 "hello world"를 검색하기 위해서는

Safari를 열고, 주소창에 google.co.kr을 입력하고, "hello world"를 검색합니다.

이러한 작업이 반복되지 않는다면 Workflow를 사용하지 않아도 되지만 반복이 된다면 이 작업을 Workflow를 사용해서 생산성을 향상시킬 수 있습니다.

요약하면, Alfred Workflow는 반복되는 작업을 command하나로 실행 할 수 있게 도와줍니다.


### Awesome Workflow

만들기 전에는 항상 다른 사람들이 만들어 놓은 것이 있는지 확인하면 중복 개발을 피할 수 있고 나의 시간을 아낄 수 있습니다.

아래의 링크들은 이미 만들어진 workflow들을 모아둔 곳 입니다.

* [alfred forums](https://www.alfredforum.com/forum/3-share-your-workflows/)
* [github](https://github.com/zenorocha/alfred-workflows)

이미 만들어진 Workflow가 없거나 Workflow가 마음에들지 않은 경우에는 직접 만들어야 합니다.

### Workflow example

#### create worfklow

뭐뭐 클릭

아래를 입력
```
```

Alfred에서 목록으로 보여지고 싶다면 아래와 같은 json구조로 print해야 한다.
``` json
{
    items: [
        {
            "title": "",
            "subtitle": "",
            "arg": "",
        },
    ]
}
```

* title: alfred 목록에서 큰 글씨로 표현되는 값이다.
* subtitle: alfred 목록에서 작은 글씨로 표현되는 값이다.
* arg: 다음 step의 query에 전달되는 값이다.
더 자세한 정보는 [여기](https://www.alfredapp.com/help/workflows/inputs/script-filter/json/)에서 확인할 수 있습니다.

실행 화면

#### how to debug

오른쪽 위 벌레 버튼을 누르면 아래에 console이 나타난다.

꼭, stderr로 out해야 한다.

* bash

  ```bash
  >&2 echo -n "hello world"
  ```

* python

  ```python
  sys.stderr.write("hello world!")
  ```

#### environment variable

Static하면서, 사람마다 달라진 variable의 경우
여기를 누르고 지정
export시 주의

#### workflow variable

바로 다음 step에서 사용하지 않지만, 나중에 사용될 경우에

#### python package

python으로 workflow를 만들 수 있다. 하지만, python2.7 기반이기 때문에

* EOL(End Of Life)
* pip install이 잘 안된다.
* https 접근이 어려울 수 있다.

#### Tip

scratch부터 만들지 말고 다른 사람의 workflow중 필요한 부분은 copy & paste 한뒤에 수정.
