
---

# 📌 오늘의 큰 주제 한 줄 요약

> **“Vue 3 + Vite로 영화 추천 SPA를 만들기 위해, 프로젝트 구조·상태관리·API 연동·Git 워크플로우까지 한 번에 세팅한 날.”**

---

# 1. Vite + Vue3 프로젝트 구조 이해

## 1-1. Vite로 Vue 프로젝트 생성

```bash
npm create vite@latest 11-pjt
cd 11-pjt
npm install
npm run dev
```

* `npm create vite@latest`: Vite 템플릿으로 프로젝트 뼈대 생성
* Vite의 특징:

  * 개발 서버 빠름
  * 설정이 가볍고 Vue3랑 궁합 좋음
  * `.env`도 Vite 방식으로 처리 (`VITE_` prefix)

---

## 1-2. src 폴더 구조 설계

우리가 만든 구조:

```text
src/
  api/axios.js          # 외부 API 통신 전담
  assets/main.css       # 전역 스타일
  components/           # 재사용 가능한 UI
    Navbar.vue
    MovieCard.vue
    VideoModal.vue
  router/index.js       # 페이지 라우팅
  stores/               # Pinia 상태 관리
    movies.js
    reviews.js
    weather.js
  views/                # 화면 단위 페이지
    HomeView.vue
    MovieListView.vue
    MovieDetailView.vue
    ReviewSearchView.vue
    RecommendedView.vue
  App.vue               # 루트 컴포넌트
  main.js               # 앱 진입점
```

여기서 중요한 개념:

* **components**: 작고 재사용 가능한 UI 조각들 (카드, 모달 등)
* **views**: 라우터와 연결되는 “페이지 단위 컴포넌트”
* **stores**: 페이지 간에 공유할 상태(데이터, 로딩, 에러 등)를 모아둔 중앙 창고
* **api/axios.js**: Axios 인스턴스를 모아둔 파일 → 여기서만 baseURL, key 설정

---

# 2. SPA, Vue Router, 페이지 구조

## 2-1. SPA란?

* **Single Page Application**
* 페이지 이동해도 실제로는 딱 한 개의 HTML에서 동작
* URL에 따라 **다른 컴포넌트를 렌더링**해서 마치 여러 페이지인 것처럼 보이게 함
* 장점:

  * 새로고침 없이 부드러운 화면 전환
  * 프론트에서 상태를 들고 있을 수 있음 (Pinia랑 궁합 좋음)

---

## 2-2. Vue Router 개념

`src/router/index.js` 에서 설정:

```js
import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'
import MovieListView from '../views/MovieListView.vue'
import MovieDetailView from '../views/MovieDetailView.vue'
import ReviewSearchView from '../views/ReviewSearchView.vue'
import RecommendedView from '../views/RecommendedView.vue'

const routes = [
  { path: '/', name: 'home', component: HomeView },
  { path: '/movies', name: 'movies', component: MovieListView },
  { path: '/movies/:movieId', name: 'movie-detail', component: MovieDetailView, props: true },
  { path: '/review-search', name: 'review-search', component: ReviewSearchView },
  { path: '/recommended', name: 'recommended', component: RecommendedView },
]

const router = createRouter({
  history: createWebHistory(),
  routes,
})

export default router
```

핵심 개념:

* **path**: 주소
* **component**: 그 주소에 들어갔을 때 보여줄 Vue 컴포넌트
* `:movieId` 처럼 콜론(`:`) 붙이면 → **동적 라우트 파라미터**

  * 예: `/movies/123` → `movieId = 123`

`App.vue`에서:

```vue
<template>
  <div class="app">
    <Navbar />
    <main class="app-main">
      <RouterView />
    </main>
  </div>
</template>
```

* `<RouterView />` : 현재 URL에 맞는 View 컴포넌트를 표시하는 자리
* `<RouterLink>` : a 태그 대신 쓰는 Vue Router 전용 링크

---

# 3. 상태 관리: Pinia 개념 이해

## 3-1. 왜 Pinia를 쓰는지?

* 여러 페이지에서 같은 정보를 공유해야 할 때:

  * 예: 영화 목록, 선택된 영화 상세, YouTube 검색 결과, 날씨 정보
* prop으로 계속 넘기기 귀찮고 복잡해지는 걸 해결
* Pinia는 Vuex보다 가볍고, Composition API랑 잘 맞음

## 3-2. Pinia 기본 패턴

```js
// movies.js
import { defineStore } from 'pinia'
import { tmdb } from '@/api/axios'

export const useMovieStore = defineStore('movies', {
  state: () => ({
    topRated: [],
    currentMovie: null,
    isLoading: false,
    error: null,
  }),
  actions: {
    async fetchTopRated() {
      try {
        this.isLoading = true
        this.error = null
        const res = await tmdb.get('movie/top_rated')
        this.topRated = res.data.results
      } catch (err) {
        this.error = '영화 목록을 불러오는데 실패했습니다.'
      } finally {
        this.isLoading = false
      }
    },
  },
})
```

포인트:

* `state`: 화면에서 사용할 데이터들의 묶음
* `actions`: 비동기 로직(API 호출), 상태 변경 담당
* 로딩/에러도 함께 관리 → View 쪽에서 깔끔하게 처리 가능

  * `v-if="store.isLoading"` → “불러오는 중…”
  * `v-else-if="store.error"` → 에러 메시지 출력

---

# 4. Axios, 외부 API, axios 인스턴스 패턴

## 4-1. axios 인스턴스 만들기

```js
// src/api/axios.js
import axios from 'axios'

const TMDB_API_KEY = import.meta.env.VITE_TMDB_API_KEY
const YT_API_KEY = import.meta.env.VITE_YOUTUBE_API_KEY
const WEATHER_API_KEY = import.meta.env.VITE_WEATHER_API_KEY

export const tmdb = axios.create({
  baseURL: 'https://api.themoviedb.org/3/',
  params: {
    api_key: TMDB_API_KEY,
    language: 'ko-KR',
  },
})

export const youtube = axios.create({
  baseURL: 'https://www.googleapis.com/youtube/v3/',
  params: {
    key: YT_API_KEY,
    part: 'snippet',
    maxResults: 12,
  },
})

export const weather = axios.create({
  baseURL: 'https://api.openweathermap.org/data/2.5/',
  params: {
    appid: WEATHER_API_KEY,
    units: 'metric',
  },
})
```

개념:

* 인스턴스 한 번 만들어두면, 각 API 호출할 때 **baseURL, 공통 params** 반복 안 해도 됨
* TMDB / YouTube / Weather API를 역할별로 분리 → 가독성 & 유지보수 좋음

---

## 4-2. TMDB API 사용 패턴

* Top Rated 영화 목록:

```js
const res = await tmdb.get('movie/top_rated')
// res.data.results -> 영화 배열
```

* 영화 상세 정보:

```js
const res = await tmdb.get(`movie/${id}`)
```

* 영화 예고편 영상 목록:

```js
const res = await tmdb.get(`movie/${id}/videos`)
const trailer = res.data.results.find(v => v.type === 'Trailer') || res.data.results[0]
```

---

## 4-3. YouTube Data API 사용 패턴

* 검색 API:

```js
const res = await youtube.get('search', {
  params: {
    q: `${query} 영화 리뷰`,
    type: 'video',
  },
})
// res.data.items -> 영상 목록
```

주의:

* YouTube는 **403 에러(Invalide API key / 권한 문제)** 자주 뜸
* 해결 포인트:

  * API 활성화: YouTube Data API v3 사용 설정
  * API 키 제한: 해당 키에 YouTube Data API v3만 허용
  * HTTP referrer 설정에서 `http://localhost:5173/*` 허용

---

## 4-4. OpenWeatherMap API 사용 패턴

```js
const res = await weather.get('weather', {
  params: {
    q: this.city, // 예: Seoul
  },
})

this.weatherData = res.data
```

그리고 날씨 상태에 따라 장르 매핑:

```js
const main = this.currentMain.toLowerCase()
if (main.includes('rain')) return 10749 // Romance
if (main.includes('cloud')) return 18   // Drama
if (main.includes('clear')) return 35   // Comedy
if (main.includes('snow')) return 16    // Animation
return 28                               // Action 기본
```

---

# 5. .env와 API Key 보안

## 5-1. Vite에서 env 변수 네이밍 규칙

* 반드시 `VITE_`로 시작해야 `import.meta.env`에서 접근 가능

```env
VITE_TMDB_API_KEY=...
VITE_YOUTUBE_API_KEY=...
VITE_WEATHER_API_KEY=...
VITE_OPENWEATHER_CITY=Seoul
```

코드에서:

```js
const TMDB_API_KEY = import.meta.env.VITE_TMDB_API_KEY
```

## 5-2. 왜 .env를 git에 올리면 안 되는지?

* API Key는 외부에 유출되면 **남이 내 키로 호출해서 요금/쿼타 다 써버림**
* 그래서 `.gitignore`에 `.env`를 반드시 포함

```gitignore
.env
.env.*
!.env.example
```

---

# 6. Git 개념: add, commit, push, 브랜치, 되돌리기

## 6-1. 기본 흐름

```bash
git add .
git commit -m "메시지"
git push
```

* **add**: 변경된 파일을 “스테이징(staging)” 영역에 올림
* **commit**: 그 상태를 하나의 스냅샷으로 기록
* **push**: 원격 저장소(GitLab)에 올리기

---

## 6-2. main vs master 브랜치 차이

오늘 겪었던 에러:

```text
error: src refspec master does not match any
On branch main
```

의미:

* **로컬 브랜치 이름은 main**
* 근데 `git push origin master` 해서, 존재하지 않는 브랜치(master)를 push하려고 한 것
* 해결은:

```bash
git push -u origin main
```

→ 이후엔 그냥 `git push`만 하면 됨.

---

## 6-3. 자주 쓰는 Git 되돌리기

* `git restore --staged .`

  * `git add` 취소 (파일 내용은 유지)
* `git reset --soft HEAD~1`

  * 마지막 커밋 취소 (코드는 그대로, 다시 커밋 가능)
* `git reset --mixed HEAD~1`

  * 커밋 + add 둘 다 취소, 코드만 작업 디렉토리에 남김
* `git reset --hard HEAD~1`

  * 커밋 + 코드까지 전부 이전 상태로 완전 되돌림 (위험)
* 이미 push한 커밋 되돌리기:

  * `git revert 커밋ID` (새로운 “되돌림 커밋” 생성)

---

# 7. 오늘 만든 컴포넌트/페이지 개념 정리

## 7-1. Navbar.vue

* 역할: 상단 네비게이션
* 특징:

  * `RouterLink`로 각 페이지로 이동
  * SPA에서 “페이지 이동처럼 보이게 해주는” 핵심 UI

## 7-2. MovieCard.vue

* 역할: 영화 정보를 카드 형태로 보여주는 컴포넌트
* 기능:

  * 포스터 이미지
  * 평점 뱃지
  * 제목, 개봉일, 줄거리
  * 카드 클릭 시 해당 영화 상세 페이지로 라우팅

## 7-3. VideoModal.vue

* 역할: YouTube 영상 재생 모달
* props: `videoId`
* 기능:

  * 배경 클릭 시 닫기 (`@click.self`)
  * iframe으로 YouTube 영상 임베드

---

# 8. “프롬프트로 프로젝트 설명하기” 개념

이것도 오늘 중요한 포인트였음.

* 지금 만드는 게 단순 코드가 아니라 **하나의 프로젝트 컨벤션**이라서
* ChatGPT에게 “내 프로젝트는 이런 구조야, 이런 규칙을 따라줘”라고 알려주는 **시스템 프롬프트**를 만들었음

핵심 아이디어:

1. **프로젝트 구조 설명**
2. **사용 기술 & 규칙 (Vue3 + script setup, Pinia, Axios 인스턴스, Router)** 명시
3. 앞으로 이 규칙을 기반으로 코드/리팩토링/문서만 생성해달라고 요청

이렇게 해두면:

* 다음부터 “이 컴포넌트 수정해줘”라고 했을 때
  → 완전히 엉뚱한 Vue 스타일로 나오지 않고
  → 지금 프로젝트 구조에 딱 맞게 코드가 생성됨

---

# ✏️ 오늘 TIL용 한 줄 요약 

> 오늘은 Vite + Vue3로 영화 추천 SPA 뼈대를 만들었다.
> router, Pinia, axios 인스턴스를 이용해서 구조를 깔끔하게 나누고,
> TMDB / YouTube / OpenWeatherMap API를 연결해서
> Top Rated, 리뷰 검색, 날씨 기반 추천 기능을 구현할 준비를 했다.
> 또, .env로 API 키를 안전하게 관리하는 법과
> Git에서 main 브랜치 기준으로 add/commit/push, 되돌리기를 정리했다.
> 마지막으로, 이 프로젝트를 AI에게 설명하는 “프로젝트 전용 프롬프트”도 만들었다.

---
