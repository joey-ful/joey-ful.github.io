---
icon: "☀️"
title: '[Solar System] 드래그 효과 업그레이드 (trailing, 배경레이어) - 삼각함수편'
category: "FrontEnd"
date: "2021-02-18 12:00:00 +09:00"
desc: "layer, background, gradient"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

> **이번 글에서 다룰 내용**
드래그 효과 업그레이드
  - 1. trailing 효과
  - 2. 밤하늘 배경 레이어 추가
    - 레이어 추가하기
    - 배경 레이어 칠하기
    - 그래디언트 설정하기
    - 빛나는 별 그리기
    - 태양 빛나는 효과 넣기
  
[Solar System 애니메이션 참고](/canvas/solar-system)
<br>
[Solar System 애니메이션 드래그 인터렉션 참고](/canvas/solar-system-drag)

# 애니메이션 업그레이드
- [Solar System 드래그 인터렉션](/canvas/solar-system-drag)에서 태양을 드래그해보면, 태양을 드래그하는건지 화면 전체가 이동하는 건지 헷갈린다

![dragweird](https://user-images.githubusercontent.com/52592748/108348044-a35b7080-7224-11eb-9321-2883ac3abe60.gif)

- 드래그 되고 있다는 효과를 주기 위해 trailing효과를 주거나 단색이 아닌 배경을 그려볼 예정이다. 다만 둘 다 적용하는 방법은 찾지 못했다.
  - trailing 효과
  - 별이 빛나는 배경

## [1. trailing 효과](https://github.com/joey-ful/SolarSystem/commit/2022b7458fa9722289aa55be530b9da6e61241de)
![trailing](https://user-images.githubusercontent.com/52592748/108345252-63df5500-7221-11eb-931d-7d14f21ecea8.gif)

- trailing 효과는 간단하다
- 매 프레임 애니메이션을 그리기 전, clearRect로 캔버스를 지우거나 fillRect로 캔버스를 특정 색으로 칠해주었다
- 여기서 투명도가 들어간 색으로 칠해주면 지난번 애니메이션의 잔상이 남게 된다

```javascript
//app.js

...

animate() {
  window.requestAnimationFrame(this.animate.bind(this));
  
  // opacity 20%
  this.ctx.fillStyle = 'rgba(77, 87, 105, 0.2)';
  this.ctx.fillRect(0, 0, this.stageWidth, this.stageHeight);
}
```

## 2. 밤하늘 배경 레이어 추가

![background](https://user-images.githubusercontent.com/52592748/108346069-55de0400-7222-11eb-967a-c5d52b0ae648.gif)

- trailing 대신에 배경 레이어를 추가하면 드래그 효과가 뚜렷하게 보인다
- 고정된 배경을 그리려고하는데 이를 위해서는 두 가지 정도의 방법을 생각해볼 수 있다
  - 매 프레임 배경을 그리는 방법으로 비효율적이지만 간단하다
  - 새로운 레이어에 배경을 그리고 그 위에 애니메이션을 그려준다

### 레이어 추가하기
- 포토샵이나 피그마 등의 디자인 툴들은 레이어라는 기능을 제공한다
- 캔버스 역시 여러개 만들 수 있으며 각각의 캔버스에 그림을 그릴 수 있다
- 먼저 캔버스들을 한데 담을 수 있도록 stage를 생성했다

```javascript
//app.js
constructor() {
  this.stage = document.createElement('div');
  this.stage.setAttribute('id', 'stage');
  
  document.body.appendChild(this.stage);
}
```

- 캔버스는 모두 같은 위치에 오도록 position: absolute를 해줬다
- stage도 높낮이 등을 설정해줬다

```css
/*
  stylesheet.css
*/
canvas {
  ...
  position: absolute;
}

#stage {
  position: relative;
  width: 100%;
  height: 100%;
}
```

- 그 후 배경 캔버스를 만들어 stage에 넣어줬다
- 여기서 배경 캔버스를 먼저 append 해줘야지 배경 위에 그림이 그려진다
  - 더 위에 위치한 캔버스가 먼저 그려지는 것이라 추측
  ![image](https://user-images.githubusercontent.com/52592748/108316240-fff86480-71ff-11eb-9442-bc4f375ed261.png)
  - 캔버스에 z-index를 적용하는 방법도 있다

```javascript
//app.js
constructor() {
  this.backcanvas = document.createElement('canvas');
  this.backcanvas.setAttribute('id', 'backcanvas');
  this.backctx = this.backcanvas.getContext('2d');

  this.stage.appendChild(this.backcanvas);
  this.stage.appendChild(this.canvas);
}
```

- resize()를 할 때 배경 캔버스도 함께 사이즈 조정을 해야 한다

```javascript
//app.js
resize() {
  ...
  
  this.backcanvas.width = this.stageWidth * 2;
  this.backcanvas.height = this.stageHeight * 2;

  this.backctx.scale(2, 2);
}
```

### 배경 레이어 칠하기

![singlecolorbackground](https://user-images.githubusercontent.com/52592748/108346082-59718b00-7222-11eb-98a4-31be21296686.gif)

- 이제 배경 레이어가 생겼으니 배경을 그리는 클래스를 생성해보자
- background.js 단색으로 캔버스를 채우는 클래스를 만들었다
- 한번만 그릴 것이기 때문에 따로 draw()함수를 만드는 대신 constructor에서 바로 색칠하는 함수를 호출해주었다

```javascript
//background.js

export default class Background {
  constructor(ctx, stageWidth, stageHeight) {
    this.stageWidth = stageWidth;
    this.stageHeight = stageHeight;

    this.colorBackground(ctx);
  }

  colorBackground(ctx) {
    ctx.fillStyle = '#214568';
    ctx.fillRect(0, 0, this.stageWidth, this.stageHeight);
  }
}
```

- 방금 생성한 배경 그리기 클래스를 constructor에서 호출해주면 끝난다
- 주의할 것은 배경 컨텍스트를 인자로 넘겨줘야한다는 점이다
- 고정된 배경이기 때문에 animate() 함수에서 호출해줄 필요는 없다

```javascript
//app.js
constructor() {
  ...
  
  new Background(this.backctx, this.stageWidth, this.stageHeight);
  this.createPlanets();
  this.animate();
}
```

- 대신 애니메이션을 매번 그리는 캔버스는 fillRect로 칠하는 대신 clearRect로 비워준다

```javascript
//app.js
animate() {
  ...

  this.canvas.clearRect(0, 0, this.stageWidth, this.stageHeight);
}
```




### 그래디언트 설정하기

![gradientbackground](https://user-images.githubusercontent.com/52592748/108346087-5aa2b800-7222-11eb-984f-ef4bb4ac0787.gif)

- 이번에는 배경에 원형 그래디언트를 적용해보려고 한다
- 원형 그래디언트를 위해서는 그래디언트가 시작하는 원과 끝나는 원에 대한 정보가 필요하다
  - x0, y0, r0은 시작하는 원의 중심과 반지름
  - x1, y1, r1은 끝나는 원의 중심과 반지름이다

```javascript
CanvasGradient ctx.createRadialGradient(x0, y0, r0, x1, y1, r1);
```

- 그래디언트 시작원과 끝원을 모두 캔버스 중심을 설정하고 원의 반지름은 각각 너비의 1/3과 1/1.5로 설정해주었다
- gif를 잘 보면 중심으로부터의 거리가 너비의 1/3인 지점부터 1/1.5인 지점까지 그래디언트가 적용되었다

```javascript
colorBackground(ctx) {
  let gradient = ctx.createRadialGradient(
    this.stageWidth / 2,
    this.stageHeight / 2,
    this.stageWidth / 3,
    this.stageWidth / 2,
    this.stageHeight / 2,
    this.stageWidth / 1.5
  );
}
```

- 이번에는 그래디언트 색을 설정해 준다
  - 처음과 끝, 이렇게 두 색을 설정했다
- 그리고 fillStyle을 만든 gradient로 해주면 완성

```javascript
//background.js
colorBackground(ctx) {
  ...
  
  gradient.addColorStop(0, '#214568');
  gradient.addColorStop(1, '#162133');
  
  ctx.fillStyle = gradient;
}
```

### 빛나는 별 그리기

![starbackground](https://user-images.githubusercontent.com/52592748/108346323-99387280-7222-11eb-8167-e992de12b682.gif)

- 작은 원으로 별을 그리려고 한다
- 별의 중심과 반지름은 랜덤으로 생성한다

```javascript
let radius = Math.random() * 1 + 0.1;
let x = radius + Math.random() * (this.stageWidth - radius * 2);
let y = radius + Math.random() * (this.stageHeight - radius * 2);
```

- 별은 흰색으로 하며 shadowColor 또한 흰색으로 해준다
- Blur는 30으로 해서 빛나는 효과를 적용했다

```javascript
ctx.beginPath();
ctx.arc(x, y, radius, 0, Math.PI * 2);
ctx.fillStyle = 'white';
ctx.shadowColor = 'white';
ctx.shadowBlur = 30;
ctx.fill();
ctx.closePath();
```

- 이를 모두 fillStars 라는 함수에 넣고 constructor에서 바로 호출했다
- 별의 개수도 인자로 받도록 했다

```javascript
//background.js
constructor(startNumber, ctx, stageWidth, stageHeight) {
  ...

  this.starNumber = starNuber;
  fillStars(ctx);
}
```

- 이 함수에서는 별의 개수만큼 반복문을 돌려주었다

```javascript
//background.js
fillStars(ctx) {
  for (let i = 0; i < this.starNumber; i++) {
    let radius = Math.random() * 1 + 0.1;
    let x = radius + Math.random() * (this.stageWidth - radius * 2);
    let y = radius + Math.random() * (this.stageHeight - radius * 2);

    ctx.beginPath();
    ctx.arc(x, y, radius, 0, Math.PI * 2);
    ctx.fillStyle = 'white';
    ctx.shadowColor = 'white';
    ctx.shadowBlur = 30;
    ctx.fill();
    ctx.closePath();
  }
}
```

- 별의 개수는 처음 호출시 받아오도록 했다

```javascript
//app.js
export default class App {
  constructor() {
    
  ...
  
  //별 개수 400개로 전달
  new Background(400, this.backctx, this.stageWidth, this.stageHeight);
```

### 태양 빛나는 효과 넣기

![shiningsun](https://user-images.githubusercontent.com/52592748/108346094-5c6c7b80-7222-11eb-83fa-bd3747152b26.gif)

- 태양인 경우만 빛효과를 추가해보려고 한다
- 앞서 별이 빛나게 한 것처럼 shadowColor를 태양색으로 설정하고 shadowBlur를 줘서 빛나는 것처럼 보이게 했다

```javascript
//app.js
animate() {
  ...

  this.planets.forEach((planet) => {
    if (planet.name === 'sun') {
      this.ctx.save();
      this.ctx.shadowColor = planet.color;
      this.ctx.shadowBlur = planet.radius * 1.5;
      planet.update(this.ctx);
      this.ctx.restore();
    } else {
      planet.update(this.ctx);
    }
  });
}
```



## [밤하늘 완성 코드](https://github.com/joey-ful/SolarSystem/commit/090e314b17850a8d2a32f691dff2994b518e088a)