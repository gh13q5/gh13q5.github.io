---
title: "Tymeleaf와 JSP 동시에 사용하기"
author: gh13
date: 2022-06-10 15:56:00 +0800
categories: [Web, Spring]
tags: [Spring Boot, Tymeleaf, Error 기록]
render_with_liquid: false
---

## 발생한 문제

> Forwarding to \[/WEB-INF/jsp/detailBook.jsp\]  
> "FORWARD" dispatch for GET "/WEB-INF/jsp/detailBook.jsp", parameters={}  
>   
> Mapped to `ResourceHttpRequestHandler` \[classpath \[META-INF/resources/\], classpath \[resources/\], classpath \[static/\], classpath \[public/\], ServletContext \[/\]\]  
>   
> "Path with "WEB-INF" or "META-INF": \[WEB-INF/jsp/detailBook.jsp\]"  
> `Resource not found`  
> Exiting from "FORWARD" dispatch, status 404  
{: .prompt-warning }

Entity 주입 문제를 해결하고 나니, 경로 관련으로 `404 오류`가 발생했다. 이번에는 View 문제였다... 프로젝트 마감일로 인해 Tymeleaf html 페이지와 JSP를 같이 사용하기로 했는데, 이때문에 발생한 문제 같았다.

<br/>

## 수정한 파일구조

![file path](/assets/img/post_img/2022-06-10-01.png){: width="972" }

Thymeleaf 파일은 html 정적 파일이기 때문에 `src/main/resources/templates/thymeleaf` 구조를 만들어줬다. 이 폴더가 없어서 ReourceHttpRequestHandler에서 매핑을 못 하고 헤매고 있는 것같더라. fragments 파일은 나중에 Thymeleaf의 fragment 기능을 이용하기 위해 미리 만들어둔 것이다. 자바 파일은 기존 웹프로젝트와 같이 `src/main/webapp/WEB-INF/jsp` 아래에 만들어줬다.

<br/>

## 수정한 코드

### pom.xml
```html
<dependency>
	<groupId>org.apache.tomcat.embed</groupId>
	<artifactId>tomcat-embed-jasper</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

JSP를 파싱하여 서블릿 코드로 컴파일하기 위한 `jasper`와 Thymeleaf를 위한 `thymeleaf`, 총 2가지 의존성을 추가해준다.

### application.properties와 Controller

```
# JSP views
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp

# Tyhmeleaf html views
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
spring.thymeleaf.cache=false
spring.thymeleaf.view-names=thymeleaf/*
```

JSP파일들의 위치와 Thymeleaf 파일들의 위치를 prefix(접두사)와 suffix(접미사)를 이용해 외부 설정 파일에 추가해준다. 이 설정 파일에 따르면 JSP 파일들은 `/WEB-INF/jsp/파일이름.jsp`로 위치하고, Thymeleaf 파일들은 `/META-INF/resources/templates/파일이름.html`로 위치하고 있다. Tymeleaf의 cache를 false로 해둔건, 기본 설정이 true로 되어 있어서 html에 수정한 것이 즉각적으로 반영되지 않기 때문이다. 

그리고 JSP와 Thymeleaf를 같이 사용할 때 제일 중요한 것, `view-names`를 설정해주자. 이걸 설정해줘야지만 Controller에서 JSP페이지를 반환하는지 Thymeleaf를 반환하는지 구분할 수 있게 된다. Controller에서 앞에 `thymeleaf/가 붙은 페이지를 반환하면 해당 페이지는 Thymeleaf 페이지로 구분`되는 원리다.

```java
@Controller
public class BookController {
	@Autowired
	public BookService bookService;
	
    /*
     * Thymeleaf 페이지 반환
    */
	@RequestMapping("/book/view/create.do")
	public String viewCreateBook() {
		return "thymeleaf/createBook";
	}
	
    /*
     * JSP 페이지 반환
    */
	@RequestMapping("/book/view/{bookId}.do")
	public ModelAndView viewDetailBook(@PathVariable int bookId) {
		Book book = bookService.findBookById(bookId);
		System.out.println(bookId);
		
		ModelAndView mav = new ModelAndView();
		mav.setViewName("detailBook");
		mav.addObject("book", book);
		return mav;
	}
}
```

<br/>

## 마치며

아예 실행이 안 되는 오류라 고치는 데 시간이 많이 걸렸는데, 해결법을 적어두니 생각보다 짧은 것같다. 덕분에 Spring Boot가 어떻게 돌아가는지 체감하게 된 듯^-T  

맞다, 그리고 Spring Boot App으로 Run하게 되면 기존에 Tomcat Server로 실행돌렸을 때처럼 바로 웹페이지가 빡! 뜨진 않으니 직접 브라우저에 주소를 입력해서 들어가야 하더라. 나의 경우는 위의 컨트롤러를 실험할 때, `http://localhost:8080/book/view/1.do` 를 사용했다.
