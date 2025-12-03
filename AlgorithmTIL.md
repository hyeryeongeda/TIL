## 🧱 1️⃣ Stack (스택)

### 📘 개념 정리

- **정의:** 데이터를 한쪽 방향으로만 넣고(LIFO: Last In First Out) 한쪽 방향으로만 꺼내는 구조.
- **자료 저장/삭제의 방향이 하나**만 존재한다는 점이 핵심.

| 연산        | 설명                         |
| ----------- | ---------------------------- |
| `push(x)`   | 데이터를 스택의 맨 위에 추가 |
| `pop()`     | 맨 위의 데이터를 꺼냄        |
| `peek()`    | 맨 위 데이터를 조회(삭제 X)  |
| `isEmpty()` | 스택이 비었는지 확인         |

### 🧩 스택의 실전 활용

| 분야      | 예시                       | 설명                                           |
| --------- | -------------------------- | ---------------------------------------------- |
| 수식 계산 | 후위 표기식 (Postfix) 계산 | 괄호와 연산자 우선순위 처리에 사용             |
| 백트래킹  | 미로 탐색 / 완전탐색       | 되돌아가기 기능은 스택 기반                    |
| 재귀 함수 | 함수 호출 기록 저장        | 함수 호출 시 Call Stack에 쌓였다가 복귀 시 Pop |
| 브라우저  | 뒤로가기 기능              | 방문 페이지 순서가 Stack 구조                  |

---

### 🧠 중위 → 후위 표기법 변환 예시

```python
def infix_to_postfix(expr):
    stack = []
    result = ''
    priority = {'*':2, '/':2, '+':1, '-':1, '(':0}
    for ch in expr:
        if ch.isalpha() or ch.isdigit():
            result += ch
        elif ch == '(':
            stack.append(ch)
        elif ch == ')':
            while stack and stack[-1] != '(':
                result += stack.pop()
            stack.pop()
        else:  # 연산자
            while stack and priority[stack[-1]] >= priority[ch]:
                result += stack.pop()
            stack.append(ch)
    while stack:
        result += stack.pop()
    return result
```

➡ **핵심 포인트:**

- 연산자 우선순위가 낮은 것은 나중에 처리.
- 괄호는 “스택 제어용 구분자” 역할.
- 후위 표기식은 컴퓨터가 **괄호 없이도 계산 순서를 정확히 알 수 있는 표현 방식**.

---

## 🧭 2️⃣ Queue (큐) & BFS (너비 우선 탐색)

### 📘 큐 기본 개념

- **FIFO (First In First Out)** 구조.
- 먼저 들어온 데이터가 먼저 처리.
- 실생활 비유: “은행 줄”, “프린터 대기열”, “콜센터 대기 큐”.

| 연산         | 설명                      |
| ------------ | ------------------------- |
| `enqueue(x)` | 큐의 뒤쪽에 데이터 추가   |
| `dequeue()`  | 큐의 앞쪽에서 데이터 제거 |
| `peek()`     | 큐의 맨 앞 데이터를 조회  |

**파이썬 구현**

```python
from collections import deque
q = deque()
q.append(1)
q.append(2)
print(q.popleft())  # 1
print(q[0])         # 2
```

---

### 🌐 BFS (Breadth-First Search)

**그래프 탐색 알고리즘 중 하나로**,
시작점에서 가까운 노드를 **레벨 단위로** 탐색하는 방식.

#### 🔹 작동 원리

1️⃣ 시작 노드를 큐에 넣고 방문 표시
2️⃣ 큐에서 하나씩 꺼내면서 인접 노드 탐색
3️⃣ 방문하지 않은 노드를 큐에 추가
4️⃣ 큐가 빌 때까지 반복

```python
from collections import deque

def bfs(start):
    visited = [False] * (N+1)
    q = deque([start])
    visited[start] = True

    while q:
        now = q.popleft()
        print(now, end=' ')
        for nxt in graph[now]:
            if not visited[nxt]:
                visited[nxt] = True
                q.append(nxt)
```

#### 🔹 BFS의 장점

- **최단 거리 보장** (가중치가 없는 그래프일 때)
- **레벨별 탐색** → Flood Fill, 미로 탐색, 영역 크기 계산 등에 응용

#### 🔹 핵심 키워드 요약

| 항목       | 내용                                       |
| ---------- | ------------------------------------------ |
| 자료구조   | Queue                                      |
| 방문 순서  | 시작점 → 가까운 노드 → 먼 노드             |
| 시간복잡도 | O(V + E)                                   |
| 주요 활용  | 최단 거리, 영역 탐색, 네트워크 연결성 판별 |

---

## 🌳 3️⃣ Tree & Graph (그래프 구조)

### 📘 트리(Tree)란?

- **사이클이 없는 그래프**
- 루트(root)에서 시작해 자식 노드로 뻗어나가는 계층적 구조
- **노드(Node)**와 **간선(Edge)**로 구성

| 트리 유형                | 설명                                               |
| ------------------------ | -------------------------------------------------- |
| **이진 트리**            | 모든 노드의 자식이 최대 2개                        |
| **이진 탐색 트리 (BST)** | 왼쪽 < 루트 < 오른쪽                               |
| **완전 이진 트리**       | 마지막 레벨을 제외하고 노드가 꽉 차 있음           |
| **힙 트리 (Heap)**       | 부모가 자식보다 항상 작거나(최소 힙) 크다(최대 힙) |

---

### 🧩 그래프(Graph)

그래프는 **정점(Vertex)**과 **간선(Edge)**의 집합.

| 그래프 유형              | 설명                                              |
| ------------------------ | ------------------------------------------------- |
| 무방향 그래프            | 간선의 방향이 없음 (양방향 이동 가능)             |
| 방향 그래프              | 간선에 방향 존재                                  |
| 가중치 그래프            | 간선에 비용(weight) 부여                          |
| DAG (유향 비순환 그래프) | 사이클 없는 방향 그래프 (Topological Sort에 사용) |

---

### 🧮 그래프 표현 방법

| 표현 방식                        | 특징                           | 복잡도                 |
| -------------------------------- | ------------------------------ | ---------------------- |
| **인접 행렬 (Adjacency Matrix)** | NxN 2차원 배열, 연결 유무 표시 | 공간 O(N²), 조회 O(1)  |
| **인접 리스트 (Adjacency List)** | 각 노드별 연결 리스트 보유     | 공간 O(N+E), 조회 O(k) |

```python
# 인접 리스트 예시
graph = [[] for _ in range(6)]
graph[1].append(2)
graph[1].append(3)
graph[2].append(4)
```

---

## ⚙️ 4️⃣ Heap (힙)과 우선순위 큐

### 📘 기본 개념

- **완전 이진트리 기반의 우선순위 큐**
- 항상 **부모 노드 ≤ 자식 노드 (Min Heap)** 조건 유지
- 루트에 **최솟값(또는 최댓값)** 이 존재.

### 🧩 Python `heapq` 사용법

```python
import heapq

heap = []
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 2)
print(heapq.heappop(heap))  # 1
```

| 연산        | 시간복잡도 |
| ----------- | ---------- |
| 삽입 (push) | O(logN)    |
| 삭제 (pop)  | O(logN)    |
| 조회        | O(1)       |

📍 **최대 힙 구현 팁**
`heapq`는 **기본적으로 최소 힙** →
`heapq.heappush(heap, -x)` 로 넣고
`-heapq.heappop(heap)` 으로 꺼내면 된다.

---

### 🧠 힙의 응용

- 다익스트라 (최단경로)
- 힙 정렬 (Heap Sort)
- 우선순위 처리 시스템 (스케줄러, 대기열)
- 실시간 데이터 스트림 (상위 K개 요소 유지)

---

## 🔍 5️⃣ 탐색 기법 — Sliding Window & Two Pointer

### 📘 Sliding Window (슬라이딩 윈도우)

- 연속된 구간의 합, 평균, 최댓값을 빠르게 구할 때 사용
- **이전 윈도우에서 빠진 값 빼고, 새로 들어온 값 더하기**

```python
arr = [1, 2, 3, 4, 5]
window_sum = sum(arr[:3])
max_sum = window_sum

for i in range(3, len(arr)):
    window_sum += arr[i] - arr[i-3]
    max_sum = max(max_sum, window_sum)
print(max_sum)
```

✅ **시간복잡도:** O(N)

---

### 📘 Two Pointer (투 포인터)

- 두 개의 포인터를 이동시키며 조건을 만족하는 구간을 찾는 기법.
- **정렬된 배열**, **연속된 구간** 문제에서 효율적.

```python
arr = [1, 2, 3, 4, 5]
target = 7
left, right, total = 0, 0, 0
count = 0

while right < len(arr):
    total += arr[right]
    while total > target:
        total -= arr[left]
        left += 1
    if total == target:
        count += 1
    right += 1
print(count)
```

💡 **핵심 포인트**

- 슬라이딩 윈도우는 “고정 길이 구간”
- 투 포인터는 “조건 만족 구간(가변 길이)”

---

## ✅ 정리

| 주제        | 핵심 키워드            | 한 줄 정리                         |
| ----------- | ---------------------- | ---------------------------------- |
| Stack       | LIFO / 재귀 / 백트래킹 | 재귀 함수의 원리는 Stack 구조다.   |
| Queue & BFS | FIFO / 최단거리        | BFS는 가까운 노드부터 탐색한다.    |
| Tree        | 계층적 비순환 구조     | 트리는 그래프의 특수한 형태다.     |
| Heap        | 최소 힙 / 최대 힙      | 부모 ≤ 자식, 우선순위 큐 구현 가능 |
| Two Pointer | 연속 구간 탐색         | 구간합, 부분합, 조건 탐색에 효율적 |

---

📘 **실전에서의 연결**

- **Stack → DFS / 백트래킹**
- **Queue → BFS / 시뮬레이션**
- **Tree → 탐색 & 정렬의 기반**
- **Heap → 우선순위 최적화 문제**
- **Two Pointer → 구간/합 문제 최적화**

# 9월 알고리즘 수업

## ⚖️ 1️⃣ 알고리즘 복잡도와 Big-O

### 📘 알고리즘의 효율성

- **시간복잡도(Time Complexity)**: 입력 크기(N)에 따라 알고리즘이 얼마나 오래 걸리는지를 수학적으로 표현.
- **공간복잡도(Space Complexity)**: 알고리즘이 사용하는 메모리의 크기.

---

### 📊 Big-O 표기법

| 표기     | 설명      | 예시 코드        | 시간(대략)   |
| -------- | --------- | ---------------- | ------------ |
| O(1)     | 상수 시간 | 배열 인덱스 접근 | 즉시         |
| O(logN)  | 로그 시간 | 이진 탐색        | 1만 → 14회   |
| O(N)     | 선형      | 1중 for문        | 1만 → 1만회  |
| O(NlogN) | 준선형    | 병합 정렬        | 1만 → 14만회 |
| O(N²)    | 이중 반복 | 버블 정렬        | 1만 → 1억    |
| O(2ⁿ)    | 지수      | 부분집합 탐색    | n=20 → 백만  |
| O(N!)    | 순열      | 완전 탐색        | n=10 → 362만 |

💡 **실전 기준**

> 1초 ≈ 2천만 번 연산 가능
> → `O(N²)`은 N ≤ 5000,
> → `O(NlogN)`은 N ≤ 100만 정도까지 안전.

---

### 🧠 복잡도 최적화 사고법

1️⃣ 중첩 반복문을 줄인다
2️⃣ 불필요한 조건문·리스트 탐색 제거
3️⃣ DP / 누적합 / 정렬 후 탐색으로 치환
4️⃣ 정렬은 필수 도구 — 이후 탐색이나 포인터 기반 최적화 가능

---

## 🔢 2️⃣ 진법 변환 & 비트 연산

### 💡 진법 변환의 원리

- **10진수 → 2진수:** 나머지를 역순으로 모으면 된다.
- **2진수 → 10진수:** 각 자리의 값을 2ⁿ으로 곱해 더한다.

```python
def decimal_to_binary(n):
    res = ''
    while n > 0:
        res = str(n % 2) + res
        n //= 2
    return res
```

📍 **내장함수 요약**

| 변환    | 함수            |
| ------- | --------------- |
| 10 → 2  | `bin(n)`        |
| 10 → 16 | `hex(n)`        |
| 2 → 10  | `int('101', 2)` |
| 16 → 10 | `int('FF', 16)` |

---

### ⚙️ 비트 연산자 핵심 정리

| 연산자 | 의미        | 설명             |                  |
| ------ | ----------- | ---------------- | ---------------- |
| `&`    | AND         | 둘 다 1일 때만 1 |                  |
| `      | `           | OR               | 둘 중 하나라도 1 |
| `^`    | XOR         | 서로 다르면 1    |                  |
| `<<`   | Left shift  | 2ⁿ배             |                  |
| `>>`   | Right shift | 2ⁿ 나눔          |                  |

```python
# 예시
a = 5  # 0101
b = 3  # 0011
print(a & b)  # 0001 -> 1
print(a | b)  # 0111 -> 7
print(a ^ b)  # 0110 -> 6
print(a << 1) # 1010 -> 10
```

💎 **비트 연산의 활용**

- 부분집합 생성 (1 << n)
- 특정 비트 확인 `num & (1 << i)`
- 빠른 곱셈/나눗셈 (2의 제곱배 단위)
- XOR 암호화: `A ^ B ^ B = A`

---

### ⚙️ 컴퓨터의 수 표현

- **음수:** 2의 보수(Two’s Complement)
  1️⃣ 비트를 뒤집고
  2️⃣ +1을 더함
- **실수:** IEEE754 부동소수점 (근사값 저장 → 오차 발생 가능)

---

## 🔁 3️⃣ 완전 탐색 (Brute Force) & 재귀

### 📘 기본 개념

- 가능한 모든 경우를 시도하는 탐색 방법
- 반복문으로 구현 가능하지만, **재귀**가 더 일반적

---

### 🧩 재귀 함수 기본 구조

```python
def recur(level):
    if level == N:
        print(path)
        return
    for i in range(1, 4):
        path.append(i)
        recur(level + 1)
        path.pop()
```

| 개념               | 설명                                  |
| ------------------ | ------------------------------------- |
| **Base Case**      | 재귀 종료 조건                        |
| **Recursive Case** | 자신을 다시 호출                      |
| **Stack 구조**     | LIFO (나중에 호출된 함수가 먼저 종료) |

---

### ⚙️ 재귀의 핵심 2가지

1️⃣ **값만 복사됨**
→ 함수 내부에서 바꿔도 원본엔 영향 없음

2️⃣ **함수는 돌아온다**
→ 모든 함수는 자신을 호출한 위치로 돌아옴

💡 **스택과 재귀는 구조적으로 동일**

---

## 🧩 4️⃣ 조합론적 탐색 (Subset · Combination · Permutation)

### 🌱 부분집합 (Subset)

원소 n개 → 부분집합 개수는 **2ⁿ개**

```python
arr = [1,2,3]
for i in range(1 << len(arr)):
    subset = [arr[j] for j in range(len(arr)) if i & (1 << j)]
    print(subset)
```

---

### 🌼 조합 (Combination)

서로 다른 n개 중 r개를 **순서 없이** 고름.

```python
arr = ['A','B','C','D']
def comb(start, path):
    if len(path) == 2:
        print(path)
        return
    for i in range(start, len(arr)):
        comb(i+1, path+[arr[i]])
comb(0, [])
```

---

### 🌸 중복 조합 (Combination with repetition)

```python
def comb_dup(start, path):
    if len(path) == 2:
        print(path)
        return
    for i in range(start, len(arr)):
        comb_dup(i, path+[arr[i]])
comb_dup(0, [])
```

---

### 🌻 순열 (Permutation)

순서 O, 중복 X → visited 배열 사용

```python
visited = [False]*3
def perm(path):
    if len(path) == 3:
        print(path); return
    for i in range(3):
        if not visited[i]:
            visited[i] = True
            perm(path+[i])
            visited[i] = False
```

---

### 🌼 중복 순열

순서 O, 중복 O → 모든 후보 반복 가능

```python
def perm_dup(path):
    if len(path)==2:
        print(path); return
    for i in range(3):
        perm_dup(path+[i])
```

---

### 💡 핵심 비교표

| 구분      | 순서 | 중복 | 핵심 로직     |
| --------- | ---- | ---- | ------------- |
| 부분집합  | X    | -    | `1<<n` / 재귀 |
| 조합      | X    | X    | `start=i+1`   |
| 중복 조합 | X    | O    | `start=i`     |
| 순열      | O    | X    | visited[]     |
| 중복 순열 | O    | O    | for문 전체    |

---

## 💰 5️⃣ 탐욕 알고리즘 (Greedy Algorithm)

### 📘 개념 요약

> “현재 순간에 가장 좋아 보이는 선택을 반복하여 전체 최적해를 구하는 알고리즘.”

---

### ✅ 탐욕 알고리즘이 가능한 조건

1️⃣ **탐욕적 선택 속성 (Greedy Choice Property)**

> 각 단계에서의 최선의 선택이 이후 결과에 영향을 주지 않아야 한다.

2️⃣ **최적 부분 구조 (Optimal Substructure)**

> 전체 최적해가 부분 문제의 최적해들로 구성되어야 한다.

---

### 💡 예시: 동전 거스름돈 문제

```python
coins = [500, 100, 50, 10]
target = 1260
cnt = 0
for c in coins:
    cnt += target // c
    target %= c
print(cnt)
```

💬 단, 모든 경우에서 최적해가 되려면 **동전 단위가 배수 관계**여야 한다.

---

## ⚙️ 6️⃣ 분할 정복 (Divide and Conquer)

### 📘 구조

1️⃣ **Divide (분할)**: 문제를 더 작은 단위로 쪼갠다.
2️⃣ **Conquer (정복)**: 각각 해결한다.
3️⃣ **Combine (통합)**: 결과를 합쳐서 원래 문제의 답을 구한다.

---

### 📍 대표 알고리즘

| 알고리즘      | 핵심 아이디어                  | 시간복잡도    |
| ------------- | ------------------------------ | ------------- |
| **이진 탐색** | 정렬된 배열의 중간값 기준 탐색 | O(logN)       |
| **병합 정렬** | 분할 후 병합하면서 정렬        | O(NlogN)      |
| **퀵 정렬**   | 피벗 기준 좌/우로 분할         | 평균 O(NlogN) |

---

### 🧭 이진 탐색 코드

```python
arr = [2,4,7,9,11,19,23]
def binary_search(target):
    l, r = 0, len(arr)-1
    while l <= r:
        mid = (l+r)//2
        if arr[mid]==target:
            return mid
        elif target<arr[mid]:
            r=mid-1
        else:
            l=mid+1
    return -1
```

---

### 🧩 병합 정렬 (Merge Sort)

```python
def merge_sort(arr):
    if len(arr)<=1: return arr
    mid=len(arr)//2
    left=merge_sort(arr[:mid])
    right=merge_sort(arr[mid:])
    return merge(left,right)

def merge(left,right):
    res=[]; i=j=0
    while i<len(left) and j<len(right):
        if left[i]<right[j]:
            res.append(left[i]); i+=1
        else:
            res.append(right[j]); j+=1
    res.extend(left[i:]); res.extend(right[j:])
    return res
```

---

### ⚡ 퀵 정렬 (Quick Sort)

```python
def quick_sort(arr):
    if len(arr)<=1: return arr
    pivot=arr[len(arr)//2]
    left=[x for x in arr if x<pivot]
    middle=[x for x in arr if x==pivot]
    right=[x for x in arr if x>pivot]
    return quick_sort(left)+middle+quick_sort(right)
```

📎 **Quick Sort**

- 평균 O(NlogN)
- 피벗이 극단적으로 편향될 경우 최악 O(N²)

---

## 🧭 7️⃣ 그래프 탐색 & Union-Find

### 📘 DFS / BFS 정리 복습

- **DFS:** 깊이 우선 탐색 (Stack / 재귀)
- **BFS:** 너비 우선 탐색 (Queue)

---

### ⚙️ 서로소 집합 (Union-Find)

#### 🧩 개념

- 여러 원소를 **서로 겹치지 않는 그룹(집합)** 으로 관리
- “두 원소가 같은 그룹인가?”를 빠르게 판별

#### 🧠 핵심 연산

1️⃣ `find(x)` : x의 대표자(root)를 찾음
2️⃣ `union(a,b)` : 두 집합을 합침

```python
def find(x):
    if parent[x]!=x:
        parent[x]=find(parent[x])  # 경로 압축
    return parent[x]

def union(a,b):
    a=find(a); b=find(b)
    if a==b: return
    if rank[a]<rank[b]:
        parent[a]=b
    else:
        parent[b]=a
        if rank[a]==rank[b]:
            rank[a]+=1
```

---

## 🌉 8️⃣ 최소 신장 트리 (MST)

### 📘 개념

- 모든 노드를 **최소 비용으로 연결**하는 트리
- 간선의 개수 = 노드 수 - 1
- 사이클 없음

---

### 📗 Kruskal 알고리즘 (간선 중심)

1️⃣ 간선을 가중치 기준으로 정렬
2️⃣ 작은 간선부터 순서대로 연결
3️⃣ 사이클 발생 시 스킵 (Union-Find로 판별)

```python
edges.sort(key=lambda x:x[2])
for a,b,w in edges:
    if find(a)!=find(b):
        union(a,b)
        total+=w
```

---

### 📘 Prim 알고리즘 (정점 중심)

1️⃣ 임의의 시작점 선택
2️⃣ 인접한 간선 중 최소비용 간선 선택
3️⃣ 모든 정점 연결될 때까지 반복 (우선순위 큐 사용)

```python
import heapq
def prim(start):
    visited=[0]*(V+1)
    pq=[(0,start)]
    total=0
    while pq:
        w,now=heapq.heappop(pq)
        if visited[now]: continue
        visited[now]=1
        total+=w
        for cost,nxt in graph[now]:
            if not visited[nxt]:
                heapq.heappush(pq,(cost,nxt))
```

---

## 🚗 9️⃣ 다익스트라 알고리즘 (Dijkstra)

### 📘 개념

- 가중치가 있는 그래프에서 한 시작점 → 모든 노드까지의 **최단 거리**
- **음수 간선 X** 조건 필요
- 우선순위 큐(힙) 기반 구현

---

### 🧠 구현

```python
import heapq
def dijkstra(start):
    pq=[(0,start)]
    dist=[float('inf')]*(N+1)
    dist[start]=0
    while pq:
        d,u=heapq.heappop(pq)
        if d>dist[u]: continue
        for w,v in graph[u]:
            if dist[v]>d+w:
                dist[v]=d+w
                heapq.heappush(pq,(dist[v],v))
```

💡 **시간 복잡도:** O(E logV)

---

## ✅ 9월 핵심 정리

| 분류       | 키워드            | 핵심 문장                              |
| ---------- | ----------------- | -------------------------------------- |
| 복잡도     | Big-O             | 효율성은 ‘최악의 경우’를 기준으로 측정 |
| 비트연산   | 비트마스크        | 부분집합, 상태 관리에 최적             |
| 완전탐색   | 재귀              | 모든 경우의 수 탐색 구조               |
| 조합론     | start 인덱스 제어 | i+1 → 중복X, i → 중복O                 |
| 그리디     | 탐욕적 선택 속성  | 부분해의 최적이 전체 해를 구성         |
| 분할정복   | Divide-Conquer    | 문제를 쪼개서 정복하고 합친다          |
| 그래프     | DFS/BFS           | 경로·최단거리 탐색의 기본              |
| Union-Find | 집합 판별         | MST, 그룹화, 사이클 제거               |
| MST        | Kruskal / Prim    | 간선 or 정점 중심 최소 연결            |
| 다익스트라 | 힙 기반 최단거리  | 가중 그래프의 핵심 알고리즘            |

---

## 💎 9월 학습 인사이트

> 9월은 “자료구조를 활용해 사고하는 능력”의 단계였다.
> 8월의 Stack·Queue가 문제를 ‘저장하고 꺼내는’ 수준이었다면,
> 9월은 그 위에서 ‘판단하고 줄이는’ 전략으로 확장되었다.

🧩 **사고력 변화**

- 단순히 “탐색하는 코드”에서
- “탐색 방식을 설계하는 사고”로 전환됨

🧠 **핵심 관점**
1️⃣ **탐색 구조를 시각화하라** — BFS는 레벨, DFS는 경로
2️⃣ **중복을 제거하라** — visited, pruning, set
3️⃣ **시간복잡도를 인식하라** — 효율성은 습관이다
