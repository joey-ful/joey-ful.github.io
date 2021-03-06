---
icon: "๐"
title: '[Bouncing Ball] clip()์ผ๋ก ๋ํ์ ์ด๋ฏธ์ง ๋ฃ๊ณ  ์ ๋๋ฉ์ด์ ์ ์ฉ'
category: "FrontEnd"
date: "2021-02-05 12:00:00 +09:00"
desc: "clip image"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

[๋ฒฝ์ ๋ถ๋ชํ๋ ๊ณต ์ ๋๋ฉ์ด์](https://velog.io/@jehjong/JS-Canvas-Animation-Bouncing-Ball-klwrfb15)

> **์ด๋ฒ ๊ธ์์ ๋ค๋ฃฐ ๋ด์ฉ**

- clip ์ฌ์ฉํ๊ธฐ
- ์บ๋ฒ์ค์ ์ด๋ฏธ์ง ๊ทธ๋ฆฌ๊ธฐ
- ๊ณต์ ์ด๋ฏธ์ง ๋ฃ๊ธฐ
- clip๋ ๊ทธ๋ฆผ์ ์ ๋๋ฉ์ด์ ์ ์ฉ
- ๋ฐ๋ณต ๋ฐ ๋๋ค ํจ์๋ฅผ ํตํด ๊ณต ์ฌ๋ฌ๊ฐ ์์ฑ


- [์ฝ๋ ๊ธฐ๋ณธ ์ธํ ์ฐธ๊ณ ](/canvas/default-code-setting)
- [๊นํ](https://github.com/joey-ful/CanvasAnimationDefault)

## clip ์ฌ์ฉํ๊ธฐ

### ๋ณด๋ผ์ ์์์ ํํฌ์ ๊ณต

```javascript
//app.js
class App{
  constructor() {
    ...
	this.createBall();
    this.animate();
  }

  createBall() {
    this.ball = new Ball();
  }

  animate() {
	this.ball.draw(this.ctx);
  }
}
```

```javascript
//ball.js
export default class Ball {
  draw(ctx) {
    // ํํฌ์ ๊ณต
    ctx.beginPath();
    ctx.arc(400, 400, 100, 0, 2 * Math.PI);
    ctx.fillStyle = '#faa2c1';
    ctx.fill();
    ctx.closePath();

    // ๋ณด๋ผ์ ์์
    ctx.beginPath();
    ctx.rect(375, 250, 50, 300);
    ctx.fillStyle = '#b197fc';
    ctx.fill();
  }
}
```

![](https://images.velog.io/images/jehjong/post/267c90ed-2d4e-47e5-81e0-dc16150d776a/image.png)

### ํํฌ์ ๊ณต์ ๋ณด๋ผ์ ์์ ๋ฃ๊ธฐ

- clip() ๊ธฐ๋ฅ์ ์ฌ์ฉํด ํํฌ์ ๊ณต ์์ ๋ณด๋ผ์ ์์๋ฅผ ๋ฃ์ ์ ์๋ค

```javascript
//ball.js

//ํํฌ์ ๊ณต
this.ctx.clip();
//๋ณด๋ผ์ ์์
```

- ์ฆ, ํํฌ์ ๊ณต์ ๋ณด๋ผ์ ์์ ๋ด๋ถ์๋ง ์กด์ฌ
  ![](https://images.velog.io/images/jehjong/post/9f76fe7b-1d71-41ad-b724-6d5cede6fcd7/image.png)

## ์บ๋ฒ์ค์ ์ด๋ฏธ์ง ๊ทธ๋ฆฌ๊ธฐ

```javascript
//app.js
...
createBall() {
  this.ball = new Ball('image path');
}

animate() {
  this.ball.draw(this.ctx);
}
```

์ด๋ฏธ์ง ๊ทธ๋ฆฌ๋ ๋ฐฉ๋ฒ

- new Image()๋ก ์ด๋ฏธ์ง๋ฅผ ์์ฑ
- ์ด๋ฏธ์ง๊ฐ ๋ก๋๋๋ฉด [drawImage](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage)๋ก ๊ทธ๋ฆฌ๊ธฐ (์ถ๊ฐ ๊ณต๋ถ๊ฐ ํ์. ์ธ์  ๋ก๋๋๋์ง๋ฅผ ๋ชฐ๋ผ์ ์ด๋ป๊ฒ ํ๋ฉด ๋๊ณ  ์ด๋ป๊ฒ ํ๋ฉด ์๋จ)
- ์ด๋ฏธ์ง ์์ค ์ง์ 

```javascript
//ball.js
export default class Ball {
  constructor(src) {
    this.img = new Image();
    this.src = src;
  }

  draw(ctx) {
    this.img.onload = () => {
      ctx.drawImage(this.img, 200, 200, 60, 60);
    };

    this.img.src = this.src;
  }
}
```

- img.onload๋ ์ด๋ฒคํธ๋ฆฌ์ค๋๋ก ์์ฑํด๋ ๋จ

```javascript
img.addEventListener('load', () => {
  ctx.drawImage(img, x, y, width, height);
});
```

- ์ด๋ฏธ์ง ์ถ๋ ฅ

![](https://images.velog.io/images/jehjong/post/69a813cd-20e1-47fc-88cb-1c0c05290d3f/image.png)

## ๊ณต์ ์ด๋ฏธ์ง ๋ฃ๊ธฐ

- ์ ๊ทธ๋ฆฌ๊ณ  clip()ํ ๋ค์์ ์ด๋ฏธ์ง ๋ฃ๊ธฐ
- ์์ ๊ทธ๋ฆฌ๋ arc๋ ์์ ์ค์ฌ x,y์ขํ๋ฅผ ๋ฐ๋๋ค
- ํ์ง๋ง drawImage๋ ์ด๋ฏธ์ง๋ฅผ ๊ทธ๋ฆฌ๊ธฐ ์์ํ๋ ๊ฐ์ฅ ์ข์ธก ์๋จ์ ์ขํ๋ฅผ ๋ฐ๋๋ค

```javascript
ctx.arc(center_x, center_y, radius, startAngle, endAngle);
ctx.drawImage(img, x, y, width, height);
```

- ๋์ ์ ๋ ฌ์ํค๊ณ  ์ถ๋ค๋ฉด x, y๋ถํฐ ์์ํ๋ ๊ณณ์ drawImage๋ก ์ด๋ฏธ์ง๋ฅผ ๊ทธ๋ฆฐ ํ `x + width / 2`, `y + height / 2`๋ฅผ ์์ ์ค์ฌ ์ขํ๋ก ์ค์ 
- ์๋ ์ด๋ฏธ์ง๋ 200, 200๋ถํฐ ๊ทธ๋ฆฌ๊ธฐ ์์ํ๊ณ  ๊ฐ๋ก ์ธ๋ก๊ฐ 60์ด๋ฏ๋ก ์ ์ค์ฌ์ 230, 230
- ์ ํฌ๊ธฐ๋ ์ด๋ฏธ์ง๋ฅผ ๊ฒจ์ฐ ๋ด์ ์ ์๋๋ก ๋ฐ์ง๋ฆ์ 30์ผ๋ก ์ค์ 
- ๋ฐฐ๊ฒฝ์ ํฐ์ ์ ์ฉ

```javascript
//ball.js
export default class Ball {
  constructor(src) {
    this.img = new Image();
    this.src = src;
  }

  draw(ctx) {
    //์(200 + 60/2, 200 + 60/2)
    ctx.beginPath();
    ctx.fillStyle = 'white';
    ctx.arc(230, 230, 30, 0, Math.PI * 2, true);
    ctx.fill();
    ctx.closePath();

    ctx.clip();

    this.img.addEventListener('load', () => {
      ctx.drawImage(this.img, 200, 200, 60, 60);
    });

    this.img.src = this.src;
  }
}
```

![](https://images.velog.io/images/jehjong/post/28ad8f92-98e8-4ee3-9623-7f81ea275b8a/image.png)

### ๊ทธ๋ฆผ์

- ๊ทธ๋ฆผ์ ํจ๊ณผ ์ถ๊ฐ

```javascript
draw(ctx){
  ...
  ctx.shadowColor = '#dee2e6';
  ctx.shadowBlur = 10;
  ctx.shadowOffsetX = 5;
  ctx.shadowOffsetY = 3;
}
```

![](https://images.velog.io/images/jehjong/post/e35f6cb8-f6ab-45eb-b573-e410bcbfef6a/image.png)

## clip๋ ๊ทธ๋ฆผ์ ์ ๋๋ฉ์ด์ ์ ์ฉ

### ์ขํ์ ํฌ๊ธฐ ๋ณ์๋ก ๋ฐ์ ์ ๋๋ฉ์ด์ ์ ์ฉ

- Ballํด๋์ค์์ stageWidth์ stageHeight๋ฐ์ x, y ์ขํ๋ฅผ ๋๋ค์ผ๋ก ์์ฑ
  - ์์ ์ขํ๊ฐ ๋ธ๋ผ์ฐ์  ๋ฐ์ผ๋ก ๋๊ฐ์ง ์๋๋ก ๊ณ์ฐ

```javascript
//ball.js
export default class Ball {
  constructor(stageWidth, stageHeight, radius, speed, src) {
    this.img = new Image();
    this.radius = radius;
    this.diameter = this.radius * 2;
    this.vx = speed;
    this.vy = speed;
    this.src = src;

    this.x = this.radius + Math.random() * (stageWidth - this.diameter);
    this.y = this.radius + Math.random() * (stageHeight - this.diameter);
  }
}
```

- ์ขํ๋ง ๋ณ์๋ก ๊ต์ฒด
- ์๋์ bounceWindow()๋ [[Bouncing Ball] ์๋์ฐ ์ฐฝ์ ๋ถ๋ชํ๋ ๊ณต ์ ๋๋ฉ์ด์](https://velog.io/@jehjong/JS-Canvas-Animation-Bouncing-Ball-klwrfb15) ์ฐธ๊ณ 

```javascript
//ball.js
export default class Ball {
  ...
  draw(ctx, stageWidth, stageHeight) {
    this.x += this.vx;
    this.y += this.vy;

    this.bounceWindow(stageWidth, stageHeight);

    //์
    ctx.beginPath();
    ctx.fillStyle = 'white';
    ctx.shadowColor = '#dee2e6';
    ctx.shadowBlur = 10;
    ctx.shadowOffsetX = 5;
    ctx.shadowOffsetY = 3;
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, true);
    ctx.fill();
    ctx.closePath();

    //ํด๋ฆฝ
    ctx.clip();

    //์ด๋ฏธ์ง
    this.img.addEventListener('load', () => {
      ctx.drawImage(
        this.img,
        this.x - this.radius,
        this.y - this.radius,
        this.diameter,
        this.diameter
      );
    });
    this.img.src = this.src;
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

- app.js์์ ์ ์ ํ ๊ฐ ์๋ ฅ
- requestAnimation ํธ์ถ ๋ฐ clearRect๋ก ํ๋ ์ ์์๋ง๋ค ์บ๋ฒ์ค ์ง์ฐ๊ธฐ
  - [[Bouncing Ball] ์๋์ฐ ์ฐฝ์ ๋ถ๋ชํ๋ ๊ณต ์ ๋๋ฉ์ด์](https://velog.io/@jehjong/JS-Canvas-Animation-Bouncing-Ball-klwrfb15) ์ฐธ๊ณ 

```javascript
//app.js
...
createBall() {
  this.ball = new Ball(
    this.stageWidth,
    this.stageHeight,
    30,
    20,
    './srcs/js.png'
  );
}

animate() {
  window.requestAnimationFrame(this.animate.bind(this));
  this.ctx.clearRect(0, 0, this.stageWidth, this.stageHeight);
  this.ball.draw(this.ctx, this.stageWidth, this.stageHeight);
}
```

### ์ด๋ฏธ์ง load EventListener

- ์ด๋ฏธ์ง๋ฅผ ํ๋๋ง ๊ทธ๋ฆฌ๋ ๊ฒฝ์ฐ์๋ ์ด๋ฏธ์ง์ load ์ด๋ฒคํธ๋ฅผ ๋ฌ์์ฃผ๊ณ  ๊ทธ๋ ธ์ด์ผ ํ๋ค

```javascript
//ball.js
draw() {
  ...
  this.img.addEventListener('load', () => {
    ctx.drawImage(
      this.img,
      this.x - this.radius,
      this.y - this.radius,
      this.diameter,
      this.diameter
    );
  });
}
```

- ํ์ง๋ง load ๋ ํ์๋ ๋งค ํ๋ ์ drawImage๋ฅผ ์คํํด์ค์ผํ๋ฏ๋ก ์ด๋ฒคํธ ๋ฐ์์๋ drawImage ํธ์ถ์ด ํ์ํ๋ค

```javascript
//ball.js
draw() {
  ...
  this.img.addEventListener('load', () => {
    ctx.drawImage(
      this.img,
      this.x - this.radius,
      this.y - this.radius,
      this.diameter,
      this.diameter
    );
  });
  ctx.drawImage(
    this.img,
    this.x - this.radius,
    this.y - this.radius,
    this.diameter,
    this.diameter
  );
}
```

- ํ์ง๋ง ์ด๋ ๊ฒ ๋ ๋ฒ ์์ฑํ๋ฉด ํ๋ฉด ์์ธ์ง ๋ฌด์ฒ ๋๋ ค์ง๋ค๋ ๋จ์ ์ด ์๋ค
- ๋ฐ๋ผ์ eventListener๋ ์ง์ฐ๊ธฐ๋ก ํ๋ค
  - ์ง์ฐ๋ฉด ์ ๋๋ฉ์ด์์ด ์กฐ๊ธ ๋ฆ๊ฒ ์์ํ  ๊ฒ ๊ฐ๊ธฐ๋ํ์ง๋ง ์ก์์ผ๋ก๋ ์ฐจ์ด๊ฐ ์์ด๋ณด์ธ๋ค

### save()์ restore()

- ์์ฒ๋ผ ์ ๋๋ฉ์ด์์ ์คํํ๋ฉด **ํด๋ฆฝ๋ ์์ญ์ ๊ณ ์ **๋ ์ฑ๋ก ์ด๋ฏธ์ง๋ง ์์ง์ด๊ฒ ๋๋ค
- ์๊ณผ ์ด๋ฏธ์ง์ ํฌ๊ธฐ๋ฅผ ๋งค์ฐ ํฌ๊ฒํ๊ณ  speed๋ฅผ ๋๋ฆฌ๊ฒ ํด๋ณด๋ฉด

```javascript
//app.js
...
  createBall() {
    this.ball = new Ball(
      this.stageWidth,
      this.stageHeight,
      300,
      1,
      'image path'
    );
```

- ์ด๋ฏธ์ง๋ง ํผ์ ์์ง์ด๋ ๊ฒ์ ํ์ธํ  ์ ์๋ค
  ![](https://images.velog.io/images/jehjong/post/512eeb9b-0d5e-4475-9e21-0dfd7ed28d18/image.png)

- ์ด๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด save()์ restore()๋ฅผ ์ฌ์ฉํ๋ค
- save๋ก ํด๋ฆฝ๋๊ธฐ ์ ์ ์บ๋ฒ์ค๋ฅผ ์ ์ฅํ๊ณ  ๊ทธ๋ฆฌ๊ธฐ๊ฐ ๋๋ ํ restore()๋ฅผ ์ด์ฉํด ์ ์ฅํ๋ ์บ๋ฒ์ค๋ก ๋๋๋ ค์ค๋ค

```javascript
//ball.js
...
draw(ctx, stageWidth, stageHeight) {
  ...
  ctx.save();
  //์๊ทธ๋ฆฌ๊ธฐ
  //ํด๋ฆฝ
  //์ด๋ฏธ์ง ๊ทธ๋ฆฌ๊ธฐ
  //์ด๋ฏธ์ง ์์ค
  ctx.restore();
}
```

- ๊ทธ๋ฌ๋ฉด ์์ง์ด๋ ๊ณต ์์ฑ

![](https://images.velog.io/images/jehjong/post/3e4d73b0-f0f3-4c22-8e9f-e56e40ba9a99/image.png)

## ๋ฐ๋ณต ๋ฐ ๋๋ค ํจ์๋ฅผ ํตํด ๊ณต ์ฌ๋ฌ๊ฐ ์์ฑ

### ๋ฐ๋ณต

- ๋ก๊ณ  ์ด๋ฏธ์ง ํ์ผ๋ค์ ์ ์ฅ ํ ์ด๋ฆ๋ค์ logos ๋ฐฐ์ด์ ๋ฃ์ด์ฃผ์๋ค
- ํ์ผ ์ด๋ฆ์ด c.png, csharp.png ์ ๊ฐ์ ํ์์ด๋ฉฐ ๋ชจ๋ ๊ฐ์ ํด๋์ ์กด์ฌ

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

- this.balls ๋ฐฐ์ด์ ์์ฑ
- ๋ฐ๋ณต๋ฌธ์ ํตํด Ball ์ธ์คํด์ค๋ฅผ ์์ฑํ๊ณ  ๊ทธ๋ ค์ฃผ๊ธฐ

```javascript
//app.js
import {logos} from './consts.js';				//๋ก๊ณ  ์ด๋ฆ๋ค์ด ๋ด๊ธด ๋ฐฐ์ด logos

class App {
  constructor() {
  ...
  this.balls = [];
  }

  createBall() {
    //์ด๋ฏธ์ง ์ด๋ฆ์ด ๋ด๊ธด logos ๋ฐฐ์ด
    for (let i = 0; i < logos.length; i++) {
      let radius = Math.ceil(Math.random() * 30) + 10;
      let speed = Math.ceil(Math.random() * 30) + 10;

      //logos์ ์ด๋ฏธ์ง ์ด๋ฆ์ผ๋ก
      //Ball ์ธ์คํด์ค ์์ฑ
      //this.balls์ ๋ด๊ธฐ
      this.balls.push(
        new Ball(
          this.stageWidth,
          this.stageHeight,
          radius,
          speed,
          `path/${logos[i]}.png`
        )
      );
    }
  }

  animate() {
    window.requestAnimationFrame(this.animate.bind(this));

    this.ctx.clearRect(0, 0, this.stageWidth, this.stageHeight);

    //this.balls ๋ชจ๋  ๊ณต ๊ทธ๋ฆฌ๊ธฐ
    this.balls.forEach(ball => {
      ball.draw(this.ctx, this.stageWidth, this.stageHeight);
    });
  }
}
```

### ๋๋ค ์๋์ ๋ฐฉํฅ

- ๋์ฑ ๋ ๋๋คํ ์์ง์์ ์ํด vx์ vy์ ๋ค๋ฅธ ๊ฐ์, ๊ทธ๋ฆฌ๊ณ  vx์ vy์ ๋ค๋ฅธ ๋ถํธ๋ฅผ ์ง์ ํด์คฌ๋ค
- speedX์ speedY๋ฅผ ๋ฐ๋ก ์์ฑํด์ฃผ๊ณ  ๊ฐ๊ฐ์ ๋ถํธ๋ ์๋๊ฐ ์ง์์ธ์ง ํ์์ธ์ง ์ฌ๋ถ๋ก ๊ฒฐ์ ํด์ฃผ์๋ค

```javascript
//app.js
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
```

- vx์ vy์ ๊ฐ๊ธฐ ๋ค๋ฅธ ๊ฐ ์ ์ฉ

```javascript
//ball.js
constructor(stageWidth, stageHeight, radius, speedX, speedY, src) {
  ...
  this.vx = speedX;
  this.vy = speedY;
  ...
}
```

## ์์ฑ ์ฝ๋

<details>
  <summary><b>cilp ์์ฑ์ฝ๋</b></summary>
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
    this.animate();
  }

  resize() {
    this.stageWidth = document.body.clientWidth;
    this.stageHeight = document.body.clientHeight;

    this.canvas.width = this.stageWidth * 2;
    this.canvas.height = this.stageHeight * 2;

    this.ctx.scale(2, 2);
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
    this.vx = speedX;
    this.vy = speedY;
    this.src = src;

    this.x = this.radius + Math.random() * (stageWidth - this.diameter);
    this.y = this.radius + Math.random() * (stageHeight - this.diameter);
  }

  draw(ctx, stageWidth, stageHeight) {
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