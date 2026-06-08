# Round 1 실습 문제지 🧩

> 스터디 전날까지 GitHub 본인 폴더에
> `round1_quiz.md` 로 올려주세요!

---

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

---

## 📌 문제 2. [1주차] 제어문
해설 담당: 전재희

```python
고객 문의 로그에서 'error' 키워드가 포함된 메시지만 필터링하려고 합니다.
messages = ['정상 처리', 'error: timeout', '완료', 'error: 404']
위 리스트에서 'error'가 포함된 항목만 출력하는 코드를 작성하세요.
```

**for 반복문과 if 조건문을 사용해 'error'가 포함된 메시지만 print하는 코드 1-2줄을 작성하세요.**

💡 힌트: for msg in messages: 로 시작하고, if 'error' in msg: 조건을 사용하세요

```python
messages = ['정상 처리', 'error: timeout', '완료', 'error: 404']

for msg in messages:
    if 'error' in msg:
        print(msg)
```
error: timeout
error: 404

🌍 실무에서는 이렇게 써요:
실무에서는 Azure OpenAI API 응답 로그나 ML 모델 학습 로그를 모니터링할 때 특정 키워드(에러, 경고 등)를 필터링해야 합니다. 수천 개의 로그 중 문제가 있는 항목만 빠르게 찾아내기 위해 for/if 조합을 가장 많이 사용합니다. 예를 들어 Azure ML 파이프라인 실행 로그에서 실패한 스텝만 추출할 때 이런 패턴을 씁니다.

---

## 📌 문제 3. [1주차] NumPy·Pandas
해설 담당: 김예찬

```python
고객 데이터를 분석하려고 합니다. 단순 숫자 리스트 [100, 200, 300]은 NumPy 배열로, 고객명과 구매액이 함께 있는 데이터는 Pandas 데이터프레임으로 만들어야 합니다.
```

**NumPy 배열 arr = np.array([100, 200, 300])을 만드는 코드와, 'name'열에 ['김철수', '이영희'], 'amount'열에 [100, 200]을 담은 Pandas 데이터프레임 df를 만드는 코드를 각각 작성하세요.**

💡 힌트: np.array()와 pd.DataFrame()을 사용하며, 데이터프레임은 딕셔너리 형태로 전달

🌍 실무에서는 이렇게 써요:
Azure ML에서 AI 모델 학습 시 NumPy 배열은 숫자 연산과 모델 입력 데이터로 사용되고, Pandas 데이터프레임은 CSV/데이터베이스에서 불러온 실제 업무 데이터를 전처리할 때 씁니다. 예를 들어 Azure OpenAI API 응답을 정리하거나, 고객 로그 데이터를 분석할 때 데이터프레임을 사용하면 컬럼명으로 직관적으로 다룰 수 있어요.

---

## 📌 문제 4. [1주차] Pandas 전처리
해설 담당: 황선주

```python
고객 설문 데이터프레임 df에 일부 나이(age) 값이 비어있고, 동일한 고객ID(customer_id)가 중복 입력되어 있습니다. df는 customer_id, age, feedback 컬럼을 가지고 있습니다.
```

**age 컬럼의 결측값을 평균값으로 채우고, customer_id 기준으로 중복 행을 제거하는 코드 2줄을 작성하세요.**

💡 힌트: fillna(df['age'].mean())과 drop_duplicates(subset='customer_id')를 사용하세요

🌍 실무에서는 이렇게 써요:
실무에서 Azure ML 파이프라인에 데이터를 투입하기 전, 결측값과 중복 데이터를 정제하는 전처리 단계는 필수입니다. 특히 Azure OpenAI로 고객 피드백을 분석할 때 중복된 응답이나 비어있는 속성값이 있으면 모델 성능이 떨어지기 때문에, dropna/fillna로 결측 처리하고 drop_duplicates로 중복 제거한 뒤 학습이나 추론을 진행합니다. 이 두 가지만 잘 다뤄도 데이터 품질이 크게 개선됩니다.

---

## 📌 문제 5. [1주차] 시각화 라이브러리
해설 담당: 제니

```python
고객 연령대별 구매액 데이터를 분석 중입니다. Pandas DataFrame에 '연령대'와 '구매액' 컬럼이 있고, 연령대별 평균 구매액을 막대그래프로 시각화하려고 합니다. 경영진 보고용이므로 깔끔한 스타일이 필요합니다.
```

**Matplotlib, Seaborn, Folium 중 어떤 라이브러리를 사용해야 하며, 해당 라이브러리로 막대그래프를 그리는 기본 코드 1줄을 작성하세요. (df는 DataFrame 변수명, x='연령대', y='구매액' 가정)**

💡 힌트: Seaborn은 통계 시각화에 최적화되어 있으며, barplot 함수 사용

🌍 실무에서는 이렇게 써요:
실무에서는 Seaborn을 사용해 고객 세그먼트 분석, 매출 트렌드 등 비즈니스 데이터를 시각화합니다. Matplotlib은 세밀한 커스터마이징이 필요할 때, Folium은 매장 위치나 배송 경로 등 지도 시각화가 필요할 때 사용합니다. Azure ML에서 모델 성능을 대시보드로 보여줄 때도 이런 라이브러리들을 조합해 활용합니다.

---

## 📌 문제 6. [1주차] 객체지향(OOP)
해설 담당: 임치영

```python
고객 주문 처리 시스템에서 배송비 계산 로직이 5개 페이지에 복붙되어 있습니다. 무게(weight_kg)와 거리(distance_km)를 받아 배송비를 계산하는 코드가 중복됩니다. 배송비 공식은 (무게 × 1000) + (거리 × 500) 원입니다.
```

**ShippingCalculator 클래스를 만들고, calculate_fee(weight_kg, distance_km) 메서드를 작성하세요. (Python 기준)**

💡 힌트: class로 묶고 def 메서드 안에 계산 로직 작성

🌍 실무에서는 이렇게 써요:
실제로 Azure OpenAI API 호출 코드를 여러 곳에서 쓸 때, API키 설정·재시도 로직·응답 파싱을 AzureOpenAIClient 클래스로 만들어두면 10곳에서 복붙하던 30줄 코드가 client.get_completion(prompt) 한 줄로 줄어듭니다. 나중에 GPT-4에서 GPT-4o로 모델 변경할 때도 클래스 안 1곳만 수정하면 전체 시스템에 반영돼요.

---

## 📌 문제 7. [1주차] 전처리 모듈화
해설 담당: 정승영

```python
데이터 전처리 함수 remove_duplicates()가 있습니다. 이 함수를 DataCleaner 클래스 안에 메서드로 옮기려고 합니다. 현재 함수는 df.drop_duplicates()를 반환합니다.
```

**DataCleaner 클래스를 만들고, remove_duplicates 메서드를 추가하세요. 메서드는 self와 df를 매개변수로 받아야 합니다.**

💡 힌트: class DataCleaner: 로 시작하고, def remove_duplicates(self, df): 형태로 메서드 정의

🌍 실무에서는 이렇게 써요:
Azure ML 파이프라인에서 데이터 전처리 단계를 클래스로 모듈화하면, 같은 전처리 로직을 여러 실험에서 재사용할 수 있어요. 예를 들어 고객 데이터 중복 제거, 결측치 처리 등을 DataCleaner 클래스 하나로 관리하면 코드가 깔끔해지고, 팀원들과 공유하기도 쉽습니다. 나중에 Azure OpenAI로 보내기 전 데이터 정제 과정을 표준화할 때도 유용하게 쓰입니다.

---

## 📌 문제 8. [1주차] Time Series 분석
해설 담당: 박성은

```python
일별 매출 데이터가 DataFrame df에 'date'(문자열)와 'sales'(숫자) 컬럼으로 있습니다. 'date'를 datetime으로 변환하고 인덱스로 설정한 뒤, 주간(weekly) 평균 매출을 구하려고 합니다.
```

**df['date']를 datetime 인덱스로 변환하고, 주간 평균 매출을 계산하는 pandas 코드 2줄을 작성하세요.**

💡 힌트: pd.to_datetime()과 resample('W').mean() 활용

🌍 실무에서는 이렇게 써요:
실무에서는 Azure ML Pipeline에서 시계열 데이터를 전처리할 때 이 패턴을 자주 사용합니다. 예를 들어 IoT 센서 데이터를 시간 단위로 수집했지만 분석은 일별/주별로 해야 할 때, resample로 집계하고 rolling으로 이동평균을 내서 트렌드를 파악한 뒤 Azure OpenAI API로 인사이트를 생성하는 리포트 자동화 시스템을 만들 수 있습니다.

---

## 📌 문제 9. [1주차] 웹 크롤링·스크래핑
해설 담당: 이준석

```python
뉴스 사이트에서 기사 제목을 수집하려고 합니다. 아래 코드는 requests로 HTML을 가져온 후 BeautifulSoup으로 파싱한 상태입니다.

from bs4 import BeautifulSoup
soup = BeautifulSoup(html_content, 'html.parser')
# 제목이 <h2 class='article-title'> 태그 안에 있습니다
```

**soup 객체에서 class가 'article-title'인 모든 h2 태그의 텍스트를 리스트로 추출하는 코드 1줄을 작성하세요.**

💡 힌트: soup.find_all() 메서드와 리스트 컴프리헨션을 사용하세요

🌍 실무에서는 이렇게 써요:
실무에서는 경쟁사 제품 리뷰, 뉴스 모니터링, 부동산 매물 정보 등을 자동 수집할 때 사용합니다. 특히 AI 모델 학습용 데이터를 대량으로 모을 때 유용하지만, 최근에는 많은 사이트가 봇 차단 기술을 사용하거나 API를 유료화해서 Selenium이나 공식 API를 써야 하는 경우가 많아졌어요. Azure OpenAI에 넣을 산업 특화 텍스트 데이터를 수집할 때도 자주 활용됩니다.

---

## 📌 문제 10. [1주차] Azure 데이터 파이프라인
해설 담당: 최진웅

```python
Azure Data Factory에서 Blob Storage의 sales.csv 파일을 읽어 SQL Database의 sales 테이블로 매일 자정에 자동으로 적재하는 파이프라인을 만들려고 합니다. 데이터 이동을 담당하는 활동(Activity)과 실행 시간을 정하는 구성요소가 필요합니다.
```

**이 파이프라인에 필요한 핵심 Activity 유형 1개와 스케줄링을 담당하는 구성요소 이름 1개를 작성하세요.**

💡 힌트: Copy Activity, Trigger

🌍 실무에서는 이렇게 써요:
실제 회사에서는 매일 생성되는 판매 데이터, 로그 파일, IoT 센서 데이터 등을 자동으로 수집해서 데이터베이스나 Data Lake에 저장하는 파이프라인을 만듭니다. 예를 들어 매일 밤 12시에 전국 매장의 판매 데이터를 자동으로 모아서 중앙 데이터베이스에 적재하면, 다음날 아침 경영진이 실시간 대시보드로 어제 매출을 바로 확인할 수 있죠. Azure Data Factory는 이런 반복 작업을 코드 없이 구성할 수 있어서 데이터 엔지니어들이 가장 많이 사용하는 도구입니다.

---

