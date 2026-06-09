## 📌 문제 1. [1주차] Python 자료구조
해설 담당: 김석영

```python
고객 피드백 데이터를 처리하는 API 응답에서 고유한 제품 카테고리 목록을 추출해야 합니다.
raw_data = ['laptop', 'mouse', 'laptop', 'keyboard', 'mouse', 'monitor']
중복을 제거한 고유 카테고리를 자동으로 저장하려고 합니다.
```

**raw_data에서 중복을 제거하여 고유한 카테고리만 담은 변수 unique_categories를 생성하는 코드 1줄을 작성하세요.**

💡 힌트: set()은 중복을 자동으로 제거합니다

🌍 실무에서는 이렇게 써요:
Azure OpenAI API 응답을 처리할 때 실무에서 자주 사용됩니다. 예를 들어 여러 문서에서 추출한 키워드 중복 제거, ML 파이프라인에서 고유한 라벨 추출, 로그 데이터에서 유니크한 사용자 ID 집계 등에 set()을 활용합니다. dict는 API 응답 파싱이나 설정 값 저장에, list는 순차 데이터나 배치 처리에 사용되죠. 

답: 
unique_categories = list(set(raw_data))

set(raw_data)가 중복을 제거하고, list()로 다시 리스트 형태로 변환하여 unique_categories에 저장합니다.
결과 예시: ['laptop', 'mouse', 'keyboard', 'monitor'] (set 자료형은 순서를 보장하지 않아 중간에 순서가 달라질 수 있음)
