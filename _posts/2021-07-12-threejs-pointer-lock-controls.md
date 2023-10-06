---
title: PointerLockControls와 raycster 사용해보기
author: gh13
date: 2021-07-12 14:46:00 +0800
categories: [Web, three.js]
tags: [three.js javascript 5]
render_with_liquid: false
image:
  path: /assets/img/post_img/2021-07-12-02.PNG
---

## 시도한 것

- PointerLockControls를 사용한 카메라 이동 구현
- raycaster를 이용해서 더욱 입체적으로 보이게 하기

> 참고한 코드: <https://github.com/mrdoob/three.js/blob/master/examples/misc\_controls\_pointerlock.html>
{: .prompt-info }

![Render result](/assets/img/post_img/2021-07-12-01.gif){: width="972" }
_천장이 낮아서 Doom 생각난다..._

<br/>

## PointerLockControls과 이동 이벤트

처음에는 1인칭 카메라 구현에는 당연히 FirstPersonCamera를 사용하겠지! 라는 단순한 생각으로 접근했는데... FPS 게임같이 원하는 시야에만 카메라가 딱딱 멈춰서는 느낌이 아닌, 24시간 빙글빙글 도는 카메라가 구현돼버렸다. 마우스가 중앙에서 아주 조금만 벗어나도 그방향으로 훼까닥 넘어가는 듯 했다. ㅡㅡ 하여튼 이런 불편한 카메라는 누가 써도 사양이라 이것저것 찾아보는 중 PointerLockControls를 찾았다. 실제 Three.js 예제에 구현되어 있는 걸 실행시켜 보니 딱 내가 원하던 느낌이었다.

### lock과 unlock 이벤트

PointerLockControls는 `웹브라우저 안에 마우스를 가두는(lock) 컨트롤러`로, 가지고 있는 이벤트도 마우스의 이동과 lock, unlock 이렇게 세 가지이다. FPS 구현에도 좋다고 한다. lock의 개념이 모호해서 찾아보니 Mouse.Capture와 비슷한 개념이더라. 웹 브라우저 안에 마우스가 없으면 (마우스가 외부 클릭 시) unlock, 웹 브라우저 안에 마우스가 있으면(capture) lock 이벤트가 발생하는 방식인가?

```javascript
//다른 필요한 변수들 선언(camera, scene 등...)
let controls;
const objects[];

init();
//이후, animate 구현 후 실행 코드 추가

function init(){
	//scene, camera, light 등 선언하고 추가
	...
    
	controls = new PointerLockControls( camera, document.body );

	//html요소 가져오기
	const blocker = document.getElementById( 'blocker' );
	const instructions = document.getElementById( 'instructions' );

	//처음 실행 + Esc로 일시정지 + 브라우저 바깥으로 벗어났을 때 띄우는 안내문 이벤트
	instructions.addEventListener( 'click', function () {
		controls.lock();
	} );
	//안내문 클릭 시 사라졌다가, 아니면 다시 나타남
	controls.addEventListener( 'lock', function () {
		instructions.style.display = 'none';
		blocker.style.display = 'none';
	} );
	controls.addEventListener( 'unlock', function () {
		blocker.style.display = 'block';
		instructions.style.display = '';
	} );

	scene.add( controls.getObject() );
    
	...
}
```

### move 이벤트

이동 이벤트는 유니티나 lua 언어에서 했던 것과 비슷해서 금방 이해할 수 있었다. 같은 스크립트 언어인 lua가 이럴 때 도움이 되는구나. init() 함수 안에 switch문으로 각 키보드에 대한 조건문을 완성해주고, 해당 이벤트를 이벤트리스너로 추가해주면 끝이다. 여기서는 boolean을 이용해 그 키가 눌렸는지 안 눌렸는지 확인만 해주고, 자세한 이동은 animate(), 즉 render() 쪽에서 추가해준다.

```javascript
const onKeyDown = function ( event ) {
	switch ( event.code ) {
		case 'ArrowUp':
		case 'KeyW':
			moveForward = true;
			break;
		case 'ArrowLeft':
		case 'KeyA':
			moveLeft = true;
			break;
		...
	}
};
const onKeyUp = function ( event ) {
	switch ( event.code ) {
		case 'ArrowUp':
		case 'KeyW':
			moveForward = false;
			break;
		...
	}
};

document.addEventListener( 'keydown', onKeyDown );
document.addEventListener( 'keyup', onKeyUp );
```

## Raycaster 사용

Raycaster는 `광원(혹은 카메라)에서 발사한 레이저가 물체에 부딪혀 돌아오는 시간으로 거리를 계산해서 깊이감을 측정하고 표현`할 수 있는 기술이다. 현재는 많이 쓰이고 있는 기술이고, 나 역시 유니티를 배울 때 사용해봐서 알고 있긴 했으나... Three.js에도 있는줄 몰랐다. 생각해보니 Three.js도 3D 기술이니 있을 법했구나. 전부터 Three.js 축에서 왜 z축이 세로가 아닌 가로로 있을까 신기해 했었는데, 이것도 raycaster때문같다. 우리가 보통 입체! Z축! 하면 위아래 높이를 생각하는데, 여기서는 입체를 깊이로 나타내니까? `오브젝트들간의 깊이를 나타내는 가로가 z축`이 되는 게 당연지사였다.  

선언은 아래와 같이 해줬다. 자세한 계산과 구현은 역시 animate()에서 한다.

```javascript
raycaster = new THREE.Raycaster( new THREE.Vector3(), new THREE.Vector3( 0, - 1, 0 ), 0, 10 );
```

그런데 생각해보니 나는 GLTF파일을 import해서 사용하게 되는데, 이 때도 raycaster가 필요할까 싶다. 전에 raycaster 없이 FirstPersonCamera로 구현했을 때도 3D의 느낌은 잘 났던 것같아서? 깊이감과 현실감 때문에 사용하는 걸까... 나중에 raycaster 없는 코드도 시도해봐야겠다.

<br/>

## animate() 함수 안에서 각도와 방향 계산

```javascript
function animate() {
	requestAnimationFrame( animate );

	const time = performance.now();

	//컨트롤러(카메라)가 lock 걸려있을 때 실행(마우스가 브라우저 안에 있을 때)
	//raycaster를 이용한 화면 구성 및 이동 구현(얼마나 이동했는지)
	if ( controls.isLocked === true ) {
		raycaster.ray.origin.copy( controls.getObject().position );
		raycaster.ray.origin.y -= 10;

		const intersections = raycaster.intersectObjects( objects );
		const onObject = intersections.length > 0;
		const delta = ( time - prevTime ) / 1000;

		//velocity는 속도
		velocity.x -= velocity.x * 10.0 * delta;
		velocity.z -= velocity.z * 10.0 * delta;
		velocity.y -= 9.8 * 100.0 * delta; // 100.0 = mass

		//이동방향 정하기 (boolean을 1이랑 0으로 바꾼 후에 계산)
		direction.z = Number( moveForward ) - Number( moveBackward );
		direction.x = Number( moveRight ) - Number( moveLeft );
		direction.normalize(); // this ensures consistent movements in all directions

		if ( moveForward || moveBackward ) velocity.z -= direction.z * 400.0 * delta;
		if ( moveLeft || moveRight ) velocity.x -= direction.x * 400.0 * delta;

		if ( onObject === true )
			velocity.y = Math.max( 0, velocity.y );

		//카메라(컨트롤러)도 이동
		controls.moveRight( - velocity.x * delta );
		controls.moveForward( - velocity.z * delta );
		controls.getObject().position.y += ( velocity.y * delta ); // new behavior

		if ( controls.getObject().position.y < 10 ) {
			velocity.y = 0;
			controls.getObject().position.y = 10;
		}
	}

	prevTime = time;
	renderer.render( scene, camera );
}
```

구문 별로 어떻게 동작하는지는 이해했지만, 수식이라던가 이런 자세한 건 아직 헷갈린다... 나중에 물리 공부와 함께 더 공부해볼 용도로 올려둔다.

<br/>

## 마치며

3D 관련 코드를 만질수록 물리를 다시 공부해야겠다는 생각이 팍팍 드는 요즘이다. 고등학생 때 이후로 안 보고 살았는데 이렇게 다시 공부하게 되는구나; 앞으로 손봐야할 건 raycaster가 내 코드에 정말 필요하냐 아니냐, 그리고 animate() 코드에 사용된 물리식을 더욱 이해하기 정도인 듯. 아 맞다, 저 가운데 기둥을 돌 때마다 렉이 걸리는 느낌이 들던데 이것도 손봐야겠다. 또 장기적으로는 물리랑 수학 공부 다시 시작해야 할 것같구.
