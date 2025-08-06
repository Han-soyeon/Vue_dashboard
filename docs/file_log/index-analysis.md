# index.html 파일 분석 (Vue CoreUI Admin Template)

이 파일은 **Vue 앱의 진입점 HTML**로, `main.js`에서 생성한 Vue 앱이 실제로 **붙는 화면의 틀**을 구성

---

## 기본 구조

```html
<!DOCTYPE html>
<html>
  <head> ... </head>
  <body>
    <div id="app"></div>   <!-- 여기 Vue 앱이 연결됨 -->
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

---

## 주요 구성 요소

1. <!DOCTYPE html>
    . 해당 파일이 HTML5 형식임을 선언

2. <head> 영역

| 항목                                                   | 설명                     |
| ---------------------------------------------------- | ---------------------- |
| `<meta charset="utf-8">`                             | 문자 인코딩을 UTF-8로 설정      |
| `<meta name="viewport"...>`                          | 반응형 디자인을 위한 뷰포트 설정     |
| `<meta name="description">`                          | SEO 및 소셜 미리보기용 페이지 설명  |
| `<title>`                                            | 브라우저 탭에 표시될 페이지 제목     |
| `<link rel="icon">`, `<link rel="apple-touch-icon">` | 각종 기기/플랫폼에 맞는 앱 아이콘 등록 |
| `<meta name="theme-color">`                          | 브라우저의 상단 색상 지정 (모바일용)  |

3. <script> 영역
    . 다크모드 테마 설정 스크립트

```html
<script>
  const userMode = localStorage.getItem('coreui-free-vue-admin-template-theme');
  const systemDarkMode = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
  if (userMode === 'dark' || (userMode !== 'light' && systemDarkMode)) {
    document.documentElement.dataset.coreuiTheme = 'dark';
  }
</script>
```
* 사용자 또는 시스템 설정에 따라 앱을 다크모드로 자동 설정
* dataset.coreuiTheme = 'dark'은 이후 CSS에서 테마 스타일을 바꾸는 기준이 된다.

4. <body> 영역

| 요소                                          | 설명                             |
| ------------------------------------------- | ------------------------------ |
| `<noscript>`                                | 브라우저에서 자바스크립트가 꺼져있을 때 안내문 표시   |
| `<div id="app"></div>`                      | `main.js`에서 마운트되는 Vue 앱의 루트 위치 |
| `<script type="module" src="/src/main.js">` | Vue 앱의 실행 시작점 연결               |

* 이 파일은 Vite 또는 Vue CLI 빌드 시 자동으로 포함됨
* public/ 폴더 안에 있어야 하며, 배포 시 최종 HTML 파일이 됨