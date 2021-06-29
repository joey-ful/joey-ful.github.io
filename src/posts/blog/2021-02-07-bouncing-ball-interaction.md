---
icon: "🏀"
title: '[Bouncing Ball] Interactions만들기 -mouseover시 속도 변경, 이미지 집어서 드래그'
category: "FrontEnd"
date: "2021-02-07 12:00:00 +09:00"
desc: "bounce interaction"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

> **이번 글에서 다룰 내용**

- 브라우저 창에 마우스 호버하면(mouseover) 슬로우모션으로 애니메이션 실행
- 마우스로 움직이는 공 잡아서 드래그

  - mousedown, mousemove, mouseup, mouseout

- [코드 기본 세팅 참고](/canvas/default-code-setting)
- [깃헙](https://github.com/joey-ful/CanvasAnimationDefault)

## 브라우저 창에 마우스 호버하면 슬로우모션으로 애니메이션 실행

- `mouseover` 로 호버했을 때 공들의 속도를 늦추기
- `mouseout` 으로 마우스가 브라우저 밖으로 나갔을 때 속도 원상복귀

```javascript
//app.js
constructor(){
  ...
  this.slowDown();
}
...
slowDown() {
  this.canvas.addEventListener('mouseover', () => {
    this.balls.forEach((ball) => {
      ball.vx /= 20;
      ball.vy /= 20;
    });
  });

  this.canvas.addEventListener('mouseout', () => {
    this.balls.forEach((ball) => {
      ball.vx *= 20;
      ball.vy *= 20;
    });
  });
}
```

- 브라우저 창에 마우스를 호버하면 슬로우모션으로 공이 움직이는 것을 확인할 수 있다

## 마우스로 움직이는 공 잡아서 드래그

>

- 마우스로 움직이는 공을 클릭
- 클릭한 공은 마우스가 움직이는 대로 드래그될 것
- 공을 놓는 순간 원래 움직이던 방향으로 이어서 이동

### interaction()

- interaction() 함수에서 mousedown, mouseup, mouseout을 관리
  - mousedown은 마우스 클릭한 상태
  - mouseup은 마우스 클릭 뗀 상태
  - mouseout은 마우스가 브라우저 밖으로 나간 상태

```javascript
//ball.js
...
draw() {
  this.interaction();
}

interaction() {
  this.canvas = document.getElementById('canvas');

  this.canvas.addEventListener('mousedown', this.onMouseDown);
  this.canvas.addEventListener('mouseup', this.onMouseUp);
  this.canvas.addEventListener('mouseout', this.onMouseOut);
}

onMouseDown = (e) => {
  console.log("mouse down");
}

onMouseUp = (e) => {
  console.log("mouse up");
}

onMouseOut = (e) => {
  console.log("mouse out");
}
```

### mousedown - 클릭

- 캔버스의 그림에는 html element처럼 바로 이벤트를 달 수 없다
- 따라서 마우스로 움직이는 공을 클릭하기 위해서는 공의영역 안에서 마우스가 눌렸을 때 해당 이미지를 클릭하는 방법으로 해야한다
- 공의 영역 안에 있는 것은 두 가지로 확인 가능하다
  - 원 중심의 x좌표와 마우스 포인터의 x좌표 사이가 반지름 이내
  - 원 중심의 y좌표와 마우스 포인터의 y좌표 사이가 반지름 이내

```javascript
//ball.js
mousedown() {
    this.offsetX = e.clientX - this.x;
    this.offsetY = e.clientY - this.y;

    if (
      Math.abs(this.offsetX) <= this.radius &&
      Math.abs(this.offsetY) <= this.radius
    ) {
      this.canvas.addEventListener('mousemove', this.onMouseMove);
    }
```

- 눌렸으면 mousemove 이벤트를 달아 드래그되도록 한다

### mousemove - 드래그

- 마우스가 움직이면 공의 중심이 마우스 포인터에 따라 바뀌도록 한다
  - 아까 계산해둔 offset을 도로 빼주면 정확한 마우스 포인터와 원중심의 거리를 유지할 수 있다

```javascript
//ball.js
onMouseMove = (e) => {
  this.x = e.clientX - this.offsetX;
  this.y = e.clientY - this.offsetY;

  this.vx = 0;
  this.vy = 0;
};
```

- 마우스에 따라서만 이동하도록 잠시 **vx와 vy를 0으로 설정**해준다
- 대신 기존 속도를 기억하기 위해 **this.speed에 속도를 저장**해둔다

```javascript
//ball.js
constructor(stageWidth, stageHeight, radius, speedX, speedY, src) {
  ...
  this.speedX = speedX;
  this.speedY = speedY;
  this.vx = this.speedX;
  this.vy = this.speedY;
  ...
}
```

### mouseup - 클릭 해제

- 클릭 해제시 vx와 vy를 this.speed에 저장해둔 속도로 도로 설정해준다
- 주의할 점은 지금은 브라우저에 mouseover한 상태라 슬로우모션이 적용된 상태다
- 따라서 **슬로우모션이 적용된 속도인 this.speed/20으로 설정**해줘야 한다
- 마지막으로 mousedown과 mousemove 이벤트를 제거해준다

```javascript
//ball.js
onMouseUp = (e) => {
  this.vx = this.speedX / 20;
  this.vy = this.speedY / 20;

  this.canvas.removeEventListener('mousedown', this.onMouseDown);
  this.canvas.removeEventListener('mousemove', this.onMouseMove);
};
```

#### 여기까지 하면 두 가지 문제가 발생한다

1. 클릭을 해제하지 않은 상태로 mouseout하면 (브라우저 밖으로 마우스를 이동하면) 공은 그대로 멈춰있게 된다.

- mousemove와 mousedown 이벤트가 제거되지 않고 vx, vy도 계속 0인 상태
  <br>

2. 클릭했다 해제하면 vx, vy는 무조건 this.speed가 된다

- this.speed가 양수였다면 vx, vy는 클랙 해제시 무조건 양수가 된다는 뜻
- 아무리 기존 공이 음수 vx, vy를 가져 좌측 상단으로 이동 중이었다해도
  ![](https://images.velog.io/images/jehjong/post/18e03e54-cf3d-41cb-b8e1-08477d294932/image.png)
- 클릭했다 떼면 우측 하단으로 이동하게 된다
  ![](https://images.velog.io/images/jehjong/post/ea627c8c-227b-4d4f-b38f-3ece158184f7/image.png)

🥊 따라서 mouseout 설정과 기존 방향을 기억하는 기능이 필요

### mouseout - 클릭한 상태로 브라우저 밖으로 나간 경우

- mouseup과 마찬가지로 vx, vy를 도로 원상복귀하고 mousedown과 mousemove시 달려있는 이벤트를 제거해줘야 한다
- 다만 이번엔 mouseup과 달리 마우스 포인터가 브라우저 밖으로 나간 상태기 때문에 슬로우모션이 적용되지않은 this.speed 값 그대로로 설정해주면 된다

```javascript
//ball.js
onMouseOut = (e) => {
  this.vx = this.speedX;
  this.vy = this.speedY;

  this.canvas.removeEventListener('mousedown', this.onMouseDown);
  this.canvas.removeEventListener('mousemove', this.onMouseMove);
};
```

### 집기 전 방향으로 적용하기

#### 방향 기억하기

- 방향은 vx, vy가 양수인지 음수인지로 판단하면 된다
- 속도가 음수면 -1, 양수면 1로 설정

```javascript
//ball.js
constructor() {
  ...
  this.vx_minus = this.speedX < 0 ? -1 : 1;
  this.vy_minus = this.speedY < 0 ? -1 : 1;
  ...
}
```

- 그리고 setMinus 함수를 만들어 vx와 vy값에 따라 음수인지 아닌지를 기억한다

```javascript
//ball.js
...
setMinus = () => {
  this.vx_minus = this.vx < 0 ? -1 : 1;
  this.vy_minus = this.vy < 0 ? -1 : 1;
};
```

- 방향을 기억해야 하는 순간은 두 경우밖에 없다 >> 맨 처음과 mousedown으로 공을 클릭한 순간

```javascript
//ball.js
onMouseDown = (e) => {
  this.setMinus();
  ...
}
```

#### 기존 방향 적용하기

- applyMinus 함수를 만들어 방향을 적용하도록 한다
- 음수인지 판별하는 변수와 속도의 부호가 다르면 vx, vy부호를 변경해준다

```javascript
//ball.js
applyMinus = () => {
  if (this.vx_minus * this.speedX < 0) {
    this.vx *= -1;
  }
  if (this.vy_minus * this.speedY < 0) {
    this.vy *= -1;
  }
};
```

- 방향을 적용해야하는 경우는 클릭이 해제되어야 하는 순간으로 두 가지다 >> mouseup과 mouseout

```javascript
//ball.js
onMouseUp = (e) => {
  //vx, vy 슬로모션 속도로 원상복귀
  this.applyMinus();
  //이벤트 제거
};

onMouseOut = (e) => {
  //vx, vy 속도 원상복귀
  this.applyMinus();
  //이벤트 제거
};
```

## 완성 코드

<details>
  <summary><b>Bouncing Ball 인터렉션 완성 코드</b></summary>
  <div markdown="1">


```javascript
//app.js
import Ball from './velogballs.js';
import {logos} from './consts.js';

class App {
  constructor() {
    this.canvas = document.createElement('canvas');
    this.canvas.setAttribute('id', 'canvas');
    this.ctx = this.canvas.getContext('2d');

    document.body.appendChild(this.canvas);

    window.addEventListener('resize', this.resize.bind(this), false);
    this.resize();

    this.balls = [];
    this.createBall();
    this.slowDown();
    this.animate();
  }

  resize() {
    this.stageWidth = document.body.clientWidth;
    this.stageHeight = document.body.clientHeight;

    this.canvas.width = this.stageWidth * 2;
    this.canvas.height = this.stageHeight * 2;

    this.ctx.scale(2, 2);
  }

  slowDown() {
    this.canvas.addEventListener('mouseover', () => {
      this.balls.forEach((ball) => {
        ball.vx /= 20;
        ball.vy /= 20;
      });
    });

    this.canvas.addEventListener('mouseout', () => {
      this.balls.forEach((ball) => {
        ball.vx *= 20;
        ball.vy *= 20;
      });
    });
  }

  createBall() {
    for (let i = 0; i < logos.length; i++) {
      let radius = Math.ceil(Math.random() * 30) + 10;
      let speedX = Math.ceil(Math.random() * 50) + 5;
      let speedY = Math.ceil(Math.random() * 50) + 5;
      let signX = speedX % 2 === 0 ? -1 : 1;
      let signY = speedY % 2 === 0 ? -1 : 1;

      this.balls.push(
        new Ball(
          this.stageWidth,
          this.stageHeight,
          radius,
          speedX * signX,
          speedY * signY,
          `./srcs/${logos[i]}.png`
        )
      );
    }
  }

  animate() {
    window.requestAnimationFrame(this.animate.bind(this));
    this.ctx.clearRect(0, 0, this.stageWidth, this.stageHeight);

    this.balls.forEach((ball) => {
      ball.draw(this.ctx, this.stageWidth, this.stageHeight);
    });
  }
}

new App();
```

```javascript
//ball.js
export default class Ball {
  constructor(stageWidth, stageHeight, radius, speedX, speedY, src) {
    this.img = new Image();
    this.radius = radius;
    this.diameter = this.radius * 2;
    this.speedX = speedX;
    this.speedY = speedY;
    this.vx_minus = this.speedX < 0 ? -1 : 1;
    this.vy_minus = this.speedY < 0 ? -1 : 1;
    this.vx = this.speedX;
    this.vy = this.speedY;
    this.src = src;

    this.x = this.radius + Math.random() * (stageWidth - this.diameter);
    this.y = this.radius + Math.random() * (stageHeight - this.diameter);
  }

  draw(ctx, stageWidth, stageHeight) {
    this.interaction();
    this.x += this.vx;
    this.y += this.vy;

    this.bounceWindow(stageWidth, stageHeight);

    ctx.save();
    ctx.beginPath();
    ctx.fillStyle = 'white';
    ctx.shadowColor = '#dee2e6';
    ctx.shadowBlur = 10;
    ctx.shadowOffsetX = 5;
    ctx.shadowOffsetY = 3;
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, true);
    ctx.fill();
    ctx.closePath();
    ctx.clip();
    ctx.drawImage(
      this.img,
      this.x - this.radius,
      this.y - this.radius,
      this.diameter,
      this.diameter
    );
    this.img.src = this.src;
    ctx.restore();
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

  interaction() {
    this.canvas = document.getElementById('canvas');

    this.canvas.addEventListener('mousedown', this.onMouseDown);
    this.canvas.addEventListener('mouseup', this.onMouseUp);
    this.canvas.addEventListener('mouseout', this.onMouseOut);
  }

  setMinus = () => {
    this.vx_minus = this.vx < 0 ? -1 : 1;
    this.vy_minus = this.vy < 0 ? -1 : 1;
  };

  applyMinus = () => {
    if (this.vx_minus * this.speedX < 0) {
      this.vx *= -1;
    }
    if (this.vy_minus * this.speedY < 0) {
      this.vy *= -1;
    }
  };

  onMouseDown = (e) => {
    this.setMinus();

    this.offsetX = e.clientX - this.x;
    this.offsetY = e.clientY - this.y;

    if (
      Math.abs(this.offsetX) <= this.radius &&
      Math.abs(this.offsetY) <= this.radius
    ) {
      this.canvas.addEventListener('mousemove', this.onMouseMove);
    }
  };

  onMouseMove = (e) => {
    this.x = e.clientX - this.offsetX;
    this.y = e.clientY - this.offsetY;

    this.vx = 0;
    this.vy = 0;
  };

  onMouseUp = (e) => {
    this.vx = this.speedX / 20;
    this.vy = this.speedY / 20;
    this.applyMinus();

    this.canvas.removeEventListener('mousedown', this.onMouseDown);
    this.canvas.removeEventListener('mousemove', this.onMouseMove);
  };

  onMouseOut = (e) => {
    this.vx = this.speedX;
    this.vy = this.speedY;
    this.applyMinus();

    this.canvas.removeEventListener('mousedown', this.onMouseDown);
    this.canvas.removeEventListener('mousemove', this.onMouseMove);
  };
}
```

```javascript
//consts.js
export const logos = [
  'c',
  'csharp',
  'c++',
  'java',
  'js',
  'mongoDB',
  'MySQL',
  'oracle3',
  'php',
  'PostgreSQL',
  'python',
  'r',
  'SQLite',
  'SQLServer',
  'angular',
  'backbone',
  'django',
  'ember',
  'flask',
  'laravel',
  'node',
  'preact',
  'rails',
  'react',
  'spring',
  'svelte',
  'vue',
];
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
}

canvas {
  width: 100%;
  height: 100%;
}
```
</div>
</details>