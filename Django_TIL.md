# 🧠 Django 핵심 개념 총정리

## 1️⃣ Django의 기본 구조와 원리

### 📌 웹의 기본 원리

- 웹은 **요청(Request)** 과 **응답(Response)** 으로 작동한다.
- **클라이언트**: 요청을 보내는 쪽 (예: 브라우저)
- **서버**: 요청을 받아 데이터를 처리하고 응답하는 쪽

---

### 📌 Django란?

- 파이썬 기반의 **웹 프레임워크**
- 서버를 빠르고 효율적으로 만들 수 있게 도와줌
- **MTV 패턴 (Model–Template–View)** 으로 작동

| 구성요소 | 역할 |
| --- | --- |
| Model | 데이터(DB) 구조 정의 |
| Template | 사용자에게 보여질 HTML 화면 |
| View | 요청 처리 및 로직 담당 |

---

### 📌 가상환경 (venv)

- 프로젝트별로 독립된 개발 환경을 만들기 위함
- 명령어
    
    ```bash
    python -m venv venv
    source venv/Scripts/activate
    pip freeze > requirements.txt
    pip install -r requirements.txt
    
    ```
    
- ⚠️ `venv/` 폴더는 `.gitignore`에 반드시 포함해야 함

---

## 2️⃣ Template & URL

### 📌 DTL (Django Template Language)

- 파이썬 데이터와 HTML을 분리하기 위한 템플릿 시스템
- 주요 문법
    
    ```html
    {{ variable }}           <!-- 변수 출력 -->
    {{ variable|filter }}    <!-- 출력 전에 형식 변경 -->
    {% tag %} ... {% endtag %} <!-- 제어문 (if, for 등) -->
    {# comment #}            <!-- 주석 -->
    
    ```
    

---

### 📌 템플릿 상속 (Template Inheritance)

- 반복되는 HTML 구조를 재사용하기 위한 기능

**예시**

```html
{% extends 'base.html' %}
{% block content %}
  <h1>게시글 목록</h1>
{% endblock %}

```

---

### 📌 HTML Form & Method

| 구분 | GET | POST |
| --- | --- | --- |
| 데이터 전송 방식 | URL 뒤에 붙음 (`?key=value`) | 요청 본문(body)에 숨김 |
| 사용 목적 | 조회(Read) | 생성/수정/삭제(Create/Update/Delete) |
| 특징 | 캐싱 가능, 데이터 노출 | 보안성 높음, 캐싱 불가 |
- `<form action="..." method="POST">`
- `<input name="title">` → name은 key 역할

---

### 📌 Variable Routing (동적 URL)

- URL 일부를 변수로 사용
- 예:
    
    ```python
    path('articles/<int:pk>/', views.detail)
    
    ```
    
    → views.py에서 `def detail(request, pk):`로 접근 가능
    

---

## 3️⃣ Model & ORM

### 📌 Model의 개념

- DB 테이블을 코드로 정의하는 “설계도”
- 클래스 기반
    
    ```python
    class Article(models.Model):
        title = models.CharField(max_length=20)
        content = models.TextField()
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True)
    
    ```
    

---

### 📌 주요 필드

| 타입 | 설명 |
| --- | --- |
| CharField(max_length=n) | 짧은 문자열 |
| TextField | 긴 문자열 |
| DateTimeField | 날짜/시간 저장 |
| BooleanField | True/False |

**옵션**

- `null=True`: DB에 NULL 허용
- `blank=True`: 입력 폼에서 비워둘 수 있음
- `auto_now_add=True`: 생성 시 자동 날짜
- `auto_now=True`: 수정 시 자동 날짜

---

### 📌 Migrations

- Model 변경사항을 DB에 반영하는 과정

```bash
python manage.py makemigrations  # 설계도 생성
python manage.py migrate         # DB에 반영

```

---

### 📌 ORM (Object Relational Mapping)

- 파이썬 객체 ↔ SQL 변환 “통역사” 역할

| 기능 | 코드 예시 |
| --- | --- |
| 전체 조회 | `Article.objects.all()` |
| 조건 조회 | `Article.objects.filter(title='first')` |
| 단일 조회 | `Article.objects.get(pk=1)` |
| 생성 | `Article(title='a', content='b').save()` |
| 수정 | `article.title = 'new'; article.save()` |
| 삭제 | `article.delete()` |

---

### 📌 Admin 사이트

- 관리자 페이지에서 CRUD 수행 가능
- 등록 방법
    
    ```python
    from django.contrib import admin
    from .models import Article
    admin.site.register(Article)
    
    ```
    
- 접속: `localhost:8000/admin`

---

## 4️⃣ View & CRUD 흐름

### 📌 View의 역할

- 요청을 받고 응답을 반환하는 **핵심 로직**
- `urls.py`에서 연결

```python
path('articles/', views.index, name='index')

```

---

### 📌 CRUD 기본 흐름

| 기능 | HTTP Method | View 동작 | 비고 |
| --- | --- | --- | --- |
| Create | POST | DB에 저장 후 redirect | form + csrf_token 필수 |
| Read | GET | 데이터 조회 후 render | 안전한 요청 |
| Update | POST | 수정 후 redirect | 기존 데이터 유지 |
| Delete | POST | 데이터 삭제 후 redirect | 반드시 POST 사용 |

---

### 📌 CSRF 보호

- Cross Site Request Forgery(사이트 간 요청 위조) 방지
- POST 요청 시 `<form>` 내부에 반드시 `{% csrf_token %}` 추가

---

### 📌 Post–Redirect–Get 패턴

1. 사용자가 POST로 데이터 전송
2. 서버가 DB 반영 후 redirect 응답
3. 브라우저가 새 GET 요청으로 안전하게 페이지 표시
    
    → 새로고침 시 중복 등록 방지
    

---

## 5️⃣ Authentication (Cookie / Session / Login)

### 📌 HTTP의 한계

- HTTP는 **무상태(stateless)**
- 로그인 상태 유지 불가능 → 해결: 쿠키 & 세션

---

### 📌 Cookie

- 브라우저에 저장되는 key-value 데이터
- 예시: 로그인 유지, 장바구니
- `Set-Cookie` 헤더로 저장, 이후 요청마다 전송됨
- 보안 취약 → 민감한 정보 저장 금지

---

### 📌 Session

- 서버가 관리하는 로그인 정보
- 클라이언트는 세션 ID만 쿠키에 보관
- 실제 인증 정보는 서버에 저장되어 더 안전

---

### 📌 Django Authentication System

- 로그인, 로그아웃, 회원가입 기능을 제공하는 내장 시스템

| 주요 구성 | 설명 |
| --- | --- |
| `User` 모델 | 사용자 계정 정보 |
| `AuthenticationForm` | 로그인 폼 |
| `UserCreationForm` | 회원가입 폼 |
| `auth_login(request, user)` | 세션 생성 |
| `auth_logout(request)` | 세션 종료 |

---

### 📌 Custom User Model

- 기본 `User` 모델은 한계가 있으므로 직접 정의 가능
    
    ```python
    from django.contrib.auth.models import AbstractUser
    class User(AbstractUser):
        pass
    
    ```
    
- `settings.py`에
    
    ```python
    AUTH_USER_MODEL = 'accounts.User'
    
    ```
    

---

## ✅ Django 핵심 요약표

| 구분 | 키워드 | 핵심 요약 |
| --- | --- | --- |
| 기본 원리 | 요청–응답 | Client → URL → View → Template |
| 구조 | MTV 패턴 | Model, Template, View |
| 템플릿 | DTL 문법 | {{ 변수 }}, {% 태그 %}, {# 주석 #} |
| 폼 | GET/POST | 조회 vs 데이터 변경 |
| 모델 | ORM | 객체 기반 DB 제어 |
| 마이그레이션 | makemigrations → migrate | 설계도 반영 |
| View | render(), redirect() | 화면 출력 / URL 이동 |
| 보안 | {% csrf_token %} | POST 위조 방지 |
| 로그인 | 세션 관리 | auth_login(), auth_logout() |
| 사용자 | Custom User | AbstractUser 상속 |

---