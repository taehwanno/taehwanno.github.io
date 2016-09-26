---
layout: post
title: "Angular2 한번 살펴보기"
date: 2016-09-21 17:20:00 +0900
categories: framework
description: 
image: 
---

## Overview

1. Typescript
1. Component Architecture
1. Dependency Injection
1. Binding
1. Angular Zone
1. HTTP
1. System.js and Webpack
1. NPM packages
1. Style

## 해당 글의 가정
ng1을 통해 실제 제품에 도입하여 개발한 경험이 있으며 Typescript 및 ng2에 사용된 기술에는 처음인 제가
ng2 공식 릴리즈 후 조금씩 살펴보면서 정리한 내용입니다. 빠르게 살펴보면서 정리했으므로 잘못된 내용이 포함되어 있을 가능성이 존재합니다.
피드백 주신다면 검토 후 바로 수정하도록 하겠습니다.

### [Typescript](http://typescript.com/)

#### javascript의 Superset이며 js의 단점을 보완하기 위해 MS가 개발하고 있는 언어  
하지만 ng2 학습할 때와 제품 도입 고려시 장벽이 될 가능성이 매우 높다. ng2에서 지원하는 3가지 언어에서 주언어로 
밀고 있기에 stackoverflow 및 구글링했을 떄 참조할 수 있는 소스코드들이 Typescript로 작성될 가능성이 높기에 아래 5가지만 
[Typescript Document Handbook](http://www.typescriptlang.org/docs/handbook/interfaces.html)에서
살펴보고 초기 학습시에는 ng2를 살펴보는 것에 집중하는 게 좋다고 생각한다.

  - Interfaces, Classes
  - Functions
  - Generics
  - Decorator
  - Module & Module Resolution
  
[JSCONF:16](https://taehwanno.github.io/post/javascript/2016/09/jsconf-16)에서 다수의 개발자가 공감했듯이
java스러워 지고 있다는 느낌은 혼자 느끼는 기분이 아닌 듯하다.
  
### Component Architecture
- Style
  - 디자이너와의 협업시 활용 가능한 점


#### 읽을 자료
 -  Google 검색 : "Why react.js forgive MVC pattern related to component architecture?"
   - [Why you might not need MVC with React.js - The Code Experience](http://www.code-experience.com/why-you-might-not-need-mvc-with-reactjs/)
   - [React.js vs traditional MVC (Backbone, Angular) - The Code Experience](http://www.code-experience.com/react-js-vs-traditional-mvc-backbone-ember-angular/)
 - MVC Architecture
   - [MVC Architecture - Google Chrome Developer](https://developer.chrome.com/apps/app_frameworks)
 - Web Components
   - [WebComponents.org](http://webcomponents.org/)
   - [웹 컴포넌트 - Naver D2](http://d2.naver.com/helloworld/188655)
   - [Web Components - MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components)
   - [html5rocksko](http://html5rocksko.blogspot.kr/2014/02/mashup-web-component-evolution-of-web-development.html)
   
### Dependency Injection

#### "`$injector`가 하나만 존재하는 것이 아닌 `tree` 형태를 이룬다."
[ng2 guide의 hierarchical-dependecy-injection](https://angular.io/docs/ts/latest/guide/hierarchical-dependency-injection.html)을
살펴보면 기존에는 하나의 `$injector`가 모든 의존성 주입을 담당했었다면 component tree가 생성되는 것과 비슷한 구조로 `injector` 인스턴스가 생성된다.

> Angular has a Hierarchical Dependency Injection system. There is actually a tree of injectors that parallel an application's component tree. 
> We can reconfigure the injectors at any level of that component tree with interesting and useful results.

비슷한 구조로 인스턴스가 생성된다고 완곡히(?) 표현을 한 것은 실제로는 완벽하게 parallel한 구조로 각 컴포넌트마다 생성이 되는 것이 아니기 때문이며 
모든 컴포넌트가 `injector` 인스턴스를 가지는 것은 맞으나 컴포넌트 간에 공유되어 공동 소유되는 `injector` 인스턴스가 존재할 수 있다.

> Angular doesn't literally create a separate injector for each component. Every component doesn't need its own injector and it would be horribly inefficient to create masses of injectors for no good purpose.

> But it is true that every component has an injector (even if it shares that injector with another component) and there may be many different injector instances operating at different levels of the component tree.

> It is useful to pretend that every component has its own injector.

`injector` 트리가 생성됨으로서 현재 컴포넌트에 연결된 `injector` 인스턴스에게 필요한 의존성을 요청하고 없다면 부모 컴포넌트의 `injector`에게 요청을
하는 방식으로 chaning 과정을 거쳐 원하는 의존성을 주입받게 된다.

`@Component`를 통해 컴포넌트 정의시 `providers`에 넘겨주게 되면 해당 컴포넌트부터 시작하여 자식들까지만 의존성을 주입받을 수 있게 된다. (~~protected?~~) 
의존성 주입 과정을 `injector` 트리 구성을 통해 구현했을 경우 큰 변화라 생각되는 점은 ng1에서 `Service`는 모두 singleton임이 보장이 되었는 데 ng2에서는 알맞은 컴포넌트에
선언함으로서 singleton일 수도 아닐 수도 있다는 사실이다. 즉, 어느 컴포넌트의 정의에 서비스를 `providers`에 선언할 것인가가 핵심이 된다.

### Binding

1. Interpolation `{{ interpolation }}`
2. Property binding `[ property ]`
3. Event binding `( event )`
4. Two-way binding `[( two-way )]`

### [Angular Zone](https://github.com/angular/zone.js)

"3rd party library에 대해 `$rootScope.$apply()` 적용은 필요없다."

### HTTP

"Until ng1 `return Promise;`, Now `return Observable;`"

### System.js vs Webpack

### NPM packages

 - CLI
 - Universal
 - React Native Renderer

