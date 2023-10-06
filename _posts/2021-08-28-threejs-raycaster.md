---
title: 카메라 방향으로 raycaster 쏘기
author: gh13
date: 2021-08-28 18:32:00 +0800
categories: [Web, three.js]
tags: [three.js, javascript 5, Error 기록]
render_with_liquid: false
---

## 해낸 일

- 이곳저곳 마구잡이로 쏴지던 raycaster를 카메라가 보는 방향으로만 쏘도록 만들기

이번엔 다른 팀원이 작성한 코드에서 일부에 수정만 가하고 왔다. 본래 우리 생각대로면 raycaster가 일직선으로 카메라 방향을 향해 쭉 나아가야 할텐데, 생각보다 적용되는 거리가 너무 짧더라. near와 far 변수 문제일까 싶었는데 이걸 고쳐도 잘 안 되고... 그렇게 애를 먹은 것 치고는 해결법이 생각보다 너무 간단해서 조금 눈물도 난 듯.

<br/>

## raycaster.setFromCamera(...) 이용

먼저 raycaster가 어떻게 나가고 있는지 확인하기 위해 우리 눈에 보이도록 해줬다.

```javascript
function render(time){
	...
	// render 시마다 빨간 화살표를 scene에 추가
	scene.add(new THREE.ArrowHelper(raycaster.ray.direction,
    	raycaster.ray.origin, 300, 0xff0000) );
    
    ...
}
```

![fail raycaster](/assets/img/post_img/2021-08-28-01.png){: width="972" }
_으악;;;;_

음~ ㅋㅋ;; 그래도 쭉 보면 raycaster가 카메라로부터 나가고 있는 건 맞는 것같은데 방향이 자꾸 땅으로 치솟는 게 문제인 것같았다. 이 때 코드를 하나하나 살펴보던 중 raycaster.setFromCamera() 메소드가 사용되고 있는 것을 발견했다. 뭔가 잘 만지면 될 것같아서 three.js 문서 페이지에서 관련 메소드를 살펴보니, 새로운 원점과 방향을 가지도록 raycaster를 업데이트하는 메소드 `raycaster.setFromCamera(Vector2, camera)`가 있었다.

Vetcor2에는 raycaster의 방향을 정해줄 단위벡터 값이 들어가고(보통은 마우스 위치를 정규화시켜서 사용하는 듯), camera에는 내가 사용하고 있는 camera 변수가 들어가면 된다. 레이저가 모두 똑같은 위치에서 나오고 있는 걸 보니 원점의 문제는 아닌 듯 했고, 방향의 문제 같아서 첫번째 변수만 수정해줬다.

```javascript
// 카메라가 보는 방향을 가리키는 Vector3 변수 생성
let cameraDirection = new THREE.Vector3();

function render(time){
	...

	raycaster.setFromCamera(camera.getWorldDirection(cameraDirection), camera);

	...
}
```

카메라가 바라보고 있는 방향의 Vetcor3 값을 가져오는 `camera.getWorldDirection()` 메소드의 변수로 cameraDirection을 넣어주면, 카메라가 보는 방향을 향해 올곧게 raycaster가 진행하는 모습을 볼 수 있었다. 참고로 raycaster 변수를 선언해줄 때 별다른 매개변수를 넣어주지 않아야 올바르게 적용이 됐었다.

<br/>

## 마치며

무슨 문제든 눈에 보이게 만드는 게 문제점을 알아차리기 쉬운 것같다. 안 보이는 raycaster로 그렇게 끙끙거렸는데 눈에 보이도록 만들고 나니 생각보다 금방 문제점을 찾을 수 있었다. 물론 해결방법을 찾는 건 별개라 이건 또 오래 걸리긴 했지만 ㅋㅋㅠ 그래도 뭐든 문제점부터 알아야 해결할 수 있으니...
