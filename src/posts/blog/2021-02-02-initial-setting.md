---
title: '[애니메이션 초기작업] 윈도우 창이 변할 때마다 Canvas 크기 새로 지정'
category: "FrontEnd"
date: "2021-02-02 12:00:00 +09:00"
desc: "resize canvas"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

# [캔버스 애니메이션](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial)

> **이번 글에서 다룰 내용**

- canvas란?
- canvas 크기를 브라우저 창 크기로 지정
- 브라우저 크기가 변할 때마다 canvas 크기 새로 설정
- 고화질의 애니메이션을 위한 canvas 크기 설정

## canvas란?

- 2차원 비트맵 그래픽과 애니메이션을 그릴 수 있는 HTML 요소
- 패스, 도형, 글자 등을 그리고 이미지를 추가하는 [다양한 함수들](https://www.w3schools.com/tags/ref_canvas.asp)이 존재
- width와 height 속성만 존재 (default width: 150, height: 300)
- 캔버스는 고정 크기의 드로잉 영역을 생성하고 하나 이상의 rendering context를 통해 출력할 컨텐츠를 다룸
  - 캔버스에 무언가를 그리기 위해서는 랜더링 컨텍스트에 접근 필요

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

- contextType `2d` - 2차원 랜더링 컨텍스트

## canvas 크기를 브라우저 창 크기로 지정

- 캔버스의 기본 크기는 300 \* 150
- rect는 직사각형을 그리는 함수
- 그림 그리기는 [캔버스에 그림 그리기](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes) 참조

```
ctx.rect(x, y, width, height);
```

- 캔버스 크기만큼 색을 채우면

```javascript
class App {
  constructor() {
	...

    // 직사각형 생성
    this.ctx.rect(0, 0, this.canvas.width, this.canvas.height);
    this.ctx.fillStyle = '#b197fc';
    this.ctx.fill();
  }
}

new App();

```

- 300 \* 150만 채워져서 나온다

![](https://images.velog.io/images/jehjong/post/8d21e9e0-e179-4ad3-91f9-49b08f8cb381/image.png)

### clientWidth, clientHeight로 지정

캔버스가 너무 작으니 클라이언트창만하게 바꿔보자

```javascript
class App {
  constructor() {
	...
    this.canvas.width = document.body.clientX;
    this.canvas.height = document.body.clientY;

    // 직사각형 생성
    this.ctx.rect(0, 0, this.canvas.width, this.canvas.height);
    this.ctx.fillStyle = '#b197fc';
    this.ctx.fill();
  }
}

new App();
```

- 특이하게도 크롬창의 1446 \* 154만큼이 채워졌다

![](https://images.velog.io/images/jehjong/post/dd8cdc54-7a0c-44d1-98d9-fe481a201803/image.png)

### html 크기 조정

html을 확인해보면 사이즈가 제한되어있음을 확인할 수 있다
![](https://images.velog.io/images/jehjong/post/e6480989-3ad9-443c-9221-4945774aca6c/image.png)

- html 크기를 100%로 지정해주었다

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

### body 크기 조정

- 하지만 이번에는 body사이즈가 제한된 것을 볼 수 있다
  ![](https://images.velog.io/images/jehjong/post/de7aa89a-b942-4d21-831a-cb0eadd676de/image.png)

- 따라서 body 사이즈도 100%로 조정해주었다

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

## 브라우저 크기가 변할 때마다 canvas 크기 새로 설정

- 한번 캔버스 사이즈가 지정되면 브라우저 창의 크기가 바뀌어도 캔버스 사이즈는 그대로 남아있게 된다

### resize()

- 브라우저 크기에 맞춰 캔버스 크기를 지정하는 resize 함수를 생성
- 윈도우 창이 바뀔 때마다 resize 호출

```javascript
//app.js
class App {
  constructor() {
	...

    //윈도우 창 크기가 변할 때마다 resize 함수를 호출
    window.addEventListener('resize', this.resize.bind(this));
    this.resize();
  }

  //캔버스 사이즈 지정하는 함수
  resize() {
    this.canvas.width = document.body.clientWidth;
    this.canvas.height = document.body.clientHeight;

    this.animate();
  }

  //그림 그리는 함수
  animate() {
    //보라색으로 캔버스 채우기
    this.ctx.rect(0, 0, this.canvas.width, this.canvas.height);
    this.ctx.fillStyle = '#b197fc';
    this.ctx.fill();

    //핑크색 공 그리기
    this.ctx.beginPath();
    this.ctx.arc(200, 200, 50, 0, 2 * Math.PI);
    this.ctx.fillStyle = '#faa2c1';
    this.ctx.fill();
  }
}

new App();
```

- 브라우저 창 크기가 변하면 보라색 상자크기도 맞춰 변한다
  ![](https://images.velog.io/images/jehjong/post/da77a9be-127c-45bb-ab70-b5b136db87e3/image.png)

- 핑크 공은 지정사이즈라 변하지 않는다
  ![](https://images.velog.io/images/jehjong/post/53d5b15f-faed-43d1-8e42-9c4ac3b4a79f/image.png)

## 고화질의 애니메이션을 위한 canvas 크기 설정

### retina display (옵션)

- [Interactive Developer](https://www.youtube.com/watch?v=sLCiI6d5vTM&t=164s)는 레티나 디스플레이에서도 선명한 애니메이션을 위해(뇌피셜) 캔버스의 크기를 일부러 네배로 키워준다고 한다
- 나중에 유명해지면 해보자

### width와 height를 두 배로

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

- stageWidth와 stageHeight는 브라우저 크기를 기억하기 위한 변수다
  - 당장은 쓸모없지만 브라우저크기를 매번 document.body.clientWidth로 불러오는 번거로움을 해결하기 위해 따로 변수에 담았다.
  - 공이 브라우저 벽면에 튕기는 애니메이션을 할 경우 canvas.width가 아니라 stageWidth에 튕기도록 해줘야 한다. canvas.width에 튕기게 하면 공이 밖으로 삐져나가버린다
- 캔버스의 width와 height를 가각 두 배 해주면 캔버스를 브라우저 창에 다 담지 못하게 된다
  - 스크롤하면 볼 수 있다
  - 따라서 캔버스를 브라우저 내에 담아내도록 해야한다
    ![](https://images.velog.io/images/jehjong/post/e9bd4d4d-7626-41c8-94b2-c5e2b18574a5/image.png)

### canvas width, hegiht: 100%;

- 캔버스를 브라우저 창에 맞추기 위해 이번에는 css를 건드려보자

```css
/*
  stylesheet.css
*/
canvas {
  width: 100%;
  height: 100%;
}
```

- 캔버스의 크기가 브라우저의 두배지만 압축해서 브라우저에 다 담아낸 것을 확인할 수 있다
  ![](https://images.velog.io/images/jehjong/post/36883334-2e3f-4fc7-a4d8-7d3be5c22744/image.png)

### ctx.scale(2, 2)

하지만 ctx로 그린 공의 크기가 너무 작아졌다. 캔버스의 크기가 커진만큼 ctx의 크기도 키워줘야 처음에 의도했던 비율대로 그림이 그려지게 된다

```javascript
//app.js
resize() {
  ...
  this.ctx.scale(2, 2);
  ...
}
```

![](https://images.velog.io/images/jehjong/post/4c1a1d72-db05-47fc-b250-9304b8beab26/image.png)

## 완성 코드

<details>
  <summary><b>초기 세팅 완성 코드</b></summary>
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