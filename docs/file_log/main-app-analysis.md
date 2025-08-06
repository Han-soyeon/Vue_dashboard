# main.js, App.vue 분석

## main.js 코드 분석 

  ```js
  // 1. Vue 앱을 만들기 위한 함수 가져오기
  import { createApp } from 'vue'

  // 2. 전역 상태 관리 도구 Pinia 가져오기 (Vuex랑 비슷한 역할)
  import { createPinia } from 'pinia'

  // 3. 앱의 루트 컴포넌트(App.vue) 가져오기
  import App from './App.vue'

  // 4. 라우팅 설정 파일 가져오기 (페이지 이동 기능)
  import router from './router'

  // 5. CoreUI 컴포넌트 라이브러리 가져오기 (UI를 쉽게 만들 수 있는 템플릿)
  import CoreuiVue from '@coreui/vue'

  // 6. CoreUI에서 사용하는 아이콘 컴포넌트 가져오기
  import CIcon from '@coreui/icons-vue'

  // 7. 우리가 사용할 아이콘 세트 가져오기 (assets 폴더 안의 icons.js)
  import { iconsSet as icons } from '@/assets/icons'

  // 8~10. 문서용 샘플 컴포넌트 3개 가져오기 (예제용 컴포넌트들)
  import DocsComponents from '@/components/DocsComponents'
  import DocsExample from '@/components/DocsExample'
  import DocsIcons from '@/components/DocsIcons'

  // ==============================
  // Vue 앱 초기 설정 시작
  // ==============================

  // 1. App.vue를 기반으로 앱을 생성
  const app = createApp(App)

  // 2. 전역 상태 관리 기능(Pinia) 등록
  app.use(createPinia())

  // 3. 라우터 등록 (페이지 이동 기능)
  app.use(router)

  // 4. CoreUI 등록 (디자인 컴포넌트 사용 가능해짐)
  app.use(CoreuiVue)

  // 5. 전체 앱 어디서나 사용할 수 있도록 'icons' 데이터를 제공
  //    (provide/inject 방식으로 하위 컴포넌트에서 불러올 수 있음)
  app.provide('icons', icons)

  // 6~8. 특정 컴포넌트들을 전역으로 등록
  //    - 이제 이 컴포넌트들은 import 없이도 어디서나 바로 사용할 수 있음
  app.component('CIcon', CIcon)
  app.component('DocsComponents', DocsComponents)
  app.component('DocsExample', DocsExample)
  app.component('DocsIcons', DocsIcons)

  // 9. 실제 HTML 문서(index.html)에 있는 <div id="app">에 이 앱을 보여줌
  app.mount('#app')
  ```

## App.vue 코드 분석 

### App.vue 전체 코드 요약

1. 사용자가 접속하면
2. URL 또는 저장된 설정을 기준으로 테마를 설정하고
3. 라우터에 따라 알맞은 화면을 보여주며
4. 스타일을 전체적으로 적용함

---
```vue
<script setup>        // 1. 테마 설정과 초기 실행 코드
<template>            // 2. 실제로 화면에 보여지는 부분
<style lang="scss">   // 3. 앱의 전역 스타일 적용

<!-- 1. 초기 설정 및 테마 처리 -->
<script setup>
import { onBeforeMount } from 'vue' // 컴포넌트가 화면에 나타나기 전에 실행되는 훅(hook)
import { useColorModes } from '@coreui/vue' // CoreUI에서 테마 설정 도구 가져오기
import { useThemeStore } from '@/stores/theme.js' // 테마 상태 관리하는 Store 가져오기 (Pinia)

const { isColorModeSet, setColorMode } = useColorModes(
  'coreui-free-vue-admin-template-theme',  // 테마 설정을 저장할 로컬스토리지 키
)

const currentTheme = useThemeStore() // 현재 테마 상태 불러오기

// 컴포넌트가 마운트되기 전에 테마 설정을 결정
onBeforeMount(() => {
  // 주소(URL)에 '?theme=dark' 같은 파라미터가 있으면 그걸 가져옴
  const urlParams = new URLSearchParams(window.location.href.split('?')[1])
  let theme = urlParams.get('theme')

  // theme 값이 있으면 알파벳/숫자만 필터링
  if (theme !== null && theme.match(/^[A-Za-z0-9\s]+/)) {
    theme = theme.match(/^[A-Za-z0-9\s]+/)[0]
  }

  // 1순위: URL에 theme 값이 있으면 그걸 사용
  if (theme) {
    setColorMode(theme)
    return
  }

  // 2순위: 로컬스토리지에 테마 설정된 게 있으면 그대로 사용
  if (isColorModeSet()) {
    return
  }

  // 3순위: store에 있는 기본 테마를 사용
  setColorMode(currentTheme.theme)
})
</script>

<!-- 2. 화면에 표시될 부분 -->
<template>
  <router-view />
</template>
```