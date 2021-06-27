---
layout: post
title: <React> react-router-dom 리액트 라우터
categories: React
tags: [React, react-router-dom]
---

### 리액트 라우터란 ?
React 는 SPA 이기 때문에 말 그대로 페이지가 하나인 어플리케이션

페이지가 하나이기 때문에 페이지 이동이 불가능한데 페이지가 하나인 경우에도 일반 웹 사이트처럼 URL 에 따른 페이지 이동을 해주는게 `React Router` 이다


### SPA란 ?
- **S**ingle **P**age **A**pplication (싱글 페이지 어플리케이션) 의 약자 
- 페이지가 1개인 어플리케이션 

![img](https://media.vlpt.us/images/cyongchoi/post/a855f02f-28d7-4494-8f45-f987da4e2707/main-qimg-6fed27dbf163e2f518b369d5e25bcfc4.png)

#### 기존 웹 서비스 vs SPA 웹 서비스

**기존 웹 서비스**
- 요청 시마다 서버로부터 데이터 해석 후 화면에 랜더링 
- 화면 이동시 화면 이동에 필요한 HTML을 서버에서 받아서 처음부터 다시 로딩 하기 때문에 시간이 많이 걸림 
  - 클라이언트가 서버에게 HTML, CSS, JS 등 화면에 보여줄 데이터를 요청하고 서버는 요청된 파일을 뿌려줌 

**SPA 웹 서비스**
- 브라우저에 최초에 한번 페이지 전체를 로드하고, 이후로부터는 특정 부분만 Ajax를 통해 데이터를 바인딩 
- 화면 구성에 필요한 모든 HTML을 클라이언트가 가지고 있고 서버사이드에는 필요한 데이터를 요청 후 JSON으로 받기 때문에 화면을 구성하는 속도가 빠름 
  - 처음 브라우저 들어왔을 때 HTML 을 뿌려주고, 클라이언트가 필요한 부분만 서버에 요청해서 업데이트 

#### 장점 , 단점 

**장점**
1. 하나하나 화면 전체를 렌더링할 필요가 없기 때문에 화면이동이 빠르다.
2. 화면에 필요한 부분의 데이터만 받아서 렌더링 하기 때문에 처리과정이 효율적이다.
3. 유저에 입장에서 사용하기 편리하다.

**단점**
1. 처음 화면을 로딩할 때, 모든 화면이 미리 준비되어 있어야 하기 때문에 로딩에 시간이 걸린다.
2. 어플리케이션을 구현하는데 보다 시간이 걸리며 복잡하다.
3. 앱의 규모가 커지면 자바스크립트 파일이 커짐 (페이지 로딩시 방문하지 않을 수도 있는 페이지도 로딩 하기 때문에)
   => 코드 스플리팅(Code Splitting)으로 해결 

### react-router 종류 
- react-router - 웹&앱
- react-router-dom - 웹
- react-router-native -앱

세 가지는 종류가 다른데 오늘은 웹에 사용하는 react-router-dom 에 대해서 알아보려고 한다 

### React-router-dom 설치 

```react
npm install react-router-dom
```

### React-router-dom 사용하기 

**App.js** 

```react
import React, { Component } from 'react';
import { BrowserRouter, Route, Link, Switch } from 'react-router-dom';

function App() {
  return (
    <div className="App">
      <BrowserRouter>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
        </ul>
        <Switch>
          <Route path="/" component={Auth(LadingPage, null)}/>
        </Switch>
      </BrowserRouter>
    </div>
  );
}
```

- BrowserRouter : HTML5의 history API를 활용하여 UI 업데이트
- Route : 요청받은 pathname에 해당하는 컴포넌트를 렌더링
- Link :  'a'태그와 비슷합니다. to속성에 설정된 링크로 이동, 기록이 history스택에 저장
- Switch : 자식 컴포넌트 Route또는 Redirect중 매치되는 첫 번째 요소를 렌더링, Switch를 사용하면 BrowserRouter만 사용할 때와 다르게 **하나**의 매칭되는 요소만 렌더링한다는 점을 보장, path와 매칭되는 Route가 없을 때에 맨 밑에 default Route의 실행이 보장

#### exact

- Switch는 매칭되는 제일 첫번째의 Route를 선별하는데, 첫번째 Route의 path에 전달한 "/"는 모든 요청에 매칭하기 때문에 항상 처음 컴포넌트만 렌더링
- exact 속성을 추가하면 pathname 과 정확히 일치할 때만 Route 를 매칭 

### 레퍼런스
[react-router :: 1장. 리액트 라우터 사용해보기](https://velopert.com/3417)<br>
[[React] react-router-dom 시작하기](https://velog.io/@kwonh/React-react-router-dom-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)<br>
[SPA(Single Page Application)에 기초 개념](https://velog.io/@josworks27/SPA-%EA%B0%9C%EB%85%90)





 

