---
icon: "๐ผ"
title: 'React Canvas Animation ์บ๋ฒ์ค ์ธํ'
category: "FrontEnd"
date: "2021-03-04 12:00:00 +09:00"
desc: "pixel density, useEffect Hook"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

>**์ด๋ฒ ๊ธ์์ ๋ค๋ฃฐ ๋ด์ฉ**<br>
>React ํ๋ก์ ํธ ์์ํ๊ธฐ<br>
>์บ๋ฒ์ค ์ ๋๋ฉ์ด์ ํ๊ฒฝ ์ธํํ๊ธฐ<br>
>- Canvas
>- ์ปดํฌ๋ํธ ๋ถ๋ฆฌ - useEffect Hook
>- resize Canvas

# React ํ๋ก์ ํธ ์์ํ๊ธฐ

```shell
npx create-react-app react-canvas-animation
cd react-canvas-animation
yarn start
```

- src ๋ค ์ง์ฐ๊ณ  ์ ๋ฆฌ๋ index.js์ App.js๋ง ๋จ๊ธฐ๊ธฐ

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

- public ๋ค ์ง์ฐ๊ณ  index.html๋ง ๋จ๊ธฐ๊ธฐ

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

- App.js๋ฅผ ๊ฐ์ฅ ๋ฉ์ธ์ผ๋ก ์ฝ๋ฉ์ ์์
- src์ ์ปดํฌ๋ํธ๋ค์ ๋๋ ค๊ฐ๋ฉฐ App.js์์ ํธ์ถํ๋ค


# ์บ๋ฒ์ค ์ ๋๋ฉ์ด์ ํ๊ฒฝ ์ธํํ๊ธฐ

## Canvas

### Canvas props

- ์บ๋ฒ์ค ์ปดํฌ๋ํธ๋ฅผ ์์ฑํ๊ณ  canvas element ๋ฆฌํดํ๋ฉฐ ์บ๋ฒ์ค๋ฅผ ์์ฑํ๋ค
- ์ด ๋ **props** ๋ฅผ ๋ฐ์ canvas element์ ๋ฃ์ด์ค๋ค

```javascript
//Canvas.js
return <canvas {...props}/>
```

- ๊ทธ๋ฆฌ๊ณ  App.js์์ Canvas ์ปดํฌ๋ํธ๋ฅผ ๋ถ๋ฌ์จ๋ค

```javascript
//App.js
return <Canvas />
```

### useRef

- ์บ๋ฒ์ค์ ๊ทธ๋ฆฌ๊ธฐ ์ํด์๋ ์บ๋ฒ์ค์ context์ DOM์ผ๋ก ์ ๊ทผํด์ผ ํ๋ค
- ๋ฆฌ์กํธ์์๋ **์ง์ ์ ์ธ DOM ์ ๊ทผ ๋์  ref๋ฅผ ์ฌ์ฉ**ํ๋ค
- canvasRef๋ผ๋ ๊ฒ์ ์์ฑํ๊ณ  canvas element์ ref์ ๋ด์์ค๋ค
  - ์ด๊ธฐ๊ฐ์ null๋ก ์ค์ ํด์คฌ๋ค

```javascript
//Canvas.js
const canvasRef = useRef(null);

return <cavas ref={canvasRef} {...props}/>
```

### getContext

- canvasRef๋ฅผ ํตํ๋ฉด canvas element์ ์ ๊ทผํด **context๋ฅผ ๊ฐ์ ธ์ฌ ์ ์๋ค**

```javascript
const canvas = canvasRef.current;
const ctx = canvas.getContext('2d');
```

### useEffect - component did mount

- ํ์ง๋ง ์์ง canvasRef๋ฅผ ํตํด canvas์ ์ ๊ทผํ๋ฉด null์ ์ ๊ทผํ๊ฒ ๋๋ค
- ์ปดํฌ๋ํธ๊ฐ mount๋์ง ์์๊ธฐ ๋๋ฌธ์ด๋ค
- ์ด ๋ **useEffect๋ฅผ ์ฌ์ฉํ๋ฉด ๋ฐ๋ก ์ ๊ทผ์ด ๊ฐ๋ฅ**ํ๋ค
  - useEffect๋ ์ปดํฌ๋ํธ๊ฐ ๋ง์ดํธ ๋์์ ๋(์ฒ์ ๋ํ๋จ), ์ธ๋ง์ดํธ ๋์ ๋(์ฌ๋ผ์ง) ๊ทธ๋ฆฌ๊ณ  ์๋ฐ์ดํธ๋  ๋ (ํน์  ๋ณ์๊ฐ ๋ฐ๋ ๋) ์คํํ๋ Hook์ด๋ค
  - ๋ฆฌํด๊ฐ์ cleanup ํจ์๋ฅผ ๋ด์ ์ปดํฌ๋ํธ๊ฐ ์ธ๋ง์ดํธ๋  ๋ ๋ท์ ๋ฆฌ๋ฅผ ํด์ค ์ ์๋ค

```javascript
//Canvas.js
useEffect(() => {
  const canvas = canvasRef.current
  const context = canvas.getContext('2d')
}, [])
```

- useEffect์ ์ฒซ ๋ฒ์งธ ์ธ์๋ก๋ **์คํํ  ํจ์**๋ฅผ ๋ด๋๋ค
- ๋๋ฒ์งธ ์ธ์์๋ dependencies๋ผ๊ณ  ์์กดํ๋ ๋ณ์๋ค์ด ๋ด๊ธด ๋ฐฐ์ด์ ๋ฃ์ด์ฃผ๋๋ฐ, ํด๋น ๋ณ์๊ฐ ๋ฐ๋ ๋๋ง๋ค useEffect์ ๋ฑ๋กํ ํจ์๊ฐ ํธ์ถ๋๋ค
  - ๋น์ฅ์ ์ปดํฌ๋ํธ๊ฐ ๋ง์ดํธ ๋  ๋ ํ๋ฒ๋ง ํจ์๋ฅผ ์คํํ๋ฉด ๋๊ธฐ ๋๋ฌธ์ ๋น ๋ฐฐ์ด์ ๋ฃ์ด์ค๋ค

### draw

- ์ด๋ฒ์ ๊ทธ๋ฆผ ๊ทธ๋ฆฌ๋ ํจ์ **draw๋ฅผ ์์ฑํด useEffect์์ ํธ์ถ**ํ๋ค
- canvas์ default ํฌ๊ธฐ๊ฐ width 300, height 150์ด๊ธฐ ๋๋ฌธ์ ์ด ์์ ๊ทธ๋ ค์ค์ผ ๊ทธ๋ฆผ์ด ๋ณด์ธ๋ค
  - canvas ํฌ๊ธฐ ์ค์ ์ ๋์ค์ ํ  ์์ 

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

- **๋งค ํ๋ ์ ๋ณํ๋ ๊ทธ๋ฆผ**์ ๊ทธ๋ฆฌ๋ ค๊ณ  ํ๋ค
- ์  ํ๋ ์์์ ๊ทธ๋ฆฐ ๊ทธ๋ฆผ์ **clearRect๋ก ์ง์์ค๋ค**
- ์ ๋๋ฉ์ด์์ ์งํํ๊ธฐ ์ํด์ ๋ฐฉ๊ทผ ๊ทธ๋ฆผ **๊ฒ์ ๊ณต์ ๋ฐ์ง๋ฆ์ ํ๋ ์๋ง๋ค ๋ณ๊ฒฝ**ํ๋ค
  - draw์์๋ frameCount๋ผ๋ ๋ณ์๋ฅผ ๋ฐ์ ๋ฐ์ง๋ฆ ์ ๊ณ์ฐ์ ์ฌ์ฉํ๋ค
  - frameCount๋ผ๋ ๋ณ์๋ useEffect์์ ์์ฑํด 0์ผ๋ก ์ด๊ธฐํ ํ 1์ฉ ๋ํด์ค ์์ ์ด๋ค
  - ๋ฐ์ง๋ฆ์ 0๋ถํฐ 1๊น์ง ์ปค์ก๋ค๊ฐ ๋ค์ 0์ผ๋ก ์์์ง๋ ๋์์ ๋ฐ๋ณตํ๋ค


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

- useEffect ๋ด๋ถ์ **render ํจ์**๋ฅผ ์์ฑํ๋ค
- **requestAnimationFrame์์ render**๋ฅผ ํ๋ ์๋ง๋ค ๋ถ๋ฌ์จ๋ค
  - ํ๋ ์๋ง๋ค frameCount++
  - ๋ณ๊ฒฝ๋ frameCount๋ฅผ ๋ฐํ์ผ๋ก ๊ฒ์ ๊ณต draw

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

- ์๊น์ง๋ง ๊ตฌํํด๋ ์ ๋๋ฉ์ด์์ด ์ ์๋ํ๋ค
- ํ์ง๋ง ๋ง์ฝ **requestAnimationFrame์ ํธ์ถ๋์ง๋ง render๋ ํธ์ถ๋๊ธฐ ์ ์ ์์ ์ ์ปดํฌ๋ํธ๊ฐ unmount๋๋ฉด** ๋ฌธ์ ๊ฐ ๋ฐ์ํ๋ค๊ณ  ํ๋ค
  - ๋ฐ๋ผ์ **์ปดํฌ๋ํธ๊ฐ unmount๋๋ฉด ๋ฐ๋ก ์ ๋๋ฉ์ด์์ ์บ์ฌ** ํด์ผํ๋ค
  - ๋คํํ requestAnimationFrame์ **request identifier๋ผ๋ ๊ฒ์ ๋ฆฌํด**ํ๊ธฐ ๋๋ฌธ์ ์ด ๊ฐ์ **cancelAnimationFrame์ ์ ๋ฌ**ํ๋ฉด ๋ฐ๋ก ์บ์ฌ์ ํ  ์ ์๋ค
 
```javascript
id = window.requestAnimationFrame(callback);
window.cancelAnimationFrame(id);
```

- ์ ๋๋ฉ์ด์์ **animationFrameId**์ด๋ผ๋ ์ด๋ฆ์ผ๋ก ์์ฑ ํ **๋ฆฌํด์์ ์บ์ฌ**ํด์ค๋ค
  - ์ด ๋ ๋ฆฌํด๋๋ **cleanup** ํจ์๋ **์บ๋ฒ์ค๊ฐ unmount๋๊ธฐ ์ง์ ์ ์ ๋๋ฉ์ด์์ ์บ์ฌ**ํ๋ ๋ท์ ๋ฆฌ๋ฅผ ํด์ค๋ค
  
  
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

## ์ปดํฌ๋ํธ ๋ถ๋ฆฌ

### App.js
- ๊ฒ์ ๊ณต์ ๊ทธ๋ฆฌ๋ Ball.js๋ผ๋ ์ปดํฌ๋ํธ๋ฅผ ๋ฐ๋ก ๋ง๋ค๊ธฐ๋ก ํ๋ค
- ๋ฐ๋ผ์ App.js ์์๋ Ball ์ปดํฌ๋ํธ๋ง ๋ฆฌํดํ๋ฉด ๋๋ค

```javascript
import React from 'react';
import Ball from './Ball.js';

function App() {
  return <Ball />
}
  
export default App;
```

### Ball.js

- Ball.js ์ปดํฌ๋ํธ์์๋ ๊ณต์ ๊ทธ๋ฆฌ๋ ํจ์ draw๋ฅผ ์ ์ํ๊ณ  ์บ๋ฒ์ค ์ปดํฌ๋ํธ๋ฅผ ๋ฆฌํดํ๋ค
- ์ฌ๊ธฐ์ drawํจ์๋ฅผ props๋ก Canvas์ ๋๊ฒจ์ค๋ค

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

- ์บ๋ฒ์ค ์ปดํฌ๋ํธ๋ draw ํจ์๋ฅผ ์ธ์๋ก ๋ฐ์์จ๋ค
- useCanvas๋ผ๋ ์ด๋ฆ์ useEffect Hook ๋ง๋ค์ด ๋นผ๋ด๊ณ  Canvas์๋ ์๋๋ง ๋จ๊ธด๋ค
- ์ด ํ์์ canvasRef๋ฅผ ๋ฆฌํดํด์ฃผ๊ณ  ์ด๋ฅผ ์บ๋ฒ์ค ์๋ ๋จผํธ์ ์ฐ๊ฒฐํ๋ค

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

- useCanvas๋ผ๋ useEffect Hook์ ๋ง๋ค์ด ์ฌ์ฌ์ฉ์ฑ์ ๋ํ๋ค
- canvasRef๋ฅผ ๋ฆฌํดํด Canvas ์ปดํฌ๋ํธ์ ๋๊ฒจ์ค๋ค

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

- ์บ๋ฒ์ค์ ํฌ๊ธฐ๋ ์๋์ฐ ํฌ๊ธฐ๊ฐ ๋ฐ๋ ๋๋ง๋ค ๋ณํ๋๋ก ์ค์ ํ๋๋ก ํ๋ค
  - ๊ฒ๋ค๊ฐ ๋ ํฐ๋ ๋์คํ๋ ์ด์ ๊ฒฝ์ฐ ํ์ง์ ํ๋ฅผ ๋ฐฉ์งํ๊ธฐ ์ํ ์ค์ ๋ ์ถ๊ฐํ๋ค
  - ์์ธํ ์ค๋ช์ [ํฝ์ ๋ฐ๋์ ์บ๋ฒ์ค ์ ๋๋ฉ์ด์](/canvas/pixel-density-canvas-animation) ์ฐธ๊ณ 
  
- useEffect Hook์ resize ํจ์๋ฅผ ์ ์ธํด์ค๋ค

```javascript
//useCanvas.js
useEffect(() => {
  ...
  
  const resize = () => {
  }
}, [draw]);
```

- ์ ๋๋ฉ์ด์์ด ๊ทธ๋ ค์ง ์คํ์ด์ง๋ฅผ ์๋์ฐ์ [innerWidth, innerHeight](https://developer.mozilla.org/en-US/docs/Web/API/Window/innerWidth)๋ก ์ค์ ํ๋ ค๊ณ  ํ๋ค
  - ์๋์ฐ์ฐฝ ์ ์ฒด๋ฅผ ์ฌ์ฉํด ๊ทธ๋ ค์ง๊ฒ ๋๋ค
- ํด๋น ๊ฐ๋ค์ stageWidth์ stageHeight์ ์ ์ฅํด์ค๋ค
  - ์ด ๊ฐ๋ค์ resize ๋ฐ์์๋ ์ฌ์ฉํ  ๊ฒ์ด๋ผ resize ๋ฐ์์ ์ ์ธํด์ค๋ค
  - ์ค์๊ฐ์ผ๋ก ๋ฐ๋๋ ์๋์ฐ ์ฐฝ ํฌ๊ธฐ๋ฅผ ์บ๋ฒ์ค์ ์ ์ฉํ๊ธฐ ์ํด resize์์ ๋งค๋ฒ innerWidth์ innerHeight๋ฅผ ๊ฐ์ ธ์จ๋ค

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

- ์ด๋ฒ์ [devicePixelRatio](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio)๋ฅผ ํตํด ์๋์ฐ์ ํฝ์ ๋ฐ๋๋ฅผ ๊ฐ์ ธ์ stageWidth์ stageHeight์ ๊ณฑํด ์บ๋ฒ์ค ํฌ๊ธฐ๋ฅผ ์ค์ ํ๋ค

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

- ์บ๋ฒ์ค ํฌ๊ธฐ๋ฅผ ํค์ ์ผ๋ ์ด๋ฒ์ ์บ๋ฒ์ค๊ฐ ์ค์ ๋ก ๋ณด์ฌ์ง ์ฌ์ด์ฆ๋ฅผ ์๋์ฐ ์ฐฝ๊ณผ ๊ฐ๊ฒ ๋ง์ถฐ์ค๋ค
  - ์บ๋ฒ์ค์ CSS width์ height๋ฅผ ์ค์ ํ๋ฉด ๋๋ค
  
```javascript
//useCanvas.js
const reszie = () => {
  ,,,
    
  canvas.style.width = stageWidth + 'px';
  canvas.style.height = stageHeight + 'px';
}
```

- ์บ๋ฒ์ค ์ฌ๋ฌ๊ฐ๋ฅด ๊ฒน์น  ์ ์๋๋ก position:absolute๋ก ์ค์ ํ๊ณ  body ๋ง์ง์ ์์  ์๋์ฐ ์ฐฝ์ ์บ๋ฒ์ค๊ฐ ๊ฝ ์ฐจ๋๋ก ํ๋ค

```javascript
canvas.style.position = 'absolute';
document.body.style.margin = '0';
```

- ์๋์ฐ์ฐฝ ํฌ๊ธฐ๊ฐ ๋ณํ  ๋๋ง๋ค ์ด resize ํจ์๋ฅผ ๋ถ๋ฌ์ค๋ ์ด๋ฒคํธ๋ฅผ ๋จ๋ค

```javascript
//useCanvas.js
const resize = () => {
  ,,,
    
  window.addEventListener('resize', resize);
}
```

- useEffect์์ resize๋ฅผ ํ๋ฒ ์คํํด์ฃผ๊ณ  ๋ง์ง๋ง์ผ๋ก return์ cleanup ํจ์์์ ์ด๋ฒคํธ๋ฅผ ํด์ ํด์ค๋ค

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

## [์์ฑ ์ฝ๋](https://github.com/joey-ful/ReactCanvasAnimationDefault.git)