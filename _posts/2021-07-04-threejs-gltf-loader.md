---
title: GLTF Loader를 이용해 scene import하기
author: gh13
date: 2021-07-04 21:22:00 +0800
categories: [Web, three.js]
tags: [three.js, javascript 5, node.js]
render_with_liquid: false
image:
  path: /assets/img/post_img/2021-07-14-05.png
---


## 오늘과 어제 겪은 문제점

- three.js에 scene 파일 import할 때 JSON을 이용한 방법이 실패한 것
- 그래서 GLTFLoader를 이용하기로 했는데 이것도 애를 먹는 것  

<br/>

## JSON 파일을 이용한 시도

three.js 홈페이지에 있는 editor를 사용해서 scene을 export하면 기본 확장자가 JSON이더라. 그래서 당연히 import 과정도 잘 구현되어 있겠거니 해서 시도해보는데 sceneLoader가 잘 안 되질 않나... 애를 엄청 먹었다. 그래서 objLoader를 먼저 시도해보기로 했는데 이것도 잘 안 되고. 스택오버플로우부터 별별 게시글은 다 참고하고 뒤적거려봤지만 콘솔창에 뻘건 에러만 자꾸 띄우더라. 그러던 중 찾은 게 glTF 확장자. 이 확장자도 자주 사용되고 있는 것같아서 이걸 시도해보기로 마음 먹었다.  

<br/>

## GLTFLoader를 사용해서 scene import

내가 GLTFLoader를 사용하기 위해 작성한 코드는 아래와 같다. Three.js 홈페이지의 문서에 나온 [예시](https://threejs.org/docs/?q=gl#examples/en/loaders/GLTFLoader)를 참고했다.  

```javascript
const loader = new THREE.GLTFLoader();
loader.load(
	// resource URL
	'./scene01.gltf',
	// called when the resource is loaded
	function ( gltf ) {

		scene.add( gltf.scene );

		gltf.animations; // Array<THREE.AnimationClip>
		gltf.scene; // THREE.Group
		gltf.scenes; // Array<THREE.Group>
		gltf.cameras; // Array<THREE.Camera>
		gltf.asset; // Object

	}
);

function render() {
	// render using requestAnimationFrame
	requestAnimationFrame(render);
	renderer.render(scene, camera);
}

render();
```

분명 코드에는 에러가 발생할 부분이 없다. 근데 왜 나는 이걸로 그렇게 오래 삽질을 했을까...  

### <script> 종료 태그 오류

첫 번째로 발생한 문제는 <script src="./glTFLoader.js" />를 추가할 때 발생한 오류였다. 나는 <div>에 렌더링한 scene을 붙이는 방식을 사용하는데, 이게 아예 실행이 안 되고 있었는지 <body>태그가 비어있다고 나오더라. 하얀 화면만 덩그러니 나오는 상태로 말이다. 알고보니 <script>의 종료 태그가 제대로 작성되지 않아서 <style>이 아예 인식도 안 되고 있었다. <script src="..."></script>로 종료 태그를 작성해주니 바로 해결됐다.  

### Uncaught TypeError: THREE.GLTFLoader is not a constructor

종료 태그 문제를 해결한 후 발생한 문제는 `Uncaught TypeError: THREE.GLTFLoader is not a constructor` 에러다. 처음에는 내 three.js와 gltfLoader의 ES5와 ES6 버젼 차이에서 발생한 문제인 것같아서 three.js import 방식 자체를 바꿨다. <script src="..." /> 대신 import 구문과 three.module.js 파일을 사용하는 방식으로. `type을 module로 바꿔주는 거 잊지말자.`

```html
<script type="module">
	import * as THREE from './libs/three.module.js'
    ...
</script>
```

### Failed to load module script: Expected a JavaScript module script ... (MIME type 에러)

module 대체 무슨 오류인지도 감이 안 오는 에러가 콘솔창에 떠버렸다... 내가 모듈을 사용하면서 별도의 localhost도 없이 html을 크롬에 띄우고 있어서 발생한 오류였나보다. 이전에 node.js를 설치한 경험이 있어서 그대로 `node.js http 서버를 설치`해줬다. node.js 설치 후 cmd 창을 켜주자.  

```bash
npm install http-server -g
```

먼저 node.js http를 설치해준 후, 'cd 내파일경로'를 이용해 cmd 내 현재 디렉터리를 이동해줬다.  

```bash
http-server . -p 8000
```

그리고 위 명령어를 치면 로컬 서버가 열린다. Ctrl+C로 언제든지 종료할 수 있다는 게 편하다... 개인적으로 톰캣보다 편하다. 굳이 cd로 이동 안 해도 http-server 명령에 내 파일 경로를 입력하면 그 디렉터리에 로컬 서버를 호스팅할 수 있는 듯.  

![npm server](/assets/img/post_img/2021-07-14-01.png){: width="972"}
![npm index](/assets/img/post_img/2021-07-14-02.png){: width="972"}

좋아, 그래도 여기까진 해냈다. 그런데 여기서 또 오류가 발생하더라. 이젠 날 오류도 없어 보이는데 ㄱ-  

### GLTFLoader 별도 import

`Uncaught TypeError: THREE.GLTFLoader is not a constructor` 익숙한 이 에러가 또 떠버렸다... 로컬 서버로 열어줬으니 모듈도 적용될텐데 대체 왜? 싶었던 그 때, Three.js 홈페이지 document에서 무언가 발견해버렸다. `GLTFLoader는 별도로 import`해줘야 하나보다.

```html
<script type="module">
	import * as THREE from './libs/three.module.js';
	import { GLTFLoader } from './libs/GLTFLoader.js';
    ...
</script>
```

이전에 사용하던 glTFLoader.js는 교재에서 제공하던 옛날 파일이라 새로운 GLTFLoader.js를 받아줬다. 3D 모델 load 관련 js 파일은 이 [예시 깃허브](https://github.com/mrdoob/three.js/tree/dev/examples/jsm/loaders)에서 받아주자.

### net::ERR\_ABORTED 404 (Not Found)

진짜진짜 다 된줄 알았는데 웬걸... 갑작스러운 404 에러... import 구문에서 파일 경로 지정할 때 문제가 생겼나 보다. 내 경우에는 GLTFLoader.js 하나만 덩그러니 디렉터리에 넣어두고 쓰고 있었기 때문에 기존에 three.js Git에서 제공하는 경로랑 달랐기 때문에 에러가 발생했다. 혹시 이런 문제가 생긴다면, GLTFLoader.js 파일을 열고 `GLTFLoader.js 파일 기준으로 three.module.js가 어디에 위치해있는지` 경로를 바꿔주자. 나는 같은 디렉터리에 있었기 때문에 아래처럼 바꿔줬다. 처음에 '../../../build/three.module.js' 이런 식으로 써져 있어서 해당 파일을 못 찾고 있었나 보다.  

```javascript
//GLTFLoader.js 파일에서 맨처음 등장하는 import 구문
import {
	...
} from './three.module.js';
```

그랬더니!  

![my import result](/assets/img/post_img/2021-07-14-03.png){: width="972"}

드디어 glTF 파일이 import 됐다! 👏👏👏👏👏 애초에 import 기능을 시도해보고 있던 거라 예시가 저렇긴 하지만, 하여튼 성공한 게 어디람. 아주 그냥 묵은 체증이 싸악 내려간다.

> 참고로 [glTF 뷰어 사이트](https://gltf-viewer.donmccurdy.com/)를 통해 내 glTF 파일을 확인할 수 있다.
{: .prompt-tip }

<br/>

## 마치며

import로 이렇게 고생할 줄은 몰랐던 지라 까먹기 전에 일지를 쓰러 와봤다. 다른 일지들 보면 에러, 발생 이유, 해결법, 후기 등으로 깔끔하게 작성들 하시던데 난 쓰고보니 일기마냥 중구난방 돼버렸다. (ㅋㅋ) 예전에 수박 게임 만든다고 설치한 node.js가 이렇게 도움 되는 날이 올줄야. 괜히 많이 쓰는 게 아니었다. 근 4~5일은 고민하고 있던 문제라 어우... 내가 다 뿌듯하다 지금. 이 경험이 앞으로 도움되는 날이 오기를... 아래는 내 전체 코드다.  

```html
<!DOCTYPE html>

<html>
<head>
    <title>gltfExample01</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
        }
    </style>
</head>
<body>
    <!-- Div which will hold the Output -->
    <div id="WebGL-output"></div>
    <script type="module">
        import * as THREE from './libs/three.module.js';
        import { GLTFLoader } from './libs/GLTFLoader.js';

        var scene;
        var camera;
        var renderer;

        function init() {
            scene = new THREE.Scene();

            camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
            scene.add(camera);

            renderer = new THREE.WebGLRenderer();

            renderer.setClearColor(new THREE.Color(0xEEEEEE, 1.0));
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.shadowMapEnabled = true;

            camera.position.x = -30;
            camera.position.y = 40;
            camera.position.z = 30;
            camera.lookAt(scene.position);

            // add the output of the renderer to the html element
            document.getElementById("WebGL-output").appendChild(renderer.domElement);

            const loader = new GLTFLoader();
            loader.load(
                // resource URL
                './scene01.gltf',
                // called when the resource is loaded
                function ( gltf ) {

                    scene.add( gltf.scene );

                    gltf.animations; // Array<THREE.AnimationClip>
                    gltf.scene; // THREE.Group
                    gltf.scenes; // Array<THREE.Group>
                    gltf.cameras; // Array<THREE.Camera>
                    gltf.asset; // Object

                }
            );

            function render() {
                // render using requestAnimationFrame
                requestAnimationFrame(render);
                renderer.render(scene, camera);
            }

            render();
        }
        window.onload = init

    </script>
</body>
</html>
```
