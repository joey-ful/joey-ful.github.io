---
icon: "☀️"
title: '[Solar System] 직사각형 그림자 드리우기 - 삼각함수편'
category: "FrontEnd"
date: "2021-02-21 12:00:00 +09:00"
desc: "orbit, shadow"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

> **이번 글에서 다룰 내용**
- 막대 그림자 효과 추가
  - 그림자 캔버스 추가하기
  - 그림자 그리기
  - 인터렉티브한 그림자 만들기
  
[Solar System 애니메이션 참고](/canvas/solar-system)<br>
[Solar System 애니메이션 드래그 인터렉션 참고](/canvas/solar-system-drag)<br>
[Solar System 배경 레이어 추가 버전 참고](/canvas/solar-system-drag-upgrade)

# 막대 그림자 효과 추가
![shadow](https://user-images.githubusercontent.com/52592748/108507123-2e0f9e80-72fd-11eb-97b0-53636b26fd24.gif)
## 그림자 캔버스 추가하기
- 그림자가 다른 행성들에게도 드리울 수 있도록 새로운 캔버스를 추가해 그 위에 그려준다
- 배경, 애니메이션, 그림자 이렇게 총 세 개의 캔버스가 생기므로 캔버스 생성 및 설정 함수들을 만들어준다
- 먼저 createAndAppendCanvas로 배경, 애니메이션, 그림자 순으로 캔버스를 append해준다
  - 먼저 append된 것이 아래에 깔린다
  - z-index로 추후에 설정할 수도 있다

```javascript
//app.js
constructor() {
  this.stage = document.createElement('div');
  this.stage.setAttribute('id', 'stage');
  document.body.appendChild(this.stage);

  this.stage.appendChild(this.createAndAppendCanvas('back'));
  this.stage.appendChild(this.createAndAppendCanvas(''));
  this.stage.appendChild(this.createAndAppendCanvas('shadow'));

  window.addEventListener('resize', this.resize.bind(this));
  this.resize();

  new Background(400, this.backctx, this.stageWidth, this.stageHeight);
  this.createPlanets();
  this.animate();
}

createAndAppendCanvas(name) {
  this[`${name}canvas`] = document.createElement('canvas');
  this[`${name}canvas`].setAttribute('id', `${name}canvas`);

  this[`${name}ctx`] = this[`${name}canvas`].getContext('2d');

  return this[`${name}canvas`];
}
```

- sizeCanvasAndScaleCtx 함수를 만들어 캔버스와 컨텍스트 크기를 설정해주었다

```javascript
//app.js
resize() {
  this.stageWidth = document.body.clientWidth;
  this.stageHeight = document.body.clientHeight;

  this.sizeCanvasAndScaleCtx('');
  this.sizeCanvasAndScaleCtx('back');
  this.sizeCanvasAndScaleCtx('shadow');
}

sizeCanvasAndScaleCtx(name) {
  this[`${name}canvas`].width = this.stageWidth * 2;
  this[`${name}canvas`].height = this.stageHeight * 2;

  this[`${name}ctx`].scale(2, 2);
}
```

- 그림자는 shadowctx에 그리도록 한다
![image](https://user-images.githubusercontent.com/52592748/108631343-dfa10200-74ac-11eb-8a8f-5662aa5f9a2c.png)

## 그림자 그리기
- 간단하게 막대 그림자를 넣어보려 한다
  - 편의상 태양과 달은 제외한다
- 이를 위해 moveTo와 lineTo를 사용하려고 한다
  - moveTo로 행성의 중심 좌표로 이동한다
  - lineTo로 태양의 반대방향으로 일정 거리만큼 이동한다
  - lineWidth를 행성의 지름만큼 지정한다
  - 두 좌표를 stroke()를 사용해 이어준다

```javascript
ctx.moveTo(this.x, this.y);
ctx.lineTo(toX, toY);
ctx.lineWidth = this.radius;
ctx.stroke();
```

- 그럼 아래와 같은 두꺼운 흰 선이 그려지게 된다
![image](https://user-images.githubusercontent.com/52592748/108696914-8507b480-7545-11eb-8fb4-84a209f16f5a.png)

- toX와 toY의 좌표는 삼각함수를 이용할 수 있다

![image](https://user-images.githubusercontent.com/52592748/108696825-64d7f580-7545-11eb-8046-0ac7f7c8c671.png)

- 그림자의 길이 D는 this.radius + 30 으로 설정했다

```javascript
toX = this.x + (this.radius + 30) * Math.cos(this.theta);
toY = this.y + (this.radius + 30) * Math.sin(this.theta);
```

- drawShadow() 함수를 생성해 그림자를 그려주고 이를 update()함수에서 호출 한다
  - app.js에서 update를 호출할 때 shadowctx도 넘겨주도록 한다
- 그림자의 색상은 행성 위에서만 보이도록 배경과 같은 색으로 설정했다
  - 그리고 opacity를 0.3으로 설정했다

```javascript
//planet.js
update(ctx, shadowctx) {
  ,,,
    
  this.drawShadow(shadowctx);
}

drawShadow(shadowctx) {
  if (this.name !== 'sun' && this.name !== 'moon') {
    this.toX = this.x + (this.radius + 30) * Math.cos(this.theta);
    this.toY = this.y + (this.radius + 30) * Math.sin(this.theta);

    shadowctx.beginPath();
    shadowctx.strokeStyle = 'rgba(33, 69, 104, 0.3)';
    shadowctx.lineWidth = this.radius * 2;
    shadowctx.moveTo(this.x, this.y);
    shadowctx.lineTo(this.toX, this.toY);
    shadowctx.lineWidth = this.radius * 2;
    shadowctx.stroke();
    shadowctx.closePath();
  }
}
```

## 인터렉티브한 그림자 만들기

![interactiveshadow](https://user-images.githubusercontent.com/52592748/108632065-5c81ab00-74b0-11eb-8dba-fe18aa201732.gif)

- 지금 만든 그림자는 아래처럼 클릭이 되지 않은 상태에서만 행성과 함께 돌아간다

![noninteractiveshadow](https://user-images.githubusercontent.com/52592748/108631477-69e96600-74ad-11eb-8c0e-34d220eef012.gif)

- 드래그 중일 때에도 행성 위치에 따라 태양 반대쪽으로 그림자를 드리우도록 해보려 한다
  - 이것 역시 해당 행성이 태양과 달일 때는 제외한다
- 이를 위해 드래그 도중 업데이트해야하는 것은 세 가지로 restore함수와 drawShadow함수의 식을 그대로 갖다쓰면 된다
  - orbitRadius
  - theta
  - toX, toY

```javascript
//planet.js
onMouseMove = (e) => {
  this.mouse.x = e.clientX - this.offsetX;
  this.mouse.y = e.clientY - this.offsetY;

  if (this.name !== 'sun' && this.name !== 'moon') {

    this.orbitRadius = Math.sqrt(
      Math.pow(this.mouse.x - this.star.x, 2) +
      Math.pow(this.mouse.y - this.star.y, 2)
    );

    this.theta = Math.acos((this.mouse.x - this.star.x) / this.orbitRadius);

    if (this.mouse.y - this.star.y < 0) {
      this.theta = 2 * Math.PI - this.theta;
    }

    this.toX = this.mouse.x + (this.radius + 30) * Math.cos(this.theta);
    this.toY = this.mouse.y + (this.radius + 30) * Math.sin(this.theta);
  }
};
```

- 드래그 중이기 때문에 `this.x, this.y` 대신 `this.mouse.x, this.mouse.y`를 대입해서 계산했다

## [완성 코드](https://github.com/joey-ful/SolarSystem/tree/shadow)