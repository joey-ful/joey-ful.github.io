---
icon: "๐"
title: '[Bouncing Ball] Interactions๋ง๋ค๊ธฐ -mouseover์ ์๋ ๋ณ๊ฒฝ, ์ด๋ฏธ์ง ์ง์ด์ ๋๋๊ทธ'
category: "FrontEnd"
date: "2021-02-07 12:00:00 +09:00"
desc: "bounce interaction"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

> **์ด๋ฒ ๊ธ์์ ๋ค๋ฃฐ ๋ด์ฉ**

- ๋ธ๋ผ์ฐ์  ์ฐฝ์ ๋ง์ฐ์ค ํธ๋ฒํ๋ฉด(mouseover) ์ฌ๋ก์ฐ๋ชจ์์ผ๋ก ์ ๋๋ฉ์ด์ ์คํ
- ๋ง์ฐ์ค๋ก ์์ง์ด๋ ๊ณต ์ก์์ ๋๋๊ทธ

  - mousedown, mousemove, mouseup, mouseout

- [์ฝ๋ ๊ธฐ๋ณธ ์ธํ ์ฐธ๊ณ ](/canvas/default-code-setting)
- [๊นํ](https://github.com/joey-ful/CanvasAnimationDefault)

## ๋ธ๋ผ์ฐ์  ์ฐฝ์ ๋ง์ฐ์ค ํธ๋ฒํ๋ฉด ์ฌ๋ก์ฐ๋ชจ์์ผ๋ก ์ ๋๋ฉ์ด์ ์คํ

- `mouseover` ๋ก ํธ๋ฒํ์ ๋ ๊ณต๋ค์ ์๋๋ฅผ ๋ฆ์ถ๊ธฐ
- `mouseout` ์ผ๋ก ๋ง์ฐ์ค๊ฐ ๋ธ๋ผ์ฐ์  ๋ฐ์ผ๋ก ๋๊ฐ์ ๋ ์๋ ์์๋ณต๊ท

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

- ๋ธ๋ผ์ฐ์  ์ฐฝ์ ๋ง์ฐ์ค๋ฅผ ํธ๋ฒํ๋ฉด ์ฌ๋ก์ฐ๋ชจ์์ผ๋ก ๊ณต์ด ์์ง์ด๋ ๊ฒ์ ํ์ธํ  ์ ์๋ค

## ๋ง์ฐ์ค๋ก ์์ง์ด๋ ๊ณต ์ก์์ ๋๋๊ทธ

>

- ๋ง์ฐ์ค๋ก ์์ง์ด๋ ๊ณต์ ํด๋ฆญ
- ํด๋ฆญํ ๊ณต์ ๋ง์ฐ์ค๊ฐ ์์ง์ด๋ ๋๋ก ๋๋๊ทธ๋  ๊ฒ
- ๊ณต์ ๋๋ ์๊ฐ ์๋ ์์ง์ด๋ ๋ฐฉํฅ์ผ๋ก ์ด์ด์ ์ด๋

### interaction()

- interaction() ํจ์์์ mousedown, mouseup, mouseout์ ๊ด๋ฆฌ
  - mousedown์ ๋ง์ฐ์ค ํด๋ฆญํ ์ํ
  - mouseup์ ๋ง์ฐ์ค ํด๋ฆญ ๋ ์ํ
  - mouseout์ ๋ง์ฐ์ค๊ฐ ๋ธ๋ผ์ฐ์  ๋ฐ์ผ๋ก ๋๊ฐ ์ํ

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

### mousedown - ํด๋ฆญ

- ์บ๋ฒ์ค์ ๊ทธ๋ฆผ์๋ html element์ฒ๋ผ ๋ฐ๋ก ์ด๋ฒคํธ๋ฅผ ๋ฌ ์ ์๋ค
- ๋ฐ๋ผ์ ๋ง์ฐ์ค๋ก ์์ง์ด๋ ๊ณต์ ํด๋ฆญํ๊ธฐ ์ํด์๋ ๊ณต์์์ญ ์์์ ๋ง์ฐ์ค๊ฐ ๋๋ ธ์ ๋ ํด๋น ์ด๋ฏธ์ง๋ฅผ ํด๋ฆญํ๋ ๋ฐฉ๋ฒ์ผ๋ก ํด์ผํ๋ค
- ๊ณต์ ์์ญ ์์ ์๋ ๊ฒ์ ๋ ๊ฐ์ง๋ก ํ์ธ ๊ฐ๋ฅํ๋ค
  - ์ ์ค์ฌ์ x์ขํ์ ๋ง์ฐ์ค ํฌ์ธํฐ์ x์ขํ ์ฌ์ด๊ฐ ๋ฐ์ง๋ฆ ์ด๋ด
  - ์ ์ค์ฌ์ y์ขํ์ ๋ง์ฐ์ค ํฌ์ธํฐ์ y์ขํ ์ฌ์ด๊ฐ ๋ฐ์ง๋ฆ ์ด๋ด

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

- ๋๋ ธ์ผ๋ฉด mousemove ์ด๋ฒคํธ๋ฅผ ๋ฌ์ ๋๋๊ทธ๋๋๋ก ํ๋ค

### mousemove - ๋๋๊ทธ

- ๋ง์ฐ์ค๊ฐ ์์ง์ด๋ฉด ๊ณต์ ์ค์ฌ์ด ๋ง์ฐ์ค ํฌ์ธํฐ์ ๋ฐ๋ผ ๋ฐ๋๋๋ก ํ๋ค
  - ์๊น ๊ณ์ฐํด๋ offset์ ๋๋ก ๋นผ์ฃผ๋ฉด ์ ํํ ๋ง์ฐ์ค ํฌ์ธํฐ์ ์์ค์ฌ์ ๊ฑฐ๋ฆฌ๋ฅผ ์ ์งํ  ์ ์๋ค

```javascript
//ball.js
onMouseMove = (e) => {
  this.x = e.clientX - this.offsetX;
  this.y = e.clientY - this.offsetY;

  this.vx = 0;
  this.vy = 0;
};
```

- ๋ง์ฐ์ค์ ๋ฐ๋ผ์๋ง ์ด๋ํ๋๋ก ์ ์ **vx์ vy๋ฅผ 0์ผ๋ก ์ค์ **ํด์ค๋ค
- ๋์  ๊ธฐ์กด ์๋๋ฅผ ๊ธฐ์ตํ๊ธฐ ์ํด **this.speed์ ์๋๋ฅผ ์ ์ฅ**ํด๋๋ค

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

### mouseup - ํด๋ฆญ ํด์ 

- ํด๋ฆญ ํด์ ์ vx์ vy๋ฅผ this.speed์ ์ ์ฅํด๋ ์๋๋ก ๋๋ก ์ค์ ํด์ค๋ค
- ์ฃผ์ํ  ์ ์ ์ง๊ธ์ ๋ธ๋ผ์ฐ์ ์ mouseoverํ ์ํ๋ผ ์ฌ๋ก์ฐ๋ชจ์์ด ์ ์ฉ๋ ์ํ๋ค
- ๋ฐ๋ผ์ **์ฌ๋ก์ฐ๋ชจ์์ด ์ ์ฉ๋ ์๋์ธ this.speed/20์ผ๋ก ์ค์ **ํด์ค์ผ ํ๋ค
- ๋ง์ง๋ง์ผ๋ก mousedown๊ณผ mousemove ์ด๋ฒคํธ๋ฅผ ์ ๊ฑฐํด์ค๋ค

```javascript
//ball.js
onMouseUp = (e) => {
  this.vx = this.speedX / 20;
  this.vy = this.speedY / 20;

  this.canvas.removeEventListener('mousedown', this.onMouseDown);
  this.canvas.removeEventListener('mousemove', this.onMouseMove);
};
```

#### ์ฌ๊ธฐ๊น์ง ํ๋ฉด ๋ ๊ฐ์ง ๋ฌธ์ ๊ฐ ๋ฐ์ํ๋ค

1. ํด๋ฆญ์ ํด์ ํ์ง ์์ ์ํ๋ก mouseoutํ๋ฉด (๋ธ๋ผ์ฐ์  ๋ฐ์ผ๋ก ๋ง์ฐ์ค๋ฅผ ์ด๋ํ๋ฉด) ๊ณต์ ๊ทธ๋๋ก ๋ฉ์ถฐ์๊ฒ ๋๋ค.

- mousemove์ mousedown ์ด๋ฒคํธ๊ฐ ์ ๊ฑฐ๋์ง ์๊ณ  vx, vy๋ ๊ณ์ 0์ธ ์ํ
  <br>

2. ํด๋ฆญํ๋ค ํด์ ํ๋ฉด vx, vy๋ ๋ฌด์กฐ๊ฑด this.speed๊ฐ ๋๋ค

- this.speed๊ฐ ์์์๋ค๋ฉด vx, vy๋ ํด๋ ํด์ ์ ๋ฌด์กฐ๊ฑด ์์๊ฐ ๋๋ค๋ ๋ป
- ์๋ฌด๋ฆฌ ๊ธฐ์กด ๊ณต์ด ์์ vx, vy๋ฅผ ๊ฐ์ ธ ์ข์ธก ์๋จ์ผ๋ก ์ด๋ ์ค์ด์๋คํด๋
  ![](https://images.velog.io/images/jehjong/post/18e03e54-cf3d-41cb-b8e1-08477d294932/image.png)
- ํด๋ฆญํ๋ค ๋ผ๋ฉด ์ฐ์ธก ํ๋จ์ผ๋ก ์ด๋ํ๊ฒ ๋๋ค
  ![](https://images.velog.io/images/jehjong/post/ea627c8c-227b-4d4f-b38f-3ece158184f7/image.png)

๐ฅ ๋ฐ๋ผ์ mouseout ์ค์ ๊ณผ ๊ธฐ์กด ๋ฐฉํฅ์ ๊ธฐ์ตํ๋ ๊ธฐ๋ฅ์ด ํ์

### mouseout - ํด๋ฆญํ ์ํ๋ก ๋ธ๋ผ์ฐ์  ๋ฐ์ผ๋ก ๋๊ฐ ๊ฒฝ์ฐ

- mouseup๊ณผ ๋ง์ฐฌ๊ฐ์ง๋ก vx, vy๋ฅผ ๋๋ก ์์๋ณต๊ทํ๊ณ  mousedown๊ณผ mousemove์ ๋ฌ๋ ค์๋ ์ด๋ฒคํธ๋ฅผ ์ ๊ฑฐํด์ค์ผ ํ๋ค
- ๋ค๋ง ์ด๋ฒ์ mouseup๊ณผ ๋ฌ๋ฆฌ ๋ง์ฐ์ค ํฌ์ธํฐ๊ฐ ๋ธ๋ผ์ฐ์  ๋ฐ์ผ๋ก ๋๊ฐ ์ํ๊ธฐ ๋๋ฌธ์ ์ฌ๋ก์ฐ๋ชจ์์ด ์ ์ฉ๋์ง์์ this.speed ๊ฐ ๊ทธ๋๋ก๋ก ์ค์ ํด์ฃผ๋ฉด ๋๋ค

```javascript
//ball.js
onMouseOut = (e) => {
  this.vx = this.speedX;
  this.vy = this.speedY;

  this.canvas.removeEventListener('mousedown', this.onMouseDown);
  this.canvas.removeEventListener('mousemove', this.onMouseMove);
};
```

### ์ง๊ธฐ ์  ๋ฐฉํฅ์ผ๋ก ์ ์ฉํ๊ธฐ

#### ๋ฐฉํฅ ๊ธฐ์ตํ๊ธฐ

- ๋ฐฉํฅ์ vx, vy๊ฐ ์์์ธ์ง ์์์ธ์ง๋ก ํ๋จํ๋ฉด ๋๋ค
- ์๋๊ฐ ์์๋ฉด -1, ์์๋ฉด 1๋ก ์ค์ 

```javascript
//ball.js
constructor() {
  ...
  this.vx_minus = this.speedX < 0 ? -1 : 1;
  this.vy_minus = this.speedY < 0 ? -1 : 1;
  ...
}
```

- ๊ทธ๋ฆฌ๊ณ  setMinus ํจ์๋ฅผ ๋ง๋ค์ด vx์ vy๊ฐ์ ๋ฐ๋ผ ์์์ธ์ง ์๋์ง๋ฅผ ๊ธฐ์ตํ๋ค

```javascript
//ball.js
...
setMinus = () => {
  this.vx_minus = this.vx < 0 ? -1 : 1;
  this.vy_minus = this.vy < 0 ? -1 : 1;
};
```

- ๋ฐฉํฅ์ ๊ธฐ์ตํด์ผ ํ๋ ์๊ฐ์ ๋ ๊ฒฝ์ฐ๋ฐ์ ์๋ค >> ๋งจ ์ฒ์๊ณผ mousedown์ผ๋ก ๊ณต์ ํด๋ฆญํ ์๊ฐ

```javascript
//ball.js
onMouseDown = (e) => {
  this.setMinus();
  ...
}
```

#### ๊ธฐ์กด ๋ฐฉํฅ ์ ์ฉํ๊ธฐ

- applyMinus ํจ์๋ฅผ ๋ง๋ค์ด ๋ฐฉํฅ์ ์ ์ฉํ๋๋ก ํ๋ค
- ์์์ธ์ง ํ๋ณํ๋ ๋ณ์์ ์๋์ ๋ถํธ๊ฐ ๋ค๋ฅด๋ฉด vx, vy๋ถํธ๋ฅผ ๋ณ๊ฒฝํด์ค๋ค

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

- ๋ฐฉํฅ์ ์ ์ฉํด์ผํ๋ ๊ฒฝ์ฐ๋ ํด๋ฆญ์ด ํด์ ๋์ด์ผ ํ๋ ์๊ฐ์ผ๋ก ๋ ๊ฐ์ง๋ค >> mouseup๊ณผ mouseout

```javascript
//ball.js
onMouseUp = (e) => {
  //vx, vy ์ฌ๋ก๋ชจ์ ์๋๋ก ์์๋ณต๊ท
  this.applyMinus();
  //์ด๋ฒคํธ ์ ๊ฑฐ
};

onMouseOut = (e) => {
  //vx, vy ์๋ ์์๋ณต๊ท
  this.applyMinus();
  //์ด๋ฒคํธ ์ ๊ฑฐ
};
```

## ์์ฑ ์ฝ๋

<details>
  <summary><b>Bouncing Ball ์ธํฐ๋ ์ ์์ฑ ์ฝ๋</b></summary>
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