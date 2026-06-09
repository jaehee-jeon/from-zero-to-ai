# 🐍 전처리 모듈화 — 발표 노트
> Python 1주차 | 10분 발표용 | 완전 초보자 눈높이

---

## ① self vs 일반 함수 — 뭐가 다를까?

### 🔑 한 줄 핵심
> **self = "나(이 객체) 자신"을 가리키는 이름표**

### 📦 클래스가 뭔지부터 (초초초 간단 버전)
클래스 = **붕어빵 틀**, 객체 = **붕어빵**
틀로 붕어빵을 찍으면, 각 붕어빵은 메모리 어딘가에 저장됨.
`self`는 "지금 이 붕어빵"을 가리키는 포인터.

---

### 🆚 비교: 클래스 안 메서드(self) vs 클래스 밖 독립 함수

```python
# ✅ 클래스 안 메서드 — self 있음
class Preprocessor:
    def __init__(self): # def = definition의 약자, def가 있을 경우 처음 코드를 읽을 때 그 def가 정의된 코드 줄들은 건너 뛰고 읽게 됨
    # __ : 스페셜메서드 or 매직 메서드 직접 실행 버튼을 누르지 않아도, 파이썬 시스템이 특정 상황(예: 객체가 처음 생성될 때)에 자동으로 알아서 실행해 주는 특수 명령어라는 것을 표시하는 파이썬만의 고유한 약속 기호
    # init은 initialize(초기화하다)의 약자
        self.count = 0          # 객체가 상태(기억)를 가짐

    def clean_text(self, data): # self = 이 객체 자신
        self.count += 1         # 몇 번 호출됐는지 기억! , +=의 의미는 count + 1 이라는 의미
        return data.strip() # 코드들이 위에서 아래로 실행되다가 함수 안에서 return을 만나는 순간, 컴퓨터는 하던 일을 즉시 멈추고 그 뒤에 있는 결과물을 품에 안고 함수 바깥 세상으로 탈출

prep = Preprocessor()
prep.clean_text(' hello ')      # → 'hello', count = 1
prep.clean_text(' world ')      # → 'world', count = 2
print(prep.count)               # → 2  ✅ 상태를 기억함


# ✅ 클래스 밖 독립 함수 — self 없음
def clean_text(data):
    return data.strip()

clean_text(' hello ')           # → 'hello'
# 상태 기억 없음. 매번 새로 시작.
```

### 💡 메모리로 이해하기
```
prep = Preprocessor() 실행하면...

[힙 메모리]
┌──────────────────────┐
│   Preprocessor 객체  │  ← prep 이 여기를 가리킴
│   count = 0          │
│   주소: 0x7f3a...    │  ← self 가 바로 이 주소!
└──────────────────────┘

prep.clean_text(' hello ') 호출하면 Python이 내부에서:
→ Preprocessor.clean_text(prep, ' hello ') 로 자동 변환!
```

### 📌 언제 뭘 쓸까?
| 상황 | 선택 |
|---|---|
| 처리 횟수, 설정값 기억 필요 | 클래스 안 메서드 (self) |
| 그냥 변환만 하면 됨 | 클래스 밖 독립 함수 (def) |

---

## ② 초보자 실수 — TypeError 왜 나는 거야?

### ❌ 문제 코드
```python
class Preprocessor:
    def clean_text(data):    # ← self 없음!
        return data.strip()

prep = Preprocessor()
prep.clean_text(' hello ')   # → 💥 TypeError!
```

### 🚨 에러 메시지
```
TypeError: clean_text() takes 1 positional argument but 2 were given
```
> "나는 1개만 넣었는데 왜 2개라고 해?!" → Python이 **몰래 1개 더 추가**하기 때문!

### 🔍 Python 내부 동작 — 4단계
```
Step 1.  prep.clean_text(' hello ')  호출
          ↓
Step 2.  Python이 자동으로 변환:
         clean_text(prep,  ' hello ')
                    ^^^^
                    객체 자신을 몰래 끼워 넣음!
          ↓
Step 3.  함수 정의 확인:
         def clean_text(data):  ← 매개변수 1개뿐
          ↓
Step 4.  인자 2개 vs 매개변수 1개  →  💥 TypeError!
```

### ✅ 해결 방법 (2가지)
```python
# 방법 1. self 추가 — 클래스 안에 둘 때
class Preprocessor:
    def clean_text(self, data):
        return data.strip()

# 방법 2. 독립 함수로 분리 — 클래스 밖으로 꺼낼 때
def clean_text(data):
    return data.strip()
```

---

## ③ Azure ML 실무 — 클래스 vs 독립 함수 어떤 게 나아?

### 🏗️ 파이프라인 구조
```
[데이터 수집] → [결측치 제거] → [텍스트 정제] → [토큰화] → [모델 학습]
                 ↑________________이 3단계를 어떻게 묶을까?________________↑
```

### 🅐 클래스로 묶기
```python
class TextPreprocessor:
    def __init__(self, lang="ko", max_len=512):
        self.lang = lang        # 설정값 한 번만 지정
        self.max_len = max_len
        self.stats = {"processed": 0}  # 처리 통계 추적

    def remove_null(self, df):
        return df.dropna()

    def clean_text(self, text):
        return text.strip().lower()

    def tokenize(self, text):
        return text.split()[:self.max_len]  # self의 설정값 활용!

    def run_pipeline(self, df):
        df = self.remove_null(df)
        df["text"] = df["text"].apply(self.clean_text)
        return df

# 사용할 때
prep = TextPreprocessor(lang="ko", max_len=256)
result = prep.run_pipeline(raw_data)
```

### 🅑 독립 함수로 두기
```python
def remove_null(df):
    return df.dropna()

def clean_text(text):
    return text.strip().lower()

def tokenize(text, max_len=512):
    return text.split()[:max_len]
```

### 📊 비교표
| 기준 | 클래스 방식 | 독립 함수 방식 |
|---|---|---|
| 설정값 관리 | ✅ `__init__`에 한 번만 | ⚠️ 매번 인자로 전달 |
| 코드 구조 | ✅ 관련 로직 한 곳에 | ⚠️ 함수 많아지면 복잡 |
| 단위 테스트 | 약간 복잡 | ✅ 함수별로 쉬움 |
| Azure ML 추천 | ✅ 파이프라인에 적합 | 단순 변환에 적합 |

### 🏆 결론
> **설정값이 많고 단계가 여럿이면 → 클래스**
> **간단한 변환 1~2개면 → 독립 def 함수**
> Azure ML 파이프라인처럼 `lang`, `max_len` 같은 설정값이 여럿이면
> 클래스가 훨씬 관리하기 편함!

---

## 🎯 3줄 최종 요약
1. **self = 객체 자신의 주소** — Python이 자동으로 첫 번째 인자로 넣어줌
2. **self 빼먹으면 TypeError** — Python이 몰래 인자 1개를 추가하기 때문
3. **Azure ML은 클래스 추천** — 설정값 관리 + 파이프라인 재사용성 ↑
