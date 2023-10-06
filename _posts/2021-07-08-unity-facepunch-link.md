---
title: Unity와 Facepunch.Steamworks SDK 연동하기
author: gh13
date: 2021-07-08 18:57:00 +0800
categories: [C#, Unity]
tags: [Unity, Steamworks]
render_with_liquid: false
---

## 해낸 일

- FacePunch.SteamWorks SDK 유니티 파일에 적용시키기
- 테스트용 ID로 플레이어 이름이랑 ID 유니티에 가져오기

<br/>

## FacePunch.SteamWorks SDK

유니티와 Steam SDK를 연동시키기 위해 [FacePunch.SteamWorks SDK 소스를 다운](https://github.com/Facepunch/Facepunch.Steamworks)받았다. 자체 위키도 있고 해서 분명 쉬울줄 알았는데... 생각도 못 한 오류가 발생하더라. 유저 소스인만큼 아직 구글에 나오는 QnA가 적어서 조금 애를 먹었던 것같다.

### error CS0433: The type 'SteamClient' exists in both 'Facepunch.Steamworks.Posix ... '

[코드를 다운](https://github.com/Facepunch/Facepunch.Steamworks/releases)받고 위키에 나온대로 Plugins 디렉터리에 옮겨놨는데 뜬 오류다. 숨김 파일들이 잘 안 옮겨져서 그런 것같다고 하던데, 찾아보면 다 옮겨져 있다. ㄱ- 혹시 몰라서 옮겨둔 내용물 다 삭제 후, `숨김 파일 보기 처리 후에 SDK zip 파일을 풀어줬더니` 그제야 잘 돌아가더라. 휴;;

### error CS0103: The name '...' does not exist in the current context

이 오류는 SteamClient 같은 클래스명마다 뜨더라. 어디가 문제일까 싶어서 보니, 내가 맨처음에 Init 해줄 때 SteamClient만 쓰지 않고 Steamworks.SteamClient라고 줄줄 써둔 게 눈에 띄더라. 그래서 모든 SteamClient 앞마다 Steamworks를 추가해주니 바로 해결됐다. 이런 중복은 번거로운데 using 같은 걸로 어떻게 해볼 방법이 없을까 고민해봐야겠다. 아직 C#을 실전에 써본적이 별로 없어가지고...

```cs
void Update(){
	Steamworks.SteamClient.RunCallbacks();
	//앞에 Steamworks. 추가
	...
}
```

<br/>

## 스팀 플레이어 이름이랑 ID 유니티에 가져오기

설치에 애를 먹느라 이쪽은 수월하게 잘 됐다. 애초에 [위키](https://wiki.facepunch.com/steamworks/Installing_For_Unity)에 나와있는 코드를 시험해본 거라 ㅋㅋㅠ Init이 잘 됐나 확인하려고 IsValid 함수를 추가해준 게 전부다.

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class SteamManager : MonoBehaviour {
    void Start()
    {
        try
        {
            Steamworks.SteamClient.Init( 480, true );
        }
        catch ( System.Exception e )
        {
            // Something went wrong - it's one of these:
            //
            //     Steam is closed?
            //     Can't find steam_api dll?
            //     Don't have permission to play app?
            //
        }
    }

    void Update() {
        Steamworks.SteamClient.RunCallbacks();

        var playername = Steamworks.SteamClient.Name;
        var playersteamid = Steamworks.SteamClient.SteamId;

	//SteamID를 잘 가져왔나 확인
        Debug.Log("Get SteamID? (boolean): " + Steamworks.SteamClient.IsValid);
        Debug.Log("playerName: " + playername + ", playerSteamID: " + playersteamid);
    }
}
```

![steamworks id check img](/assets/img/post_img/2021-07-08-01.png){: width="972" }

무사히 잘 됐다! 스팀에서 제공해주는 테스트 ID라는 480을 이용해봤는데, 현재 내 컴퓨터로 로그인되어 있는 스팀계정이 가져와지더라.

<br/>

## 마치며

다음번에는 클라우드랑 연동해보는 걸 시도해봐야겠다. Steamworks SDK의 기능 중에서 제일 중요하고 핵심일 거라 생각하는 기능이다 보니... 제일 먼저 만지고 시도해보는 게 좋지 않을까. 아직 개발자 계정 등록도 안 한 상태고 해서 다소 얼레벌레 만지고 있다는 감이 없잖아 있긴 하다. 그래도 차근차근 뭔가 되고 있으니 아잣 👊
