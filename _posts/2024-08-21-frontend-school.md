---
layout: post
title: 8/21(수) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/21(수) React 수업

컴포넌트(함수)가 다시 렌더링(실행) 되는 이유

1. 컴포넌트 자신이 소유한 상태(states)가 변경될 때
2. 컴포넌트 외부에서 전달된 속성(props)이 변경될 때

   이 때, 전달된 속성이 primitive 값이라면 이전 값과 현재 값이 동일하지만, 지역 함수는 매번 새롭게 정의되기 때문에 계속해서 새로운 함수가 참조되는 것

   → 그래서 컴포넌트가 렌더링되면 하위 컴포넌트가 같이 렌더링 됨

   → 이를 해결하기 위해, `memo`, `useMemo`, `useCallback` 훅을 사용

   (React는 너무 사소한 리렌더링을 방지하는 것은 개발자의 경험을 헤치는 것이라 말하지만 어느 정도 성능을 최적화 시키기 위해서는 필요한 작업)

---

## memo

컴포넌트의 props가 변경되지 않은 경우, 불필요하게 리렌더링 되는 것을 방지

```jsx
function Toggler({ children, onToggle }) {
  return (
    <button type="button" className={S.button} onClick={onToggle}>
      {children}
    </button>
  );
}

export default memo(Toggler);
```

## useMemo

컴포넌트 내부에서 복잡한 계산이나 함수 실행 결과를 저장해 불필요한 계산을 피함 (레이아웃에 사용되는 값)

### vs memo

- `useMemo`

  - **계산된 값**을 \*\*\*\*메모이제이션하여, 리렌더링 시 불필요한 계산을 피함
  - **컴포넌트 내부**에서 사용
  - `memo`로 감싸진 컴포넌트에 prop로 전달하는 경우

- `memo`
  - **컴포넌트 자체**를 메모이제이션하여, props가 변경되면 리렌더링
  - **컴포넌트 외부**에서 컴포넌트를 export

## useCallback

컴포넌트가 리렌더링될 때, 내부에서 정의된 함수도 다시 생성(JS 로직). 이때, 함수가 새로운 참조를 가지기 때문에, 의도하지 않은 리렌더링이 발생

```jsx
function TimeAndCounter() {
  const { time, turnOn, onOff } = useClock();

  const handleToggleTime = () => onOff((c) => !c);

  const memoizedHandleToggleTime = useCallback(handleToggleTime, [onOff]);

  const label = `타임 ${turnOn ? "스톱" : "플레이"}`;

  return (
    <div className={S.component}>
      <div role="group" className={S.group}>
        <time>{time}</time>
        <TimeToggler onToggle={memoizedHandleToggleTime}>{label}</TimeToggler>
      </div>
      <Counter />
    </div>
  );
}
```

- `useCallback`을 사용해야 할 함수는 하위 컴포넌트로 전달하는 경우에만 필요하고, native element에 이벤트를 바인딩하는 경우는 `useCallback`을 사용하지 않아도 됨

---

## useContext

최상위 컴포넌트에서 context를 생성하여 트리 내에서 데이터를 전달할 수 있음. 컴포넌트 간 데이터를 전달하기 위해 props를 반복적으로 전달하는 props drilling을 해결할 수 있음

1. context 생성하기 / `createContext`

   ```jsx
   const pageContext = React.createContext();
   ```

1. context 내보내기 / `pageContext.Provider`

   ```jsx
   export function PageProvider({ value, children }) {
     return (
     <pageContext.Provider value={value}>
   	  {children}
     </pageContext.Provider>;
     )
   }
   ```

1. context 불러오기 / `useContext`

   ```jsx
   export function usePageContext() {
     return React.useContext(pageContext);
   }

   function PageComponent() {
     const { message, color } = usePageContext();

     return (
       <div className={S.box} style={{ backgroundColor: color }}>
         <strong className={S.label}>Grand Child</strong>
         <p>{message}</p>
       </div>
     );
   }
   ```

1. App에서 contextProvider 컴포넌트 사용

   ```jsx
   function App() {
     return (
       <PageProvider>
         <RouterProvider router={router} />
         <PageComponent>
       </PageProvider>
     );
   }
   ```
