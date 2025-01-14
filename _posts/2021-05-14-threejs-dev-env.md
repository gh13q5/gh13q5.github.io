---
title: "three.js 개발환경 구축과 첫 번째 예제 제작"
author: gh13
date: 2021-05-14 12:13:00 +0800
categories: [Web, three.js]
tags: [three.js, javascript 5]
render_with_liquid: false
image:
  path: /assets/img/post_img/2021-05-14-04.PNG
---

> 해당 게시글은 `three.js로 3D 그래픽 만들기 2/e`를 공부하며 작성했습니다.
{: .prompt-info }

## 오늘의 공부

- Three.js 개발 환경 구축
- 첫번째 예제 제작하며 Three.js 접해보기  

<br/>

## Three.js 개발 환경 구축

기존에 사용하던 이클립스와 톰캣 서버를 사용하려 했지만, 대체 어디서 자꾸 오류가 나는건지... jsp 파일이 아닌데 dynamic project를 만들어서 그랬던 걸까? 꽤 애를 먹었다... 3시간을 씨름한 것같다. 무슨 공부든 시작할 때마다 개발 환경 구축에서 제일 애를 먹는다. 해결법도 천차만별이라 더 헷갈리는 것같애... ㅋㅋ

### 폴더 생성

![Create Folder](/assets/img/post_img/2021-05-14-01.png){: width="972" }

html 파일을 넣을 폴더와 [three.js 공식 홈페이지](https://threejs.org/)에서 다운받은 Three.js 파일을 넣어줄 libs 폴더를 생성했다. 이클립스의 설정을 따라 html 대신 WebContent로 폴더 이름을 짓기도 하더라. 나도 다음에는 바꿔서 생성해봐야겠다. Three.min.js를 많이 쓴다곤 하지만 책에서는 디버깅 때문에 Three.js를 사용한다고 한다. 이후 프로젝트에서는 Three.min.js로 다운받아 해야할라나.  

### 크롬 보안 설정 비활성화

![Chrome Config](/assets/img/post_img/2021-05-14-02.png){: width="972" }

로컬 파일에 직접 접근이 가능하도록 따로 크롬 바로가기를 하나 더 만들어서 --disable-web-security를 추가해줬다. 하도 안 됐어서 해본 거긴 한데... 그냥 크롬 브라우저로도 잘 열리는 것같고? 잘은 모르겠다.  

### 텍스트 에디터로 html 코드 작성

html이라 이클립스를 사용할까 했지만 오늘 많은 우여곡절이 있었기에, Sublime Text Editor를 사용했다. 기존에 unity 코드나 lua 스크립트 편집 때 잘 쓰긴 했던 프로그램이라 역시 무난하고 좋다. 일단 어두운 배경에 UI가 깔끔해서 만족스럽다.  

<br/>

## Example01 작성 (Three.js 접하기)

- 필수가 되는 scene, camera, renderer 생성
- 2D 평면 객체와 3D 객체 생성
- 물질(material)과 광원, 그림자 추가
- fps 그래프와 애니메이션 추가(회전과 바운싱)
- dat.GUI 라이브러리로 애니메이션 실시간 조절
- 브라우저 크기에 따른 결과물 크기 조정  

![Example Gif](/assets/img/post_img/2021-05-14-03.gif){: width="972" }

객체 생성 때 new function(){ ... }을 사용했다가 중괄호 뒤에 ;를 안 붙여서 골치 아파 죽는줄 알았다. 객체를 생성할 때 사용했음에도 function이라서 자연스레 잊었나 보다.  

<br/>

## 마치며

이런 개발일지를 처음 써봐서 어떻게 써야할지... 원래 일기도 잘 안 쓰던 사람이라 감도 잘 안 오고 어렵다... ㅋㅋㅠ 그래도 써보기로 한 거 잘 써봐야지. 대부분의 공부는 필기노트에 하고 있으니 여기에는 내가 골치 아팠던 문제들과 진행상황에 대해 적는 게 좋을려나? 너무 처음 단계라 쓸 말이 없는 것도 같고. 나중에 더 공부가 되면 프로젝트와 관련해서 이런저런 말도 쓸 수 있을테다. 아직은 일지 형식도 중구난방, 내용도 중구난방 엉망진창 우당탕이지만 힘내보겠다.
 
