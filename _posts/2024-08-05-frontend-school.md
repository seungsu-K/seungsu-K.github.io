---
layout: post
title: 8/5(월) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/5(월) React 수업

setState를 통해 state를 변경할 수 있고, state변경이 감지되면 render 함수가 실행

리액트의 컴포넌트는 내부에 데이터(상태)를 기억할 수 있다.

리액트에서 다루는 데이터는 속성과 상태이다

- 이벤트핸들러를 통해 명령적으로 UI변경 : 생성되어있는 DOM 요소를 변경
- useState를 사용해서 선언적으로 UI변경 : React에 선언되어 있는 상태를 변경함에 따라 jsx가 변경되었는지 확인하고 다시 랜더링

---

## 명령형 vs 선언형 프로그래밍

### 명령형 프로그래밍

작업을 수행하는 방법 (HOW)을 알려주는 절차형 모델

UI를 조작하기 위해 발생한 상황에 따라 정확한 지침을 내려 작성.

### 선언형 프로그래밍

무엇을 수행하는지(랜더링하고 싶은지) 선언 (WHAT)하여 계산의 논리를 표현

길찾기를 예로 들자면,

명령형 프로그래밍은 목적지에 가기 위해 방향을 직접 지시하는 것

선언형 프로그래밍은 목적지를 말하는 것 (최적의 경로를 안내해줌)

## 선언적 방식으로 React 이용하기

1. 컴포넌트의 다양한 state를 확인하고, 무엇이 state를 변화시키는 트리거인지 알아낸다. 
2. useState를 사용해 메모리의 state를 표현하고 이벤트 핸들러를 연결

Class component에는 `state`와 `setState` 메서드가 있기 때문에 Class 기반으로 프로그래밍이었으나 2018년 React Hooks (functions) API를 도입하고나서 점차 functional 기반 프로그래밍으로 변화

Class component는 this.state로 상태를 설정하고 setState로 선언된 상태를 감지해 reactDOM을 리랜더링

function component에서는 일반 변수를 사용하면 state가 변한 것을 인식할 수 없기 때문에 useState (Hook API)를 사용해 컴포넌트 상태를 선언
``` JavaScript
const [checked, setChecked] = React.useState(false);

const handleCheck = (e) => {
  const nextCheckedValue = e.target.checked;

  setChecked(nextCheckedValue);
}
```

---

## 정리

리액트는 "선언적"이다.

리액트의 컴포넌트는 내부에 데이터를 기억할 수 있다. (이것을 "상태 state"라 부른다)
리액트에서 다루는 데이터는 "속성(props)"과 "상태(states)"이고, 이 중에서 읽기/쓰기가 가능한 데이터는 "상태", 읽기만 가능한 데이터는 "속성"이다.

리액트에 의해 "선언된 상태"는 업데이트가 가능하다.
리액트는 업데이트 된 상태를 감지해 JSX를 반환하는 함수(또는 render 메서드)를 다시 실행한다.
JSX를 반환하는 함수(또는 render 메서드)를 다시 실행되면 이를 ReactDOM이 실제 DOM에 반영(commit)한다.
반영된 DOM을 브라우저는 다시 페인팅(Painting) 한다.