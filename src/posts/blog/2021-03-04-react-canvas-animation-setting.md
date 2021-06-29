---
icon: "🖼"
title: 'React Canvas Animation 캔버스 세팅'
category: "FrontEnd"
date: "2021-03-04 12:00:00 +09:00"
desc: "pixel density, useEffect Hook"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

>**이번 글에서 다룰 내용**<br>
>React 프로젝트 시작하기<br>
>캔버스 애니메이션 환경 세팅하기<br>
>- Canvas
>- 컴포넌트 분리 - useEffect Hook
>- resize Canvas

# React 프로젝트 시작하기

```shell
npx create-react-app react-canvas-animation
cd react-canvas-animation
yarn start
```

- src 다 지우고 정리된 index.js와 App.js만 남기기

```javascript
//src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App.js';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

```javascript
//src/App.js
import React from 'react'

function App() {
}

export default App
```

- public 다 지우고 index.html만 남기기

```html
<!--
public/index.html
-->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <title>React Canvas Animation</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

- App.js를 가장 메인으로 코딩을 시작
- src에 컴포넌트들을 늘려가며 App.js에서 호출한다


# 캔버스 애니메이션 환경 세팅하기

## Canvas

### Canvas props

- 캔버스 컴포넌트를 생성하고 canvas element 리턴하며 캔버스를 생성한다
- 이 때 **props** 를 받아 canvas element에 넣어준다

```javascript
//Canvas.js
return <canvas {...props}/>
```

- 그리고 App.js에서 Canvas 컴포넌트를 불러온다

```javascript
//App.js
return <Canvas />
```

### useRef

- 캔버스에 그리기 위해서는 캔버스의 context에 DOM으로 접근해야 한다
- 리액트에서는 **직접적인 DOM 접근 대신 ref를 사용**한다
- canvasRef라는 것을 생성하고 canvas element의 ref에 담아준다
  - 초기값은 null로 설정해줬다

```javascript
//Canvas.js
const canvasRef = useRef(null);

return <cavas ref={canvasRef} {...props}/>
```

### getContext

- canvasRef를 통하면 canvas element에 접근해 **context를 가져올 수 있다**

```javascript
const canvas = canvasRef.current;
const ctx = canvas.getContext('2d');
```

### useEffect - component did mount

- 하지만 아직 canvasRef를 통해 canvas에 접근하면 null에 접근하게 된다
- 컴포넌트가 mount되지 않았기 때문이다
- 이 때 **useEffect를 사용하면 바로 접근이 가능**하다
  - useEffect는 컴포넌트가 마운트 되었을 때(처음 나타남), 언마운트 됐을 때(사라짐) 그리고 업데이트될 때 (특정 변수가 바뀔 때) 실행하는 Hook이다
  - 리턴값에 cleanup 함수를 담아 컴포넌트가 언마운트될 때 뒷정리를 해줄 수 있다

```javascript
//Canvas.js
useEffect(() => {
  const canvas = canvasRef.current
  const context = canvas.getContext('2d')
}, [])
```

- useEffect의 첫 번째 인자로는 **실행할 함수**를 담는다
- 두번째 인자에는 dependencies라고 의존하는 변수들이 담긴 배열을 넣어주는데, 해당 변수가 바뀔 때마다 useEffect에 등록한 함수가 호출된다
  - 당장은 컴포넌트가 마운트 될 때 한번만 함수를 실행하면 되기 때문에 빈 배열을 넣어준다

### draw

- 이번엔 그림 그리는 함수 **draw를 생성해 useEffect에서 호출**한다
- canvas의 default 크기가 width 300, height 150이기 때문에 이 안에 그려줘야 그림이 보인다
  - canvas 크기 설정은 나중에 할 예정

```javascript
//Canvas.js
const draw = ctx => {
  ctx.beginPath();
  ctx.arc(100, 100, 20, 0, Math.PI * 2);
  ctx.fillStyle = 'black';
  ctx.fill();
}

useEffect(() => {
  const canvas = canvasRef.current;
  const ctx = canvas.getContext('2d');

  draw(ctx);
}, [draw])
```
![image](https://user-images.githubusercontent.com/52592748/109375900-c90f0680-7903-11eb-92b0-8ef6acda94ae.png)

### animate

- **매 프레임 변하는 그림**을 그리려고 한다
- 전 프레임에서 그린 그림은 **clearRect로 지워준다**
- 애니메이션을 진행하기 위해서 방근 그림 **검은 공의 반지름을 프레임마다 변경**한다
  - draw에서는 frameCount라는 변수를 받아 반지름 의 계산에 사용한다
  - frameCount라는 변수는 useEffect에서 생성해 0으로 초기화 후 1씩 더해줄 예정이다
  - 반지름은 0부터 1까지 커졌다가 다시 0으로 작아지는 동작을 반복한다


```javascript
//Canvas.js
const draw = (ctx, frameCount) => {
  ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
  ...
  ctx.arc(100, 100, 20 * Math.sin(frameCount * 0.05) **2, 0, Math.PI * 2);
  ...
}
```

### render

- useEffect 내부에 **render 함수**를 생성한다
- **requestAnimationFrame에서 render**를 프레임마다 불러온다
  - 프레임마다 frameCount++
  - 변경된 frameCount를 바탕으로 검은 공 draw

```javascript
//Canvas.js
useEffect(() => {
  ...

  let frameCount = 0;
  
  const render = () => {
    frameCount++;
    draw(ctx, frameCount);
    window.requestAnimationFrame(render);
  }
  
  render();
}, [draw])
```

![animatedblackball](https://user-images.githubusercontent.com/52592748/109385157-16f03280-7935-11eb-82fe-51b9b969534f.gif)

### cleanup function

- 위까지만 구현해도 애니메이션이 잘 작동한다
- 하지만 만약 **requestAnimationFrame은 호출됐지만 render는 호출되기 전의 시점에 컴포넌트가 unmount되면** 문제가 발생한다고 한다
  - 따라서 **컴포넌트가 unmount되면 바로 애니메이션을 캔슬** 해야한다
  - 다행히 requestAnimationFrame은 **request identifier라는 것을 리턴**하기 때문에 이 값을 **cancelAnimationFrame에 전달**하면 바로 캔슬을 할 수 있다
 
```javascript
id = window.requestAnimationFrame(callback);
window.cancelAnimationFrame(id);
```

- 애니메이션을 **animationFrameId**이라는 이름으로 생성 후 **리턴에서 캔슬**해준다
  - 이 때 리턴되는 **cleanup** 함수는 **캔버스가 unmount되기 직전에 애니메이션을 캔슬**하는 뒷정리를 해준다
  
  
```javascript
//Canvas.js
useEffect(() => {
  ...
  let animationFrameId;
  
  const render = () => {
    animationFrameId = window.requestAnimationFrame(render);
  }
  
  render();
  
  return () => {
    window.cancelAnimationFrame(animationFrameId);
  }
}, [draw])
```

## 컴포넌트 분리

### App.js
- 검은 공을 그리는 Ball.js라는 컴포넌트를 따로 만들기로 한다
- 따라서 App.js 에서는 Ball 컴포넌트만 리턴하면 된다

```javascript
import React from 'react';
import Ball from './Ball.js';

function App() {
  return <Ball />
}
  
export default App;
```

### Ball.js

- Ball.js 컴포넌트에서는 공을 그리는 함수 draw를 정의하고 캔버스 컴포넌트를 리턴한다
- 여기서 draw함수를 props로 Canvas에 넘겨준다

```javascript
//App.js
function Ball() {

  const draw = (ctx, frameCount) => {
    ctx.clearRect(0, 0, stageWidth, stageHeight);
    ctx.beginPath();
    ctx.arc(stageWidth/2, stageHeight/2, 30*Math.sin(frameCount * 0.05)**2, 0, Math.PI * 2);
    ctx.fillStyle = 'pink';
    ctx.fill();
    ctx.closePath();
  }

  return <Canvas draw={draw}/>
}

export default Ball
```


### Canvas.js

- 캔버스 컴포넌트는 draw 함수를 인자로 받아온다
- useCanvas라는 이름의 useEffect Hook 만들어 빼내고 Canvas에는 아래만 남긴다
- 이 훅에서 canvasRef를 리턴해주고 이를 캔버스 엘레먼트에 연결한다

```javascript
//Canvas.js
import React from 'react';
import useCanvas from './Hooks/useCanvas.js';

const Canvas = props => {
  const { draw, ...rest } = props;
  const canvasRef = useCanvas(draw);
  
  return <canvas ref={canvasRef} {...rest}/>
}

export default Canvas;
```

### useCanvas.js

- useCanvas라는 useEffect Hook을 만들어 재사용성을 높힌다
- canvasRef를 리턴해 Canvas 컴포넌트에 넘겨준다

```javascript
import React, { useRef, useEffect } from 'react';

const useCanvas = draw => {
  const canvasRef = useRef(null);
  
  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    let frameCount = 0;
    let animationFrameId;
    
    const render = () => {
      frameCount++;
      draw(ctx, frameCount);
      animationFrameId = window.requestAnimationFrame(render);
    }
    render();
    
    return () => {
      window.cancelAnimationFrame(animationFrameId);
    }
  }, [draw]);
  
  return canvasRef
}

export default useCanvas;
```

## resize Canvas

- 캔버스의 크기는 윈도우 크기가 바뀔 때마다 변하도록 설정하도록 한다
  - 게다가 레티나 디스플레이의 경우 화질저하를 방지하기 위한 설정도 추가한다
  - 자세한 설명은 [픽셀 밀도와 캔버스 애니메이션](/canvas/pixel-density-canvas-animation) 참고
  
- useEffect Hook에 resize 함수를 선언해준다

```javascript
//useCanvas.js
useEffect(() => {
  ...
  
  const resize = () => {
  }
}, [draw]);
```

- 애니메이션이 그려질 스테이지를 윈도우의 [innerWidth, innerHeight](https://developer.mozilla.org/en-US/docs/Web/API/Window/innerWidth)로 설정하려고 한다
  - 윈도우창 전체를 사용해 그려지게 된다
- 해당 값들을 stageWidth와 stageHeight에 저장해준다
  - 이 값들은 resize 밖에서도 사용할 것이라 resize 밖에서 선언해준다
  - 실시간으로 바뀌는 윈도우 창 크기를 캔버스에 적용하기 위해 resize에서 매번 innerWidth와 innerHeight를 가져온다

```javascript
//useCanvas.js
useEffect(() => {
  ,,,
  
  let stageWidth = window.innerWidth;
  let stageHeight = window.innerHeight;

  const resize = () => {
    stageWidth = window.innerWidth;
    stageHeight = window.innerHeight;
  }

  ...

}, [draw]);
```

- 이번엔 [devicePixelRatio](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio)를 통해 윈도우의 픽셀 밀도를 가져와 stageWidth와 stageHeight에 곱해 캔버스 크기를 설정한다

```javascript
//useCanvas.js
const resize = () => {
  stageWidth = window.innerWidth;
  stageHeight = window.innerHeight;
  
  const ratio = window.devicePixelRatio;

  canvas.width = stageWidth * ratio;
  canvas.height = stageHeight * ratio;

  ctx.scale(ratio, ratio);
}
```

- 캔버스 크기를 키웠으니 이번엔 캔버스가 실제로 보여질 사이즈를 윈도우 창과 같게 맞춰준다
  - 캔버스의 CSS width와 height를 설정하면 된다
  
```javascript
//useCanvas.js
const reszie = () => {
  ,,,
    
  canvas.style.width = stageWidth + 'px';
  canvas.style.height = stageHeight + 'px';
}
```

- 캔버스 여러개르 겹칠 수 있도록 position:absolute로 설정하고 body 마진을 없애 윈도우 창에 캔버스가 꽉 차도록 한다

```javascript
canvas.style.position = 'absolute';
document.body.style.margin = '0';
```

- 윈도우창 크기가 변할 때마다 이 resize 함수를 불러오는 이벤트를 단다

```javascript
//useCanvas.js
const resize = () => {
  ,,,
    
  window.addEventListener('resize', resize);
}
```

- useEffect에서 resize를 한번 실행해주고 마지막으로 return의 cleanup 함수에서 이벤트를 해제해준다

```javascript
//useCanvas.js
useEffect(() => {
  ...
  
  return () => {
    ...
    window.removeEventListener('resize', resize);
  }
}, [draw]);
```

## [완성 코드](https://github.com/joey-ful/ReactCanvasAnimationDefault.git)