---
layout: post
title: "Git을 한번 정리해보자"
date: 2016-09-21 01:36:00 +0900
categories: VCS
description: 
image: 
---

{% highlight bash linenos %}

{% endhighlight %}

Pro Git 2/E

``` bash
$ git reflog
```
브랜치와 `HEAD`가 지난 몇 달 동안에 가리켰었던 커밋을 볼 때 사용한다.  
`git log`의 경우 현재 브랜치에 대한 로그만을 조회한다.

``` bash
$ git show HEAD@{5}
```

``` bash
$ git show master@{yesterday}
```

이름 끝에 `^`를 붙이면 Git은 해당 커밋의 부모를 찾는다. `HEAD^`는 'HEAD의 부모'를 의미하므로 바로 이전 커밋을 의미하며
`d921970^2`는 `d921970`의 두 번째 부모를 의미한다.

``` bash
$ git log master..experiment
```
experiment 브랜치의 커밋들 중에서 아직 master 브랜치에 merge하지 않은 것을 조회  

``` bash
$ git log experiment..master
```
experiment 브랜치에는 없고 master 브랜치에만 있는 커밋을 조회

``` bash
$ git log origin/master..HEAD
$ git log origin/master..
```
origin 저장소의 master 브랜치에는 없고 현재 Checkout 중인 브랜치에만 있는 커밋 조회

``` bash
$ git log master..experiment
```
master와 experiment 브랜치의 공통부분은 제외한 커밋 조회

``` bash
$ git add -p
$ git reset -p
```
파일의 일부분만 stage하고 unstage 하기

## 7.5 검색
``` bash
$ git grep
$ git grep -n (line number 포함하여 표시)
$ git grep --count (개수만 표시)
$ git grep -p (함수 혹은 메소드 검색)
$ git grep $0 --and $1 (조합)
```

``` bash
$ git log -S (문자열 검색)
$ git log -L (함수나 한 라인의 히스토리 검색)
$ git log -L :function_name:filename.extension
```

## 7.6 히스토리 단장하기

1. 커밋들의 순서 변경
2. 커밋 메세지와 커밋한 파일 변경
3. 여러 개의 커밋을 하나로 합침
4. 하나의 커밋을 여러 개로 분리
5. 커밋 전체 삭제

**이 모든 것은 다른 사람과 코드를 공유하기 전에 해야 한다.**  
```
git commit --amend
```


## 7.7 Reset 명확히 알고 가기

## 7.8 고급 Merge

## 7.9 Rerere

## 7.13 Replace
