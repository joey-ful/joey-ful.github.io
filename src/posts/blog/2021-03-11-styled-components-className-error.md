---
title: 'Styled-Components error: className did not match'
category: "FrontEnd"
date: "2021-03-11 12:00:00 +09:00"
desc: "babel"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---


### 문제
styled-components 사용시 다음고 같은 에러가 떴다
![image](https://user-images.githubusercontent.com/52592748/110747900-58aca180-8282-11eb-870f-828ef222944e.png)

### 해결법
`babel-plugin-styled-components`를 설치하고
```shell
npm i babel-plugin-styled-components
```

`.babelrc` 파일을 생성해 아래처럼 작성했더니 문제가 사라졌다

```json
{
  "presets": ["next/babel"],
  "plugins": [["styled-components", {"ssr": true}]]
}
```

#### 참고 링크
[github 이슈의 해결법](https://github.com/vercel/next.js/issues/7322#issuecomment-762896059)<br>
[npm babel-plugin-styled-components 문서](https://www.npmjs.com/package/babel-plugin-styled-components)