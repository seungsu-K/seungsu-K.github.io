---
layout: post
title: 8/16(금) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/16(금) React 수업

## useEffect

컴포넌트의 사이드 이펙트(DOM 조작, fetch, 타이머 등 컴포넌트의 렌더링과 직접적으로 관련이 없는 작업)를 처리하기 위해 사용되는 훅

### 의존성 배열

useEffect가 다시 실행되어야 하는 조건이되는 값

```jsx
useEffect(()=>{
	// 사이드 이펙트 코드

	// clean-up 함수
	return () => {}
},[의존성 배열])
```

- 의존성 배열이 없는(`undefined`) 경우 : 렌더링 될 때마다 실행
- 빈 배열(`[]`)을 전달할 경우 : 컴포넌트가 처음 마운트될 때 한 번만 실행
  (서버에서 데이터를 가져오는 경우 처음 마운트될 때 한 번만 실행해서 가져올 것)
- 특정 값이 있는 경우 : 해당 값이 변경될 때마다 실행

### Clean-up 함수

useEffect 훅에서 반환하는 함수는 타이머를 해제하거나 구독을 취소하거나 이벤트 리스너를 제거하는 데 사용

### 유의 사항

- 의존성 배열 관리

  의존성 배열을 사용하지 않을 경우 `react-hooks/exhaustive-deps`

- 비동기 함수 수행 시, `useEffect` 내부에서 비동기 함수를 선언하고 즉시 호출하는 패턴으로 사용할 것

---

## useImmer

불변성을 유지하면서 상태를 쉽게 업데이트할 수 있도록 도와주는 라이브러리

```jsx
npm install immer
```

state 대신 `draft`라는 [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) 형식의 특별한 객체 타입을 사용해, 상태 변경을 기록함

```jsx
function Form() {
  const [person, updatePerson] = useImmer({
    name: "Niki de Saint Phalle",
    artwork: {
      title: "Blue Nana",
      city: "Hamburg",
      image: "https://i.imgur.com/Sd1AgUOm.jpg",
    },
  });

  const handleNameChange = (e) => {
    updatePerson((draft) => {
      draft.name = e.target.value;
    });
  };

  return (
    <>
      <label>
        Name:
        <input value={person.name} onChange={handleNameChange} />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {" by "}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>
    </>
  );
}
```

`useImmer`는 업데이트 핸들러를 간결하게 관리할 수 있는 좋은 방법이며, 특히 state가 중첩되어 있고 객체를 복사하는 것이 중복되는 코드를 만들 때 유용
