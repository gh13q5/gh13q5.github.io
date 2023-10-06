---
title: "Spring Data JPA의 @IdClass 어노테이션"
author: gh13
date: 2022-06-10 15:22:00 +0800
categories: [Web, Spring]
tags: [Spring Boot, Spring Data JPA, Error 기록]
render_with_liquid: false
---

## 기존 코드와 문제점

> @Embedded를 사용한 기존 코드: <https://gh13q5.github.io/posts/spring-data-jpa-embedded/>
{: .prompt-info }

기존에 @Embedded를 사용해서 식별자를 만들어줬는데, 생각보다 한눈에 들어오지 않길래 수정하기로 했다. `@IdClass 어노테이션`을 사용해서 바꿔줬는데, 비슷한 역할을 하면서도 훨씬 더 DB에 가까운 구조로 보여줘서 한눈에 코드를 알아보기도 편했다.

<br/>

## 수정한 코드

```java
@SuppressWarnings("serial")
@EqualsAndHashCode
class MembershipPK implements Serializable {
	Chat_room chat_room;
	Member member;
	
	public MembershipPK() {}
}

@Entity
@Data
@AllArgsConstructor 
@NoArgsConstructor
@Table(name="MEMBERSHIP")
@IdClass(MembershipPK.class)
public class Membership {
	@Id
	@ManyToOne
	@JoinColumn(name="CHAT_ROOM_ID")
	Chat_room chat_room;
	@Id
	@ManyToOne
	@JoinColumn(name="MEMBER_ID")
	Member member;
}
```

MemberShipPK 클래스는 복합 식별자 역할만 하는 만큼 어노테이션의 사용을 최소한으로 해줬다. 대신 기존 Membership 클래스에 어떤 필드가 복합 식별자로 사용되었는지 @Id 어노테이션을 이용해 구분해줬다. 이 도메인의 경우 FK로만 구성되어 있기 때문에 이쪽이 훨씬 더 가독성이 좋았다.

<br/>

## @Embedded와의 차이점

물리적 관점에서는 큰 차이가 없다. 대신, `@Embedded는 복합 식별자에 별도의 의미를 부여하거나 재사용`할 때 효과를 발한다. (예 : Address 등) `@IdClass는 복합 식별자가 별 의미가 없을 때` 사용하기 좋으며, `DB 구조와 비슷`해서 코드를 직관적으로 읽을 수 있다.

<br/>

## 기타 수정사항

원래는 외래키로 member_id 필드를 생성했었는데, @ManyToOne은 객체 참조 시에만 가능하다고 한다. 그 부분 주의해서 Entity 작성하도록 해야겠다.
