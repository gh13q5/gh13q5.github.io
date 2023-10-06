---
title: JSP 파일에 mySQL DB 연결
author: gh13
date: 2021-08-08 18:57:00 +0800
categories: [Web, 백엔드]
tags: [JDBC, mySQL, Error 기록]
render_with_liquid: false
---

## 오늘 해낸 것

- 기존 EER Diagram로 테이블 생성
- jdbc 이용해서 프로젝트에 DB 연결
- jsp 웹페이지에 db 내용 불러오기

<br/>

## EER Diagram을 DB Table로 가져오기

이건... 내가 MySQL WorkBench만 다운받고 Server나 이런건 안 받아놔서 처음부터 받느라 시간이 오래 걸려버렸다. 뭐든 처음에 잘 다운받아놓자; 하여튼 이것저것 다 받고 3306 서버도 열어놨더니 DB가 없다고 하더라. 뭐가 문제지 싶어서 MySQL Command 창에서 한참 뒤적거려보니 테이블이고 뭐고 DB 생성이 한나도 안 돼있었다. 알고 보니 내가 만진 건 정말 다이어그램 그 자체라 그걸 테이블로 바꾸는 작업이 필요했던 것.

![EER at workbench](/assets/img/post_img/2021-08-08-01.png){: width="972" }

이렇게 변환하고 나니, EER 다이어그램으로 작성한 테이블들이 mydb라는 기본 db를 생성한 후 옮겨져 있었다. 여기에 예제로 쓸 데이터를 INSERT해주면 DB 사용할 준비는 끝이다.

![EER to DB](/assets/img/post_img/2021-08-08-02.png){: width="972" }

<br/>

## jdbc로 프로젝트에 DB 연결

먼저 [MySQL Connector를 다운](https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.18)받아주자. files의 jar파일을 받아주면 된다. jsp 파일 상단에 추가한 코드는 아래와 같다. 물론 `<%@ page language="" ... %>` 구문 다음에 작성해야 한다.

```java
<%@ page import="java.sql.*" %>
<%
	String url = "jdbc:mysql://localhost:3306/mydb?characterEncoding=UTF-8&serverTimezone=UTC";
	String id = "root";
	String pw = "1234";
	
	Class.forName("com.mysql.cj.jdbc.Driver");
	
	//MySQL에 대한 정보 제공 후 connection이 참조
	Connection con = DriverManager.getConnection(url,id,pw);
	
	Statement stmt = con.createStatement();
	
	String sql = "select * from user";
	ResultSet rs = stmt.executeQuery(sql);
%>
```

### java.lang.ClassNotFoundException: com.mysql.cj.jdbc.Driver

`Class.forName(" ... ")`에서 난 오류인줄 알았더라면 더 쉽게 해결했을텐데... 하필 `String pw="..."` 구문에서 오류난다고 떠버린 바람에 더 헤매버렸다. ㄱ- 해결법은 간단하다. 위에서 다운받아준 MySQL Connector 파일을 내 프로젝트의 WEB_INF에 넣어주는 게 아니었다. `사용할 서버에 있는 WEB_INF`에 넣어줘야 하는 거였다.... 나는 톰캣을 사용하고 있었어서 `Tomcat9.0/webapps/ROOT/WEB\_INF/lib`에 넣어줬다.

### java.sql.SQLException: No database selected

쿼리를 생성하는 객체인 Statement에서 발생한 오류다. 간결한 오류인만큼 다행히 해결이 빨랐다. db가 선택 안 된 것같아서 url을 살펴보니 내가 localhost:3306 (MySQL Server 열어줄 때 사용한 포트 번호가 3306) 이후에 어느 db를 선택할 건지 써두지 않았더라. 현재 사용하는 db 이름인 mydb를 뒤에 붙여주니 바로 끝끝.

### 쿼리 생성

일단은 연결이 잘 되나 확인하는 게 우선이기 때문에 샘플 데이터가 들어가 있는 user 테이블만 가져와보기로 했다. sql과 ResultSet으로 쿼리 지정해주기만 하면 된다.

<br/>

## jsp 페이지에 db 불러오기

샘플로 INSERT한 데이터를 jsp로 불러와봤다. ResultSet 쿼리로 가져온 User 데이터에서 nickname column의 String을 가져오는 방식이다.

```html
<table>
	<tr>
		<th id="profile_title" colspan="2">
			<%=rs.getString("nickname") %>
		</th> <!-- user 이름 -->
	</tr>
</table>
```

### java.sql.SQLException: Before start of result set

에러코드만 보면 뭔가 세팅되기 전에 내가 뭔가 시작한 듯... 그리고 진짜로 그랬다; `rs.getString(" ... ")`을 사용하기 전에 `rs.next()`를 이용해서 검사를 한 번 해줘야 했다. while문으로 아예 `<table>` 안의 코드들을 감싸는 형태로 작성해봤다.

```html
<table>
	<% while(rs.next()){ %>
	<tr>
		<th id="profile_title" colspan="2">
			<%=rs.getString("nickname") %>
		</th> <!-- user 이름 -->
	</tr>
    <% } %>
</table>
```

<br/>

## 마치며

MySQL의 설치로 생각보다 시간은 오래 걸렸지만 그래도 생각보다는 수월하게 잘 됐다. db를 너무 오랜만에 다뤘는데 조금 워밍업도 한 것 같고? 앞으로의 관건은 DAO 작성과, DTO 등과의 연결이겠지만... 어떻게든 잘 될 거라 믿는다 아잣~
