---
layout: post
title: 8/19(월) React 수업
categories: [Frontend School]
tags: [class]
---

# 8/19(월) React 수업

## 비동기 함수로 CRUD 메서드 작성

fetch 함수 사용 시 에러 핸들링 꼭 작성할 것

```jsx
// 백엔드 API 엔드포인트
const ENDPOINT = "http://127.0.0.1:8090"; // pocketbase
const REQUEST_OPTIONS = {
  headers: {
    "Content-Type": "application/json",
  },
};

export async function createNote(newNote) {
  // 외부 시스템(서버)에 데이터 생성 요청
  // POST 요청 URL
  const REQUEST_URL = `${ENDPOINT}/api/collections/notes/records`;

  // POST 요청 시, 전송할 JSON 포멧 문자열
  const body = JSON.stringify({
    title: newNote.title,
    description: newNote.description,
  });

  // 서버에서 응답
  const response = await fetch(REQUEST_URL, {
    method: "POST",
    body,
    ...REQUEST_OPTIONS,
  });

  // 에러 핸들링
  if (!response.ok) {
    throw new Response(
      JSON.stringify({ message: "서버에서 요청에 응답하지 않습니다." }),
      { status: 500 }
    );
  }

  const responseData = await response.json();
  return responseData;
}

export async function readNotes(page = 1, perPage = 10) {
  const REQUEST_URL = `${ENDPOINT}/api/collections/notes/records?page=${page}&perPage=${perPage}`;
  const response = await fetch(REQUEST_URL);

  if (!response.ok) {
    throw new Response(
      JSON.stringify({ message: "서버에서 요청에 응답하지 않습니다." }),
      { status: 500 }
    );
  }

  const responseData = await response.json();

  return responseData;
}

export async function readNoteOne(noteId) {
  const REQUEST_URL = `${ENDPOINT}/api/collections/notes/records/${noteId}`;
  const response = await fetch(REQUEST_URL);

  if (!response.ok) {
    throw new Response(
      JSON.stringify({ message: "서버에서 요청에 응답하지 않습니다." }),
      { status: 500 }
    );
  }

  const responseData = await response.json(noteId);
  return responseData;
}

export async function updateNote(editNote) {
  const REQUEST_URL = `${ENDPOINT}/api/collections/notes/records/${editNote.id}`;
  const body = JSON.stringify({
    title: editNote.title,
    description: editNote.description,
  });

  const response = await fetch(REQUEST_URL, {
    method: "PATCH",
    body,
    ...REQUEST_OPTIONS,
  });

  if (!response.ok) {
    throw new Response(
      JSON.stringify({ message: "서버에서 요청에 응답하지 않습니다." }),
      { status: 500 }
    );
  }

  const responseData = await response.json();

  return responseData;
}

export async function deleteNote(deleteId) {
  const REQUEST_URL = `${ENDPOINT}/api/collections/notes/records/${deleteId}`;

  const response = await fetch(REQUEST_URL, {
    method: "DELETE",
    ...REQUEST_OPTIONS,
  });

  if (!response.ok) {
    throw new Response(
      JSON.stringify({ message: "서버에서 요청에 응답하지 않습니다." }),
      { status: 500 }
    );
  }

  return response;
}
```

---

## Single Page Application

### react-router-dom

클라이언트 사이드 렌더링을 구현하기 위한 page간 경로를 관 하는 라이브러리

### routes 만들기

- `createBrowserRouter`

  DOM History API를 사용하여 URL을 업데이트하고 라우터를 관리

  ```jsx
  const routes = [
    {
      path: "/",
      element: <RootLayout />,
      errorElement: <ErrorPage />,
      children: [
        {
          path: "/",
          element: <HomePage />,
        },
        {
          path: "/notes",
          element: <NoteListPage />,
          children: [
            {
              path: "/notes/new",
              element: <NewNotePage />,
            },
            {
              path: "/notes/detail",
              element: <NoteDetailPage />,
            },
          ],
        },
      ],
    },
  ];

  const router = createBrowserRouter(routes);
  ```

- `createRoutesFromElements`
  JSX 구문으로 `<Route>` 요소를 사용해 route 객체를 만들어서 URL 경로를 만듦 (Legacy)

  ```jsx
  const routes = createRoutesFromElements(
    <>
      <Route path="/" element={<HomePage />} />
      <Route path="/notes" element={<NoteListPage />}>
        <Route path="/notes/new" element={<NewNotePage />} />
        <Route path="/notes/detail" element={<NoteDetailPage />} />
      </Route>
    </>
  );

  const router = createBrowserRouter(routes);
  ```

### RouterProvider

`<RouterProvider>` 컴포넌트에 Route를 전달해 URL 경로에 따라 특정 컴포넌트가 렌더링할 수 있음

```jsx
function App() {
  return (
    <>
      <RouterProvider router={router} />
    </>
  );
}

export default App;
```

### <link>

페이지의 경로를 설정할 수 있는 요소로, <a> 요소와 비슷하지만 새로운 페이지를 불러오지 않고 리렌더링 함

```jsx
function Navigation() {
  return (
    <nav>
      <ul>
        <li>
          <Link to={{ pathname: "/" }}>홈</Link>
        </li>
        <li>
          <Link to="/notes/new">노트 작성</Link>
        </li>
        <li>
          <Link to="/notes/detail">노트 1</Link>
        </li>
        <li>
          <Link to="/notes/detail">노트 2</Link>
        </li>
      </ul>
    </nav>
  );
}
```

### <Route>

경로와 해당 경로에서 렌더링할 요소(컴포넌트, 페이지)를 지정

```jsx
function AppRoutes() {
  return (
    <>
      <Route path="/" element={<HomePage />} />
      <Route path="/about" element={<AboutPage />} />
    </>
  );
}
```

### <RootLayout>

Header, Footer, Navigation 등을 포함하는 애플리케이션의 공통 레이아웃을 정의

```jsx
function RootLayout() {
  return (
    <div className={S.component}>
      <GlobalNav />
      <Outlet />
    </div>
  );
}

export default RootLayout;
```

### <Outlet>

부모 경로 내 자식 경로를 설정했을 경우 `<Outlet>` 요소로 자식 컴포넌트가 렌더링될 출구를 지정

### <NavLink>

"active", "pending", or "transitioning” 등의 상태를 알 수 있는 <Link> 요소의 종류, 상태를 파악해 스타일을 지정할 수 있음

하위 경로는 URL이 겹치는 부분이 있기 때문에 정확한 URL만 스타일을 지정해야 할 경우 `end` 속성을 추가

```jsx
function GlobalNav() {
  return (
    <nav>
      <NavLink
        to={{ pathname: "/" }}
        end
        className={({ isActive }) => {
          return isActive ? S.active : undefined;
        }}
      >
        홈
      </NavLink>
      <NavLink to="/notes/new">노트 작성</NavLink>
      <NavLink to="/notes/detail">노트 1</NavLink>
      <NavLink to="/notes/detail">노트 2</NavLink>
    </nav>
  );
}
```
