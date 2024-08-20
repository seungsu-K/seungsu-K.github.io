---
layout: post
title: 8/13(화) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/13(화) React 수업

## forwardRef

`key`, `ref` 속성(props)은 상위 컴포넌트에서 하위 컴포넌트로 전달할 수 없도록 설계되어 있기 때문에, 일반적으로 컴포넌트 내부에서 DOM 노드가 아닌 하위 컴포넌트로 `ref`를 전달하는 것은 에러

→ `forwardRef`는 함수형 컴포넌트가 부모 컴포넌트로부터 `ref`를 전달 받을 수 있도록 도와주는 고차함수

```jsx
// 부모 컴포넌트
function ParentCompoenet() {
  const svgRef = useRef(null);

  const handleAnimation = () => {
    const { current: element } = svgRef;

    animate(element, {
      /* 애니메이션 동작 */
    });
  };

  return (
    <div className={S.component}>
      <button className={S.button} type="button" onClick={handleAnimation}>
        키프레임 애니메이션
      </button>

      <Child ref={svgRef} size={60} />
    </div>
  );
}

// 자식 컴포넌트
function ChildeCompoenet() {
  return (
    <svg
      ref={ref}
      className={S.component}
      viewBox="-105 -105 210 210"
      width={size}
      height={size}
      {...restProps}
    >
      ...
    </svg>
  );
}

const Child = forwardRef(ChildComponent);

export default Child;
```

- forwardRef 함수로 컴포넌트를 감싸면 prop-types가 동작하지 않음

  (TypeScript 사용을 권장)

  ```jsx
  /* eslint-disable react/prop-types */
  ```

- 컴포넌트를 설계할 때 refForwarding을 고려해 설계하지 않으면 협업하는데 걸림돌이 될 수 있음

---

렌더링 이후 DOM 노드에 접근하고 조작하는 것은 리액트가 관여하는 일이 아니기 때문에 여러 라이브러리의 도움으로 애니메이션 효과를 적용시킬 수 있다.

## vanilla-tilt

바닐라 스크립트로 작성된 애니메이션 라이브러리

```jsx
// DOM 렌더링 후 vanilla tilt의 config 속성 추가 후
// 바닐라 스크립트 방식으로 애니메이션 제어
const refCallback = (el /* domElementNode */) => {
  if (el) {
    VanillaTilt.init(el, TILT_CONFIG);
  }
};

const Ref = useRef(null);

// 이벤트 핸들러
const handleEnter = () => {
  const title = titleRef.current;
  title.style.opacity = "1";
  // 접근성 관점에서 매우 중요
  // title.closest('a').focus();
};

const handleLeave = () => {
  titleRef.current.style.opacity = "0";
};
```

## motion one

네이티브 브라우저 API를 기반으로 하는 최신 웹 애니메이션 라이브러리

### 특징

- 간단한 API

  css 선택자 또는 DOM 노드에 style 속성을 간단하게 적용할 수 있음

  ```jsx
  animate(".component", { backgroundColor: "blue" });
  ```

- 가벼움
  Wep Animation API([WAAPI](https://developer.mozilla.org/ko/docs/Web/API/Web_Animations_API))를 사용하기 때문에 용량이 작음
  - **Motion One** 6.3kb (Tree Shaking 지원, 향후 1.8kb까지 줄일 계획)
  - **Framer Motion** 17kb
  - **GSAP** 23.5kb (Tree Shaking 미지원)
- 성능
  하드웨어 가속 애니메이션을 사용해 작업 부하가 많을 때에도 UI가 빠르게 반응
- TypeScript
  TypeScript로 작성되어 JavaScript, TypeScript 환경에서 잘 작동

### animate

HTML 요소에 애니메이션을 적용하는 함수

```jsx
// CSS 선택자
animate(".component", { backgroundColor: "blue" });

// DOM 요소 또는 요소 집합
const component = document.querySelector(".component");

animate(compoenet, { backgroundColor: "blue" });
```

- 마지막 인수로 `duration`, `delay`, `easing` 등 옵션 객체를 추가로 전달할 수 있음

  ```jsx
  animate(
    '.compoenet',
    // keyframes
    {  x: [0, -100, 100, 0] },
    **{**
      duration: 0.5,
      easing: 'ease-in-out',
      ****offset: [0, 0.25, 0.75],
    **},**
  );
  ```

- `keyframes`는 배열을 사용해 지속시간에 걸쳐 균등하게 간격이 지정되며, 별도로 지정할 경우 `offset`을 옵션으로 전달하면 됨

### timeline

여러 애니메이션을 순차적으로 또는 겹쳐서 실행할 수 있음. `at` 옵션으로 각 애니메이션 시작 시점을 조정할 수 있음

```jsx
const animationControls = [
  [
    circle1Element,
    { strokeDashoffset: [1, 0], visibility: "visible" },
    { duration: 1 },
  ],
  [
    lineElement,
    { strokeDashoffset: [1, 0], visibility: "visible" },
    { at: 0.5, duration: 1 },
  ],
  [
    circle2Element,
    { strokeDashoffset: [1, 0], visibility: "visible" },
    { at: 0.5, duration: 1 },
  ],
];

timeline(animationControls);
```

### inView

특정 요소가 뷰포트에 들어왔을 때 애니메이션을 작동시킴

ex) 페이지를 스크롤할 때 특정 요소가 보이면 애니메이션 작동

```jsx
inView(element, ({ target }) => {
  animate(target, { opacity: [0, 1] }, { duration: 0.6 });
});
```

### scroll

스크롤 위치에 따라 애니메이션을 조정

ex) 스크롤 높이에 따라 진행 정도 표현

```jsx
scroll(
  animate(".component", {
    opacity: [0, 1],
    transform: `translateY(${y.current}px)`
  });
);

// 콜백 함수로 전달 가능
scroll(({ y }) => {
  video.currentTime = video.duration * y.progress
});
```

### stagger

여러 요소 집합에 애니메이션을 적용할 때 `delay`옵션에 stagger 함수를 사용하면 각 요소에 순차적으로 애니메이션을 적용할 수 있음

```jsx
animate(
  mapArray,
  { x: [0, 400, 0], rotate: [0, 360, -360] },
  {
    duration: 2,
    delay: stagger(0.3),
  }
);
```

- 여러 요소에 동일한 애니메이션을 적용하려 할 때, 요소 집합을 관리하는 방법

  1. data-id로 관리

     컴포넌트를 생성할 때 공통된 속성을 미리 생성

     ```jsx
     const soccerBalls = Array.from(
       document.querySelectorAll('[data-testid="component"]')
     );
     ```

     → 유지 보수 시 공통 속성을 추가하려면 컴포넌트 구조를 바꿔야하거나 미리 설계를 하고 만들어야 하기 때문에 번거로움

  2. 배열로 관리

     ```jsx
     const soccorBalls = Array.from(new Set(componentRef.current));
     ```

     → 비교적 간단하지만 중복을 고려해 Set으로 데이터를 관리

  3. Map

     ```jsx
     const map = getMap();

     if (map.size > 0) {
       animate(
         map.values(),
         { x: [0, 400, 0], rotate: [0, 360, -360] },
         {
           delay: stagger(0.3),
           duration: 2,
         }
       );
     }
     ```

     → Map은 중복을 허용하지 않고 iterable이기 때문에 순회하면서 value를 받아서 애니메이션을 적용하면 됨
