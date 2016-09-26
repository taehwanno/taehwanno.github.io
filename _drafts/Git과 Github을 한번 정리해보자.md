---
layout: post
title: "Git과 Github을 한번 정리해보자"
date: 2016-09-19 20:27:00 +0900
categories: VCS
description: javascript 2016 컨퍼런스 참가 후기
image: 
---

github을 통해 공개된 저장소들을 구경할 때 드는 생각이 있었다.

- 얘네들은 커밋 개수가 의외로 작네?  
- 커밋이 왜 이렇게 깔끔하지?
- 이 커밋이 어떻게 이렇게 될 수 있지? 한번에 가능한 가?

그리고 pull request에서 해당 커밋 메세지를 수정하고 더 명확히 작성해달라는 메인테이너의 요구 사항은
쉽게 볼 수 있는데 "어떻게 하면 커밋을 불필요하게 추가하지 않고 히스토리를 남길 수 있을까"가 나의 핵심적인 관심사였다.
[소셜 코딩으로 이끄는 Github 실천기술](http://book.naver.com/bookdb/book_detail.nhn?bid=8657208)과 
[https://git-scm.com/book/ko/v2](https://git-scm.com/book/ko/v2) 2가지를 주로 참고했으며 구글링을 통해 정리했다.

## commit을 조작하자
`HEAD`, `Stage`, `Working Space` 3가지를 이해하지 않고는 자유롭게 사용할 수 없다.  
지금 당장 `git reset`의 `hard`, `mixed`, `soft`의 차이를 설명할 수 있는가?

## rebase를 피하지 말고 한번 파고 들어보자

``` bash
$ git rebase -i HEAD~2
```


## Github은 URL을 통해 활용 가능하다
[Ruby on Rails](https://github.com/rails/rails) 저장소를 기준으로 합니다.

1. 브랜치 사이의 변경 내역 확인  
4-0-stable 브랜치와 3-2-stable 브랜치의 변경 내역을 확인하고 싶을 때 다음과 같이 URL에 브랜치 이름을 명시해준다.  
[https://github/rails/rails/compare/4-0-stable...3-2-stable](https://github/rails/rails/compare/4-0-stable...3-2-stable)

2. 특정한 기간 전부터의 변경 내역 확인  
master 브랜치에서 7일 동안 변경된 내역을 확인하고 싶을 때는 다음과 같이 URL에 기간을 적어준다.  
[https://github.com/rails/rails/compare/master@{7.day.age}...master](https://github.com/rails/rails/compare/master@{7.day.age}...master)

기간 지정시 다음과 같은 것들도 이용 가능하며 변경 내역이 너무 많을 때는 최근 변경 내용만 나온다.

- day
- week
- month
- year

### 지정한 날부터의 변경 내역 확인
master 브랜치에 2014년 10월 1일부터의 변경 내역을 보고 싶을 경우에는 다음과 같이 URL에 날짜를 입력한다.

https://github.com/rails/rails/compare/master@{2014-10-01}...master

## Issue
이슈를 관리하는 시스템은 BTS(Bug Tracking System, 버그 관리 시스템) 이라 부른다.
활용 가능한 상황은 다음과 같다

- 소프트웨어에서 버그를 발견해서 보고하려는 경우
- 저장소의 관리자에게 상담 또는 문의를 하는 경우
- 이후에 만들 기능을 적어 두는 경우

## `CONTRIBUTING.md`
https://github.com/blog/1184-contributing-guidelines 읽자

## Commit 메세지로 Issue 조작
이런 것도 가능하다니!!!

## 특정 Issue를 Pull Request로 변환
뭐시라! 이것도 가능하다니!!!

## Pull Request

### Conversation : 댓글 인용은 블록 지정 후 R을 눌러준다.
### Files Changed : 공백 변경에 대한 내용 표시 무시는 URL에 `?w=1`을 추가한다. 
### 개발 도중에도 토론을 위한 Pull Request 보내어 활용할 수 있다.
개발 중 이라는 것을 알리는 방법은 PR 제목 앞에 `[WIP]` (Working In Progress)를 접두어로 붙혀준다

## Github과 연계되는 툴과 서비스
1. [hub 명령어](https://hub.github.com/)
2. [Travis CI](http://travis-ci.com/)
3. [Coveralls](https://coveralls.io/) : Travis CI 또는 Jenkins와 같은 지속적 통합 서버로 실행된 자동 테스트를 정리해서 보여준다.
4. [Gemnasium](https://gemnasium.com/) : Github 저장소의 소프트웨어가 이용하고 있는 루비젬(RubyGems) 또는 npm을 확인하여 최신 버젼의 모듈을 사용해 개발되었는지 알려 주는 서비스
5. [Code Climate](https://codeclimate.com/) : 코드 분석 리포트 서비스
6. [Jenkins](https://jenkins.io/) : 지속적 통합 서버

## Github를 사용하는 경우의 개발 진행 과정

### Github Flow
항상 Deploy 상태를 유지하므로 배포(deploy)라는 개념이 희석되어 없어진다.
다음과 같은 전제조건이 붙는다.

1. Deploy 작업 자동화
도구를 활용한다. Deploy, rollback이 가능해야하며 개발자 이외의 팀원(디자이너, 퍼블리셔 등)도 원래 상태로 되돌릴 수 있게 하는 것이 포인트
Deploy 작업 진행 중에 다른 Deploy 작업을 못하도록 해주는 것이 문제 발생 가능성을 낮춰준다.
2. 테스트 자동화
