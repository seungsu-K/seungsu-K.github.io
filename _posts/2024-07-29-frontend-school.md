---
layout: post
title: 7/29(월) React 수업
categories: Frontend School
tags: [class]
---

# 7/29(월) React 수업

> clone으로 복사하면 .git 파일까지 복제
>
> degit은 .git 파일을 복제하지 않음

---

## Vite로 개발 환경 구성

Vite : ESbuild 기반으로 만들어진 모던 프론트엔드 개발 도구

- ES Module을 넘어 다양한 기능을 제공
- Webpack에 비해 속도가 빠름

[Vite를 사용해야 하는 이유](https://ko.vitejs.dev/guide/why) <br />
[다른 빌드 도구와의 차이점](https://ko.vitejs.dev/guide/comparisons.html)

### Vite auto scaffolding

`pnpm create vite@latest --template react`
https://ko.vitejs.dev/guide/#scaffolding-your-first-vite-project

자동으로 개발 환경 구성 요소들 다운로드 받음 (여러 템플릿이 제공됨)

→ 편하지만 나중에 세부적인 개발 환경을 커스텀하기 위해서 직접 구성해볼 필요성이 있음

### custom template

1.  README, gitignore 설정

    ```MARKDOWN
    git이 관리하지 않을 폴더

    .DS_Store (Mac일 경우)

    node_modules
    build
    dist
    ```

2.  manual installation

    - 개발자 모드로 Vite 설치
      `pnpm add vite -D # --save-dev`

          window의 경우 package.json이 있는 폴더를 찾아가기 때문에 해당 프로젝트에 package.json을 미리 생성해야 함

      <br />

    - React, React-dom 설치
      `pnpm add react react-dom`
      <br /><br />
    - @types 설치
      `pnpm add @types/react @types/react-dom @types/node -D`

          TS 적용을 위한 타입 선언 패키지

          수업에서는 JS를 사용하지만 개발자 경험 향상을 위해서 사용하는 것이 좋음 (자동완성, 오류 안내(console창에 출력해야만 알 수 있는 오류를 바로 인지시켜줌))

      <br />

    - Vite 플러그인 설치 (vite.config.js 파일 작성)

          `pnpm add @vitejs/plugin-react -D`

          `React.createElement()`를 대신할 `jsx()` API를 사용해 React를 import 하지 않고 플러그인으로 jsx를 사용

          https://legacy.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html

      <br />

    - ESLint 설치
      `pnpm create @eslint/config@latest`

          jsx 런타임 환경이라는 것을 알려주어서 오류가 나오지 않게 eslint.config.js 파일 설정

          ESLint 명령어 인터페이스

          `pnpm eslint ./src --report-unused-disable-directives --ignore-pattern .gitignore`

      <br />

    - ESLint 플러그인 설치
      `pnpm add eslint-plugin-react-hooks eslint-plugin-react-refresh -D`
      <br /><br />
    - 절대 경로 설정
      vite.config.js 에서 alias 설정

      vs code도 절대 경로를 인식하도록 jsconfig.json 파일 설정
