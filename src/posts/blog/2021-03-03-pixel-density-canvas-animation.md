---
icon: "📱"
title: '픽셀 밀도(Pixel Density)와 캔버스 애니메이션'
category: "FrontEnd"
date: "2021-03-03 12:00:00 +09:00"
desc: "pixel density, ppi, dpi"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

>**이번 글에서 다룰 내용**
>- 픽셀 밀도 Pixel Density
>   - 픽셀 밀도란?
>   - 픽셀 밀도의 단위 PPI/DPI
>   - 픽셀 밀도와 화질
>- 픽셀 밀도를 고려한 캔버스 애니메이션
>   - devicePixelRatio
>   - 화질 비교
>   - 픽셀 밀도 고려해 코드 구현하기


### 참고
[Pixel Density Medium Post](https://medium.com/@peternowell/pixel-density-demystified-a4db63ba2922)<br>
[픽셀 밀도 한글 포스트](https://webclub.tistory.com/629)

## 픽셀 밀도 Pixel Density

### 픽셀 밀도란?

- **픽셀 밀도(pixel density)**란 **물리적인 1픽셀에 얼마나 많은 픽셀이 들어가는지**를 나타내는 것으로 첫 맥에서는 1인치에 72픽셀을 담았다고 한다
  - 현재 100 ~ 150ppi(pixel per inch)사이면 좋은 모니터라고 하며 보통 115 ~ 160ppi 사이라고 한다
  - 2010년 애플이 **인치당 두 배의 픽셀**을 출력하는 **레티나 디스플레이**를 선보였고 이후 3x(높이 너비 각각 3배), 4x 등 픽셀 밀도가 높은 화면들이 많이 출시되었다
  

### 픽셀 밀도의 단위 PPI/DPI

- **ppi(pixel per inch)**는 **디스플레이 장치에서 1인치가 몇 개의 픽셀**로 이루어졌는지를 나타낸다
- **DPI(dots per inch)**는 **인쇄물에서 1인치가 몇 개의 점**으로 이루어졌는지를 나타낸다
  - ppi는 디스플레이에서, DPI는 인쇄물에 쓰이는 것으로 서로 다른 개념이지만 흔히 **컴퓨터 스크린을 표현하는데 둘 다 같은 의미로 사용**된다 [참고](https://www.sebastien-gabriel.com/designers-guide-to-dpi/)
- [모니터 PPI 사이트](https://www.sven.de/dpi/)에서 제품별 ppi를 확인해볼 수 있고 [모니터 DPI 체크 사이트](https://www.infobyip.com/detectmonitordpi.php)에서는 현재 모니터가 인치당 몇 픽셀을 담고 있는지 알려준다
  - 맥북 프로는 [모니터 PPI 사이트](https://www.sven.de/dpi/)를 확인해보면 226ppi를 가지며 [모니터 DPI 체크 사이트](https://www.infobyip.com/detectmonitordpi.php)에서는 192dpi를 갖는다고 한다 (대략 2x => 레니타 디스플레이).

  
### 픽셀 밀도와 화질
- 픽셀 밀도가 높아지면 아래 [그림](http://styleguide.co.kr/content/resolution-grid/ratio-design.php)과 같이 픽셀 하나의 크기는 점점 작아지게 되지만 **같은 너비는 더 많은 픽셀들로 이루어지게 된다**
![image](https://user-images.githubusercontent.com/52592748/109468735-5db66780-7ab0-11eb-8c42-3aa29b33324d.png)

- 따라서 아래 [그림](https://www.giffgaff.com/blog/pixel-density-how-to-calculate-ppi/)처럼 **ppi가 높아질수록 더욱 정밀한 묘사가 가능**해진다
![image](https://user-images.githubusercontent.com/52592748/109455520-30f75580-7a9a-11eb-865a-e5d2f5fd79d4.png)

- 하지만 1픽셀짜리 그림을 2x의 고밀도 디스플레이 (레티나 디스플레이)에서 그릴 경우 아래처럼 그림이 작아지게되고, 그림의 크기를 강제로 **의도했던 크기로 키울 경우 그림이 깨지게 된다**
![](https://images.velog.io/images/jehjong/post/751a0a37-3187-4370-9fa6-0c6d7b78e05f/image.png)

- [그림](https://www.giffgaff.com/blog/pixel-density-how-to-calculate-ppi/)을 보면 장치마다 픽셀 밀도가 달라 이를 고려하지 않은 디자인을 하면 쉽게 화질 저하로 이어질 수 있다
![image](https://user-images.githubusercontent.com/52592748/109460758-76b91b80-7aa4-11eb-92a9-c8830b29ce6d.png)

## 픽셀 밀도를 고려한 캔버스 애니메이션

- 픽셀 밀도를 고려하기 위해서는 일단 픽셀 밀도를 구해야 한다. 이는 devicePixelRatio로 쉽게 구할 수 있다

### devicePixelRatio

- [devicePixelRatio](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio)는 장치의 픽셀 밀도를 나타내는 값으로 default값인 비율 1은 **96DPI** 디스플레이를 뜻한다
  - 맥북은 레티나 디스플레이기 때문에 해당 비율이 2가 나온다 (인치당 두 배의 픽셀)

```javascript
window.devicePixelRatio;
```

### 화질 비교
- 화질 비교는 **96dpi 일반 모니터**에서 진행했다. 캔버스에 300px*400px 직사각형을 그린 후 **윈도우 창을 500% 확대**했다. 이렇게 하면 **devicePixelRatio는 5**가 나오며 이는 맥북 프로보다도 높은 픽셀 밀도를 갖게된다
- 같은 그림이지만 **첫 이미지는** 픽셀 밀도를 고려하지 않은 것이라 가장자리가 **깨져보이고** **두 번째처럼 픽셀 밀도를 고려해 그리면 선명하게 출력**된다. (캡처 사진이라 차이가 명확하지 않지만 실제로 확인해보면 차이가 확연하다)
![image](https://user-images.githubusercontent.com/52592748/109763211-19a09f80-7c35-11eb-8c77-b50601a9cc73.png)
![image](https://user-images.githubusercontent.com/52592748/109765024-9df42200-7c37-11eb-989e-1adaeff14a84.png)


### 픽셀 밀도 고려해 코드 구현하기

- 픽셀 밀도를 고려한 캔버스 애니메이션을 하기 위해서는 두 가지를 해야 한다
  - 1. devicePixelRatio만큼 캔버스 키우기
  - 2. 뷰포트에 맞게 캔버스의 CSS너비 축소하기

#### 1. devicePixelRatio만큼 캔버스 키우기
- 먼저 원하는 캔버스 사이즈로 stageWidth, stageHeight를 설정한다. default 값인 300px * 150px로 놔둬도 되고 특정 픽셀로 해도 지정해도 된다
- 윈도우 창에 꽉 차도록 innerWidth와 innerHeight 값으로 설정해봤다

```javascript
let stageWidth = window.innerWidth;
let stageHeight = window.innerHeight;
```

- 그 다음에는 장치의 pixelRatio를 구해서 그만큼 캔버스의 크기를 키운다
- 캔버스의 width와 height뿐만 아니라 컨텍스트도 동일한 비율로 키워야 한다

```javascript
let ratio = window.devicePixelRatio;

canvas.width = stageWidth * ratio;
canvas.height = stageHeight * ratio;

ctx.scale(ratio, ratio);
```

#### 2. 뷰포트에 맞게 캔버스의 CSS너비 축소하기

- 위 단계까지 진행했으면 캔버스가 의도했던 크기로 나타나지 않고 캔버스가 윈도우 창에 다 담기지 못 해 스크롤을 해야하는 상황이 발생할 수도 있다. 이는 보여지는 css 크기도 달라졌기 때문으로 css를 따로 설정해줘야 한다

```javascript
canvas.style.width = stageWidth + 'px';
canvas.style.height = stageHeight + 'px';
```

- 이제 캔버스는 `stageWdith * devicePixelRatio`, `stageHeight * devicePixelRatio`의 가로세로 길이를 갖게 되며 브라우저에서는 stageWidth, stageHeight의 가로세로로 보여지게 된다 (큰 그림을 축소한 것이라고 생각하면 된다)