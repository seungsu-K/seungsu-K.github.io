---
layout: post
title: 8/6(화) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/6(화) React 수업

<label> htmlFor 속성 : 자바스크립트에는 for가 반복문인 뜻하는 예약어로 등록되어 있기 때문에

레이블 요소를 마크업할 때 id를 직접 만들지말고 `useId()` hook을 사용할 것

리액트의 렌더링 동작에는 3가지 단계가 있다.

1. 렌더링 **트리거** (손님의 주문을 주방으로 전달)

   컴포넌트 렌더링이 일어나는 두 가지 요인

   - 초기 렌더링
   - 컴포넌트 **state가 업데이트된 경우**
     set 함수를 통해 상태를 업데이트하여 추가적인 렌더링을 트리거 할 수 있음

2. 컴포넌트 **렌더링** (주방에서 주문 준비하기)

   초기 렌더링에서 Root 컴포넌트를 호출, 이후 렌더링에서는 state 업데이트가 일어나는 컴포넌트를 호출

3. DOM에 **커밋** (테이블에 주문한 요리 내놓기)

   virtual DOM을 real DOM에 표시, 리렌더링의 경우 변경이 일어난 부분만 작업

---

## State

현재 입력값, 이미지, 장바구니와 같이 컴포넌트가 기억해야할 메모리를 state(상태)라고 한다.

일반 변수를 이용하여 컴포넌트를 렌더링하면 초기 렌더링에서는 반영이 되지만 이벤트를 발생시켜 변수를 재할당하는 경우에 리액트가 리렌더링을 일으키지 않는다.

### useState Hook

1. 렌더링 간 데이터를 유지하는 state 변수
2. 변수를 업데이트하고 React가 다시 렌더링하도록 유발하는 state setter 함수

```JavaScript
const [state, setState] = useState('초기값');

function handleClick() {
  setState(state + 1);
}
```

handleClick 함수가 일어날 때마다 setState 함수가 호출되어 state+1이 저장되고 리-렌더링이 유발됨

- 컴포넌트는 여러 개의 state 변수를 가질 수 있음
- 여러 개의 컴포넌트를 렌더링하면 각 컴포넌트는 개별 state를 가짐

---

## 상태 끌어올리기

Lifting state up : 여러 컴포넌트가 동일한 state로 관리가 필요할 때 state를 상위의 공통 부모 컴포넌트로 끌어올려 다른 컴포넌트와 상태를 공유하는 것

1. 자식 컴포넌트의 state를 제거
2. 하드 코딩된 값을 공통 부모에 전달
3. 공통 부모에 state를 추가하고 이벤트 핸들러와 함께 전달
