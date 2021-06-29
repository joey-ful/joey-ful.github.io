---
title: '[Solar System] 둥근 그림자 드리우기 (Bezier Curve) - 삼각함수편'
category: "FrontEnd"
date: "2021-02-22 12:00:00 +09:00"
desc: "orbit, trigonometry, drag, interaction, bezier"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

> **이번 글에서 다룰 내용**<br><br>
>둥근 그림자 효과 추가
>- 그림자 시작 좌표 구하기
>- Bezier Curve로 그림자 그리기
>- 행성 일러스트 추가하기
  
[Solar System 애니메이션 참고](/canvas/solar-system)<br>
[Solar System 애니메이션 드래그 인터렉션 참고](/canvas/solar-system-drag)<br>
[Solar System 배경 레이어 추가 버전 참고](/canvas/solar-system-drag-upgrade) <br>[Solar System Elastic Drag 참고](/canvas/solar-system-elastic-drag) <br>
[Solar System 직사각형 그림자 참고](/canvas/solar-system-shadow)

![roundshadow1](https://user-images.githubusercontent.com/52592748/108710894-ff413480-7557-11eb-96da-03343115e1dd.gif)

# 둥근 그림자 효과 추가


- 직사각형 그림자가 어색한 면이 있어 이번엔 둥근 그림자를 그려보려 한다
![image](https://user-images.githubusercontent.com/52592748/108697978-e4b28f80-7546-11eb-9a0f-84269d2d20a8.png)


- 둥근 그림자는 직사각형 그림자와 달리 직선 하나로 표현할 수 없고 그림자 도형을 직접 그려 색을 채워넣어야 한다


## 그림자 좌표 구하기

### A, B 좌표 계산하기
![image](https://user-images.githubusercontent.com/52592748/108699756-0280f400-7549-11eb-8a3d-0c66165870ba.png)


- 먼저 그림자의 네 좌표를 구해야 한다
- A와 B만 구하면 C, D는 쉽게 구할 수 있다
  - [Solar System 직사각형 그림자](/canvas/solar-system-shadow) 참고

```javascript
Cx = Bx + Dist * Math.cos(theta);
Cy = By + Dist * Math.sin(theta);
Dx = Ax + Dist * Math.cos(theta);
Dy = Ay + Dist * Math.sin(theta);
```
![image](https://user-images.githubusercontent.com/52592748/108699826-1d536880-7549-11eb-9766-12dd5751d2e2.png)

- A, B는 태양의 중심과 T 자 형태를 이루는 지점에 위치한다
  - A와 B를 잇는 직선은 지구의 중심을 지나며, 이는 태양과 지구를 잇는 직선와 직각이다

![image](https://user-images.githubusercontent.com/52592748/108699968-4ffd6100-7549-11eb-859d-6b607412c905.png)

- 지구를 확대해서 살펴보자
- 분홍색 선은 태양의 중심과 지구의 중심을 잇는 선으로 둘 사이의 각도는 θ 라 하자
- 지구의 중심에서 A까지 회색 선을 그어 직각삼각형을 만들어보면 A쪽의 각도가 θ가 됨을 알 수 있다

![image](https://user-images.githubusercontent.com/52592748/108699600-c352a300-7548-11eb-9c48-3cd7dc647475.png)

- `Ax`는 `this.x`에서 `r * sin(θ)` 만큼 우측으로(+) 이동했고
- `Ay`는 `this.y`에서 `r * cos(θ)` 만큼 위로(-) 이동했다

![image](https://user-images.githubusercontent.com/52592748/108700579-25f86e80-754a-11eb-8a62-19f39dbbcd3d.png)

- 이를 식으로 표현하면 아래와 같다

```javascript
Ax = this.x + this.radius * Math.sin(this.theta);
Ay = this.y - this.radius * Math.cos(this.theta);
Bx = this.x - this.radius * Math.sin(this.theta);
By = this.y + this.radius * Math.cos(this.theta);
```

### 구현
- 그림자는 drawShadow() 함수에 구현해주며 이는 update()에서 호출한다
- 매번 계산하기 번거로우니 x축, y축 거리를 findXYdistance()에서 계산해 변수에 담아준다
  - 그림자의 길이는 `this.radius + 20` 으로 설정한다 (Dist)
  
```javascript
//planet.js
findXYdistance() {
  this.radiusX = this.radius * Math.sin(this.theta);
  this.radiusY = this.radius * Math.cos(this.theta);
  this.shadowLengthX = (this.radius + 20) * Math.cos(this.theta);
  this.shadowLengthY = (this.radius + 20) * Math.sin(this.theta);
}
```

- findShadowPoints() 함수를 생성해 A, B, C, D를 계산해준다

```javascript
//planet.js
drawShadow(shadowctx) {
  if (this.name !== 'sun' && this.name !== 'moon) {
  this.findXYdistance();
  this.findShadowPoints();
  
  ...
  }
}

...
  
findShadowPoints() {
  this.Ax = this.x + this.radiusX;
  this.Ay = this.y - this.radiusY;
  this.Bx = this.x - this.radiusX;
  this.By = this.y + this.radiusY;
  this.Cx = this.Bx + this.shadowLengthX;
  this.Cy = this.By + this.shadowLengthY;
  this.Dx = this.Ax + this.shadowLengthX;
  this.Dy = this.Ay + this.shadowLengthY;
}
```

- A, B 좌표가 잘 구해졌는지 해당 위치에 각각 핑크색과 cyan색 점을 찍어보자

```javascript
//planet.js
test(shadowctx) {
  if (this.name !== 'sun' && this.name !== 'moon') {
    shadowctx.beginPath();
    shadowctx.arc(this.Ax, this.Ay, 2, 0, Math.PI * 2);
    shadowctx.fillStyle = 'hotpink';
    shadowctx.fill();
    shadowctx.closePath();

    shadowctx.beginPath();
    shadowctx.arc(this.Bx, this.By, 2, 0, Math.PI * 2);
    shadowctx.fillStyle = 'cyan';
    shadowctx.fill();
    shadowctx.closePath();
  }
}
```

- 행성과 함께 공전하며 제대로 그려지는 것을 확인할 수 있다

![ABtest](https://user-images.githubusercontent.com/52592748/108705107-37dd1000-7550-11eb-88a1-c53466e6d590.gif)

## Bezier Curve로 그림자 그리기

![checkbezier](https://user-images.githubusercontent.com/52592748/108705494-c2be0a80-7550-11eb-9518-60dfe6313ff4.gif)

[[W3 School] Bezier Curve](https://www.w3schools.com/tags/canvas_beziercurveto.asp) 참고

- Bezier curve를 하나 그리기 위해서는 좌표가 네 개 필요하다
- 시작점 A와 도착점 B 사이에 control point 두 개를 설정한다
- CP1과 CP2는 각각 태양과 지구를 잇는 직선과 평행이 되는 지점에 위치한다 (ACP1 // BCP2 // 태양중심부터지구중심을잇는직선)

![image](https://user-images.githubusercontent.com/52592748/108706450-ffd6cc80-7551-11eb-8279-022a4e207bef.png)

- 각각의 control point는 똑같이 삼각함수를 이용해 구할 수 있다
- 여기서 얼만큼 깊게 둥글게 할지를 정해줘야하는데 this.radius / 1.5 정도로 정했다
  - 이 값이 커지면 곡선이 더 많이 꺾이고 이 값이 작아질수록 AB직선에 가까워진다

![image](https://user-images.githubusercontent.com/52592748/108707236-13366780-7553-11eb-81e4-d841e0c3933c.png)

- control point 좌표를 구하기 위해 위의 계산식을 BezierX와 BezierY에 담아준다

```javascript
//planet.js
findShadowCurveDepth() {
  this.BezierX = (this.radius / 1.5) * Math.cos(this.theta);
  this.BezierY = (this.radius / 1.5) * Math.sin(this.theta);
}
```

- 이제 A에서 B를 잇는 커브를 그려보자
  - moveTo로 시작점 A로 이동 한다
  - bezierCurveTo에 CP1, CP2, B의 좌표를 넣는다

```javascript
//planet.js
drawShadow(shadowctx) {
  ,,,
    
  shadowctx.beginPath();
  shadowctx.moveTo(this.Ax, this.Ay);
  shadowctx.bezierCurveTo(
    this.Ax + this.BezierX,
    this.Ay + this.BezierY,
    this.Bx + this.BezierX,
    this.By + this.BezierY,
    this.Bx,
    this.By
  );
  
  shadowctx.lineWidth = 5;
  shadowctx.strokeStyle = 'rgba(255, 255, 255, 0.5)';
  shadowctx.stroke();
  shadowctx.closePath();
}
```

- 선 두께와 색상을 지정해 그려보면 아래처럼 A와 B까지의 커브가 그려진 것을 확인할 수 있다

![bezierdrawn](https://user-images.githubusercontent.com/52592748/108709965-be94eb80-7556-11eb-83da-f9da4c49db71.gif)

- 지금 좌표는 B에 있다
- 아래 그림을 완성하기 위해 C까지 선을 이어주고
- C에서 D까지 곡선을 그려준다 (곡선의 모양은 같게 해준다)
- 마지막으로 D에서 A까지 이어주면 된다
  
![image](https://user-images.githubusercontent.com/52592748/108705680-fb5de400-7550-11eb-8f43-02849196e405.png)

- drawShadow()에 마저 추가해주면 된다

```javascript
//planet.js
drawShadow(shadowctx) {
  if (this.name !== 'sun' && this.name !== 'moon') {
    this.findXYdistance();
    this.findShadowCurveDepth();
    this.findShadowPoints();

    //AB 커브 그리기
    ,,,
    
    shadowctx.lineTo(this.Cx, this.Cy);
    
    shadowctx.bezierCurveTo(
      this.Cx + this.BezierX,
      this.Cy + this.BezierY,
      this.Dx + this.BezierX,
      this.Dy + this.BezierY,
      this.Dx,
      this.Dy
    );
    
    shadowctx.lineTo(this.Ax, this.Ay);

    ,,,
  }
}
```

- 그림자 테두리가 잘 그려진 것을 확인할 수 있다

![roundshadowoutline](https://user-images.githubusercontent.com/52592748/108710446-69a5a500-7557-11eb-8c44-0b88278126ed.gif)

- stroke()대신 fill을 이용해서 색을 채워주면 된다
- 그림자 색상은 배경색과 같은 것을 사용해 행성 위에서만 드리워진 것처럼 보이도록 한다

```javascript
//planet.js
drawShadow() {
  ,,,
    
  // shadowctx.lineWidth = 5;
  // shadowctx.strokeStyle = 'rgba(255, 255, 255, 0.5)';
  // shadowctx.stroke();
    
  shadowctx.fillStyle = 'rgba(33, 69, 104, 0.4)';
  shadowctx.fill();
  
  shadowctx.closePath();
}
```

- test() 함수까지 지워주면 자연스러운 그림자가 완성된다

![roundshadow1](https://user-images.githubusercontent.com/52592748/108710894-ff413480-7557-11eb-96da-03343115e1dd.gif)

## 문제들

### 그림자 길이

- 그림자가 맞아떨어지도록 orbitRadius를 각각 조정해 행성을 배치해뒀는데 초록색 행성이 너무 커서 빨간 행성이 드리우는 그림자가 너무 짧다
- 그리고 하늘색 행성이 빨간행성에 드리우는 그림자도 짧다
  - 아래 모양은 theta = 0, velocity = 0으로 설정하고 보면 된다

![image](https://user-images.githubusercontent.com/52592748/108712163-b9856b80-7559-11eb-8c0b-1b9372c85cbc.png)

- 빨간 행성과 하늘색 행성의 경우에만 그림자 길이를 길게 설정해주는 방식으로 임시로 해결했다

```javascript
findXYdistance() {
  ,,,
    
  if (this.name === 'mars') {
    this.shadowLengthX = (this.radius + 46) * Math.cos(this.theta);
    this.shadowLengthY = (this.radius + 46) * Math.sin(this.theta);
  } else if (this.name === 'earth') {
    this.shadowLengthX = (this.radius + 30) * Math.cos(this.theta);
    this.shadowLengthY = (this.radius + 30) * Math.sin(this.theta);
  }
}
```

### 그림자 색상

- 그림자의 색상을 배경색으로 해줬기 때문에 배경의 그래디언트 색상 중 바깥쪽으로 이동하면 그림자를 볼 수 있다
- 보기 좋지 않다는 문제가 있다

![image](https://user-images.githubusercontent.com/52592748/108712904-cfdff700-755a-11eb-94c8-54c5869a7014.png)


### 그림자 위치

- 그림자는 shadow-cavas라는 별도의 상위 캔버스에 그려졌다
- 따라서 모든 그림자는 그 어떤 행성보다도 상위에 그려지게 된다
- 아래처럼 행성들이 겹쳐질 경우 행성이 다른 행성에 가려져도 그 행성의 그림자는 그대로 그려지는 기이한 현상이 발생한다

![image](https://user-images.githubusercontent.com/52592748/108712946-dff7d680-755a-11eb-8e93-344fbaed5ad1.png)


## [완성 코드](https://github.com/joey-ful/SolarSystem/tree/shadow-round)