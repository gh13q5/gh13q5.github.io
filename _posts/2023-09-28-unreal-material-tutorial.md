---
title: (작성 중) 언리얼 머티리얼 기초
author: gh13
date: 2023-09-28 16:32:00 +0800
categories: [Unreal Engine 5, Material]
tags: [ue5, material]
render_with_liquid: false
image:
  path: /assets/img/post_img/2023-09-28-02.png
---

> 해당 게시글은 언리얼 엔진 5.1 공식 문서의 [머티리얼 항목](https://docs.unrealengine.com/5.1/ko/essential-unreal-engine-material-concepts/)을 공부하며 작성했습니다.
{: .prompt-info }


## 머티리얼(Material)이란?

머티리얼은 씬에서 오브젝트의 표면 프로퍼티(속성)을 정의한다.  
  
즉, `물건의 표면을 표현하는 것이 머티리얼`이다. 나무로 된 가구의 표면이 많이 거친지, 금속 손잡이의 표면이 많이 거친지, 유리가 얼마나 투명한지 등을 표현하게 된다.
언리얼 공식 문서에서는 메시에 적용되어 시각적인 형태를 제어하고 보여주는 일종의 '페인트'로, 메시의 표면이 씬의 라이트와 어떻게 상호작용해야 하는지를 엔진에 알려주는 것이 머티리얼이하고 나와 있다.

## 셰이딩 파이프라인과 비주얼 스크립팅

셰이딩이라고 하면 보통 `HLSL(High Level Shading Language)` 코드를 작성하는 일부터 떠올리지만, 언리얼에서는 HLSL 코드 대신 `머티리얼 에디터`라는 비주얼 스크립팅 인터페이스를 사용한다.
머티리얼 표현식이라는 노드를 조합해 가장 오른쪽에 있는 최종 노드, `메인 머티리얼 노드`에 입력하면 자동으로 HLSL로 변환되어 머티리얼을 생성한다. 언리얼에서도 이 HLSL 코드를 볼 수는 있지만 읽기 전용이라 편집은 불가능하다.

> 본래 [렌더링 파이프라인](https://ko.wikipedia.org/wiki/%EA%B7%B8%EB%9E%98%ED%94%BD%EC%8A%A4_%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8)의 [셰이더](https://ko.wikipedia.org/wiki/%EC%85%B0%EC%9D%B4%EB%8D%94)는 각 버텍스나 픽셀이 렌더링되는 방법을 정의하는 프로그램이다.
> 작성한 HLSL 코드가 GPU 하드웨어가 실행할 수 있는 어셈블리 언어로 변환되고, 최종 픽셀의 색을 디스플레이에 출력하는 과정을 거쳐야 사용자도 머티리얼을 볼 수 있다.
{: .prompt-info }

## 메인 머티리얼 노드

### 베이스 컬러(Base Color)

### 메탈릭(Metalic)

### 스페큘러(Specular)

### 러프니스(Roughness)

### 애니소트로피(Anisotropy)

### 이미시브 컬러(Emissive Color)

### 오퍼시티(Opacity)

### 오퍼시티 마스크(Opacity Mask)

### 노멀(Normal)

### 월드 포지션 오프셋(World Position Offset)

### 서브서피스 컬러(Subsurface Color)

### 앰비언트 오클루전(Ambient Occlusion)

### 리프렉션(Refraction)

### 픽셀 뎁스 오프셋(Pixel Depth Offset)

### 셰이딩 모델(Shading Model)
