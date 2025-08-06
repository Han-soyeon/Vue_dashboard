# router/index.js 코드 분석

## 개요

이 파일은 Vue 3 + Vue Router 기반 SPA(Single Page Application)의 라우팅 설정을 담당한다.
주요 역할은 각 URL 경로(path)에 따라 어떤 컴포넌트를 렌더링할지 정의하고, 라우팅을 Hash 모드로 구성하며, 스크롤 동작 설정도 포함되어 있다.

---

## 주요 구성

### 1. 라우터 설정 초기화

```js
import { h, resolveComponent } from 'vue'
import { createRouter, createWebHashHistory } from 'vue-router'
import DefaultLayout from '@/layouts/DefaultLayout'
```

| 항목                      | 설명                                       |
| ----------------------- | ---------------------------------------- |
| `h`, `resolveComponent` | 동적으로 `<router-view>` 같은 컴포넌트를 렌더링할 때 사용됨 |
| `createRouter`          | Vue Router 인스턴스를 생성                      |
| `createWebHashHistory`  | URL에 `#`을 포함한 hash 모드 히스토리 사용            |
| `DefaultLayout`         | 전체 앱에 적용되는 기본 레이아웃 컴포넌트                  |

---

### 2. 최상위 라우터 구조 

```js
const routes = [ ... ]
```

* 라우팅 전체 정의는 배열 형태
* 기본 경로 /는 DefaultLayout 컴포넌트를 사용하며 내부에 여러 자식 라우트를 포함함

---

### 3. 주요 라우트 구조

| 경로               | 이름            | 설명                        |
| ---------------- | ------------- | ------------------------- |
| `/`              | Home          | 루트, `/dashboard`로 리디렉션    |
| `/dashboard`     | Dashboard     | 대시보드 컴포넌트                 |
| `/theme`         | Theme         | `/theme/typography`로 리디렉션 |
| `/base`          | Base          | 다양한 기본 UI 컴포넌트 라우트 포함     |
| `/buttons`       | Buttons       | 버튼 관련 UI 컴포넌트 라우트 포함      |
| `/forms`         | Forms         | 폼 관련 컴포넌트 라우트 포함          |
| `/charts`        | Charts        | 차트 뷰                      |
| `/icons`         | Icons         | 아이콘 관련 라우트 그룹             |
| `/notifications` | Notifications | 알림 관련 UI 컴포넌트 라우트 포함      |
| `/widgets`       | Widgets       | 위젯 컴포넌트                   |
| `/pages`         | Pages         | 로그인/404 등 페이지 전용 경로 그룹    |


---

### /base 경로 상세

- 중첩 라우터를 사용 (<router-view> 활용)
- 여러 기본 컴포넌트로 구성됨

| 하위 경로                | 컴포넌트         |
| -------------------- | ------------ |
| `/base/accordion`    | Accordion    |
| `/base/breadcrumbs`  | Breadcrumbs  |
| `/base/cards`        | Cards        |
| `/base/carousels`    | Carousels    |
| `/base/collapses`    | Collapses    |
| `/base/list-groups`  | ListGroups   |
| `/base/navs`         | Navs         |
| `/base/paginations`  | Paginations  |
| `/base/placeholders` | Placeholders |
| `/base/popovers`     | Popovers     |
| `/base/progress`     | Progress     |
| `/base/spinners`     | Spinners     |
| `/base/tables`       | Tables       |
| `/base/tabs`         | Tabs         |
| `/base/tooltips`     | Tooltips     |

---

### /buttons 경로 상세

| 하위 경로                       | 컴포넌트         |
| --------------------------- | ------------ |
| `/buttons/standard-buttons` | Buttons      |
| `/buttons/dropdowns`        | Dropdowns    |
| `/buttons/button-groups`    | ButtonGroups |

---

### /forms 경로 상세

| 하위 경로                    | 컴포넌트           |
| ------------------------ | -------------- |
| `/forms/form-control`    | FormControl    |
| `/forms/select`          | Select         |
| `/forms/checks-radios`   | ChecksRadios   |
| `/forms/range`           | Range          |
| `/forms/input-group`     | InputGroup     |
| `/forms/floating-labels` | FloatingLabels |
| `/forms/layout`          | Layout         |
| `/forms/validation`      | Validation     |

---

### /icons 경로 상세

| 하위 경로                 | 컴포넌트        |
| --------------------- | ----------- |
| `/icons/coreui-icons` | CoreUIIcons |
| `/icons/brands`       | Brands      |
| `/icons/flags`        | Flags       |

---

### /notifications 경로 상세

| 하위 경로                   | 컴포넌트   |
| ----------------------- | ------ |
| `/notifications/alerts` | Alerts |
| `/notifications/badges` | Badges |
| `/notifications/modals` | Modals |
| `/notifications/toasts` | Toasts |

---

### /pages 경로 상세

| 하위 경로             | 컴포넌트     |
| ----------------- | -------- |
| `/pages/404`      | Page404  |
| `/pages/500`      | Page500  |
| `/pages/login`    | Login    |
| `/pages/register` | Register |

* 독립 페이지(에러, 로그인, 회원가입 등)를 처리

---

#### 기타 설정

```js
const router = createRouter({
  history: createWebHashHistory(import.meta.env.BASE_URL),
  routes,
  scrollBehavior() {
    return { top: 0 }
  },
})
```

| 항목                         | 설명                                  |
| -------------------------- | ----------------------------------- |
| `createWebHashHistory()`   | 브라우저 URL의 해시(`#`)를 이용한 클라이언트 라우팅 방식 |
| `import.meta.env.BASE_URL` | 앱의 기본 경로 (Vite 환경변수 사용)             |
| `scrollBehavior()`         | 라우트 이동 시 항상 상단으로 스크롤 이동             |

---

## 4. 최종 Export

- 라우터 인스턴스를 앱에서 사용할 수 있도록 내보낸다.

```js
export default router
```

---

## 5. 특징

1) 라우팅 구조가 명확하게 나뉨 (Base, Buttons, Forms 등)
2) Lazy loading(지연 로딩)을 활용해 성능 최적화
3) 중첩 라우트(router-view)를 적극 활용
4) 에러 페이지, 로그인, 회원가입 등 독립 페이지 처리 포함
5) Hash 모드 사용: 서버 설정 없이 동작 가능

---

## 6. router 폴더를 따로 분리하는 목적

| 목적                                  | 설명                                                                   |
| ----------------------------------- | -------------------------------------------------------------------- |
| **관심사 분리 (Separation of Concerns)** | 라우터 설정은 앱의 "경로 관리"만 담당하고, 다른 로직과 독립적으로 관리돼야 해.                       |
| **유지보수성 향상**                        | 경로가 늘어나거나 조건부 라우팅, 권한 체크 등 복잡해질 때 관리가 쉬워져.                           |
| **구조의 일관성**                         | 라우트 정의 파일을 `router/index.js` 또는 `router/routes.js`로 고정하면 협업 시 직관적이야. |
| **확장성 확보**                          | 대형 프로젝트에선 권한별, 레이아웃별, 모듈별로 라우트를 나눠서 동적 등록하는 구조도 가능해.                 |
| **미들웨어 로직 적용 용이**                   | 예: 네비게이션 가드, 권한 체크, 스크롤 위치 저장 등의 글로벌 설정이 router에서 잘 동작함.             |

---

## 7. 예시

1) 일반적인 프로젝트 구조 예시

    ```plaintext
    - Vue            
            src/
        ├── components/
        ├── views/
        ├── router/
        │   ├── index.js         ← Vue Router 인스턴스 생성
        │   └── routes.js        ← 경로 정의만 분리하는 경우도 있음
        ├── store/
        ├── assets/
        ├── App.vue
        └── main.js

    - React

            src/
        ├── components/
        ├── pages/
        ├── router/
        │   └── index.jsx        ← React Router 설정
        ├── App.jsx
        └── main.jsx
    ```
    * 즉, Vue든 React든 router/ 폴더 따로 관리하는 게 유지보수와 협업에 훨씬 유리하다

2) 응용
    - 권한 기반으로 라우트 분리

        ```js
        // router/index.js
        import publicRoutes from './publicRoutes'
        import adminRoutes from './adminRoutes'
        import userRoutes from './userRoutes'

        const routes = [...publicRoutes, ...userRoutes, ...adminRoutes]
        ```

    - 권한 체크 후 등록(비동기 라우트 등록)

        ```js
        if (user.role === 'admin') {
        router.addRoute(adminRoutes)
        }
        ```

---

## 8. 정리

- 작은 프로젝트라도 router 폴더 분리는 추천
- 구조화된 라우터 설정은 코드 가독성, 확장성, 유지보수성에서 큰 이점
- React/Vue 모두 동일한 원칙 적용 가능

| 기능              | Vue                  | React                       |
| --------------- | -------------------- | --------------------------- |
| 권한별 라우트 파일 분리   | O                    | O                           |
| 라우트 동적 등록       | `router.addRoute()`  | 조건부 `<Route>` 렌더링           |
| Lazy Loading 방식 | `import()`           | `React.lazy()` + `Suspense` |
| 권한 체크 방식        | `meta`, `beforeEach` | `PrivateRoute`, 조건부 렌더링     |
