# ✏️ 전처리 모듈화 — 퀴즈 풀이
> Python 1주차 | 발표 대비 퀴즈 3문제

---

## Q1. self를 받는 것과 일반 함수처럼 데이터만 받는 것의 차이는?

### 핵심 답변
**self를 받는 메서드**는 객체의 상태(변수)를 읽고 바꿀 수 있고,
**독립 함수**는 객체와 완전히 분리되어 데이터만 받고 바로 처리함.

```python
# 🔵 self 있는 메서드 — 객체 상태를 기억
class Preprocessor:
    def __init__(self):
        self.count = 0

    def clean_text(self, data):
        self.count += 1       # ← 객체의 count를 바꿀 수 있음
        return data.strip()

prep = Preprocessor()
prep.clean_text(' hello ')
print(prep.count)             # → 1  (기억하고 있음!)


# 🟢 독립 함수 — 상태 없음, 입력만 처리
def clean_text(data):
    return data.strip()       # 그냥 받아서 처리 끝. 기억 없음.
```

### 각각 언제 유용?
- **self 메서드** → 처리 횟수 추적, 설정값 재사용, Azure ML 파이프라인
- **독립 함수** → 간단한 변환, import해서 어디서든 쓰고 싶을 때

---

## Q2. 왜 에러가 발생할까?

### 문제 코드
```python
class Preprocessor:
    def clean_text(data):    # ← self 없음
        return data.strip()

prep = Preprocessor()
prep.clean_text(' hello ')   # → TypeError!
```

### 답변 — 원인 3단계로 설명
```
① prep.clean_text(' hello ') 를 호출하면

② Python이 내부적으로 자동 변환:
   Preprocessor.clean_text(prep, ' hello ')
                            ^^^^
                      객체 자신을 몰래 추가!

③ 그런데 함수는 def clean_text(data) → 매개변수 1개뿐
   인자는 2개(prep + ' hello') → 💥 TypeError!
```

### 에러 메시지 해석
```
TypeError: clean_text() takes 1 positional argument but 2 were given
                         ↑ 함수는 1개만 받음    ↑ Python이 2개를 넣으려 함
```

> 💡 "내가 1개만 넘겼는데 왜 2개야?" → Python이 **객체 자신(prep)을 몰래 첫 번째 인자로** 추가하기 때문!

### 올바른 코드
```python
# ✅ 해결 1: self 추가 (클래스 안에 유지)
class Preprocessor:
    def clean_text(self, data):
        return data.strip()

# ✅ 해결 2: 클래스 밖 독립 함수로 분리
def clean_text(data):
    return data.strip()
```

---

## Q3. Azure ML — 클래스 vs 독립 함수, 어느 게 더 나을까?

### 답변 — 클래스 방식이 유리한 이유

#### 상황
`lang="ko"`, `max_len=512` 같은 설정값이 여럿 있고,
결측치 제거 → 정제 → 토큰화 등 단계가 여러 개인 경우.

#### 클래스 방식의 장점
```python
class TextPreprocessor:
    def __init__(self, lang="ko", max_len=512):
        self.lang = lang      # ← 설정값 한 번만 지정!
        self.max_len = max_len

    def clean_text(self, text):
        return text.strip().lower()

    def tokenize(self, text):
        return text.split()[:self.max_len]  # ← self로 설정값 재사용

# 실험 A
prep_ko = TextPreprocessor(lang="ko", max_len=256)

# 실험 B — 설정만 바꾸면 됨
prep_en = TextPreprocessor(lang="en", max_len=512)
```

#### 독립 함수 방식의 단점
```python
# 설정값을 매번 직접 넘겨야 함 → 실수 가능성 ↑
clean_text(text)
tokenize(text, max_len=256)   # ← 매번 이렇게 써야 함
```

### 재사용성 vs 유지보수 관점
| 관점 | 클래스 | 독립 함수 |
|---|---|---|
| **재사용성** | ✅ 설정값 포함 통째로 재사용 | ⚠️ 설정값 따로 관리 필요 |
| **유지보수** | ✅ 관련 코드가 한 클래스에 | ⚠️ 함수 많아지면 흩어짐 |
| **테스트** | 약간 복잡 | ✅ 함수 하나씩 테스트 쉬움 |

### 최종 답변 한 줄
> **설정값이 많고 처리 단계가 여럿인 Azure ML 파이프라인에서는 클래스가 더 유리하다.**
> 독립 함수는 단순 변환 1~2개일 때 적합하다.

---

## 📌 세 문제 공통 핵심 키워드

| 키워드 | 한 줄 설명 |
|---|---|
| `self` | 객체 자신의 메모리 주소를 가리키는 이름 |
| 메서드 | 클래스 안에 정의된 함수 (첫 인자가 self) |
| 독립 함수 | 클래스 밖에 정의된 일반 `def` 함수 |
| TypeError | self 빠졌을 때 Python이 인자 개수 안 맞아서 내는 에러 |
| `__init__` | 객체 만들 때 자동으로 실행되는 초기화 메서드 |
