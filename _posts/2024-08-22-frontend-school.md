---
layout: post
title: 8/22(목) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/22(목) React 수업

## Reducer

한 컴포넌트에서 state 업데이트가 여러 이벤트 핸들러로 분산되면 관리가 어려워지기 때문에 컴포넌트 외부에서 관리할 수 있도록 도와주는 훅

```jsx
function TaskApp() {
  const [tasks, setTasks] = useState(initialTasks);

  function handleAddTask(text) {
    setTasks([
      ...tasks,
      {
        id: nextId++,
        text: text,
        done: false,
      },
    ]);
  }

  function handleChangeTask(task) {
    setTasks(
      tasks.map((t) => {
        if (t.id === task.id) {
          return task;
        } else {
          return t;
        }
      })
    );
  }

  function handleDeleteTask(taskId) {
    setTasks(tasks.filter((t) => t.id !== taskId));
  }

  return (
    <>
      <h1>Prague itinerary</h1>
      <AddTask onAddTask={handleAddTask} />
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}
```

위 예시의 TaskApp 컴포넌트의 task state는 세 가지의 이벤트 핸들러를 사용해 상태를 업데이트 한다.

컴포넌트가 더 커질 경우 그 안에서 state를 다루는 로직의 양도 늘어나게 되고, 복잡성을 줄이고 접근성을 높이기 위해 state 로직을 컴포넌트 외부의 `reducer` 함수로 리팩토링한다.

### 1. action을 dispatch로 전달

이벤트 핸들러에 state를 설정해 무엇을 할 지 지시하는 것이 아니라, 해야할 일, 즉 `action` 객체를 전달

```jsx
function handleAddTask(text) {
  dispatch({
    type: "added",
    id: nextId++,
    text: text,
  });
}

function handleChangeTask(task) {
  dispatch({
    type: "changed",
    task: task,
  });
}

function handleDeleteTask(taskId) {
  dispatch({
    type: "deleted",
    id: taskId,
  });
}
```

### 2. reducer 함수 작성

state에 대한 로직을 작성하는 공간

```jsx
function reducer(state, action) {
  // React가 설정하게될 다음 state 값을 반환합니다.
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case "added": {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }

    case "changed": {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }

    case "deleted": {
      return tasks.filter((t) => t.id !== action.id);
    }

    default: {
      throw Error("Unknown action: " + action.type);
    }
  }
}
```

### 3. 컴포넌트에서 reducer 사용

`useReducer` 훅을 불러와 작성한 reducer 함수를 사용

```jsx
function TaskApp() {
  // const [tasks, setTasks] = useState(initialTasks);

  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  // 이벤트 핸들러...

  return (
    <>
      <h1>Prague itinerary</h1>
      <AddTask onAddTask={handleAddTask} />
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}
```

reducer 함수는 컴포넌트 외부에서 불러오는 것이기에 별도의 파일로 분리해서 관리하면 깔끔하게 관리할 수 있음

## useState와 useReducer 비교

- 코드 크기
  이벤트 핸들러가 여러 개일 경우 `useReducer`를 사용하면 코드의 양을 줄일 수 있음
- 가독성
  복잡한 구조의 state를 다룬다면 `useReducer`를 사용해 로직이 어떻게 동작하는지 이벤트 핸들러로 무엇이 발생했는데 명확히 구분할 수 있음
- 디버깅
  `useReducer`를 이용하면 어떤 action으로 인한 것인지, reducer 로직이 잘못된 것인지 찾아내기가 비교적 쉬움
- 테스팅
  reducer는 컴포넌트에 의존하지 않는 순수 함수이다. 독립적으로 분리할 수 있고 테스트를 할 수 있음
