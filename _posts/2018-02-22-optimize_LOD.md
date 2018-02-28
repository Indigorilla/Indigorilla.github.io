---
layout: post
title: "최적화 - LOD"
description: ""
date: 2018-02-22
tags: [최적화, R&D, LOD]
comments: true
share: true
---

# LOD

## 1. 캐릭터 LOD


카메라 절두체 기반으로 화면 밖일경우 On/Off  
거리에 따라 대역을 나눠 LOD 적용 (예 0~5M : LOD0, 5 ~ 7M : LOD1, 7 ~ 10M : LOD2)


### Unity 엔진 지원
[CullingGroup API](https://docs.unity3d.com/Manual/CullingGroupAPI.html) (Unity 5.1 or a later)  

관리하려는 오브젝트의 정보를 가지고 BoundingSphere 배열(카메라 절두체 체크)과 float 배열(거리 기반 구역체크)에 값 세팅 후 CullingGroup에 넘겨서 사용 하는 방식.  

보임/안보임, 거리 별 구역에 변경 사항이 있을 때마다  callback을 받아 처리 하게 할수도 있으며 직접 쿼리하여 확인하는 방법 둘 다를 제공.


## 2. 배경 LOD

### 2.1 [거리 기반 정적 LOD](https://docs.unity3d.com/Manual/LevelOfDetail.html)  
전통적인 거리 기반의 방식의 LOD로 유니티 엔진에서도 기본적으로 제공하는 LOD 시스템 

### 장점
```
빠름  

구현 및 적용의 간결함
```

### 단점
```
FOV를 쓰는 경우 LOD에 반영이 안됨  

개별 오브젝트로 쓰이기 때문에 드로우콜 자체는 줄일 수 없음  

아트팀에 작업 하중이 큼  
```

참고 자료  
http://ozlael.egloos.com/v/4068696

### 2.2 [AutoLOD](https://github.com/Unity-Technologies/AutoLOD) (Unity 2017.3 or a later)
계층적 LOD와 LOD Generation을 이용한 최적화 및 자동화 된 LOD 에셋 생성 시스템  

다만 현재 실험으로 피쳐 된 기능이라 주의가 필요함

### 장점

```
내부적으로 옥트리를 이용하여 계층적으로 LOD 대상을 관리 하여 거리 기반에 비해 좋은 성능  

자동화 된 LOD 에셋 생성 기능으로 아트팀의 추가 작업 없이 LOD 적용 가능  

오토 컴바인 기능으로 최대 LOD에서 드로우콜 감소를 기대 할 수 있음
```

### 단점
```
배타 기능

움직이거나 On/Off가 수시로 일어나는 오브젝트에 적용 시 옥트리를 갱신해야 함으로 성능이 더 나빠질수 있음

프로젝트에 적용 비용이 비교적 큼
```

#