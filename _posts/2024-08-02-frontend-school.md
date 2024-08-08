---
layout: post
title: 8/2(금) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/2(금) React 수업

함수형 프로그래밍에서는 함수의 실행 **결과를 예측 가능하게 만드는 것**이 중요함

버그의 근본적인 원인은 개발자의 기대와 다르게 동작하는 상황이고, 
결과가 예측 가능하다면 이를 방지(또는 디버깅)할 수 있음

일반적으로 함수의 결과를 예측할 수 없게 만드는 요인

- 부수 효과 (Side Effects)
    - 외부의 상태에 의존하거나 변경 (외부의 변수 변경, 네트워크 요청 등)
    
- 일관되지 않은 결과 값 (Inconsistent outputs)
    - 함수에 return 값이 없으면 결과를 예측하기 어려움

## 순수 함수

- 함수가 호출되기 전에 존재했던 객체나 변수를 변경하지 않음 ****(부수효과가 없음)
- **같은 입력이 주어졌다면 같은 결과를 반환**

이점 : 구성(재사용) 가능, 캐시 가능, 테스트 가능, 읽기 쉬움

>그렇다면 부수 효과는 나쁜 것일까?
>
>부수 효과는 이벤트 핸들러, 웹 API 등 사용자와의 상호 작용을 포함한다. 그렇기에 예측이 어려운 것이지 사용하지 말아야 할 나쁜 것은 아니다.
>
>다만, 리액트 컴포넌트는 순수해야 하며, 컴포넌트 내부에서 부수 효과가 발생해서는 안된다.

## 순수성을 체크하는 방법

`<React.StrictMode>`를 실행하게 되면, 랜더링이 두 번 실행되고 동일한 결과값이 나온다면 부수효과가 발생하지 않은 것

개발 중일 때는 2번 실행해서 일반적인 문제를 실행하고 실제 배포되면 1번만 실행한다.

## 리액트 컴포넌트의 순수성

리액트에서 부수 효과 : 리액트의 랜더링 효과와 관련 없는 코드

리액트는 부모 요소에서 자식 요소로 데이터(props)를 전달할 수 있다. 이 때, props는 훼손하면 안되는 읽기 전용인 외부 데이터이기 때문에 하위 컴포넌트 내부에서 수정하면 안된다.

→ 원본 데이터인 props를 복사 후 수정(mutation)하는 것이 좋다.

또한, props가 아닌 외부 데이터에 의존하지 않도록 한다.

---

## 이벤트 핸들러

reactDOM(react.createRoot)이 render 메서드를 실행하면 real DOM에 랜더링됨

따라서 이벤트가 실행되는 시점은 real DOM에 랜더링이 된 이후 사용자의 동작에 의해 발생하는 순간이기에 부수 효과가 작성되어도 된다.

**컴포넌트 함수 내부의 이벤트 핸들러에서 부수 효과를 다룰 것**

---

## 이벤트 전파

- 캡처링 : 이벤트가 상위 요소에서 하위 요소로 전파
- 버블링 : 이벤트가 하위 요소에서 상위 요소로 전파

리액트는 캡쳐링을 지원 onClickCapture

`event.stopPropagation` : 이벤트 전파를 방지

`preventDefault` : 요소(a, button 등)의 기본 작동을 방지 

`element.scrollIntoView` : 스크롤 애니메이션