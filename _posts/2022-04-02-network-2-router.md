---
title: "(작성중) [네트워크 관리사 2급 실기] 라우터(Router) 문제"
author: gh13
date: 2022-04-02 15:15:00 +0800
categories: [IT Memo]
tags: [네트워크 관리사 2급]
render_with_liquid: false
---

## 실기 준비

> 실기의 대부분은 [햄릿슈 님 영상](https://www.youtube.com/channel/UCLIxBOJaBju4Ap8QoGuQYbw)을 참고해서 공부했습니다~
{: .prompt-tip }

처음에 무엇을 준비해야할 지 모를 때, 자주 나오는 유형들과 실기 문제 풀이가 어떻게 진행되는지를 익히기 좋다. 덕분에 실기가 어떻게 진행되는지를 몸에 익혀둘 수 있어서, 그 뒤로 기출문제나 ICQA 예제에서 처음 보는 유형이나 설정 방법이 나와도 응용이 매우 쉬웠다.

<br/>

## 라우터 설정

> - 아래는 햄릿슈 님 영상으로 공부하면서 메모한 것!
> - 라우터는 나오는 문제들이 자주 나오는 경향이 있는 듯
> - 라우터1인지 라우터2인지 잘 보고 클릭해서 풀기
> - 정 헷갈릴 때는 ?를 치면 라우터의 모든 명령어들이 출력되니 그거 보고 하기
{: .prompt-info }

> **자주 쓰이고 중요한 명령어**
   **en (enable)** : 사용자 모드에서 관리자 모드로 전환 (대부분 문제의 시작 때 함)  
   **conf t (configure terminal)** : 관리자 모드에서 전역설정 모드로 전환  
   **interface 이동할인터페이스이름** : 해당 인터페이스로 이동  
   **no shutdown** : 활성화 (문제에 활성화하라는 얘기 없으면 안 해도 됨)  
   **exit** : 한 단계씩 나감 (전역설정에서 쓰면 관리자로, 관리자에서 쓰면 사용자로)  
   **copy r s** : 전역설정 나가기 (완료된 설정 startup-config에 저장)  
**※ NVRAM에 저장하라는 말 == copy r s**

  
**1. 라우터의 IP와 서브넷마스크 설정**

```
en
conf t
interface fastethernet 0/0
ip add 192.168.0.100 255.255.255.0
exit
exit
copy r s
```

\*\* **ip add**는 ip를 할당, **ip route**는 라우팅(정적라우팅) \*\*

\*\* ip add 설정할IP 서브넷마스크 \*\*  
\*\* ip route 도착지IP 도착지SubnetMask 도착지GateWayIP(=도착지라우팅IP) \*\*  
**2. 라우터의 대역폭 설정**

```
en
conf t
interface serial 2/0
bandwidth 2048		#대역폭을 2048로 설정
exit
exit
copy r s
```

**3. 라우터의 클럭 속도 설정**

```
en
conf t
interface serial 2/0
clock rate 72000		#클럭 속도 72K로 설정 / 1K는 1,000
exit
exit
copy r s
```

**4. 라우터 주석(description) 설정**

```
en
conf t
interface fastethernet 0/0
description ICQA		#주석 내용 대소문자 구분!
exit
exit
copy r s
```

**5. 라우터 secondary IP 주소 설정 (두 번째 IP 설정)**

```
en
conf t
interface serial 2/0
ip add 192.168.0.101 255.255.255.0
ip add 192.168.0.102 255.255.255.0 secondary
no shutdown
exit
exit
copy r s
```

**6. 라우터 기본 게이트웨이 설정**

```
en
conf t
ip default-gateway 192.168.0.1
exit
copy r s
```

\*\* **ip default-network IP주소** 도 출제한 적 있음 \*\*

**7. 라우터의 DHCP 네트워크 설정**

```
en
conf t
ip dhcp pool icqa
network 192.168.100.0 255.255.255.0		#일종의 인터페이스 이동이라 exit 2번
exit
exit
copy r s
```

**8. 라우터 telnet에 접근하는 패스워드 설정 (로그인 여부 확인해야함)**

```
en
conf t
line vty 0 4		#텔넷 접속 / 가상터미널을 0~4까지 총 5개 사용하겠다는 의미
password icqa
login
exit
exit
copy r s
```

**9. 라우터 telnet 세션 자동종료 설정**

```
en
conf t
line vty 0 4
exec-timeout 03 50		#3분 50초동안 응답 없으면 자동 종료
exit
exit
copy r s
```

**10. 라우터 콘솔 패스워드 설정(로그인 여부 확인하기)**

```
en
conf t
line console 0		#line이 연결한다는 명령어
password ICQA
login
exit
exit
copy r s
```

**11. 라우터 활성화**

```
en
conf t
interface serial 2/0
no shutdown
exit
exit
copy r s
```

**12. 라우터 호스트네임 설정 (예시는 비밀번호도 같이 변경함)**

```
en
conf t
hostname network2
line console 0
password route5
login
exit
exit
copy r s
```

**13. 라우터 확인 문제 (정보 확인, 내용 확인 등을 하고 저장하시오)**  
**1) 인터페이스 정보 확인하고 저장**

```
en
show interface		#한참 엔터하면 다시 명령어 칠 수 있음
copy r s
```

**2) 접속한 사용자 정보 확인하고 저장**

```
en
show user
copy r s
```

**3) 라우팅 테이블 정보 확인하고 저장**

```
en
show ip route
copy r s
```

**4) 플래쉬 내용 확인하고 저장**

```
en
show flash
copy r s
```

**5) 프로세스 정보 확인하고 저장**

```
en
show process
copy r s
```

**6) 버젼 확인하고 저장**

```
en
show version
copy r s
```

**14\. 프레임 릴레이 네트워크 구성 (2021년 기출문제)**

```
en
conf t
interface serial 2/0
encapsulation frame-relay
exit
exit
copy r s
```

\*\* encapsulation frame-relay 대신 **encap frame** 가능 \*\*

**15\. telnet 접속 후 ssh로 접속 가능하도록 설정 (2021년 기출문제)**

```
en
conf t
line vty 0 4
transport input ssh
exit
exit
copy r s
```
