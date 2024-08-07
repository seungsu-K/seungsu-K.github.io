---
layout: post
title: 8/1(목) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/1(목) React 수업

데이터에는 record마다 id나 index가 있는 것이 좋은데,

클라이언트에서 생성한 데이터나 서버에서 받아온 데이터에 id가 없다면 재가공해서 사용하는 것이 좋다.

---

## 객체 데이터 렌더링

`for…in` 문으로 객체를 순환하여 배열로 정의해 데이터를 사용 (문이기 때문에 함수 몸체에서 사용 가능)

`Object.entries` 메서드를 이용해 key와 value로 이루어진 배열 데이터로 변환하여 사용 (식이기 때문에 JSX 내부에서 사용 가능)

❗객체 데이터든 배열 데이터든 원본을 훼손시키지 않는 것이 중요하기 때문에 새로운 객체를 반환하는 메서드를 사용할 것

---

## component props 타입 검사

프로젝트가 커지고 앱이 커지면 개발자가 많아지기 때문에 일관된 DX를 위해 개발자를 위한 타입 오류 안내와 같은 기능들을 추가하는 것이 좋다.

### prop-types 라이브러리 이용하기

React 19 이후 지원이 중단될 예정

<https://www.npmjs.com/package/prop-types>

```JavaScript
import PropTypes from 'prop-types';

Component.propTypes = {
  // props를 특정 형식만 가능하도록 선언
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 열거형(enum)으로 처리하여 prop가 특정 값들로 제한되도록 할 수 있습니다.
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 모든 데이터 타입이 가능한 필수값
  requiredAny: PropTypes.any.isRequired,
}
```
