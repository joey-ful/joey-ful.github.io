---
icon: "☀️"
title: '[Solar System] 드래그 인터렉션 추가하기 - 삼각함수편'
category: "FrontEnd"
date: "2021-02-16 12:00:00 +09:00"
desc: "drag interaction"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

> **이번 글에서 다룰 내용**

- 행성 드래그 하기
  - 공전의 중심이 이동하는 경우
  - 행성을 클릭해서 이동하는 경우
  - mouseup이나 mouseout시 그 위치에서 공전하기
    - 새로운 orbitRadius와 theta 계산

[Solar System 애니메이션 참고](/canvas/solar-system)

# 행성 드래그 하기

![drag](https://user-images.githubusercontent.com/52592748/108350706-e834d680-7227-11eb-9962-4092484e932e.gif)

- 행성의 이동은 (공전 제외) 두 가지로 나눠서 생각해볼 수 있다
  - 행성을 클릭해서 드래그하면 마우스를 따라다니도록 한다
  - 공전의 중심을 이동할 경우 그 주변을 도는 행성들이 이동하는 중심을 기준으로 공전하도록 한다

## 행성을 클릭해서 이동하는 경우

- 클릭 여부를 나타내는 `this.clicked`를 추가해 준다

```javascript
//planet.js
constructor(name, star, radius, color, velocity, orbitRadius) {
  ...

  this.clicked = false;
}
```

- 클릭이 되어있는 동안의 행성의 좌표는 orbitRadius나 theta랑 관계없이 곧바로 마우스 커서의 좌표로 업데이트 하면 된다
  - 커서의 좌표를 담을 this.mouse 클래스를 생성

```javascript
//planet.js
constructor(name, star, radius, color, velocity, orbitRadius) {
  ...

  this.mouse = {
    x: star.x,
    y: star.y,
  };
}
```

- 이제 캔버스에 mousedown, mouseup, mouseout 이벤트를 달아준다

```javascript
//planet.js
update(ctx) {
  this.interaction();

  ...

}

...

interaction() {
  this.canvas = document.getElementById('canvas');

  this.canvas.addEventListener('mousedown', this.onMouseDown);
  this.canvas.addEventListener('mouseup', this.restore);
  this.canvas.addEventListener('mouseout', this.restore);
}

```

- mousedown은 마우스의 x, y좌표가 각각 행성의 반지름 내에 들어가면 이동을 진행한다
- 행성이 클릭되면 this.clicked를 true로 바꿔준다
  - 캔버스 아무 곳이나 클릭하는 건 무시한다

```javascript
//planet.js
...

onMouseDown = (e) => {
  this.offsetX = e.clientX - this.x;
  this.offsetY = e.clientY - this.y;

  if (
    Math.abs(this.offsetX) <= this.radius &&
    Math.abs(this.offsetY) <= this.radius
  ) {
    this.clicked = true;
    this.canvas.addEventListener('mousemove', this.onMouseMove);
  }
};
```

- this.mouse에 곧바로 커서의 위치를 담는다

```javascript
//planet.js

onMouseMove = (e) => {
  this.mouse.x = e.clientX - this.offsetX;
  this.mouse.y = e.clientY - this.offsetY;
};
```

- 여기서 offset은 `this.x, this.y`와 마우스 커서 간의 오차를 저장한 것으로

![image](https://user-images.githubusercontent.com/52592748/108588897-dd508200-739e-11eb-9a9d-bed791b4e7ef.png)

- `mouse.x, mouse.y`을 구할 때마다 이 오차를 반영해준다
- 엄밀히 따지면 `mouse.x, mouse.y`는 마우스라기보다는 새로 이동한 행성의 좌표라고 볼 수 있다

![image](https://user-images.githubusercontent.com/52592748/108584037-5640e100-7381-11eb-933a-b1c0594558e6.png)


- update 함수를 아래처럼 바꿔준다
  - `this.x, this.y`는 `this.mouse.x, this.mouse.y`의 값을 갖는다

```javascript
  update(ctx) {
    this.interaction();
    this.theta += this.velocity;

    if (this.clicked) {
      this.x = this.mouse.x;
      this.y = this.mouse.y;
    }

    this.draw(ctx);
  }
```

## 공전의 중심이 이동하는 경우

- 공전의 중심이 이동하는 경우 행성의 이동은 interaction과 상관없이 바로 공전의 중심만 업데이트해 주면 된다
- 행성 자체가 클릭되지 않은 경우, else에 구현한다

```javascript
  update(ctx) {
    ...

    if (this.clicked) {
      //행성을 직접 드래그해 이동하는 경우
    } else {
      this.x = this.star.x + this.orbitRadius * Math.cos(this.theta);
      this.y = this.star.y + this.orbitRadius * Math.sin(this.theta);
    }

    ...
  }
```


## mouseup이나 mouseout시 그 위치에서 이어서 공전하기 (새로운 orbitRadius와 theta 계산)

- interaction() 함수에서 mouseup과 mouseout 이벤트에 모두 restore() 함수를 적용시켜 같은 동작을 수행하도록 했다

```javascript
this.canvas.addEventListener('mouseup', this.restore);
this.canvas.addEventListener('mouseout', this.restore);
```

- mousedown을 해도 행성을 클릭한 것이 아니면 this.clicked는 그대로 false로 남아있는다
- 따라서 `this.clicked === true` 일 때만 되돌리는 작업을 한다
- mousedown은 바로 제거, mousemove 이벤트는 클릭된 적이 있을 때만 제거해 준다

```javascript
//planet.js
restore = () => {
  if (this.clicked) {
    //되돌리는 작업
    this.canvas.removeEventListener('mousemove', this.onMouseMove);
  }
  this.canvas.removeEventListener('mousedown', this.onMouseDown);
};
```

- 먼저 변수들을 되돌려야 한다
  - 클릭이 해제되었으니 this.clicked는 false로 되돌린다

```javascript
//planet.js
restore = () => {
  if (this.clicked) {
    this.clicked = false;

    ...
};
```

### 새로운 orbitRadius와 theta 계산

#### orbitRadius

- 이제 공전거리 `this.orbitRadius`와 공전각 `this.theta`만 계산해주면 된다
  ![image](https://user-images.githubusercontent.com/52592748/108583653-47a4fa80-737e-11eb-8f67-fa0313606dc8.png)

- 공전의 중심 `(this.star.x, this.star.y)`과 행성까지의 거리는 피타고라스 정리를 이용한다
- 각 x좌표의 차와 y좌표의 차를 구한 뒤 공식에 대입하면 `this.orbitRadius` 를 쉽게 구할 수 있다

```javascript
//resotre()
this.orbitRadius = Math.sqrt(
  Math.pow(this.star.x - this.x, 2) + Math.pow(this.star.y - this.y, 2)
);
```

#### theta

![image](https://user-images.githubusercontent.com/52592748/108583659-568bad00-737e-11eb-9e27-168d42d74399.png)

- 공전각 theta는 삼각함수를 이용해 구할 수 있다
- 방금 구한 orbitRadius와 arccos을 사용한다

```javascript
//resotre()
this.theta = Math.acos((this.x - this.star.x) / this.orbitRadius);
```

- 거의 다 완성했지만 행성을 드래그해보면 이상한 움직임을 발견할 수 있다
  - 공전의 중심을 기준으로 행성을 **위로** 드래그하면 곧바로 밑으로 내려오는 것을 알 수 있다

![theta](https://user-images.githubusercontent.com/52592748/108348054-a6566100-7224-11eb-970f-3a32785df392.gif)

- 공전각 π/4, 3π/4, 5π/4, 7π/4 (45°, 135°, 225°, 315°)를 가진 행성을 그려보면 다음을 확인할 수 있다
  - π/4와 7π/4는 같은 공전각
  - 3π/4와 5π/4는 같은 공전각

![image](https://user-images.githubusercontent.com/52592748/108024732-75313180-7068-11eb-93bd-646e308f3736.png)

- 아래 cos 함수에서도 같은 사항을 확인할 수 있다
- 0 ~ π 까지의 범위 내에서 arccos 값을 구한 후 위아래에 위치하는지에 따라 2π에서 빼주면 된다
  - [unit circle](https://www.mathsisfun.com/geometry/unit-circle.html)에서 확인해볼 수 있다

![image](https://user-images.githubusercontent.com/52592748/108033234-07d8cd00-7077-11eb-9960-a19931da37b6.png)

- 따라서 다음을 사항을 추가해주면 된다
  - 행성이 공전의 중심보다 위에 있으면 `(this.y < this.star.y)`
  - arccos으로 구한 값을 2π (360°)에서 빼준 것을 공전각으로 삼는다

```javascript
//resotre()
if (this.y - this.star.y < 0) {
  this.theta = 2 * Math.PI - this.theta;
}
```

#### 태양

- restore 함수에서 두 가지 이유로 태양과 다른 행성들을 구분해줄 필요가 있다
  1. 태양은 다른 행성들과 달리 orbitRadius와 theta의 값이 0으로 유지돼야 한다
  2. 태양은 태양의 star의 중심이 곧 태양의 중심이므로 태양이 이동하면 태양의 중심 sunStar도 똑같이 이동시켜야 한다

- 먼저 click 된 상태면서 태양이 아닌 경우엔 앞서 했던 것처럼 orbitRadius와 theta를 구하면 된다

```javascript
restore = () => {
  if (this.clicked && this.name !== 'sun') {
    //태양이 아닌 행성
    this.clicked = false;
    this.orbitRadius = Math.sqrt(
      Math.pow(this.star.x - this.x, 2) + Math.pow(this.star.y - this.y, 2)
    );
    this.theta = Math.acos((this.x - this.star.x) / this.orbitRadius);
    if (this.y - this.star.y < 0) {
      this.theta = 2 * Math.PI - this.theta;
    }
    this.canvas.removeEventListener('mousemove', this.onMouseMove);


  } else if (this.clicked && this.name === 'sun') {

    //태양

  }
  this.canvas.removeEventListener('mousedown', this.onMouseDown);
};
```

- 태양의 경우엔 star 좌표를 자신의 좌표로 바꿔준다

```javascript
restore = () => {
  if (this.clicked && this.name !== 'sun') {

    //태양이 아닌 행성

  } else if (this.clicked && this.name === 'sun') {

    this.clicked = false;
    this.star.x = this.x;
    this.star.y = this.y;

    this.canvas.removeEventListener('mousemove', this.onMouseMove);

  }
  this.canvas.removeEventListener('mousedown', this.onMouseDown);
```

- 완성!

![thetadone](https://user-images.githubusercontent.com/52592748/108348065-a9515180-7224-11eb-801c-d3920182971f.gif)

## [완성 코드](https://github.com/joey-ful/SolarSystem/commit/27154511d3403391d4ffbe27bda236daba8bf2a5)