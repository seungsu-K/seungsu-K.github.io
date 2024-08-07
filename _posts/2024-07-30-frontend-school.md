---
layout: post
title: 7/30(화) React 수업
categories: [Frontend School]
tags: [class]
---

# 7/30(화) React 수업

## props

컴포넌트에 전달되는 모든 데이터(HTML 속성과 비교할 수 있음), 전달 후 변수를 통해 접근할 수 있다.

부모 요소에 전달된 props는 하위 요소에서 접근이 가능하다

default가 아닌 import는 \* 로 module 자체를 불러올 수 있음

---

## JSX로 마크업

1.  최상위 요소만 반환

    형제 요소들이 있다면 React.Fragmant (`<>`)로 묶어서 반환 해주어야 한다.

    <br />

2.  셀프클로징

    빈 태그들을 열린 태그로만 작성하면 에러가 발생하기 때문에 셀프클로징을 해주어야 함

    <br />

3.  class 속성 추가는 className을 사용하며 속성이름은 camelCase를 사용

    (JSX에서 class라는 예약어를 사용)

    className이 없을 경우 null이나 undefined를 사용하지 말고 ‘’ 공백을 사용할 것

    → 문자형으로 형변환이 일어나면서 null이나 undefined class를 가짐

    <br />

4.  데이터 표시

    중괄호 표기법`{...}` 으로 변수를 사용할 수 있음

    <br />

5.  조건부 렌더링

    JavaScript 로직을 따르며 문과 식을 사용해 렌더링 하는 것

    전환 비용이 높기 때문에 전환이 자주 일어난다면 조건부 표시를 사용하는 것이 좋음

    if, switch, 삼항 연산자, 논리 연산자, 함수의 return

    JSX 내부에서는 식만 가능

    <br />

6.  조건부 표시

    전환이 자주 일어나는 렌더링을 CSS 기반 스타일로 표시

    ```jsx
    <picture
      // HTML 요소 hidden 속성
      hidden={!isShowImage}
      // class 속성
      className={isShowImage ? "" : "hidden"}
      // style 속성
      style={pictureStyles}
      // style={{
      //   display: isShowImage ? 'inline-block' : 'none',
      //   fontSize: 42,
      //   lineHeight: 2.4,
      //   'letter-spacing': '2px',
      // }}
    ></picture>
    ```

    <br />

7.  주석

    ```jsx
    {
      /*` Comments`*/
    }
    {
      /*` `*/
    }
    // 공백
    ```

---

## 컴포넌트 속성 타입 검사

prop type validation

`Component.propTypes`

컴포넌트는 외부로부터 전달 받은 데이터인 props에 따라 요소를 생성해 반환한다. 따라서 정확히 요구되는 마크업 하기 위해 속성 타입을 지정해야 한다.

```jsx
const generateTypeValidation =
  (allowedType) => (props, propName, componentName) => {
    // 컴포넌트 속성의 값
    const propValue = props[propName];

    // 컴포넌트 속성 값의 타입
    const propType = typeOf(propValue);

    // 허용할 데이터 타입 이름
    // const allowedType = type;

    // 컴포넌트 속성 검사 타입 수행
    if (propType !== allowedType) {
      // 오류가 있을 경우
      throw new Error(
        `${componentName} 컴포넌트 ${propName} 속성 타입은 "${allowedType}" 타입이 요구되나, 실제 전달된 타입은 "${propType}"입니다.`
      );
    }

    // 아무런 오류가 없으니 패스~
  };
```

배열 데이터를 `li`로 렌더링

데이터를 순환하면서 `li`에 컨텐츠를 넣어서 return
