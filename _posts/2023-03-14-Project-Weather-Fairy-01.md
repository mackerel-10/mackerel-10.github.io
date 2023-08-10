---
layout: post
title: 'Weather Fairy #1 디스코드 봇 만들기'
categories:
  - Project
tags:
  - JavaScript
date: '2023-03-14'
---

> - 프로젝트 이름: 날씨 요정(Weather Fairy)
> - 목적: Discord Bot을 만들어보고 싶은게 가장 컸다. 그리고, 봇을 이용해 사용자에게 날씨를 알려주기로 했다.
> - 스킬
>   1. TypeScript
>   2. 캐시 기능

<br>

### 🤖 디스코드 봇 만들기

1. Discord Bot을 생성해보자.
   Discord Developer Portal에 접속한 후 봇의 세부사항을 작성한다.
   ![봇 세부사항 작성](/assets/img/TIL10_02.png)

2. OAuth2 > Url에서 서버에 봇을 초대할 수 있는 링크를 생성해주었다.
   링크를 생성하기 전 SCOPES에서 꼭 bot을 선택해야 한다.
   ![OAuth2 URL Generator](/assets/img/TIL10_01.png)

3. Bot 탭에 들어가서 토큰을 리셋해줘야 한다.
   생성한 토큰은 언제 쓸지 몰라 환경변수에 우선 저장.
   그 후, 생성된 링크를 통해 원하는 디스코드 서버로 봇을 초대할 수 있다.
   ![봇 서버로 초대하기](/assets/img/TIL10_03.png)

---

## Reference Link

1. [디스코드 봇 만들기(1) - 봇생성](https://scvtwo.tistory.com/196)
