---
icon: "💾"
title: 'devDependencies'
category: "FrontEnd"
date: "2021-03-09 12:00:00 +09:00"
desc: "npm, node"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

## devDependencies

- 패키지들이 package.json의 devDependencies에 저장된다
- ESLint, JEST, babel, 테스트 등 프로덕션 단계에서 꼭 필요하지 않은 패키지들(실행에 필요하지 않는 것)은 devDependencies에 저장해 다른 사람들이 알아볼 수 있도록 한다
  - `npm i --production`으로 설치하면 package.json의 devDependencies는 제외하고 설치가 가능하다
```shell
npm i --save -dev
npm i -D
```

- [npm5이후로 `npm i`를 하면 default로 dependencies에 저장된다](https://techstacker.com/npm-install-vs-npm--save-difference/)
```shell
npm i
npm i --save
npm i -S
```

### 참고
[--save vs --save -dev](https://stackoverflow.com/questions/22891211/what-is-the-difference-between-save-and-save-dev/31358981#31358981)<br>
[--save -dev = -D](https://medium.com/@sumantmishra/npm-devdependencies-vs-dependencies-in-package-json-d6c790edd725)<br>
[npm Docs](https://docs.npmjs.com/cli/v7/configuring-npm/package-json#devdependencies)<br>
[npm i defaults](https://techstacker.com/npm-install-vs-npm--save-difference/)