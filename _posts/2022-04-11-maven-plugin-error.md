---
title: Maven 플러그인 에러
author: gh13
date: 2022-04-11 14:27:00 +0800
categories: [Web, Memo]
tags: [Maven]
render_with_liquid: false
---

## 발생한 문제

`pom.xml`에 다음과 같은 에러가 발생했다.

> Could not calculate build plan: Plugin org.apache.maven.plugins:`maven-war-plugin:3.2.2` or one of its dependencies could not be resolved: org.apache.maven.plugins:maven-war-plugin:jar:3.2.2 failed to transfer from <https://repo.maven.apache.org/maven2> during a previous attempt.
{: .prompt-warning }

pom.xml에서부터 이런 에러가 발생하니 다른 bean에도 영향을 미쳐서 아예 run조차도 불가능하더라...

<br/>

## 해결법: 디렉토리 삭제!

내 파일에서는 `maven-war-plugin`에서 오류가 발생했으므로, `사용하고 있는 maven-war-plugin 버젼의 디렉토리를 삭제`해서 해결했다. 대부분은 maven-resource-plugin에서 발생한 오류를 해결한 글이었지만, 내 경우도 같은 방법으로 해결이 됐다. 파일 경로는 다음과 같다.  

> `C드라이브`-`사용자(User)`-`내 계정`-`.m2`-`repository`-`org`-`apache`-`maven`-`plugins`-`...`
{: .prompt-info }

![directory path](/assets/img/post_img/2022-04-11-03.png){: width="972" }

<br/>

## 마치며

뭐든지 코딩보다 파일 경로 등에서 나는 오류를 해결하는 것이 제일 오래 걸리고 머리를 싸매게 되는 것같다. 제대로 run 해보기도 전에 뜬 오류 항목이 매우 길어서 당황했지만, 사실은 같은 내용을 다른 말로 여러 번 말해준 것뿐이라는 걸 깨닫고 나니 해결법이 보다 쉽게 보이더라. 오류문에 당황하지 않고 침착하게 해결법을 타파하는 자세도 생각보다 중요한 듯 싶다.
