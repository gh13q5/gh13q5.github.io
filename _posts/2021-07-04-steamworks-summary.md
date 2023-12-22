---
title: "SteamWorks 간단 정리 및 개요"
author: gh13
date: 2021-07-04 21:34:00 +0800
categories: [CS (Computer Science), Memo]
tags: [SteamWorks, Tech]
render_with_liquid: false
image:
  path: /assets/img/post_img/2021-07-04-01.png
---

> 기존에 정리했던 문서를 옮겨 왔습니다.
{: .prompt-info }

## SteamWorks

> SteamWorks 페이지: <https://partner.steamgames.com/>
{: .prompt-info }

`게임을 Steam과 연동시킬 수 있는 API.` Steam에 게임 빌드 결과물을 올리고 상점 페이지를 만들기 위해선, SteamWorks에 기존 스팀 계정을 이용해 로그인한 후 계좌 정보 등을 연동해서 개발자 계정으로 등록하는 과정이 필요하다.  
  
스팀 클라우드
: 게임의 세이브 파일을 스팀 서버에 저장할 수 있게 해줌 (동기화 필수)  

스팀 도전 과제 / 스팀 리더 보드
: 게임 내 순위나 점수가 필요할 경우에 리더 보드 사용 가능  

스팀 커뮤니티 연동
: 스팀 자체의 친구 기능으로 게임 내 친구 창을 대신할 수 있음  

DRM
: 스팀에서 자체적으로 게임의 불법 복제나 접근을 방지해줌  
  
이외에도 멀티플레이 매칭, 스팀 PC방 등 많은 기능들을 제공하고 있다.  

<br/>

## SteamWorks SDK

개발자 계정 등록을 하지 않고도 SteamWorks 페이지에서 무료로 다운로드가 가능한 SDK. SteamWorks API에 구현된 추상 클래스들을(위에 서술한 기능) 이용할 수 있다. [SteamWorks 문서](https://partner.steamgames.com/doc/api)에서 API에 구현된 클래스 정보와 사용법, 추가 정보 등을 확인할 수 있다. (한국어 지원 중) 전체 게임 제작 후, SDK와 관련된 코드들을 추가하는 방식으로 사용한다.  

<br/>

## SteamWorks SDK와 Unity 연결

SteamWorks SDK는 기본으로 C와 C++를 지원하기 때문에 C#을 사용하는 유니티에 대한 지원이 부족한 편이다. C#으로 포팅한 기존 SteamWorks SDK 라이브러리는 사용법에 어려운 점이 많아서, [Facepunch 오픈 라이브러리](https://github.com/Facepunch/Facepunch.Steamworks)를 사용하는 것이 좋다. 실제로 사용법이 매우 쉽고, 예시 코드도 같이 제공되고 있다.

<br/>

## SteamWorks 개발자 계정 등록 및 상점 페이지 등록 과정

스팀 계정 생성 후, 그 스팀 계정을 이용해 SteamWorks에 로그인이 가능하다.

![Steamworks login page](/assets/img/post_img/2021-07-04-02.png){: width="972" }

단, 개발자 계정 생성 시 게임 등록 수수료 100달러(한화 약 11만원)를 지불해야 한다. 게임 1개를 등록할 때마다 지불하며, 일정 수준 이상의 판매수익을 달성하면 수수료는 판매자에게 돌려준다. 계좌 정보 기입, 비미국인 개발자를 위한 세무서, 사업자 등록까지 진행하면 계정 생성이 완료된다. 사업자 등록 관련은 필수가 아니라는 의견도 많아서 이후 실제로 계정 등록을 진행할 때 더 알아봐야 할 듯 싶다.  
  
게임 등록 수수료 지불 후 스팀에서 개발자에 대한 정보를 검토하기 때문에 최소 한 달 후에 게임 출시가 가능해진다. `출시 2주 전부터는 출시 예정 페이지를 게시`해야 하며, 개발자가 출시 전에 반드시 해야 하는 체크리스트가 형성된다.  

### 게임 빌드에 관한 체크리스트

- 디포 구성 1개 이상  
- 빌드 구성 1개 이상  
  + 빌드 관리 항목 중 SteamPipe에서 관리. .exe실행 파일을 1기가 미만의 zip로 등록.  
- 시작 옵션 정의 완료  
- 설치 디렉터리 설정  
- 디포 언어 구성 완료  
- 상점 및 Devcomp 패키지 설치  
- 패키지에 Windows 디포 포함

### 상점 페이지에 관한 체크리스트

- 게임 기본 정보 및 설명 작성  
  + 영어 포함 25개국 언어로 전부 작성해줘야 함… (영어로 대신 할 순 있다고 함)  
- 성인 콘텐츠 설정  
- 자체 등급 평가 질문서  
- 계획된 출시 날짜  
- 시스템 요구 사항  
  + 게임 구동 최소 사양, 권장 사양  
- 패키지 가격 책정 1개 이상  
- 게임 스크린샷 5개 이상  
- 캡슐 이미지  
  + Steam 상점 페이지에 뜨는 이미지 (게임 이름이 들어가 있어야 함)  
- 라이브러리 자산  
- 고객지원 정보  
- 개발자 및 배급사 이름  
- 상점 페이지 설명 태그  
- 예고편 업로드 완료  
- 앱 구성

### 커뮤니티 관련 체크리스트

- 커뮤니티 캡슐  
- 커뮤니티 아이콘  
- 클라이언트 아이콘

### 필수는 아니지만 추천하는 항목 관련 체크리스트

- Steam 클라우드 저장 항목  
- Steam 도전 과제  
  
상점 페이지에 등록 후, 불특정 다수가 참여하는 데모 테스트도 진행할 수 있다.

<br/>

## Steam 클라우드 세이브

### Steam Auto-Cloud

SteamWorks 개발자 계정을 만든 후, Steamworks 홈페이지에서 관련 설정들을 만져주는 것만으로도 Steam Cloud를 사용할 수 있다. 별도의 코드 작성이나 게임 수정이 필요 없어서 매우 편리하다. 클라우드에 저장하려는 파일만 지정하면, Steam이 게임이 시작되고 종료될 때 해당 파일을 자동으로 동기화시킨다. 이 때, `steam\_autocloud.vdf` 파일이 별도로 생성될 수 있는데 게임에 지장이 가는 건 아니다.

### Steam Cloud API

별도로 저장할 데이터를 지정해줘야 한다면 API를 사용해 저장한다. 게임에 클라우드 기능을 통합하는 과정과 코드 작성이 필요하다. 플레이어 별로 파일을 분류하기 쉽고, 개발자가 세밀하게 제어할 수 있는 부분이 많아진다.

<br/>

## 참고하기 좋은 사이트

- [FacePunch 개발자 깃허브](https://github.com/Facepunch/Facepunch.Steamworks)
  + FacePunch.Steamworks에 사용되는 코드 제공 (친구 리스트 출력, 클라우드, 서버 등)
- [FacePunch Wiki](https://wiki.facepunch.com/steamworks/)
  + FacePunch.Steamworks에 사용되는 함수와 변수의 설명 제공
- [Steamworks SDK 공식 문서](https://partner.steamgames.com/doc/home)
  + Steamworks SDK의 알고리즘과 사용 예시 등 참고하기 좋음
- [FacePunch 관련 블로그](https://onewheelstudio.com/blog/2020/12/3/steam-workshop-with-unity-and-facepunch-steamworks)
  + FacePunch.Steamworks 위키의 부족한 내용을 채워 설명해줌 (유튜브 링크도 제공)

