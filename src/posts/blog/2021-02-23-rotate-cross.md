---
icon: "✙"
title: 'rotate 사용해서 십자가 모양 만들기'
category: "FrontEnd"
date: "2021-02-23 12:00:00 +09:00"
desc: "rotate, translate"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

>**이번 글에서 다룰 내용**
>- 십자가 모양 만들기
>   - rotate
>   - 회전축
>   - translate


# 십자가 모양 만들기
- rotate()를 사용해 십자가 모양을 만들려고 한다

## rotate()

- context를 특정 각도만큼 회전시키는 함수
  - 원하는 각도에 `Math.PI / 180` 를 곱하면 된다
- 그림을 그리기 전에 context를 회전 시킨 후 그리면 된다

```javascript
this.ctx.rotate(angle);
```

## 회전축
- 위의 roate()를 이용해 아래와 같은 십자가 모양을 만들려고 한다

![](https://images.velog.io/images/jehjong/post/d0e72934-350e-4a99-830c-0dc94ac79b24/image.png)

- 먼저 회색 막대를 그리고
- 90도 회전시킨 context에 하늘색 막대를 그려주었다

```javascript
  this.ctx.fillStyle = '#ced4da';
  this.ctx.fillRect(200, 150, 140, 30);

  //90도 회전
  this.ctx.rotate((90 * Math.PI) / 180);

  //하늘색 막대
  this.ctx.fillStyle = '#99e9f2';
  this.ctx.fillRect(200, 150, 140, 30);
```

![](https://images.velog.io/images/jehjong/post/23e05b1b-c5dc-4c99-a5d6-50782b77ffd3/image.png)

- 하늘색 막대가 사라졌다
- 회전축이 0, 0부터 시작하므로 90도를 회전하면 하늘색 막대가 캔버스 밖으로 나가버린다
- 45도 회전한 막대와 비교해보면 아래와 같지 않을까 생각한다
![](https://images.velog.io/images/jehjong/post/05886abb-0a95-43ab-920e-18aa0158146a/image.png)


## translate
- 십자가를 만들기 위해서는 하늘색 막대가 회색막대의 중심을 기준으로 회전해야한다
  - 회색 막대의 좌표 200, 150은 왼쪽 위 모서리의 좌표기 때문에 중심을 따로 구해줘야 한다 => (270, 165)
![](https://images.velog.io/images/jehjong/post/7b38954e-4906-477e-9bf9-602ccf2dbc1d/image.png)

- 회전한 막대를 그리기 위해서는 아래 세가지 작업이 필요하다
  1. 중심축을 0,0에서 270, 165로 바꾸기 위해 context를 270, 165만큼 이동시킨다
  2. 90도 회전한다 (270, 165를 기준으로 회전)
  3. 회전이 끝났으니 context를 제자리로 돌려놓기 위해 -270, -165만큼 이동한다
  
```javascript
// 회색 막대

this.ctx.translate(270, 165);
this.ctx.rotate((90 * Math.PI) / 180);
this.ctx.translate(-270, -165);

// 하늘색 막대
```

- 십자가 모양 완성
![](https://images.velog.io/images/jehjong/post/0ba449f6-645c-4352-89fe-1d9f9483aa1b/image.png)


# 