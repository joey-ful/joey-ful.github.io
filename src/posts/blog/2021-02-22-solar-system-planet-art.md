---
title: '[Solar System] 행성 일러스트 추가하기 - 삼각함수편'
category: "FrontEnd"
date: "2021-02-22 12:00:00 +09:00"
desc: "interaction, "
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---
---

category: canvas
tags:
  - animation
  - orbit
  - trigonometry
  - drag
  - interaction
  - shadow
  - vector art
  - planet art
  - illustration
comments: true
toc: true
toc_label: Table of Contents
toc_sticky: true
toc_icon: far fa-list-alt
---

> **이번 글에서 다룰 내용**<br>
>행성 일러스트 추가하기
>- 이미지 그리기
>- 원이 아닌 이미지 위치 조정하기
>- 원이 아닌 이미지 그림자 위치 조정하기
  
[Solar System 애니메이션 참고](/canvas/solar-system)<br>
[Solar System 애니메이션 드래그 인터렉션 참고](/canvas/solar-system-drag)<br>
[Solar System 배경 레이어 추가 버전 참고](/canvas/solar-system-drag-upgrade) <br>[Solar System Elastic Drag 참고](/canvas/solar-system-elastic-drag) <br>
[Solar System 둥근 그림자 참고](/canvas/solar-system-shadow-round)


# 행성 일러스트 추가하기
![shadow](https://user-images.githubusercontent.com/52592748/108626000-47495400-7491-11eb-82c0-66f63b16f8a2.gif)

## 이미지 그리기

- 이미지 그리는 것은 [Bouncing Ball 공에 로고 넣기](https://joey-ful.github.io/canvas/bouncing-ball-clip/) 참고

- 행성 이미지들은 높낮이가 같은 원으로 모두 행성이름.png로 저장했다
- saturn과 uranus만 주변 띠를 둘러주었다 (높낮이가 달라 조정 필요)
  - 대략 width가 height의 두 배가 되도록 제작했다

<img src="https://user-images.githubusercontent.com/52592748/108720761-9318fd80-7564-11eb-8818-4d73d0cf4ed8.png" width="500"/>

- 이미지를 먼저 생성 후 원을 그리는 대신 이미지를 그려준다

```javascript
//planet.js
constructor(name, star, radius, color, velocity, orbitRadius) {
  ,,,
    
  this.img = new Image();
}

...

draw(ctx) {
  ctx.beginPath();
  ctx.drawImage(
    this.img,
    this.x - this.radius,
    this.y - this.radius,
    this.radius * 2,
    this.radius * 2
  );
  
  this.img.src = `./srcs/${this.name}.png`;
  ctx.closePath();
}
```

- 주변에 고리가 있는 찌그러진 saturn과 uranus를 제외하고는 잘 그려진 것을 확인할 수 있다

![distorted](https://user-images.githubusercontent.com/52592748/108720537-4b927180-7564-11eb-80c2-a7a8793f5e95.gif)

## 원이 아닌 이미지 위치 조정하기

```javascript
drawImage(img, x, y, width, height);
```

- 고리가 있는 행성은 width가 다르므로 따로 설정해줘야 한다
- width는 height, 즉 지름의 대략 두 배로 설정했다

```javascript
let width = this.radius * 2;

if (this.name === 'saturn') {
  width = this.radius * 4;
} else if (this.name === 'uranus') {
  width = this.radius * 4.2;
}

ctx.drawImage(
  this.img,
  this.x - this.radius,
  this.y - this.radius,
  width,
  this.radius * 2
);
```

- width가 늘어났지만 그림을 그리는 좌표는 그대로라 그림이 우측에 그려졌다
- 그림을 그리는 시작 좌표도 바꿔줘야 한다

![image](https://user-images.githubusercontent.com/52592748/108728223-b5167e00-756c-11eb-8d66-eb8bf71421a6.png)

- drawImage()는 이미지의 좌측 상단부터 그리기 시작하므로 원의 중심에서 각각 width와 height의 절반씩 빼줘야 한다
- imgX, imgY를 설정해주고 거기서 대략 반지름 길이만큼 한번 더 빼준다

```javascript
let width = this.radius * 2;
let imgX = this.x - this.radius;
let imgY = this.y - this.radius;

if (this.name === 'saturn') {
  width = this.radius * 4;
  imgX -= this.radius;
} else if (this.name === 'uranus') {
  width = this.radius * 4.2;
  imgX -= this.radius * 1.1;
}

ctx.drawImage(
  this.img,
  imgX,
  imgY,
  width,
  this.radius * 2
);
```

- 가운데 정렬

![image](https://user-images.githubusercontent.com/52592748/108728154-a334db00-756c-11eb-9052-26cf20ffdb69.png)

## 원이 아닌 이미지 그림자 위치 조정하기

- update()함수에서 test()함수를 호출시켜 그림자 시작위치에 점을 찍어보면 saturn만 행성 바깥에 점이 찍히는 것을 확인할 수 있다 (고리가 있는 베이지색 행성)

![](https://images.velog.io/images/jehjong/post/90dc314f-4621-43d4-b341-8c9f6196caaf/image.png)

- radius 변수를 만들어 saturn인 경우만 radius를 80%로 축소해 A, B 좌표를 구하도록 한다

```javascript
//planet.js
findXYdistance() {
  let radius = this.radius;

  if (this.name === 'saturn') {
    radius = this.radius * 0.8;
  }

  this.radiusX = radius * Math.sin(this.theta);
  this.radiusY = radius * Math.cos(this.theta);
  
  ,,,
    
}
```

## [완성 코드](https://github.com/joey-ful/SolarSystem/tree/planet-art)