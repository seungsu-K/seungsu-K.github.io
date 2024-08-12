---
layout: post
title: 8/12(월) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/12(월) React 수업

## useRef

컴포넌트 내에 데이터를 “기억”할 수 있으며, 해당 데이터가 업데이트 되어도 렌더링이 발생하지 않음

1. DOM 요소에 대한 접근

   특정 DOM 요소에 직접 접근해야 할 때 유용. 포커스를 설정하거나, 비디오 플레이어를 제어하거나, 스크롤 위치를 조정하는 등의 작업

2. 불필요한 렌더링을 하지 않고 데이터를 저장

   상태(state)와 달리 값이 변경되어도 컴포넌트가 리렌더링되지 않음. `{ current: initialValue }` 객체를 반환하기 때문에 `current` 프로퍼티로 값을 읽거나 쓸 수 있음

```JavaScript
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    inputEl.current.focus();
  };

  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>
	      Focus the input
	     </button>
    </>
  );
}
```

`ref` 속성을 이용해 React로 ref 객체를 전달한다면, React는 노드가 변경될 때마다 변경된 DOM 노드에 `current` 프로퍼티를 설정

|           | useState                                                                                                                      | useRef                                                                                                   |
| --------- | ----------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| 목적      | 컴포넌트의 상태를 관리                                                                                                        | DOM 요소나 변수의 참조 관리                                                                              |
| 리렌더링  | 상태가 변경되면 컴포넌트가 리렌더링됨                                                                                         | 참조 값이 변경되어도 컴포넌트는 리렌더링되지 않음                                                        |
| 초기값    | 초기값을 인자로 받아 `state`와 상태 업데이트 함수 `setState`를 반환                                                           | 초기값을 인자로 받아 `current` 속성을 가진 객체를 반환                                                   |
| 사용 목적 | - UI에 영향을 미치는 상태를 관리할 때 <br /> - 리액트의 것을 기억하고 리액트에게 컴포넌트(함수)를 다시 실행시키도록 요청할 때 | - DOM 접근이나 렌더링에 영향을 미치지 않는 값을 저장할 때 <br />- 리액트의 것이 아닌 것을 기억해야 할 때 |
| 반환값    | `[ state, setState ]` 배열                                                                                                    | `{ current: initialValue }` 객체                                                                         |

---

## refCallback

DOM 요소나 컴포넌트의 참조를 설정하는 방법 중 하나로 참조를 설정하거나 해제할 때 추가 로직을 수행할 수 있음

refCallback 함수는 요소가 마운트 되었을 때, 언마운트 되었을 때 호출됨

렌더링 이후 추가적인 렌더링 유발 없이 DOM 노드와 동적으로 상호작용할 수 있음

```JavaScript
function CardLinkItem({ href, label, images = {} }) {
  const titleRef = useRef(null);

  const refCallBack = (el) => {
    if (el) {
      VanillaTilt.init(el, TILT_CONFIG);
    }
  };

  const handleEnter = () => {
    titleRef.current.style.opacity = '1';
  };

  const handleLeave = () => {
    titleRef.current.style.opacity = '0';
  };

  return (
    <a
      key={'identity'}
      ref={refCallBack}
      aria-label={label}
      title={label}
      href={href}
      onPointerEnter={handleEnter}
      onPointerLeave={handleLeave}
    >
      <figure className={cardClassNames}>
        <div>
          <img src={images.cover} alt="" />
        </div>
        <img ref={titleRef} src={images.title} alt="" />
        <img src={images.character} alt="" />
      </figure>
    </a>
  );
}
```
