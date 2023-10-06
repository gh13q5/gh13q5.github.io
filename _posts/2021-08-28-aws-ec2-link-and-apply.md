---
title: EC2 연결 및 mySQL, Apache, PHP, Java, Tomcat8 설치
author: gh13
date: 2021-08-28 17:23:00 +0800
categories: [Web, 백엔드]
tags: [AWS, EC2, Error 기록]
render_with_liquid: false
---

## 해낸 일

- AWS EC2 인스턴스 생성 및 기본 설정 만지기 및 접속 (ubuntu 사용)
- 서버에 MySQL, Apache, PHP 설치 후 연동
- 서버에 Java, Tomcat8 설치

이번엔 AWS EC2 서버를 생성하고 기초 인프라에 필요한 것들을 설치하고 만지는 작업이 많아서, 유독 다른 블로그의 글들을 참고해가며 작업한 일이 많았다. cmd 창으로 계속 작업하다 보니 리눅스 시간에 배웠던 것들을 이렇게 쓰게 되나 싶기도 하고 ㅋㅋ

<br/>

## AWS EC2 생성과 접속 / MySQL, Apache, PHP 설치

> 인스턴스 설치부터 연동까지의 전반적인 과정은 [안경잡이 님의 블로그](https://ndb796.tistory.com/m/314)를 참고해 진행할 수 있었다.
{: .prompt-info }

### 기존에 깔려있던 tomcat과 apache, php 재설치 시 php 연동이 안 되는 문제점

tomcat9 대신 8을 새로 까느라 이것저것 지우고 다시 깔았더니 생긴 문제점... apache 설치하고 index 페이지가 뜨는 거랑 내가 따로 만든 index.html 페이지가 대신 뜨는 것까지도 완벽하게 잘 됐는데, php 페이지가 안 뜨는 문제가 발생하고 말았다. php 인덱스 페이지가 뜨는 대신 php 코드만 덜렁 보이고 마는 그런 오류였다. 알아봐서 하라는 대로도 다 했는데 뭐가 문제인지 해결도 안 되고, 모듈 파일을 만져봐도 안 되고, php 재설치해도 안 되고... 

어이없게도 php랑 apache를 동시 삭제하고 apache부터 차근차근 설치하니까 이번엔 되더라; 역시 뭐든 문제 생겼을 때 고쳐질 기미가 안 보이면 쿨재설치하는게 제일이다 ㄱ-

## AWS EC2 서버에 Java와 Tomcat8 설치

기존 이클립스에서도 apache tomcat 서버를 사용했기 때문에 이번 프로젝트에서도 tomcat을 활용하기로 했다. 이미 컴퓨터에 tomcat이 설치되어 있어서 안일하게 생각하고 있었는데, 서버에도 설치해줘야 하더라. 알아보니 AWS의 우분투에는 기본 프로그램들이 상당수 안 깔려있다 한다.

```bash
sudo apt-get install openjdk-8-jdk
```

슈퍼유저 권한으로 명령어 실행 후, 패키지 관리 명령어 도구를 사용해 java8을 설치했다. 중간에 `Do You Want To Continue?`하고 물음이 나오면 `Y` 입력으로 대답해주자.

```bash
java -version
```

명령어 입력 후 `openjdk version  ... ` 이 나오면 성공적으로 설치된 것!

```bash
sudo apt-get update
sudo apt-get install tomcat8
```

`tomcat8`로 설치해줬다. 윈도우에서 쓰고 있던대로 tomcat9.0을 설치했더니 나중에 배포할 때 잘 안 되더라... 알아보니 원래 tomcat9이 서버 배포가 잘 안 될 때가 있다 한다 ㄱ- 설치가 끝났으면 AWS EC2로 들어가서 `네트워크 및 보안`-`보안 그룹` 중 아까 인바운드 규칙을 수정해준 보안 그룹에 `인바운드 규칙을 추가`해준다.

![ec2 inbound rule page](/assets/img/post_img/2021-08-28-03.png){: width="972"}

`http://EC2인스턴스의_퍼블릭ip:8080` 으로 접속해보면 tomcat의 index 페이지가 뜬다. 그럼 성공적으로 연결 된 것!

<br/>

## 마치며

원래는 이클립스 프로젝트를 filezilla를 이용해 tomcat에 배포하는 것까지 하는 게 목표였는데, 이게... 이전에 tomcat9을 사용할 때 실패한 이후로 8로는 아직 배포를 안 해봤다. 게다가 배포하고 나면 프로젝트의 pom.xml 파일에 수정이 가해진다는 말도 있어서 일단은 프로젝트가 더 완성될 때까지 기다리는 중이다. 한 삽질에 비해선 글이 매우 짧은데, 그래도 이것저것 익힌 건 또 많다. 리눅스 명령어 다시 상기시키는 것부터 mysql까지... 역시 삽질하며 익히는 게 제일이다.
