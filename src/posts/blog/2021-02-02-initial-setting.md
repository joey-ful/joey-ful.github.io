---
icon: "๐ผ"
title: '[์ ๋๋ฉ์ด์ ์ด๊ธฐ์์] ์๋์ฐ ์ฐฝ์ด ๋ณํ  ๋๋ง๋ค Canvas ํฌ๊ธฐ ์๋ก ์ง์ '
category: "FrontEnd"
date: "2021-02-02 12:00:00 +09:00"
desc: "resize canvas"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

# [์บ๋ฒ์ค ์ ๋๋ฉ์ด์](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial)

> **์ด๋ฒ ๊ธ์์ ๋ค๋ฃฐ ๋ด์ฉ**

- canvas๋?
- canvas ํฌ๊ธฐ๋ฅผ ๋ธ๋ผ์ฐ์  ์ฐฝ ํฌ๊ธฐ๋ก ์ง์ 
- ๋ธ๋ผ์ฐ์  ํฌ๊ธฐ๊ฐ ๋ณํ  ๋๋ง๋ค canvas ํฌ๊ธฐ ์๋ก ์ค์ 
- ๊ณ ํ์ง์ ์ ๋๋ฉ์ด์์ ์ํ canvas ํฌ๊ธฐ ์ค์ 

## canvas๋?

- 2์ฐจ์ ๋นํธ๋งต ๊ทธ๋ํฝ๊ณผ ์ ๋๋ฉ์ด์์ ๊ทธ๋ฆด ์ ์๋ HTML ์์
- ํจ์ค, ๋ํ, ๊ธ์ ๋ฑ์ ๊ทธ๋ฆฌ๊ณ  ์ด๋ฏธ์ง๋ฅผ ์ถ๊ฐํ๋ [๋ค์ํ ํจ์๋ค](https://www.w3schools.com/tags/ref_canvas.asp)์ด ์กด์ฌ
- width์ height ์์ฑ๋ง ์กด์ฌ (default width: 150, height: 300)
- ์บ๋ฒ์ค๋ ๊ณ ์  ํฌ๊ธฐ์ ๋๋ก์ ์์ญ์ ์์ฑํ๊ณ  ํ๋ ์ด์์ rendering context๋ฅผ ํตํด ์ถ๋ ฅํ  ์ปจํ์ธ ๋ฅผ ๋ค๋ฃธ
  - ์บ๋ฒ์ค์ ๋ฌด์ธ๊ฐ๋ฅผ ๊ทธ๋ฆฌ๊ธฐ ์ํด์๋ ๋๋๋ง ์ปจํ์คํธ์ ์ ๊ทผ ํ์

```javascript
//app.js
class App () {
  constructor() {
    this.canvas = document.createElement('canvas');
    this.context = this.canvas.getContext('2d');

    document.body.appendChild(canvas);
  }
  ...
}
```

### context

`canvas.getContext(contextType, contextAttribute)`

- contextType `2d` - 2์ฐจ์ ๋๋๋ง ์ปจํ์คํธ

## canvas ํฌ๊ธฐ๋ฅผ ๋ธ๋ผ์ฐ์  ์ฐฝ ํฌ๊ธฐ๋ก ์ง์ 

- ์บ๋ฒ์ค์ ๊ธฐ๋ณธ ํฌ๊ธฐ๋ 300 \* 150
- rect๋ ์ง์ฌ๊ฐํ์ ๊ทธ๋ฆฌ๋ ํจ์
- ๊ทธ๋ฆผ ๊ทธ๋ฆฌ๊ธฐ๋ [์บ๋ฒ์ค์ ๊ทธ๋ฆผ ๊ทธ๋ฆฌ๊ธฐ](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes) ์ฐธ์กฐ

```
ctx.rect(x, y, width, height);
```

- ์บ๋ฒ์ค ํฌ๊ธฐ๋งํผ ์์ ์ฑ์ฐ๋ฉด

```javascript
class App {
  constructor() {
	...

    // ์ง์ฌ๊ฐํ ์์ฑ
    this.ctx.rect(0, 0, this.canvas.width, this.canvas.height);
    this.ctx.fillStyle = '#b197fc';
    this.ctx.fill();
  }
}

new App();

```

- 300 \* 150๋ง ์ฑ์์ ธ์ ๋์จ๋ค

![](https://images.velog.io/images/jehjong/post/8d21e9e0-e179-4ad3-91f9-49b08f8cb381/image.png)

### clientWidth, clientHeight๋ก ์ง์ 

์บ๋ฒ์ค๊ฐ ๋๋ฌด ์์ผ๋ ํด๋ผ์ด์ธํธ์ฐฝ๋งํ๊ฒ ๋ฐ๊ฟ๋ณด์

```javascript
class App {
  constructor() {
	...
    this.canvas.width = document.body.clientX;
    this.canvas.height = document.body.clientY;

    // ์ง์ฌ๊ฐํ ์์ฑ
    this.ctx.rect(0, 0, this.canvas.width, this.canvas.height);
    this.ctx.fillStyle = '#b197fc';
    this.ctx.fill();
  }
}

new App();
```

- ํน์ดํ๊ฒ๋ ํฌ๋กฌ์ฐฝ์ 1446 \* 154๋งํผ์ด ์ฑ์์ก๋ค

![](https://images.velog.io/images/jehjong/post/dd8cdc54-7a0c-44d1-98d9-fe481a201803/image.png)

### html ํฌ๊ธฐ ์กฐ์ 

html์ ํ์ธํด๋ณด๋ฉด ์ฌ์ด์ฆ๊ฐ ์ ํ๋์ด์์์ ํ์ธํ  ์ ์๋ค
![](https://images.velog.io/images/jehjong/post/e6480989-3ad9-443c-9221-4945774aca6c/image.png)

- html ํฌ๊ธฐ๋ฅผ 100%๋ก ์ง์ ํด์ฃผ์๋ค

```css
/*
  stylesheet.css
*/
html {
  width: 100%;
  height: 100%;
}
```

![](https://images.velog.io/images/jehjong/post/41b8037c-2cab-42fe-a1e4-892133a130cf/image.png)

### body ํฌ๊ธฐ ์กฐ์ 

- ํ์ง๋ง ์ด๋ฒ์๋ body์ฌ์ด์ฆ๊ฐ ์ ํ๋ ๊ฒ์ ๋ณผ ์ ์๋ค
  ![](https://images.velog.io/images/jehjong/post/de7aa89a-b942-4d21-831a-cb0eadd676de/image.png)

- ๋ฐ๋ผ์ body ์ฌ์ด์ฆ๋ 100%๋ก ์กฐ์ ํด์ฃผ์๋ค

```css
/*
  stylesheet.css
*/
body {
  width: 100%;
  height: 100%;
}
```

![](https://images.velog.io/images/jehjong/post/d082f30d-e81f-44c3-9801-1a0d0ebb6452/image.png)

## ๋ธ๋ผ์ฐ์  ํฌ๊ธฐ๊ฐ ๋ณํ  ๋๋ง๋ค canvas ํฌ๊ธฐ ์๋ก ์ค์ 

- ํ๋ฒ ์บ๋ฒ์ค ์ฌ์ด์ฆ๊ฐ ์ง์ ๋๋ฉด ๋ธ๋ผ์ฐ์  ์ฐฝ์ ํฌ๊ธฐ๊ฐ ๋ฐ๋์ด๋ ์บ๋ฒ์ค ์ฌ์ด์ฆ๋ ๊ทธ๋๋ก ๋จ์์๊ฒ ๋๋ค

### resize()

- ๋ธ๋ผ์ฐ์  ํฌ๊ธฐ์ ๋ง์ถฐ ์บ๋ฒ์ค ํฌ๊ธฐ๋ฅผ ์ง์ ํ๋ resize ํจ์๋ฅผ ์์ฑ
- ์๋์ฐ ์ฐฝ์ด ๋ฐ๋ ๋๋ง๋ค resize ํธ์ถ

```javascript
//app.js
class App {
  constructor() {
	...

    //์๋์ฐ ์ฐฝ ํฌ๊ธฐ๊ฐ ๋ณํ  ๋๋ง๋ค resize ํจ์๋ฅผ ํธ์ถ
    window.addEventListener('resize', this.resize.bind(this));
    this.resize();
  }

  //์บ๋ฒ์ค ์ฌ์ด์ฆ ์ง์ ํ๋ ํจ์
  resize() {
    this.canvas.width = document.body.clientWidth;
    this.canvas.height = document.body.clientHeight;

    this.animate();
  }

  //๊ทธ๋ฆผ ๊ทธ๋ฆฌ๋ ํจ์
  animate() {
    //๋ณด๋ผ์์ผ๋ก ์บ๋ฒ์ค ์ฑ์ฐ๊ธฐ
    this.ctx.rect(0, 0, this.canvas.width, this.canvas.height);
    this.ctx.fillStyle = '#b197fc';
    this.ctx.fill();

    //ํํฌ์ ๊ณต ๊ทธ๋ฆฌ๊ธฐ
    this.ctx.beginPath();
    this.ctx.arc(200, 200, 50, 0, 2 * Math.PI);
    this.ctx.fillStyle = '#faa2c1';
    this.ctx.fill();
  }
}

new App();
```

- ๋ธ๋ผ์ฐ์  ์ฐฝ ํฌ๊ธฐ๊ฐ ๋ณํ๋ฉด ๋ณด๋ผ์ ์์ํฌ๊ธฐ๋ ๋ง์ถฐ ๋ณํ๋ค
  ![](https://images.velog.io/images/jehjong/post/da77a9be-127c-45bb-ab70-b5b136db87e3/image.png)

- ํํฌ ๊ณต์ ์ง์ ์ฌ์ด์ฆ๋ผ ๋ณํ์ง ์๋๋ค
  ![](https://images.velog.io/images/jehjong/post/53d5b15f-faed-43d1-8e42-9c4ac3b4a79f/image.png)

## ๊ณ ํ์ง์ ์ ๋๋ฉ์ด์์ ์ํ canvas ํฌ๊ธฐ ์ค์ 

### retina display (์ต์)

- [Interactive Developer](https://www.youtube.com/watch?v=sLCiI6d5vTM&t=164s)๋ ๋ ํฐ๋ ๋์คํ๋ ์ด์์๋ ์ ๋ชํ ์ ๋๋ฉ์ด์์ ์ํด(๋ํผ์) ์บ๋ฒ์ค์ ํฌ๊ธฐ๋ฅผ ์ผ๋ถ๋ฌ ๋ค๋ฐฐ๋ก ํค์์ค๋ค๊ณ  ํ๋ค
- ๋์ค์ ์ ๋ชํด์ง๋ฉด ํด๋ณด์

### width์ height๋ฅผ ๋ ๋ฐฐ๋ก

```javascript
//app.js
resize() {
  ...
  this.stageWidth = document.body.clientWidth;
  this.stageHeight = document.body.clientHeight;
  this.canvas.width = this.stageWidth * 2;
  this.canvas.height = this.stageHeight * 2;
}
```

- stageWidth์ stageHeight๋ ๋ธ๋ผ์ฐ์  ํฌ๊ธฐ๋ฅผ ๊ธฐ์ตํ๊ธฐ ์ํ ๋ณ์๋ค
  - ๋น์ฅ์ ์ธ๋ชจ์์ง๋ง ๋ธ๋ผ์ฐ์ ํฌ๊ธฐ๋ฅผ ๋งค๋ฒ document.body.clientWidth๋ก ๋ถ๋ฌ์ค๋ ๋ฒ๊ฑฐ๋ก์์ ํด๊ฒฐํ๊ธฐ ์ํด ๋ฐ๋ก ๋ณ์์ ๋ด์๋ค.
  - ๊ณต์ด ๋ธ๋ผ์ฐ์  ๋ฒฝ๋ฉด์ ํ๊ธฐ๋ ์ ๋๋ฉ์ด์์ ํ  ๊ฒฝ์ฐ canvas.width๊ฐ ์๋๋ผ stageWidth์ ํ๊ธฐ๋๋ก ํด์ค์ผ ํ๋ค. canvas.width์ ํ๊ธฐ๊ฒ ํ๋ฉด ๊ณต์ด ๋ฐ์ผ๋ก ์์ ธ๋๊ฐ๋ฒ๋ฆฐ๋ค
- ์บ๋ฒ์ค์ width์ height๋ฅผ ๊ฐ๊ฐ ๋ ๋ฐฐ ํด์ฃผ๋ฉด ์บ๋ฒ์ค๋ฅผ ๋ธ๋ผ์ฐ์  ์ฐฝ์ ๋ค ๋ด์ง ๋ชปํ๊ฒ ๋๋ค
  - ์คํฌ๋กคํ๋ฉด ๋ณผ ์ ์๋ค
  - ๋ฐ๋ผ์ ์บ๋ฒ์ค๋ฅผ ๋ธ๋ผ์ฐ์  ๋ด์ ๋ด์๋ด๋๋ก ํด์ผํ๋ค
    ![](https://images.velog.io/images/jehjong/post/e9bd4d4d-7626-41c8-94b2-c5e2b18574a5/image.png)

### canvas width, hegiht: 100%;

- ์บ๋ฒ์ค๋ฅผ ๋ธ๋ผ์ฐ์  ์ฐฝ์ ๋ง์ถ๊ธฐ ์ํด ์ด๋ฒ์๋ css๋ฅผ ๊ฑด๋๋ ค๋ณด์

```css
/*
  stylesheet.css
*/
canvas {
  width: 100%;
  height: 100%;
}
```

- ์บ๋ฒ์ค์ ํฌ๊ธฐ๊ฐ ๋ธ๋ผ์ฐ์ ์ ๋๋ฐฐ์ง๋ง ์์ถํด์ ๋ธ๋ผ์ฐ์ ์ ๋ค ๋ด์๋ธ ๊ฒ์ ํ์ธํ  ์ ์๋ค
  ![](https://images.velog.io/images/jehjong/post/36883334-2e3f-4fc7-a4d8-7d3be5c22744/image.png)

### ctx.scale(2, 2)

ํ์ง๋ง ctx๋ก ๊ทธ๋ฆฐ ๊ณต์ ํฌ๊ธฐ๊ฐ ๋๋ฌด ์์์ก๋ค. ์บ๋ฒ์ค์ ํฌ๊ธฐ๊ฐ ์ปค์ง๋งํผ ctx์ ํฌ๊ธฐ๋ ํค์์ค์ผ ์ฒ์์ ์๋ํ๋ ๋น์จ๋๋ก ๊ทธ๋ฆผ์ด ๊ทธ๋ ค์ง๊ฒ ๋๋ค

```javascript
//app.js
resize() {
  ...
  this.ctx.scale(2, 2);
  ...
}
```

![](https://images.velog.io/images/jehjong/post/4c1a1d72-db05-47fc-b250-9304b8beab26/image.png)

## ์์ฑ ์ฝ๋

<details>
  <summary><b>์ด๊ธฐ ์ธํ ์์ฑ ์ฝ๋</b></summary>
  <div markdown="1">

```javascript
//app.js
class App {
  constructor() {
    this.canvas = document.createElement('canvas');
    this.canvas.setAttribute('id', 'canvas');
    this.ctx = this.canvas.getContext('2d');

    document.body.appendChild(this.canvas);

    window.addEventListener('resize', this.resize.bind(this));
    this.resize();
  }

  resize() {
    this.stageWidth = document.body.clientWidth;
    this.stageHeight = document.body.clientHeight;

    this.canvas.width = this.stageWidth * 2;
    this.canvas.height = this.stageHeight * 2;

    this.ctx.scale(2, 2);

    this.animate();
  }

  animate() {
    this.ctx.rect(0, 0, this.stageWidth, this.stageHeight);
    this.ctx.fillStyle = '#b197fc';
    this.ctx.fill();

    this.ctx.beginPath();
    this.ctx.arc(200, 200, 50, 0, 2 * Math.PI);
    this.ctx.fillStyle = '#faa2c1';
    this.ctx.fill();
  }
}

new App();
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