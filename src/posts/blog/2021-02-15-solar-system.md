---
title: '[Solar System] 태양을 중심으로 공전하는 행성 애니메이션 - 삼각함수편'
category: "FrontEnd"
date: "2021-02-15 12:00:00 +09:00"
desc: "trigonometry"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---


> **이번 글에서 다룰 내용**<br>
> 공전하는 행성 애니메이션

# 공전하는 행성 애니메이션

![solarsystem](https://user-images.githubusercontent.com/52592748/107965018-5772b680-6fed-11eb-99ee-aab74703420c.gif)

- 위 그림처럼 노란 공(태양) 주변을 orbit하는 하늘색 공(지구)를 만들어보려 한다

## 삼각함수를 이용한 공전

- 태양과 지구부터 살펴보자
- 먼저 태양과 지구의 중심을 각각 `(star.x, star.y)`와 `(this.x, this.y)`라고 해보자

![image](https://user-images.githubusercontent.com/52592748/108583488-1677fa80-737d-11eb-888a-41adb0fe673e.png)

- 지구는 태양으로부터 `orbitRadius`만큼 거리를 두고 회전하며 회전각은 `θ`로 매 프레임 바뀔 예정이다
- 지구의 중심 `(this.x, this.y)`는 회전각 `θ`에 따라 매 프레임 값이 바뀔 것이다

### 지구의 중심

#### 1번 지구의 중심

- 지구의 x좌표 `this.x`는 태양의 x좌표 `star.x`에 `orbitRadius`만큼을 더한 값이다
- 태양과 지구의 y좌표는 `this.y`로 같다
- 따라서 1번 지구의 중심은 `(star.x + orbitRadius, this.y)`다

#### 2번 지구의 중심

- 2번 지구의 중심은 삼각함수를 이용해 구할 수 있다
- 회전각인 회색 `θ` 는 그 **엇각**인 분홍 `θ`와 같다

![image](https://user-images.githubusercontent.com/52592748/108583493-1ed03580-737d-11eb-90d9-0210988bdbea.png)

- 지구의 x좌표 `this.x`는 태양의 x좌표 `star.x`에 `orbitRadius * cos(θ)`를 더한 값이다
- 지구의 y좌표 `this.y`는 태양의 y좌표 `star.y`에 `orbitRadius * sin(θ)`를 더한 값이다

![image](https://user-images.githubusercontent.com/52592748/108583502-2bed2480-737d-11eb-90e8-ba8b723fd0ef.png)

- 지구의 중심을 구하기 위해서는 다음 세가지가 필요하다
  - 태양의 중심 `(star.x, star.y)`
  - 공전거리 `orbitRadius`
  - 공전각 `theta`

### 공전 구현

- 태양, 지구, 달은 planet이라는 클래스를 사용해 제작한다 (태양은 star고 moon은 satellite지만 편의상 planet이라 지칭할 예정)
- [코드 기본 세팅 참고 글](/canvas/default-code-setting)
- [코드 기본 세팅 참고 깃헙](https://github.com/joey-ful/CanvasAnimationDefault)


#### planet.js

```javascript
//planet.js
export default class Planet {
  constructor(name, star, radius, color, velocity, orbitRadius) {
    this.name = name;
    this.star = star;
    this.radius = radius;
    this.color = color;
    this.velocity = velocity;
    this.orbitRadius = orbitRadius;

    this.theta = Math.random() * Math.PI * 2;
  }

  update(ctx) {
    this.theta += this.velocity;

    this.x = this.startX + this.orbitRadius * Math.cos(this.theta);
    this.y = this.startY + this.orbitRadius * Math.sin(this.theta);

    this.draw(ctx);
  }

  draw(ctx) {
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
    ctx.fillStyle = this.color;
    ctx.fill();
    ctx.closePath();
  }
}
```

#### 코드 설명

- 공전의 중심을 `star`로 받았다
  - 태양 공전의 중심은 sunStar로 따로 구현할 예정
  - 지구 공전의 중심은 태양
  - 달 공전의 중심은 지구

```javascript
export default class Planet {
  constructor(name, star, radius, color, velocity, orbitRadius) {}
}
```

- update 함수에서 매번 행성의 좌표를 계산한다
- theta는 velocity만큼 매 프레임 증가한다
  - 엄밀히 따지면 velocity는 각도의 증가량이다
  - theta는 radian으로 0 ~ 2π 단위로 한 원을 구성한다
  - [unit circle](https://www.mathsisfun.com/geometry/unit-circle.html) 참고

```javascript
update(ctx) {
  this.theta += this.velocity;
}
```

#### app.js

```javascript
//app.js
import Planet from './planet.js';

class App {
  constructor() {

    ...

    this.createPlanets();
    this.animate();
  }

  ...

  createPlanets() {
    this.planets = [];

    this.sunStar = {
      x: this.stageWidth / 2,
      y: this.stageHeight / 2,
    };

    this.sun = new Planet(this.sunStar, 100, '#FFF599', 0, 0);
    this.earth = new Planet(this.sun, 14, '#AAE7F0', 0.006, 170);
    this.moon = new Planet(this.earth, 4, '#F0F7F8', 0.02, 28);

    this.planets.push(this.sun);
    this.planets.push(this.earth);
    this.planets.push(this.moon);
  }

  animate() {
    window.requestAnimationFrame(this.animate.bind(this));

    this.ctx.fillStyle = '#4D5769';
    this.ctx.fillRect(0, 0, this.stageWidth, this.stageHeight);

    this.planets.forEach((planet) => {
      planet.update(this.ctx);
    });
  }
}
```

- this.sunStar는 태양 공전의 중심으로 캔버스 중심 좌표를 지닌다
  - 태양은 캔버스 중앙에 고정시킬 것이므로 엄밀히 따지면 공전의 중심이 필요하지는 않다
  - 애니메이션의 위치라고 봐도 무방하다
- 태양은 공전 중심으로 this.sunStar를, 지구는 태양을, 그리고 달은 지구를 갖는다
- 이번에는 ctx.clearRect대신 fillRect를 사용해 바탕색을 채워주었다

```javascript
animate() {
  this.ctx.fillStyle = '#4D5769';
  this.ctx.fillRect(0, 0, this.stageWidth, this.stageHeight);
}
```

![earthorbit](https://user-images.githubusercontent.com/52592748/107959996-6bb3b500-6fe7-11eb-999b-f6f485097e9c.gif)

### 태양계 애니메이션

- 이번에는 태양계의 행성들도 추가해보자
- 모든 별과 행성, 위성의 정보를 담은 배열을 생성해주었다
- 공전의 중심, 반지름, 색, 속도, 공전거리 등이 담겨있다
  - 색상, 크기, 속도, 거리 등은 무작위

```javascript
//planetInfo.js
export const PlanetInfo = [
  {
    id: 0,
    name: 'sun',
    star: 'sunStar',
    radius: 80,
    color: '#FFF599',
    velocity: 0,
    orbitRadius: 0,
  },
  {
    id: 1,
    name: 'mercury',
    star: 'sun',
    radius: 5,
    color: '#74B1E9',
    velocity: 0.008,
    orbitRadius: 120,
  },
  
  ...
  
];
```

- 인스턴스 생성은 반복문을 사용했다

```javascript
//app.js
import Planet from './planet.js';
import {PlanetInfo} from './planetInfo.js';

class App{

  ...

  createPlanets() {
    this.planets = [];

    this.sunStar = {
      x: this.stageWidth / 2,
      y: this.stageHeight / 2,
    };

    PlanetInfo.forEach((planet) => {
      this[planet.name] = new Planet(
        planet.name,
        this[planet.star],
        planet.radius,
        planet.color,
        planet.velocity,
        planet.orbitRadius
      );

      this.planets.push(this[planet.name]);
    });
  }
}
```

![solarsystem](https://user-images.githubusercontent.com/52592748/107965018-5772b680-6fed-11eb-99ee-aab74703420c.gif)

## [완성 코드](https://github.com/joey-ful/SolarSystem/tree/a567bb07f4a541c2eb5880cd1359874d11cb92db)