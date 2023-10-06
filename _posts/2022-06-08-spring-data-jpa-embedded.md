---
title: "Spring Data JPA의 @Embedded 어노테이션"
author: gh13
date: 2022-06-08 21:13:00 +0800
categories: [Web, Spring]
tags: [Spring Boot, Spring Data JPA, Error 기록]
render_with_liquid: false
---

## 발생한 문제

Spring Boot를 사용해 프로젝트를 진행하는 도중 `no identifier specified for entity spring data jpa` 오류가 발생했다. 찾아보니 보통은 Entity에 @Id를 빼먹었거나 잘못된 패키지를 import해서 발생한다더라. 프로젝트의 Domain들을 전부 살펴본 결과 @Id 문제는 아니었다.

알고 보니 별도의 PK 없이 FK로만 이루어진 테이블도 식별자를 만들어줘야만 Spring Boot가 자동생성할 때 식별이 가능한 것이었다. 그래서 `@Embedded 어노테이션`을 이용해 별도의 식별자 클래스를 만들어줬다.

<br/>

## 수정한 코드

```java
@SuppressWarnings("serial")
@Embeddable
class MembershipPK implements Serializable {
	//	참여한 채팅방의 id (FK)
	@ManyToOne
	@JoinColumn(name="chat_room_id")
	int chat_room_id;
	//	참여자의 id (FK)
	@ManyToOne
	@JoinColumn(name="member_id")
	int member_id;
}

@Entity
@Data
@AllArgsConstructor 
@NoArgsConstructor
public class Membership {
	@EmbeddedId
	MembershipPK id;
}
```

@Data와 Args 관련 어노테이션은 Lombok을 사용해서 있는 부분이니 이번 주제와 관련 있지는 않다.

이 때, 별도로 만든 PK클래스에 public 접근 제한자를 넣게 되면, 한 클래스에 2가지의 public 클래스가 존재하게 되면서 `The public type \[class name\] must be defined on its own file` 오류가 나니 주의하자.

<br/>

## 기타 오류

실수로 어노테이션 다음에 ;를 넣는 구문 오류를 발생시켰는데 `Syntax error, insert "EnumBody" to complete ClassBodyDeclarations` 라는 예외 문구가 발생하더라. 영어로 길게 에러문이 출력되어도, 대부분 같은 말을 반복하니 무엇을 말하고 있는지 핵심을 아는 것이 중요한 듯 싶다.

<br/>

## 마치며

이전에는 그냥 Maven만 사용해서 MVC 구조의 웹 프로젝트를 진행했었는데, 이번에 Spring을 사용해보면서 편리함과 어려움을 같이 깨닫는 중이다. 편리하게 나온 프레임워크는 그만큼 사전에 만져줘야 하는 설정이 많구나... 그래도 확실히 수정을 위해 많은 파일을 손보지 않아도 돼서 편하긴 하다. 왜 많이 쓰이는지 알 것같다.

<br/>

> 가독성을 위해 같은 기능을 하는 @IdClass로 수정해봤다. <https://gh13q5.github.io/posts/spring-data-jpa-idclass/>
{: .prompt-info }
