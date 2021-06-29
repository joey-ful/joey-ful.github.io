---
title: '[Solar System] Elastic Drag 고무줄처럼 따라오는 드래그 인터렉션 - 삼각함수편'
category: "FrontEnd"
date: "2021-02-18 12:00:00 +09:00"
desc: "elastic drag"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

> **이번 글에서 다룰 내용**
- 고무줄처럼 따라다니는 드래그 인터렉션
  - 기존 드래그 - 마우스에 붙어 움직이는 드래그
  - Elastic Drag
- Elastic한 드래그 인터렉션 적용하기
  - 행성을 움직일 때 - this.x, this.y
  - 공전 중심이 움직일 때 - this.orbitRadius
  
[Solar System 애니메이션 참고](/canvas/solar-system)<br>
[Solar System 애니메이션 드래그 인터렉션 참고](/canvas/solar-system-drag)<br>
[Solar System 배경 레이어 추가 버전 참고](/canvas/solar-system-drag-upgrade)

# 고무줄처럼 따라다니는 드래그 인터렉션

![elasticdrag](https://user-images.githubusercontent.com/52592748/108351949-81b0b800-7229-11eb-937f-afcf13c5879e.gif)

## 기존 드래그 - 마우스에 붙어 움직이는 드래그

- 먼저, 앞서 구현했던 [일반 드래그 애니메이션](/canvas/solar-system-drag)을 살펴보면 행성을 드래그하는 동안은 마우스 커서가 곧 행성의 중심 좌표가 된다 

```javascript
//planet.js
,,,

update(ctx) {
  ,,,
  
  if (this.clicked) {
  this.x += (this.mouse.x - this.x) * 0.05;
  this.y += (this.mouse.y - this.y) * 0.05;
  }
}

onMouseMove = (e) => {
  this.mouse.x = e.clientX - this.offsetX;
  this.mouse.y = e.clientY - this.offsetY;
}
```
- 행성을 드래그하면 마우스 커서에 붙은 채로 움직이는 것이다
- A라는 초록 공을 오른쪽으로 드래그하는 것을 상상해보면 아래와 같을 것이다

![image](https://user-images.githubusercontent.com/52592748/108369369-de6a9d80-723e-11eb-94c9-eef13488a6fd.png)


## Elastic Drag

- 이번에는 공이 커서를 뒤늦게 따라오는 것을 상상해보자
- B에서 A까지 커서를 이동했고 공은 아직 B에 머물러있는 상황이라 가정해보자
  - A: 커서
  - B: 공 위치
  
![image](https://user-images.githubusercontent.com/52592748/108370754-56859300-7240-11eb-8526-ea8cdff9495f.png)

- B는 천천히 커서가 위치한 A까지 이동한다

![image](https://user-images.githubusercontent.com/52592748/108370996-a4020000-7240-11eb-982c-e78daebb07ed.png)

- 커서와 공의 x좌표 거리 `mouse.x - this.x`를 균일한 간격으로 쪼개 그 단위만큼 이동시키면 공은 아마 균일한 속도로 천천히 A에 도달할 것이다

```javascript
let distance = this.mouse.x - this.x
this.x += distance / 10
```

![newelastic](https://user-images.githubusercontent.com/52592748/108372457-322ab600-7242-11eb-95d2-1cb2b42af2e0.gif)

- 조금 더 고무줄같은 효과를 주기 위해 처음엔 빨리 움직이다가 거리가 가까워질 수록 속도가 0에 수렴하도록 할 수 있다
- 행성의 위치 `this.x, this.y`는 커서와의 거리 차의 5%씩을 더해준다
  - 거리가 가까워질 수록 더해지는 값은 줄어들고
  - 커서와 행성이 동일한 곳에 위치하게 되면 아무런 값도 더해지지 않는다

```javascript
  this.x += (this.mouse.x - this.x) * 0.05;
  this.y += (this.mouse.y - this.y) * 0.05;
```

- 균일한 속도로 이동하는 모습과 미미하지만 약간의 차이가 있다
![originalelastic](https://user-images.githubusercontent.com/52592748/108372476-37880080-7242-11eb-8652-8b1933edad98.gif)

# Elastic한 드래그 인터렉션 적용하기

- 이번에는 [기존 drag 인터렉션과 배경 레이어가 적용된 Solar System](https://github.com/joey-ful/SolarSystem/tree/starry-night-background)에 elastic drag를 적용해보려고 한다
- Elastic drag는 두 곳에 적용하려고 한다
  - 행성을 직접 클릭해서 움직일 때 커서를 뒤늦게 따라오는 효과
  - 공전의 중심이 움직일 때 그 주변을 공전하고 있는 행성들이 뒤늦게 따라오는 효과
- 즉, 행성 본인을 움직일 때와 공전의 중심이 움직일 때로 나눠서 생각해볼 수 있다

## 행성을 움직일 때 - this.x, this.y

![elasticself](https://user-images.githubusercontent.com/52592748/108459370-dbf95980-72b9-11eb-91d5-657026dd9480.gif)

- 매 프레임 커서와 자신의 거리의 5%만큼 커서쪽으로 이동하도록 설정해주었다

```javascript
//planet.js
update(ctx) {
  ,,,

  if (this.clicked) {
    this.x += (this.mouse.x - this.x) * 0.05;
    this.y += (this.mouse.y - this.y) * 0.05;
  }

  ...
}
```

- 그럼 이렇게 커서를 천천히 고무줄같은 움직임으로 천천히 따라다니는 행성을 구현할 수 있다

![onlythiselastic2](https://user-images.githubusercontent.com/52592748/108467975-6301fe00-72c9-11eb-8fc2-aa663d19105d.gif)


- 하지만 아직 태양을 움직였을 때, 그 주변을 공전하는 행성들은 태양과 일정한 거리를 유지한채 따라다닌다
- 태양이 이동하면 행성들이 고무줄처럼 태양을 따라다니는 것도 구현해보도록 하자

![onlythiselastic](https://user-images.githubusercontent.com/52592748/108467848-3352f600-72c9-11eb-8ebb-4f6302f04ed4.gif)



## 공전의 중심이 움직일 때 - this.orbitRadius

![orbitRadiusElastic](https://user-images.githubusercontent.com/52592748/108468221-becc8700-72c9-11eb-8a9f-8ea9020ec422.gif)

- 행성의 자체의 elastic한 드래그가 클릭되었는지의 여부와 마우스 커서에 의존한다면
- 이번 효과는 오로지 행성과 공전 중심 사이의 거리에만 의존한다고 볼 수 있다
- 따라서 클릭 이벤트나 커서를 신경쓸 필요가 없다

### 공전거리 orbitRadius에 적용한 elastic drag

- 아래처럼 태양과 지구가 있다고 해보자. 둘 사이의 거리는 `this.orbitRadius`로 설정되어 있으며 `this.distanceFromCenter`는 실제 거리를 나타낸다

![image](https://user-images.githubusercontent.com/52592748/108470004-9abe7500-72cc-11eb-8cf2-f489d5970992.png)

- 여기서 태양을 우측으로 이동시켜버리면 실제 거리 `this.distanceFromCenter`와 `this.orbitRadius`는 서로 다른 값을 갖게 된다

![image](https://user-images.githubusercontent.com/52592748/108470018-a14cec80-72cc-11eb-926e-18b8e3b74ba7.png)

- 실제 거리를 매번 계산한 다음에 그 값이 orbitRadius에 점점 근접하도록 하면 된다

![image](https://user-images.githubusercontent.com/52592748/108470042-aca01800-72cc-11eb-922a-ac7440a9d903.png)

- distanceFromCenter는 orbitRadius를 구할 때처럼 피타고라스를 사용하면 된다

```javascript
this.distanceFromCenter = Math.sqrt(
  Math.pow(this.star.x - this.x, 2) + Math.pow(this.star.y - this.y, 2)
);
```

- 그리고 elastic하게 변하도록 하기 위해서 orbitRadius까지의 남은 거리의 특정 비율을 더해주면 된다
- 앞서 5%를 더해준 것과 달리 이번엔 20%를 더해주었다
  - 안 그러면 달이 지구와 자꾸 충돌하기에 그런 것인데 이건 달의 속도, 달의 공전거리, 지구의 속도 등을 변경해서 해결해도 된다. 비율은 원하는 대로 설정하면 된다

```javascript
this.distanceFromCenter += (this.orbitRadius - this.distanceFromCenter) * 0.2;
```

### 구현

- 먼저 `this.distanceFromCenter` 변수를 선언해준다

```javascript
//planet.js
constructor(name, star, radius, color, velocity, orbitRadius) {
  ,,,

  this.distanceFromCenter = orbitRadius;
}
```

- 그리고 행성의 초기 좌표 `this.x, this.y`도 선언해준다
- update 함수에서 `this.distanceFromCenter`를 계산하기 위해 사용되는 값이므로 미리 제대로 된 값을 지정해줄 필요가 생겼기 때문이다

```javascript
//planet.js
constructor(name, star, radius, color, velocity, orbitRadius) {
  ,,,
  
  this.x = star.x + orbitRadius * Math.cos(this.theta);
  this.y = star.y + orbitRadius * Math.sin(this.theta);
}
```

- 이번엔 업데이트 함수에서 클릭되지 않은 경우, 즉 else에 구현한다
- `this.orbitRadius` 대신 `this.distanceFromCenter`를 바탕으로 행성의 위치 `this.x, this.y`를 계산했다

```javascript
//planet.js
update(ctx) {
  ...
  
  if (this.clicked) {
    
    ...
    
  } else {
    
    this.distanceFromCenter = Math.sqrt(
      Math.pow(this.star.x - this.x, 2) + Math.pow(this.star.y - this.y, 2)
    );
    
    this.distanceFromCenter +=
      (this.orbitRadius - this.distanceFromCenter) * 0.2;
    
    this.x = this.star.x + this.distanceFromCenter * Math.cos(this.theta);
    this.y = this.star.y + this.distanceFromCenter * Math.sin(this.theta);
  }

  this.draw(ctx);
}
```

- 커서나 클릭 이벤트와는 무관하므로 이것으로 끝이다

### [Elastic Drag 완성코드](https://github.com/joey-ful/SolarSystem/tree/elastic-drag)