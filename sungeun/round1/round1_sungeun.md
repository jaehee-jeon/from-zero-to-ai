# 📈 Time Series 분석 발표 자료

> 발표자: 박성은 (sungeun) | 스터디: from-zero-to-ai | Round 1

---

## 목차

1. [날짜를 인덱스로 설정 vs 일반 컬럼](#1-날짜를-인덱스로-설정-vs-일반-컬럼)
2. [resample 에러 — 초보자가 놓치는 포인트](#2-resample-에러--초보자가-놓치는-포인트)
3. [rolling vs resample — 언제 뭘 써야 할까?](#3-rolling-vs-resample--언제-뭘-써야-할까)
4. [문제 8번 해설](#4-문제-8번-해설)

---

## 1. 날짜를 인덱스로 설정 vs 일반 컬럼

### 핵심 질문
> "날짜가 컬럼에 있는 것과 인덱스에 있는 것, 뭐가 다를까?"

### DataFrame 기본 구조 이해

```python
import pandas as pd

df = pd.DataFrame({
    'date': ['2024-01-01', '2024-01-02', '2024-01-03'],
    'sales': [100, 200, 150]
})

print(df)
```

```
   date        sales
0  2024-01-01  100
1  2024-01-02  200
2  2024-01-03  150
```

| 위치 | 이름 | 설명 |
|------|------|------|
| 맨 왼쪽 (0, 1, 2) | **인덱스** | pandas가 자동 생성하는 행 번호 |
| date, sales | **컬럼** | 딕셔너리 key가 컬럼명이 됨 |
| 날짜값, 숫자 | **데이터** | 딕셔너리 value 리스트 |

> 💡 `'date'`는 컬럼이고, `0, 1, 2`가 인덱스다. 헷갈리지 말 것!

---

### 날짜를 인덱스로 바꾸면?

```python
df['date'] = pd.to_datetime(df['date'])  # 문자열 → datetime 타입 변환
df = df.set_index('date')               # date 컬럼을 인덱스로 승격

print(df)
```

```
            sales
date
2024-01-01  100
2024-01-02  200
2024-01-03  150
```

### 왜 인덱스로 만들어야 할까?

날짜가 **인덱스**가 되면 pandas가 데이터를 인식하는 방식이 바뀐다.

- ❌ 날짜가 컬럼 → "그냥 문자열 데이터 중 하나"
- ✅ 날짜가 인덱스 → "이 데이터는 시간 기반이구나!" → 시계열 전용 기능 사용 가능

> **비유**: 도서관에서 책을 찾을 때 분류번호(인덱스)가 없으면 찾을 수 없듯이,
> resample도 "기준이 되는 시간 인덱스"가 없으면 작동하지 않는다.

---

## 2. resample 에러 — 초보자가 놓치는 포인트

### 에러 상황

```python
df = pd.DataFrame({
    'date': ['2024-01-01', '2024-01-02', '2024-01-03'],
    'sales': [100, 200, 150]
})

df.resample('W').sum()  # ❌ TypeError 발생!
```

### 초보자가 놓치는 2가지 포인트

#### ❌ 포인트 1. 날짜가 인덱스가 아니라 컬럼에 있음

pandas 입장에서 기준이 될 인덱스가 없는 상태.
"날짜 기준으로 묶어줘"라고 했는데 날짜가 그냥 데이터 중 하나인 상황.

```python
# 해결
df = df.set_index('date')
```

#### ❌ 포인트 2. 날짜 타입이 문자열(string)인 채로 있음

`'2024-01-01'` (문자열) ≠ `datetime` (날짜 타입)
인덱스로 설정해도 타입이 문자열이면 resample이 작동하지 않는다.

```python
# 해결
df['date'] = pd.to_datetime(df['date'])
```

### ✅ 올바른 순서

```python
# Step 1. 문자열 → datetime 변환 (반드시 먼저!)
df['date'] = pd.to_datetime(df['date'])

# Step 2. 인덱스로 설정
df = df.set_index('date')

# Step 3. 이제 resample 정상 작동
weekly = df.resample('W').sum()
print(weekly)
```

### resample 주요 주기 옵션

| 옵션 | 의미 |
|------|------|
| `'D'` | 일별 |
| `'W'` | 주별 |
| `'ME'` | 월별 (Month End) |
| `'QE'` | 분기별 |
| `'YE'` | 연별 |

> **핵심 요약**: resample 전에 반드시 확인할 것 — ① 날짜가 인덱스인가? ② 타입이 datetime인가?

---

## 3. rolling vs resample — 언제 뭘 써야 할까?

### 목적부터 구분하기

|  | `resample` | `rolling` (이동평균) |
|--|------------|----------------------|
| **목적** | 시간 단위를 바꾸기 (압축) | 추세/흐름을 부드럽게 보기 |
| **결과** | 행 수가 줄어듦 | 행 수 그대로 유지 |
| **핵심 질문** | "주간/월간으로 묶으면?" | "최근 N일 평균이 어떻게 흘러가?" |

---

### resample — 단위 변환이 목적

```python
# 일별 매출 → 월별 합계로 변환
monthly_sales = df.resample('ME').sum()
# 결과: 365행 → 12행으로 줄어듦
```

**실무 사용 예시**
- 일별 데이터 → 월별 리포트 생성
- 시간별 센서 데이터 → 일별 평균 집계
- 분 단위 주가 → 일 단위로 변환

---

### rolling — 추세 파악이 목적

```python
# 7일 이동평균 계산
df['rolling_7'] = df['sales'].rolling(window=7).mean()
# 결과: 행 수 그대로, 각 행에 최근 7일 평균값이 붙음
```

**실무 사용 예시**
- 주가 차트에서 단기 등락 무시하고 전체 흐름 파악
- 기온 데이터의 계절 트렌드 시각화
- 매출 데이터의 노이즈 제거

---

### 직관적 비유

> - **resample** = 일기를 **주간 일지로 요약**하는 것 → 정보 압축
> - **rolling** = 일기를 읽으면서 **최근 1주일 분위기를 계속 체크**하는 것 → 흐름 감지

---

### 실제 코드 비교

```python
import pandas as pd
import numpy as np

# 샘플 데이터 생성
dates = pd.date_range('2024-01-01', periods=30, freq='D')
df = pd.DataFrame({'sales': np.random.randint(100, 500, 30)}, index=dates)

# resample: 주별 합계 (30행 → 5행)
weekly = df.resample('W').sum()

# rolling: 7일 이동평균 (30행 그대로)
df['ma_7'] = df['sales'].rolling(window=7).mean()

print("=== resample 결과 (주별 합계) ===")
print(weekly)

print("\n=== rolling 결과 (7일 이동평균) ===")
print(df.head(10))
```

---

## 4. 문제 8번 해설

### 문제

```
일별 매출 데이터가 DataFrame df에 'date'(문자열)와 'sales'(숫자) 컬럼으로 있습니다.
'date'를 datetime으로 변환하고 인덱스로 설정한 뒤, 주간(weekly) 평균 매출을 구하려고 합니다.

df['date']를 datetime 인덱스로 변환하고, 주간 평균 매출을 계산하는 pandas 코드 2줄을 작성하세요.
```

### 정답 코드

```python
# 1줄: 문자열 → datetime 변환 + 인덱스 설정
df.index = pd.to_datetime(df['date'])

# 2줄: 주간 평균 매출 계산
weekly_avg = df['sales'].resample('W').mean()
```

### 또는 이렇게도 쓸 수 있어요

```python
# 방법 2 (더 명시적인 방식)
df['date'] = pd.to_datetime(df['date'])   # 1줄: datetime 변환
df = df.set_index('date')                 # 2줄: 인덱스 설정

weekly_avg = df.resample('W').mean()      # 주간 평균
```

### 왜 이 순서인가?

| 순서 | 코드 | 이유 |
|------|------|------|
| ① | `pd.to_datetime()` | 문자열을 날짜 타입으로 변환해야 resample이 인식 |
| ② | `set_index()` | 날짜를 인덱스로 올려야 시계열 기능 사용 가능 |
| ③ | `resample('W').mean()` | 주 단위로 묶어서 평균 계산 |

### 실행 결과 예시

```
date
2024-01-07    150.0   ← 1월 1일~7일 평균
2024-01-14    230.0   ← 1월 8일~14일 평균
2024-01-21    180.0   ← 1월 15일~21일 평균
...
```

> **핵심 포인트**: `pd.to_datetime()` → `set_index()` → `resample()` 이 3단계 순서를 기억하자!

---

## 전체 정리

| 개념 | 핵심 한 줄 요약 |
|------|----------------|
| 날짜 인덱스 | resample은 시간 인덱스가 있어야만 작동한다 |
| resample 에러 | ① datetime 변환 → ② 인덱스 설정 — 순서대로 확인 |
| rolling vs resample | resample은 단위 변환, rolling은 트렌드 파악 |
| 문제 8번 | `pd.to_datetime()` + `set_index()` + `resample('W').mean()` |

---

*발표 자료 끝 🎉 질문 환영합니다!*
