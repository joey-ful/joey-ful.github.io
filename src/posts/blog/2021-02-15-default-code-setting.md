---
title: '[애니메이션 초기작업] 캔버스 애니메이션을 위한 default 코드'
category: "FrontEnd"
date: "2021-02-15 12:00:00 +09:00"
desc: "canvas animation"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

# 캔버스 애니메이션을 위한 기초 작업
- animate()의 하단에 그리고 싶은 것 추가
- [깃헙](https://github.com/joey-ful/CanvasAnimationDefault)

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
    
    this.ctx.fillStyle = 'white';
    this.ctx.fillRect(0, 0, this.stageWidth, this.stageHeight);
  }
}

new App();
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Title</title>
  <link rel="stylesheet" href="stylesheet.css" />
</head>

<body>
  <script type="module" src="./app.js"></script>
</body>

</html>
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