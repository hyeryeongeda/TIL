## 1) HTML & CSS — 웹의 뼈대와 스타일링의 기초

### HTML (HyperText Markup Language)

- **역할**: 문서의 구조와 의미를 마크업(tag)을 통해 표현
- **주요 태그 유형**

  - **구조 태그**: `<html>`, `<head>`, `<body>`
  - **섹션 태그**: `<header>`, `<nav>`, `<section>`, `<article>`, `<footer>`
  - **텍스트 콘텐츠 태그**: `<h1>–<h6>`, `<p>`, `<blockquote>`, `<ul>`, `<ol>`, `<li>`
  - **링크/이미지/미디어 태그**: `<a>`, `<img>`, `<video>`, `<audio>`
  - **폼 태그**: `<form>`, `<input>`, `<select>`, `<textarea>`, `<button>`
  - **시맨틱 태그**: 의미 중심으로 쓰는 태그 (예: `<main>`, `<aside>`, `<figure>` 등)

- **속성(Attribute) 활용**
  태그에 `id`, `class`, `src`, `href`, `alt`, `title` 등 속성을 줘서 의미나 스타일, 연결 등을 제어
- **HTML 문서 구조**

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <title>문서 제목</title>
      <link rel="stylesheet" href="styles.css" />
    </head>
    <body>
      <!-- 본문 내용 -->
    </body>
  </html>
  ```

- **접근성과 시맨틱 마크업**
  단순히 보이게 하는 목적이 아니라, 검색 엔진 최적화(SEO)나 스크린 리더 사용자 등에게 의미를 전달할 수 있게 태그를 선택해야 함.

---

### CSS (Cascading Style Sheets)

- **역할**: HTML 요소들의 “보이는 모습(표현)”을 정의
- **기본 문법**

  ```css
  선택자 {
    속성: 값;
    속성2: 값2;
  }
  ```

- **선택자(Selectors) 종류**

  - 타입 선택자: `p`, `div`
  - 클래스 선택자: `.classname`
  - ID 선택자: `#idname`
  - 자식 선택자, 후손 선택자, 인접 형제 선택자 등 복합 선택자
  - 가상 클래스 / 가상 요소: `:hover`, `:before`, `:after` 등

- **상속(Inheritance) & 명시도(Specificity) & 우선순위**

  - 상속: 부모 요소의 스타일(예: `color`, `font-family`)이 자식에게 이어지는 경우
  - 명시도: 어떤 스타일 규칙이 우선 적용될지 결정. 인라인 스타일 > ID > 클래스 > 태그 선택자 순.
  - `!important`: 강제 우선 적용 (사용은 최소화)

- **박스 모델(Box Model)**

  - 모든 요소는 “콘텐츠(content) + 여백(padding) + 테두리(border) + 외부 여백(margin)” 구조
  - `box-sizing` 속성 (`content-box`, `border-box`)으로 크기 계산 방식 조정

- **단위와 값 타입**

  - 절대 단위: `px`, `cm`, `mm` 등
  - 상대 단위: `em`, `rem`, `%`, `vw`, `vh` 등
  - 색상 표현: `hex (#rrggbb)`, `rgb()`, `rgba()`, `hsl()`

- **배경 & 테두리 & 그림자**

  - `background-color`, `background-image`, `background-position`, `background-repeat`
  - `border`, `border-radius`
  - `box-shadow`, `text-shadow`

- **텍스트 스타일링**

  - `font-family`, `font-size`, `line-height`, `text-align`, `text-decoration`, `letter-spacing` 등

- **레이아웃 보조 속성**

  - `display`, `position`, `float`, `clear`, `overflow`
  - `z-index` (쌓임 순서)

- **미디어 쿼리 (Responsive 조정용)**

  ```css
  @media (max-width: 768px) {
    /* 작은 화면에서의 스타일 */
  }
  ```

- **전환 및 애니메이션**

  - `transition` 속성
  - `@keyframes` + `animation` 속성

---

## 2) CSS Layout — 화면 배치, 정렬, 배치 전략

화면에 요소들을 배치하는 방식이 이 파트의 핵심이야.

### 디스플레이(display) 속성

- `display: block` — 요소가 한 줄 전체를 차지, 줄바꿈
- `display: inline` — 콘텐츠만큼만 공간 차지, 줄바꿈 없음
- `display: inline-block` — inline처럼 흐르지만 크기 지정 가능
- `display: none` — DOM 상에 존재하되 렌더링하지 않음
- `display: flex` / `inline-flex` — Flexbox 레이아웃 컨테이너
- `display: grid` — CSS Grid 레이아웃 컨테이너

---

### 포지션(position) 속성

- `static` (기본) — 문서 흐름에 따라 배치
- `relative` — 원래 자리 기준으로 이동 가능
- `absolute` — 가장 가까운 position이 지정된 상위 요소를 기준으로 위치
- `fixed` — 뷰포트를 기준으로 고정 (스크롤해도 움직이지 않음)
- `sticky` — 스크롤 위치에 따라 `relative` ↔ `fixed` 전환

---

### 플렉스박스(Flexbox)

CSS3에서 나온 1차원 레이아웃 모델.
`display: flex`를 주면 자식 요소들이 주 축(main axis)과 교차 축(cross axis)을 기준으로 정렬돼.

- 주요 속성:

  - `flex-direction`: row / column / row-reverse / column-reverse
  - `justify-content`: 주 축 정렬 (start, end, center, space-between 등)
  - `align-items`: 교차 축 정렬
  - `flex-wrap`: 줄바꿈 여부
  - `flex-grow`, `flex-shrink`, `flex-basis` — 자식 요소 크기 비율 제어
  - `align-self`: 개별 아이템 교차축 정렬

장점: 수평/수직 중앙 정렬이 간편하고, 유연한 배열 제어 가능
단점: 복잡한 2차원 배치 (행+열 동시에 제어)는 제한적

---

### CSS Grid (격자 레이아웃)

Flexbox의 한계를 보완하면서 **2차원 배치** 지원
행(row)과 열(column) 기반으로 영역을 정의할 수 있어 복잡한 레이아웃 구성에 강해. ([위키백과][1])

- 주요 속성:

  - `display: grid`
  - `grid-template-columns`, `grid-template-rows`
  - `grid-gap`(또는 `gap`)
  - `grid-template-areas`
  - `grid-column-start`, `grid-column-end`, `grid-row-start`, `grid-row-end`

- 응용 예: “Holy grail layout”, 카드 레이아웃, 미디어 갤러리 등

---

### 레이아웃 선택 팁

- 한 방향 정렬 위주면 Flexbox
- 격자 구조가 명확하면 CSS Grid
- 복합 구조면 둘을 병행
- 레이아웃 복잡도가 낮으면 float / position 방식도 가능했지만 요새는 잘 안 쓰임

---

## 3) Bootstrap — 프레임워크를 통한 빠른 UI 개발

### 개요 & 역사

- 2011년에 Twitter 내부 도구 통합을 위해 마크 오토와 제이콥 손턴이 개발 → 오픈소스로 공개됨. ([위키백과][2])
- CSS + JS + Sass 등으로 구성된 UI 프레임워크
- 목적: 일관된 사용자 인터페이스와 빠른 개발 속도 제공

---

### 구성 요소

- **레이아웃 시스템**: 컨테이너, 행(row), 열(column), 브레이크포인트
- **컴포넌트**: 버튼, 네비게이션 바, 카드, 모달, 오프캔버스 등
- **유틸리티 클래스**: margin, padding, 색상, 디스플레이 속성 등
- **JS 플러그인**: 모달, 툴팁, 팝오버, 드롭다운 등

---

### 레이아웃 & 그리드

- Bootstrap 그리드는 **12 컬럼 시스템** 기반. ([getbootstrap.com][3])
- 그리드는 Flexbox 기반으로 구현됨. ([getbootstrap.com][3])
- 컨테이너 종류:

  - `.container` (고정 너비 + 반응형)
  - `.container-fluid` (전체 폭)
  - `.container-{breakpoint}`: 특정 뷰포트 기준에서 고정 너비

- 그리드 사용 예:

  ```html
  <div class="container">
    <div class="row">
      <div class="col-sm-6 col-lg-4">컬럼1</div>
      <div class="col-sm-6 col-lg-8">컬럼2</div>
    </div>
  </div>
  ```

---

### 반응형 & 유틸리티

- Bootstrap은 **모바일 우선 (mobile-first)** 접근법
- 브레이크포인트 기반 클래스 사용 (`.col-sm-`, `.col-md-`, `.col-lg-` 등)
- 유틸리티 클래스가 풍부해서 복잡한 스타일도 클래스 하나로 조정 가능
- 반응형 유틸리티: `d-none d-sm-block` 같이 특정 화면에서 보이거나 숨기기 가능

---

### 컴포넌트 & JS 기능

- 버튼, 폼, 카드, 알림(toast), 모달, 드롭다운 등
- JS 플러그인을 사용할 땐 번들 스크립트를 포함해야 하고, 일부 기능은 초기화가 필요
- 툴팁/팝오버는 `data-bs-` 속성 + 자바스크립트 초기화 필요

---

## 4) Responsive Web (반응형 웹 디자인)

### 정의 & 목적

- 다양한 화면 크기(데스크탑, 태블릿, 모바일)에서 **보기 좋고 사용성 좋은 웹페이지 디자인**
- CSS와 HTML을 이용해 콘텐츠를 **재배치**, **숨기기**, **줄이기**, **크게 하기** 등을 수행 ([W3Schools][4])

---

### 주요 기술 / 접근법

- **유동적 레이아웃 (fluid layout)**: `%`, `vw` 등 상대 단위를 사용
- **미디어 쿼리 (Media Queries)**:

  ```css
  @media (max-width: 768px) {
    .sidebar {
      display: none;
    }
  }
  ```

- **반응형 이미지 / 미디어**: `max-width: 100%; height: auto;`
- **모바일 우선 설계**: 가장 작은 뷰포트 기준 스타일을 먼저 정의 → 점차 확장
- **콘텐츠 우선순위 조정**: 화면이 작아졌을 때 덜 중요한 내용은 숨기거나 간소화
- **브레이크포인트 결정 전략**: 디자인이 깨지는 지점을 기준으로 설정

---
