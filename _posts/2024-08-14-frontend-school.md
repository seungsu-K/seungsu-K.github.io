---
layout: post
title: 8/14(수) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/14(수) React 수업

`FormData` 생성자 : form 요소의 key/value로 이루어진 객체를 전달받으며 입력된 각 요소들의 name 속성을 value로 사용

## useImperativeHandle

`useImperativeHandle` 훅을 사용하면 하위 컴포넌트에서 반환된 객체(메서드나 상태)를 상위 컴포넌트에서 제어할 수 있음

`forwardRef` 함수를 이용하지 않고 `ref`를 하위 컴포넌트에 전달하는 것은 불가능하다. 하지만, 우회적으로 \_ref, $$ref 변수를 사용해 전달할 수 있지만 이 방법은 상위 컴포넌트에서 하위 컴포넌트를 제어할 수 없음

```jsx
function _ChildComponent({ props }, ref) {
  useImperativeHandle(ref, () => ({
    clear: () => {
      inputRef.current.value = "";
    },
  }));

  return <input ref={ref} />;
}

const ChildComponent = forwardRef(_ChildComponent);
export default ChildComponent;

function ParentComponent() {
  const inputRef = useRef();

  // 부모 컴포넌트에서 자식 컴포넌트 내부의 메서드를 실행할 수 있음
  return (
    <div>
      <ChildComponent ref={inputRef} />
      <button onClick={() => inputRef.current.clear()}>Clear Input</button>
    </div>
  );
}
```

- 부모 컴포넌트가 자식 컴포넌트의 내부 메서드나 속성에 직접 접근하거나 제어할 수 있음
- 부모에서 자식으로의 단방향 데이터 흐름을 벗어남

## useEffect

서버에 접속해 통신을 기다리고 응답이 있어야 처리할 수 있기 때문에 리액트 관점에서 서버와 통신하는 것은 순수한 계산이 아니다 (부수 효과)

부수 효과를 작성할 수 있는 공간 중 `eventHandler`는 사용자의 액션이 필요하지만 `useEffect`는 액션 없이 실행 가능

`refCallback`은 DOM에 마운트되고 한 번만 실행되고 `useEffect`는 마운트 후 로직에 따라 실행
