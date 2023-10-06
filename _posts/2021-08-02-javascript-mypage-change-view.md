---
title: 마이페이지 탭에 따른 화면 전환
author: gh13
date: 2021-08-02 13:38:00 +0800
categories: [Web, 프론트엔드]
tags: [javascript 5]
render_with_liquid: false
image:
  path: /assets/img/post_img/2021-08-02-02.PNG
---

## 제작 중인 기능

- 마이 페이지에서 좌측 메뉴 클릭할 때마다 display 전환
- 메뉴 하나하나 html+css 구현해주기

![mypage result gif](/assets/img/post_img/2021-08-02-01.gif){: width="972"}

<br/>

## 페이지 전환 기능

자바스크립트로 오른편의 div가 안 보이게 `display="none"` 처리해줬다가, 클릭하면 그 div만 display 보이도록 해줬다. active 클래스에 대한 이해가 조금 부족한 것같아서 따로 알아보는 게 좋을 듯.

```html
<script>
	//오른쪽 content 페이지 변환 스크립트
	function openMenu(evt, page){
		var i, content, clickItem;

		//content 내용들 일단 전부 안 보이게
		content = document.getElementsByClassName("content");
		for (i = 0; i < content.length; i++)
			content[i].style.display = "none";

		// menuItem 다 가져온 후, active된 클래스 삭제
		clickItem = document.getElementsByClassName("menuItem");
		for (i = 0; i < clickItem.length; i++)
			clickItem[i].className = clickItem[i].className.replace(" active", "");

		// 현재 클릭한 메뉴를 보여주며 active 클래스에 추가해주기
		document.getElementById(page).style.display = "block";
		evt.currentTarget.className += " active";
		}

	//최초 실행 시 디폴트로 열린 페이지 지정 (마이 프로필)
	document.getElementById("defaultOpen").click();
</script>
```
<br/>

## 앞으로 고민해볼 일

웹을 오랜만에 다뤄서 그런지 버벅거리느라 바빴다. (ㅋㅋ) UI 생각하고 CSS 구획하는 게 제일 힘든 것같고... ㅠ 고민할 일이 산더미다.

- 이렇게 자바 스크립트로 전환하는 게 최적화에 제일일까?
- <img>가 들어간 div를 처리할 때면 로딩시간이 걸리는데, 나중에 db에서 이미지를 가져와 띄울 때는 더 로딩시간이 걸리게 되는 거 아닐까...
- 최대한 겹치는 것들은 같은 CSS 쓰도록 했는데 CSS가 너무 길어져서... 이렇게 길게 써도 되는가; 따로 CSS 문서를 빼야 좋을라나?

