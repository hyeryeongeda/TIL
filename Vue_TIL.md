# ✅ **1\. Vue 기본 명령어 (Vite 기준)**

### **1) 새 프로젝트 생성**

```
npm create vite@latest
```

→ 프로젝트 이름 입력  
→ Framework 선택: **Vue**  
→ Variant 선택: **JavaScript** 또는 **TypeScript**

---

### **2) 프로젝트 실행**

```
npm install
npm run dev
```

---

### **3) Vue 파일 기본 구조 (`.vue`)**

```
<template>
<div>여기에 화면</div>
</template>

<script setup>
// JS 영역
</script>

<style scoped>
/* 컴포넌트별 스타일 */
</style>
```

---

# ✅ **2\. Vue 기본 문법 정리**

---

# ⚡ 2-1. 데이터 표시 (Mustache 문법)

```
<p>{{ message }}</p>
```

```
<script setup>const message = "안녕 Vue!"</script>
```

---

# ⚡ 2-2. v-bind (속성 바인딩)

```
<img :src="imgUrl">
<button :disabled="isDisabled">버튼</button>
```

짧게 `:` 로 쓸 수 있음.

---

# ⚡ 2-3. v-on (이벤트)

```
<button v-on:click="count++">클릭</button>
```

축약형:

```
<button @click="count++">클릭</button>
```

메서드 호출도 가능:

```
<button @click="increment">증가</button>

<script setup>
function increment() {
console.log("증가!");
}
</script>
```

---

# ⚡ 2-4. v-model (양방향 바인딩)

```
<input v-model="text" />

<p>{{ text }}</p>

<script setup>
import { ref } from "vue";
const text = ref("");
</script>
```

---

# ⚡ 2-5. 조건문 (v-if / v-else-if / v-else)

```
<p v-if="age >= 20">성인</p>
<p v-else-if="age >= 13">청소년</p>
<p v-else>어린이</p>
```

---

# ⚡ 2-6. 반복문 (v-for)

```
<li v-for="(user, index) in users" :key="index">
{{ user.name }} / {{ user.age }}
</li>
```

---

# ⚡ 2-7. props (부모 → 자식 데이터 전달)

### 부모

```
<ChildComp :user="item" />
```

### 자식

```
<script setup>
defineProps({
user: Object,
});
</script>
```

---

# ⚡ 2-8. emit (자식 → 부모 이벤트 전달)

### 자식

```
<script setup>
const emit = defineEmits(["send"]);
emit("send", "데이터 보내기");
</script>
```

### 부모

```
<ChildComp @send="getData" />

<script setup>
function getData(msg) {
console.log(msg);
}
</script>
```

---

# ⚡ 2-9. computed (계산된 값)

```
<script setup>
import { ref, computed } from "vue";

const a = ref(10);
const b = ref(20);

const sum = computed(() => a.value + b.value);
</script>

<template>
<p>{{ sum }}</p>
</template>
```

---

# ⚡ 2-10. watch (값 변경 감지)

```
<script setup>
import { ref, watch } from "vue";

const count = ref(0);

watch(count, (newVal, oldVal) => {
console.log("변화:", oldVal, "→", newVal);
});
</script>
```

---

# ⚡ 2-11. 컴포넌트 등록

### 1) `<script setup>`에서는 import 만 하면 자동 등록됨

```
<script setup>
import MyComp from "./MyComp.vue";
</script>

<template>
<MyComp />
</template>
```

---

# ⚡ 2-12. style scoped

컴포넌트 안에서만 스타일 적용되도록 제한

```
<style scoped>
p {
color: red;
}
</style>
```

---

# 📌 **Vue가 제공하는 주요 디렉티브 정리표**

| 문법 | 설명 |
| --- | --- |
| `v-bind:` | HTML 속성에 JS 값 바인딩 |
| `v-on:` | 이벤트 감지 |
| `v-model` | 양방향 바인딩 |
| `v-if` | 조건부 렌더링 |
| `v-else-if` | 조건부 렌더링 |
| `v-else` | 조건부 렌더링 |
| `v-for` | 리스트 렌더링 |
| `v-show` | display 토글 (DOM은 남아 있음) |
| `v-html` | HTML 문자열 렌더링 |

---

# 📌 **Vue 프로젝트 구조 (Vite 기준)**

```
src/
├─ assets/
├─ components/
│ └─ HelloWorld.vue
├─ App.vue
└─ main.js
```