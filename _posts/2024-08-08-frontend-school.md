---
layout: post
title: 8/8(목) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/8(목) React 수업

NoteApp 만들기 실습

- 경로 정보에 따라 화면에 출력할 페이지를 switch 문으로 지정해서 CRUD를 구현

- 하위 페이지에서 먼저 상태를 관리하는 코드를 짠 후 다른 페이지에서 동일한 상태 관리가 필요하다면 상위의 공통 부모로 끌어올려 관리한다.

- tree 구조가 깊어질수록 상태를 전달하는 과정이 길어지는 drilling이 발생함

  → context 또는 상태 관리 라이브러리로 해결

- useState 훅의 업데이트 함수는 상태를 합성하지 않고 덮어쓴다

  → 객체 또는 배열 타입의 데이터를 상태로 관리할 경우, 데이터를 직접 합쳐 주어야 함(spread syntax)

  ```JavaScript
  const handleUpdateFormData = (e) => {

    const { name, value } = e.target;

    const nextFormData = {
      // formData가 없으면 content를 입력하는 순간
      // title 내용이 없어져 버림
      ...formData,
      [name]: value,
    };
    setFormData(nextFormData);
    };
  ```

- 지금까지 실습은 서버를 구현하지 않고 더미 데이터를 사용. 그렇기에 note를 create한 경우, 클라이언트의 리스트에는 추가되지만 실제 서버에 반영되지 않은 것이기 때문에 detail페이지로 들어갈 수 없음

- `#` : named anchor 현재 페이지의 어떤 목적지(id)를 찾아감
