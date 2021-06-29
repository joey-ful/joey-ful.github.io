---
icon: "🏀"
title: '[Bouncing Ball] 윈도우 창에 부딪히는 공 애니메이션'
category: "FrontEnd"
date: "2021-02-05 12:00:00 +09:00"
desc: "bounce animation"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

# Bouncing Ball

> **이번 글에서 다룰 내용**

- 캔버스에 공 그리기
- 애니메이션 만들기

- [코드 기본 세팅 참고](/canvas/default-code-setting)
- [깃헙](https://github.com/joey-ful/CanvasAnimationDefault)

## 캔버스에 공 그리기

### [원 그리기](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/arc)

- beginPath로 시작점 알리기
- 원의 중심, 시작각도, 끝각도, 그리는 방향 설정

```javascript
ctx.beginPath();
ctx.arc(x, y, radius, startAngle, endAngle, (, anticlockwise));
```

#### 테두리 그리기

```javascript
ctx.stroke();
```

#### 색 채우기

- fillStyle로 채울 색 설정 (이미지패턴같은 것도 가능)
- fill()로 채움

```javascript
ctx.fillStyle = 'red';
ctx.fill();
```

### app.js

- 그림을 그릴 캔버스를 생성해준다
- Ball의 draw 함수를 호출해 ball을 그려준다

```javascript
//app.js
import {Ball} from './ball.js';

class App {
  constructor() {
    this.canvas = document.createElement('canvas');
    this.ctx = this.canvas.getContext('2d');

    document.body.appendChild(this.canvas);

    window.addEventListener('resize', this.resize.bind(this), false);
    this.resize();

    this.ball = new Ball(200, 200, 30, 15);
    this.ball.draw(this.ctx, 'red');
  }
}

new App();
```

### ball.js

- 공은 외부 파일에 클래스로 생성 (재사용하기 위해)

```javascript
//ball.js
export class Ball {
  constructor(x, y, radius, speed) {
    this.radius = radius;
    this.vx = speed;
    this.vy = speed;
    this.x = x;
    this.y = y;
  }

  draw(ctx, color) {
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, true);
    ctx.fillStyle = color;
    ctx.fill();
  }
}
```

- 200, 200에 빨간 공 그리기 성공
  ![](https://images.velog.io/images/jehjong/post/3c0dfc04-fc82-4823-9afb-8fa350b02d5a/image.png)

## 애니메이션 만들기

### requestAnimationFrame()

```
raf = window.requestAnimationFrame(callback)
window.cancelAnimationFrame(raf)
```

- 보통 1초에 60회 콜백 호출
- 콜백은 다음 리페인트를 위한 애니메이션을 업데이트하는 함수
- cancel로 취소 가능

```javascript
//app.js
class App {
  constructor() {
	...
    this.animate();
  }

  animate() {
    window.requestAnimationFrame(this.animate.bind(this));
    this.ball.draw(this.ctx, 'red', this.canvas.width, this.canvas.height);
  }
}

new App();
```

- animate 함수를 콜백함수로 지정
- animate 함수는 매 frame ball을 그리는 역할을 담당

![](https://images.velog.io/images/jehjong/post/d9adbd8a-f79e-4989-912a-375e7436803f/image.png)

### bounce effects

- 공이 윈도우창에 부딪히면 방향을 트는 기능을 추가
  - this.x와 this.y는 공 중심의 좌표를 뜻한다
  - 따라서 다음 상황에서는 방향을 틀도록 설정한다
    - 좌표가 반지름보다 작거나 같을 때 (this.x <= this.radius)
      ![](https://images.velog.io/images/jehjong/post/935fe44c-d1c1-42f1-a978-1c801afe7342/image.png)
    - 창크기에서 반지름을 뺀 것보다 좌표가 크거나 같을 때 (this.x >= canvasWidth - this.radius)
      ![](https://images.velog.io/images/jehjong/post/c48b0d31-941f-4edd-8083-35416e3959f3/image.png)

```javascript
//ball.js
export class Ball {
  ...
  draw(ctx, color, canvasWidth, canvasHeight) {
	...
    this.bounceWindow(canvasWidth, canvasHeight);
	...
  }

  bounceWindow(canvasWidth, canvasHeight) {
    if (this.x <= this.radius || this.x >= canvasWidth - this.radius) {
      this.vx *= -1;
    }
    if (this.y <= this.radius || this.y >= canvasHeight - this.radius) {
      this.vy *= -1;
    }
  }
}
```

- draw에 bounceWindow() 함수를 추가해 공이 벽에 닿을 때마다 방향을 바꾸도록 설정

![](https://images.velog.io/images/jehjong/post/2386a600-6d4a-4680-a536-3c4c765c404a/image.png)

#### bounce 마무리

- 자세히 보면 공의 끝이 윈도우 창을 넘어가있다
- 공이 방향을 틀 때 좌표를 조금 조정해주면 창 내부에서만 움직이도록 할 수 있다
  - 부호가 바뀐 vx와 vy를 더해주는 방식

```javascript
//ball.js
  bounceWindow(canvasWidth, canvasHeight) {
    if (this.x <= this.radius || this.x >= canvasWidth - this.radius) {
      this.vx *= -1;
      this.x += this.vx;
    }
    if (this.y <= this.radius || this.y >= canvasHeight - this.radius) {
      this.vy *= -1;
      this.y += this.vy;
    }
  }
```

![](https://images.velog.io/images/jehjong/post/ad3693bb-0e78-4abe-8724-afd82b8d403b/image.png)

### clearRect()

- 매 프레인 새로운 그림을 그렸지만 지난 프레임의 그림 또한 그대로 남아있기 때문에 따로 지워줘야한다

```javascript
ctx.clearRect(x, y, width, height);
```

- x, y 부터 시작하며 주어진 width와 height를 가진 직사각형만큼의 영역을 지운다

```javascript
//app.js
class App() {
  ...
  animate() {
    window.requestAnimationFrame(this.animate.bind(this));
    this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
    this.ball.draw(this.ctx, 'red', this.canvas.width, this.canvas.height);
  }
}
```

- 그리기 전에 매번 화면을 지워줌으로써 애니메이션을 완성시킬 수 있다

![](https://images.velog.io/images/jehjong/post/175a9a98-25a9-451f-b041-8e6c556fd89a/image.png)

### 랜덤 시작 위치

- 공이 매번 랜덤한 위치에서 시작하도록 좌표를 정해줄 수 있다

```javascript
//ball.js
constructor(this.stageWidth, this.stageHeight, radius, speed) {
  ...
  this.x = this.radius + Math.random() * (stageWidth - this.radius * 2);
  this.y = this.radius + Math.random() * (stageHeight - this.radius * 2);
}
```

- app.js에서의 인자도 변경

```javascript
//app.js
constructor() {
  ...
  this.ball = new Ball(this.stageWidth, this.stageHeight, 30, 15);
}
```

## 완성 코드

<details>
  <summary><b>Bouncing Ball 윈도우창에 튕기기 완성 코드</b></summary>
  <div markdown="1">


```javascript
//app.js
import {Ball} from './ball.js';

class App {
  constructor() {
    this.canvas = document.createElement('canvas');
    this.ctx = this.canvas.getContext('2d');

    document.body.appendChild(this.canvas);

    window.addEventListener('resize', this.resize.bind(this), false);
    this.resize();

    this.ball = new Ball(this.stageWidgth, this.stageHeight, 30, 15);
    this.animate();
  }

  resize() {
    this.stageWidth = document.body.clientWidth;
    this.stageHeight = document.body.clientHeight;

    this.canvas.width = this.stageWidth * 2;
    this.canvas.height = this.stageHeight * 2;

    this.ctx.scale(2, 2);
  }

  animate() {
    window.requestAnimationFrame(this.animate.bind(this));
    this.ctx.clearRect(0, 0, this.stageWidth, this.stageHeight);
    this.ball.draw(this.ctx, 'red', this.stageWidth, this.stageHeight);
  }
}

new App();
```

```javascript
//ball.js
export class Ball {
  constructor(stageWidth, stageHeight, radius, speed) {
    this.radius = radius;
    this.vx = speed;
    this.vy = speed;
    this.x = this.radius + Math.random() * (stageWidth - this.radius * 2);
    this.y = this.radius + Math.random() * (stageHeight - this.radius * 2);
  }

  draw(ctx, color, canvasWidth, canvasHeight) {
    this.x += this.vx;
    this.y += this.vy;

    this.bounceWindow(canvasWidth, canvasHeight);

    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, true);
    ctx.fillStyle = color;
    ctx.fill();
  }

  bounceWindow(stageWidth, stageHeight) {
    if (this.x <= this.radius || this.x >= stageWidth - this.radius) {
      this.vx *= -1;
      this.x += this.vx;
    }
    if (this.y <= this.radius || this.y >= stageHeight - this.radius) {
      this.vy *= -1;
      this.y += this.vy;
    }
  }
}
```

```css
/*
  stylesheet.css
*/
html {
  width: 100%;
  height: 100%;
}

body {
  width: 100%;
  height: 100%;
  background-color: #161e38;
}

canvas {
  width: 100%;
  height: 100%;
}
```

</div>
</details>