---
layout: post
title: "git push --force"
date: 2016-10-01 14:55:00 +0900
categories: VCS
description: 커밋 히스토리 관리를 위한 위험할 수도 있는 실험
image: 16-10/git-push-force1.jpeg
---

git, github을 사용한 지는 꽤 오래되었다. 학교 과제를 진행할 때도 코딩에 시간을 쏟는 것보다 프로젝트 관리에 소모되는 비용이 적지않은 것 같아 사용해왔었는 데 최근에 유명한 오픈소스를 github을 통해 볼 때 커밋 개수가 생각보다 많지 않다는 것에 적지 않은 충격을 계속해서 받아왔었다. 그리고 코어 개발자나 중심을 담당하는 개발자들의 커밋 개수가 생각보다 많지 않다는 사실에 커밋 히스토리 관리를 잘못하고 있다는 느낌이 들었다.

특히 내가 모르는 어떤 점이 있다고 느꼈을 때는 개발자가 pull request(이하 PR)을 올렸을 때 메인테이너로부터 바꿔달라는 요청이 들어오면 "불필요한 지역 변수 삭제"처럼 하나같이 또 다른 단일 커밋 히스토리로 다 남긴다면 커밋 히스토리가 파편화되고 장황해질 가능성이 높은 데 한번 제출된 PR은 살아있으면서 대화 내용은 삭제되지 않고 커밋 개수는 늘어나지 않은 채 수정사항은 반영이 되는 이런 상황이 어떤 과정을 통해 가능한 지 알고 싶었다.

해당 글은 제가 실험적으로 진행해본 하나의 방법이며 피해야할 방법일 수도 혹은 실제 활용되고 있는 방법일 수도 있습니다. git, github, github-flow를 사용한다고 가정하고 진행하도록 하겠습니다. 피드백 환영합니다!

## --force는 사용할만한 가치가 있을 "수"도 있다.

``` bash
$ git push -f <remote-repository> <branch-name>
```

![git-push-force1](/assets/16-10/git-push-force1.png)

PR은 해당 issue를 참조했으며 각 커밋은 커밋 메세지를 통해 이슈 #1에 대한 참조를 "Fixes #1"과 같이 각각 남겼으며 총 3개의 커밋이 존재한다.

![git-push-force2](/assets/16-10/git-push-force2.png)

메인테이너로부터 마음에 들지만 2개의 커밋이 나눠져 있다고 합쳐달라고 하는 상황이라고 하자. 

``` bash
$ git rebase -i HEAD~3
```

``` bash
pick c50e820 Add angular.js 1
squash f6587db Add angular.js 2
pick da90224 Add react.js
```

다음과 같이 `f6584db` 커밋을 squash해서 이전의 커밋 c50e820에 합쳐지도록 한다. interactive 리베이스 진행 후 커밋 히스토리는 다음과 같다.

``` bash
$ git log --oneline

bd0ca01 Add react.js
b308479 Add angular.js (1, 2)
300dac7 Add README.md
```

원래 브랜치에 존재했던 2개의 커밋 모두 해쉬값이 변경되었다. 그리고 angular.js에 대한 커밋은 커밋 메세지도 변화했다.
여기서 옵션없이 `git push`만 실행하면 푸시가 되지 않는데 만약 `-f` 옵션을 통해 동일한 브랜치에 푸쉬를 한다면 PR에 존재했던 커밋과 대화, 이슈에 대한 참조들은 어떻게 처리될 지 궁금했다.

![git-push-force3](/assets/16-10/git-push-force3.png)

이전에 존재했던 커밋 `c50e820`, `f6587db`, `da90224`는 해쉬값이 변경되었으므로 PR에서는 존재하지 않고 코멘트는 남아있다. 그리고 해시값이 변경된 커밋이 푸쉬되어 새로운 커밋으로 인식하므로 미리 남긴 코멘트 이후에 변경 이력이 보이게 된다.

![git-push-force4](/assets/16-10/git-push-force4.png)

이슈로 넘어가면 PR에서 존재하지 않았던 커밋들이 살아있다. 이유는 커밋 메세지를 통해 `Fixes #1`와 같이 참조를 해서 이력이 삭제되지 않고 유지되고 있다는 것을 알 수 있다. 해쉬값이 변경되어 추가된 커밋들도 새롭게 참조가 되고 있다는 것을 알 수 있다. 

해당 글에서는 2개의 커밋을 합치는 작업을 리베이스를 통해 진행했는데 합치지 않고 불필요한 지역 변수 삭제와 같이 추가적인 작업을 하고 해당 커밋은 유지해야하는 작업을 PR 생성 후 해야한다면 커밋 개수는 증가시키지 않고 issue, PR 내부에서 이루어진 대화, 이력은 살아있으면서 히스토리를 관리할 수 있게 된다.

1. `--force` 옵션은 위험한 옵션이라 주의해서 사용되어야 함을 의미
1. github에서만 가능한 시나리오일 가능성이 존재
1. 다른 플로우 에서의 위험성 (`git-flow`에서 `feature`가 규모가 커서 작은 단위로 나눠 진행할 때의 위험성)

실험을 하고 나서 내가 원한 결과는 맞지만 올바른 방법인 지는 확신을 하지 못하겠다.