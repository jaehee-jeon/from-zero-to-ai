## 📌 문제 4. [1주차] Pandas 전처리
해설 담당: 황선주

```python
고객 설문 데이터프레임 df에 일부 나이(age) 값이 비어있고, 동일한 고객ID(customer_id)가 중복 입력되어 있습니다. df는 customer_id, age, feedback 컬럼을 가지고 있습니다.
```

**age 컬럼의 결측값을 평균값으로 채우고, customer_id 기준으로 중복 행을 제거하는 코드 2줄을 작성하세요.**

💡 힌트: fillna(df['age'].mean())과 drop_duplicates(subset='customer_id')를 사용하세요

🌍 실무에서는 이렇게 써요:
실무에서 Azure ML 파이프라인에 데이터를 투입하기 전, 결측값과 중복 데이터를 정제하는 전처리 단계는 필수입니다. 특히 Azure OpenAI로 고객 피드백을 분석할 때 중복된 응답이나 비어있는 속성값이 있으면 모델 성능이 떨어지기 때문에, dropna/fillna로 결측 처리하고 drop_duplicates로 중복 제거한 뒤 학습이나 추론을 진행합니다. 이 두 가지만 잘 다뤄도 데이터 품질이 크게 개선됩니다.

## 정답
1. df['age'] = df['age'].fillna(df['age'].mean()) 
2. df = df.drop_duplicates(subset='customer_id')

## 설명
 df['age'].fillna(df['age'].mean()) 
    age 결측값을 age 열의 평균을 구하는 메서드인 .mean()으로 평균값 산출
drop_duplicates(subset='customer_id') 
    drop_duplicates를 활용하여 customer_id가 중복된 행 제거 (기본적으로 첫 번째 행만 유지)