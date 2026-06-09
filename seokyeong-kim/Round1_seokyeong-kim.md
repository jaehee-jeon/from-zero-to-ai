# Python 자료구조 발표 자료
### 발표자: 김석영 

# 질문 1
### 튜플 (1, 2, 3)과 리스트 [1, 2, 3]은 둘 다 여러 값을 순서대로 담는데, 왜 두 가지로 나눠놨을까요? 실제로 어떤 상황에서 각각을 선택하나요?

# Python의 튜플(Tuple)과 리스트(List)는 왜 둘 다 있을까?

튜플 `(1, 2, 3)`과 리스트 `[1, 2, 3]`는 모두 여러 값을 순서대로 저장하는 자료구조입니다. 하지만 **값을 변경할 수 있는지 여부**가 핵심적으로 다릅니다.

## 핵심 차이

| 구분        | 리스트 (`list`) | 튜플 (`tuple`)    |
| --------- | ------------ | --------------- |
| 변경 가능 여부  | 가능 (mutable) | 불가능 (immutable) |
| 문법        | `[1, 2, 3]`  | `(1, 2, 3)`     |
| 항목 추가/삭제  | 가능           | 불가능             |
| 딕셔너리 키 사용 | 불가능          | 가능              |

### 리스트

```python
numbers = [1, 2, 3]

numbers.append(4)    _append는 맨 끝에 더하는 함수_  
numbers[0] = 10      _0번째 자리에 있는 값을 10으로 교체_

print(numbers)
# [10, 2, 3, 4]
```

### 튜플

```python
numbers = (1, 2, 3)

numbers[0] = 10
# TypeError
```

---

## 튜플이 필요한 이유

### 1. 변경되면 안 되는 데이터를 표현

좌표, RGB 색상, 날짜처럼 **고정된 값의 묶음**을 표현할 때 사용합니다. 

```python
point = (10, 20)
rgb = (255, 0, 0)
```

튜플을 사용하면 해당 데이터가 의도치 않게 변경되는 것을 방지할 수 있습니다.

---

### 2. 함수에서 여러 값을 반환할 때

```python
def get_user():
    return "Alice", 25

name, age = get_user()
```

함수가 반환하는 값들은 일반적으로 고정된 구조를 가지므로 튜플이 자연스럽게 사용됩니다.

---

### 3. 딕셔너리의 키로 사용

튜플은 변경 불가능하므로 딕셔너리 키가 될 수 있습니다. 

```python
board = {}

board[(3, 5)] = "Knight"
board[(4, 7)] = "Bishop"
```

반면 리스트는 변경 가능하므로 키로 사용할 수 없습니다. 

---

### 4. 코드의 의도를 명확하게 표현

```python
person = ("Alice", 25, "Seoul")
```

→ 하나의 사람 정보를 나타내는 **고정된 레코드**

```python
scores = [95, 87, 100]
```

→ 개수가 변할 수 있는 **점수 목록**

자료구조 선택 자체가 데이터의 성격을 설명해 줍니다.

---

## 언제 무엇을 사용할까?

### 리스트를 선택하는 경우

* 항목을 추가하거나 삭제할 예정 - 변경이 가능하니까! 
* 데이터 개수가 변할 수 있음 
* 일반적인 컬렉션 관리

```python
shopping = ["milk", "bread"]
todos = ["study", "exercise"]
```

### 튜플을 선택하는 경우

* 값이 고정되어 있음 - 변경이 불가능하니까! 
* 함수에서 여러 값을 반환
* 좌표, 날짜, RGB 색상 등 표현
* 딕셔너리 키로 사용
* 변경을 방지하고 싶음

```python
point = (10, 20)
DATABASE = ("localhost", 5432)
```

---

## 정리

> **리스트는 "변경 가능한 컬렉션"을 표현하고, 튜플은 "고정된 의미를 가진 값들의 묶음"을 표현한다.**

실무에서는 리스트를 더 자주 사용하지만, 데이터가 변경되지 않아야 하거나 고정된 구조를 표현할 때는 튜플이 더 적합하다.






# 질문 2
### Azure OpenAI API 응답을 config = {'model': 'gpt-4', 'temp': 0.7}로 받아서 나중에 config['model'] = 'gpt-3.5'로 바꾸려는데, 누군가 config를 ()로 만들었다면 어떤 문제가 생기나요? 왜 그렇게 생각하시나요?

## 1. 만약 config를 `()`로 만들었다면?

## 정상적인 경우 (dict)

```python
config = {
    "model": "gpt-4",
    "temp": 0.7
}
```

변경 가능:

```python
config["model"] = "gpt-3.5"
```

---

## 문제 있는 경우 (tuple)

```python
config = (
    ("model", "gpt-4"),
    ("temp", 0.7)
)
```

또는

```python
config = ("gpt-4", 0.7)
```

튜플(tuple)은 변경할 수 없는 자료형(immutable)입니다.

```python
config["model"] = "gpt-3.5"
```

실행 시 오류 발생:

```text
TypeError
```

---

## 주의: `()`가 항상 튜플은 아님

다음은 dict입니다.

```python
config = (
    {
        "model": "gpt-4",
        "temp": 0.7
    }
)
```

여기서 괄호는 단순히 "이 표현식을 하나로 묶어라"는 의미일 뿐입니다. 
하지만 여기에 쉼표가 있으면 튜플입니다.

```python
config = (
    {"model": "gpt-4"},
)
```

```python
type(config)
# tuple
```

따라서 config를 ()로 만들었다면 오류가 발생합니다. `config["model"] = "gpt-3.5"` 와 같은 코드를 사용할 예정이라면 `config`는 **수정 가능한 dict** 로 만드는 것이 적절합니다.



# 질문 3
### Azure ML 파이프라인에서 학습 설정값(모델명, 하이퍼파라미터 등)을 다른 함수에 전달할 때, 딕셔너리 {}와 튜플 () 중 어느 쪽이 더 적합할까요? 각각의 장단점을 데이터 추적과 실수 방지 관점에서 설명해주세요. 

# Azure ML 파이프라인 설정값 전달: 딕셔너리 vs 튜플

Azure ML 파이프라인에서 **학습 설정값(모델명, 하이퍼파라미터, 데이터 경로 등)** 을 함수 간에 전달할 때는 일반적으로 **튜플보다 딕셔너리(또는 dataclass 같은 명명된 구조체)** 가 더 적합합니다.

딕셔너리와 튜플을 **데이터 추적성(Traceability)** 과 **실수 방지** 관점에서 비교하면 다음과 같습니다.

| 항목 | 딕셔너리 `{}` | 튜플 `()` |
|--------|--------|--------|
| 가독성 | 높음 | 낮음 |
| 데이터 추적 | 쉬움 | 어려움 |
| 파라미터 추가/삭제 | 유연함 | 순서 변경 위험 |
| 함수 호출 실수 | 적음 | 많음 |
| 타입 안정성 | 보통 | 낮음 |
| 성능 | 약간 느림 | 약간 빠름 |
| Azure ML 로그 저장 | 용이 | 불편 |

---

## 1. 데이터 추적 관점

### 딕셔너리

```python
config = {
    "model_name": "xgboost",
    "learning_rate": 0.01,
    "max_depth": 8
}
```

사용 시:

```python
train_model(config)
```

함수 내부:

```python
def train_model(config):
    print(config["learning_rate"])
```

#### 장점

- 로그를 남기기 쉽다.
- Azure ML Run 기록에 그대로 저장 가능하다.
- JSON 직렬화가 쉽다.  _JSON 직렬화: 파이썬 객체(dict, list 등)를 문자열(JSON 형식)로 바꾸는 것_

```python
mlflow.log_params(config)
```

또는

```python
json.dump(config, f)
```

실험 결과 예시:

```json
{
  "model_name": "xgboost",
  "learning_rate": 0.01,           
  "max_depth": 8
}
```                           _learning_rate: 학습률_

어떤 설정으로 학습했는지 바로 확인할 수 있다.

### 튜플

```python
config = (
    "xgboost",
    0.01,
    8
)
```

함수 내부:

```python
model_name, learning_rate, max_depth = config
```

몇 달 후 코드를 보면

```python
config[1]
```

이 무엇을 의미하는지 바로 알기 어렵다.

실험 이력에서도

```python
("xgboost", 0.01, 8)
```

처럼 저장되면 의미 해석이 필요하다.

> 실험 재현성과 추적성 측면에서는 딕셔너리가 훨씬 유리하다.

---

## 2. 실수 방지 관점

### 튜플의 위험

초기 버전:

```python
config = (
    "xgboost",
    0.01,
    8
)
```

함수:

```python
def train_model(model_name, lr, depth):
    ...
```

몇 달 후 파라미터 추가:

```python
config = (
    "xgboost",
    100,      # n_estimators 추가
    0.01,
    8
)
```

기존 코드:

```python
model_name, lr, depth = config
```

문제점:

- 언패킹 오류 발생 가능
- 값이 잘못 매핑될 위험 존재
- 순서 의존성이 높음

### 딕셔너리의 안전성

```python
config = {
    "model_name": "xgboost",
    "n_estimators": 100,
    "learning_rate": 0.01,
    "max_depth": 8
}
```

필요한 값만 조회:

```python
lr = config["learning_rate"]
```

장점:

- 신규 필드 추가 시 기존 코드 영향 최소화
- 키 이름으로 의미가 명확함
- 순서 변경 문제 없음

---

## 3. Azure ML 파이프라인에서 자주 사용하는 패턴

```python
training_config = {
    "model_name": "lightgbm",
    "learning_rate": 0.05,
    "num_leaves": 31,
    "feature_columns": [
        "age",
        "income",
        "score"
    ]
}
```

```python
prepare_data(training_config)
train_model(training_config)
register_model(training_config)
```

모든 단계가 동일한 설정 객체를 공유한다.

---

## 4. 더 좋은 방법: dataclass

규모가 커지면 딕셔너리보다 `dataclass` 사용이 더 안전하다.

```python
from dataclasses import dataclass

@dataclass
class TrainingConfig:
    model_name: str
    learning_rate: float
    max_depth: int
```

사용:

```python
config = TrainingConfig(
    model_name="xgboost",
    learning_rate=0.01,
    max_depth=8
)
```

접근:

```python
config.learning_rate
```

### 장점

- 오타 방지
- IDE 자동완성 지원
- 타입 힌트 제공
- 유지보수성 향상

---

## 선호도

```text
튜플 < 딕셔너리 < dataclass / Pydantic
```

---

## 권장 사항

학습 설정값처럼 **의미가 있는 여러 파라미터를 전달하는 경우**:

- 소규모/간단한 프로젝트 → **딕셔너리**
- 중대형 프로젝트 → **dataclass**
- 튜플 → `(x, y)`, `(train_df, test_df)` 처럼 순서 자체가 의미인 짧은 데이터에만 사용

### 결론

모델명, 하이퍼파라미터, 데이터 경로 등의 학습 설정 전달 용도라면 **튜플보다 딕셔너리(또는 dataclass)가 훨씬 적합**하다.

특히 Azure ML의 **실험 추적**, **재현성 관리** 측면에서 큰 장점을 제공한다.



