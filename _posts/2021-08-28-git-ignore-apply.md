---
title: ".gitignore 파일 적용"
author: gh13
date: 2021-08-28 17:57:00 +0800
categories: [IT Memo]
tags: [git, Error 기록]
render_with_liquid: false
---

## 해낸 일

- .gitignore 파일에 무시할 파일 경로 작성
- git 프로젝트의 cache 초기화 후 .gitignore 파일 적용

프로젝트를 진행하면서 git commit 관련으로 진짜 무수한 오류를 본 것같다............... 이번에는 내가 웹 서버에 이클립스 파일을 배포해보려다가 pom.xml 파일과 관련된 것들이 변경되기 시작해서 생긴 문제점이었다. target 폴더의 내용물이 계속해서 commit 해야할 파일로 뜨는 걸 애초부터 무시했어야 했는데 내가 그걸 그대로 commit/push 해버린 것. 여러 명이 파일을 만지다 보니 자꾸 이 파일이 commit이 안 돼서 push도 안 되고 pull도 안 되는 그런 상황이 벌어져버렸다. 처음엔 MANIFEST 파일에서 오류가 자꾸 난대서 HEAD 문제인가 싶었는데 경로가 따아악 저 target 폴더였다.

어차피 당분간은 바로 배포 올릴 일도 없고 해서 고민하던 중, .gitignore 파일로 commit 대상에서 제외시킬 수 있다는 이야기를 듣고 후다닥 적용시켰다.

<br/>

## .gitignore 파일 작성 및 적용

![my local folder img](/assets/img/post_img/2021-08-28-01.png){: width="972" }

프로젝트의 최상단 폴더에 있는 .gitignore 파일을 텍스트 편집기로 켜준 후 제외시킬 폴더의 경로를 작성해준다.

```bash
/build/

# 서버 생성 시 생긴 파일
target/
```

이제 target 아래에 있는 모든 것들이 다 무시가 돼야 할텐데... 이대로 바로 이클립스를 재실행 시키면 여전히 commit 대상으로 뜰 확률이 높다. 아직 git 내에 남아있는 캐시 때문이라 하니, 마저 싹싹 지워주자.

```bash
cd C:\User\...
# git 프로젝트 최상단 폴더의 경로로 이동

git rm -r --cached .
git add .
git commit -m "삭제: gitignore 적용을 위한 캐시삭제"
```

해당 프로젝트 폴더로 경로 이동 후 캐시 삭제를 진행해주면 된다. 이동 안 하면 백날천날 git rm 명령어 쳐도 지정 파일이 없다는 오류만 뜬다. 명령어 마지막에 있는 마침표(.)는 현재 위치를 가리키는 경로니까 잊지 말기. 난 저 점을 못 봤어서 꽤 얼레벌레 했었다.

![clean commit~](/assets/img/post_img/2021-08-28-02.png){: width="972" }

깔끔! 해졌다. 다른 팀원들도 똑같이 gitignore 적용을 진행하니 발목 잡던 MANIFEST 문제는 사라졌다. 당분간은 편하게 작업할 수 있을 것같다.

<br/>

## 마치며

git을 수박게임 만드느라 만져본 게 전부여서 그런지, 본격적으로 만지니 생각보다 복잡하더라. 협업에 편한 건 사실인데 잘 활용하려면 그만큼 잘 알아야할 듯. 하루빨리 익숙해져야만...
