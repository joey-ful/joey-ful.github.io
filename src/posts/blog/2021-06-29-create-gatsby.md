---
icon: "⚒"
title: 'Gatsby로 기술블로그 만들기'
category: "Blog"
date: "2021-06-29 12:00:00 +09:00"
desc: "Github Pages vs Netlify vs Gatsby"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

## 시작하기
- 개츠비 사이트는 [Gatsby 스타터 템플릿](https://www.gatsbyjs.com/starters/?)를 이용해 쉽게 템플릿이 적용된 사이트를 만들 수 있다. 하지만 난 왠지 온갖 에러에 고통만 받고 그냥 직접 만들려고했는데 [우연히 발견한 스타터 템플릿](https://github.com/sungik-choi/gatsby-starter-apple)을 자포자기하는 심정으로 시도해봤는데 됐다!!

### Default 스타터로 사이트 만들기
- Gatsby 커맨드라인 툴을 설치하고 `gatsby new` 로 개츠비 사이트를 만든다

```shell
npm install -g gatsby-cli
gatsby new [폴더 이름]
cd [폴더 이름]
gatsby develop
```
- `gatsby develop` 을 치면 
- `localhost:8000` 에 들어가면 다음과 같은 스타터 페이지가 뜬다
![](https://images.velog.io/images/jehjong/post/47d09912-75bc-43eb-beba-39b38afa8ea6/image.png)

### 커스텀 스타터 템플릿으로 사이트 만들기
```shell
gatsby new [폴더 이름] https://github.com/sungik-choi/gatsby-starter-apple
cd [폴더 이름]
gatsby develop
```
- 드디어 되는 템플릿을 만나다니.. 눈물 😭 안되는 템플릿이 왜이리 많은걸까..
![](https://images.velog.io/images/jehjong/post/7a40d9c1-1ee7-4e1d-a977-521f0b1f0d76/image.png)

### Github Pages에 배포하기
- gh-pages 패키지 설치 필수
```shell
npm i gh-pages --save-dev
```

- `package.json` 의 scripts에 다음 내용 추가
```shell
{
    "scripts": {
        "deploy": "gatsby build && gh-pages -d public -b master"
    }
}
```

- `[username].github.io` 라는 이름의 git 레포를 생성하고 현재 디렉토리의 remote로 연결한다
```shell
git remote add origin [git 레포 주소]
```
- develop 브랜치를 생성해서 deploy 한다
```shell
git checkout -b develop
npm run deploy
```
- 잠시 기다리면 배포가 된다
![](https://images.velog.io/images/jehjong/post/c985be2b-567c-4fd6-9cc8-030ec2afdec1/image.png)
- 본인의 `[username].github.io` 주소로 이동하면 배포가 된 것을 확인할 수 있다 
![](https://images.velog.io/images/jehjong/post/3f966d9a-7f19-4f0f-8574-b0c31deba2b5/image.png)

- 특이하게도 깃헙 레포에는 아무것도 올라가있지 않은데 배포가 됐다. `[username].github.io` 주소만 빌려오고 레포는 사용하지 않아도 되는 것 같다.

### 포스트 작성
- 위 템플릿의 경우 `/src/posts/blog/` 에 포스트를 추가하면 된다. 마크다운 문법으로 작성하면 되니까 무척 편리하다
- 포스스트 내의 카테고리란에 새 카테고리 이름을 적으면 바로 새로운 카테고리가 생긴다. 반대로 특정 카테고리 이름을 적어둔 포스트들을 다 지우면 해당 카테고리는 사라진다

## 배포를 어디서 할지에 대한 고민
#### Github Pages
- Github Pages에 배포하는 것의 단점은, 버전관리는 github 레포에서 해야하는데 배포는 로컬에서 바로 된다는 점이다. Gatsby로 블로그를 직접 만들어보기 전에 '원본소스의 버전관리를 깃헙으로 하기 불편해서 netlify로 배포했다'는 글을 어디서 보고 이해가 안 갔었는데 한번 해보니 이해가 갔다. 내가 생각해본 단점을 정리해보면 다음과 같다:
  - 레포에 `git push`한다고 웹사이트가 업데이트되는 것이 아니라 `npm run deploy`로 배포를 해야한다. 즉, 레포에 올려둔 버전이 내 웹사이트 내용과 다를 수 있다는 묘한 찝찝함이 있다.
  - 로컬에서 웹사이트 소스 코드를 변경한 뒤 매번 `git push`와 `npm run deploy`를 둘 다 해줘야한다는 번거로움이 있다.
#### Netlify
- 하지만 또 장점도 있다. netlify로 배포할 경우 사이트 도메인이 `netlify.app` 으로 끝나는데 이것보다는 `github.io`라는 이름이 더 마음에 든다. 별 이유는 아닌 것 같지만 왠지 내게는 중요하게 느껴졌다 🤔
  - 그래도 무료 배포로는 Netlify가 가장 유명한 것 같아서 Github Pages가 아닌 것을 택한다면 Netlify를 택하게 될 것 같다.
#### Gatsby
- 사실 Gatsby 에서도 사이트를 만들어 클라우드에 배포가 된다. 무료 계정은 사이트를 하나 만들 수 있고 github 레포랑도 연결할 수 있다. 
![](https://images.velog.io/images/jehjong/post/3f947781-ae67-4bf0-802b-56faa0a5ce96/image.png)
  - 하지만 이 경우 사이트 이름이 길고 괴상해져서 사용하고 싶지는 않았다.
#### 도메인 구매
- 결국 내게 어디서 배포를 할지는 사이트 도메인이 무엇이 되는지가 가장 중요한 것 같다. `github.io`는 버전관리의 불편함이 있음에도 도메인이 왠지 마음에 들어 선택하게 됐다. 기존 블로그 도메인과 같았다는 이유도 있었다.
- 하지만 도메인을 구매하게 되면 `joeyful.site`나 `joeyful.me` 같은 더 마음에 드는 이름을 가질 수 있고 어디서 배포하든 상관이 없어진다. 나중에 도메인을 구매한 뒤에 Gatsby나 Netlify에서 배포하면 어떨까 싶다