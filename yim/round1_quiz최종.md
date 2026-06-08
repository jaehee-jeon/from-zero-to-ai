
 문제 6. [1주차] 객체지향(OOP)

고객 주문 처리 시스템에서 배송비 계산 로직이 5개 페이지에 복붙되어 있습니다. 
무게(weight_kg)와 거리(distance_km)를 받아 배송비를 계산하는 코드가 중복됩니다. 
배송비 공식은 (무게 × 1000) + (거리 × 500) 원입니다.
ShippingCalculator 클래스를 만들고, calculate_fee(weight_kg, distance_km) 메서드를 
작성하세요. (Python 기준)

힌트: class로 묶고 def 메서드 안에 계산 로직 작성



문제 해설 정답 코드 

class ShippingCalculator:
       
    def calculate_fee(self, weight_kg, distance_km):
      
       ## 무게와 거리를 받아 배송비를 계산하는 메서드 공식: (무게 × 1000) + (거리 × 500) 원 ##
    
        fee = (weight_kg * 1000) + (distance_km * 500)
        return fee

calculator = ShippingCalculator()
order_fee = calculator.calculate_fee(weight_kg=5, distance_km=10)
print(f"계산된 배송비: {order_fee}원") 
 # 출력: 10000원
 
===================================================================
입력값 직접입력 하기
==================================================================

class ShippingCalculator:
    def calculate_fee(self, weight_kg, distance_km):
        # 배송비 공식: (무게 × 1000) + (거리 × 500) 원
        fee = (weight_kg * 1000) + (distance_km * 500)
        return fee

# 사용자 입력 받기
weight = float(input("배송 물품의 무게(kg)를 입력하세요: "))
distance = float(input("배송 거리(km)를 입력하세요: "))

# 객체 생성 및 계산
calculator = ShippingCalculator()
order_fee = calculator.calculate_fee(weight_kg=weight, distance_km=distance)

# 결과 출력
print(f"계산된 배송비: {order_fee}원")      #소숫점도 출력될수 있음
print(f"계산된 배송비: {int(order_fee)}원")   #정수값으로 출력


**** 실무 관점 연결 ****

실무에서는 이렇게 써요: 실제로 Azure OpenAI API 호출 코드를 여러 곳에서 쓸 때, 
API키 설정·재시도 로직·응답 파싱을 AzureOpenAIClient 클래스로 만들어두면 10곳에서
복붙하던 30줄 코드가 client.get_completion(prompt) 한 줄로 줄어듭니다. 
나중에 GPT-4에서 GPT-4o로 모델 변경할 때도 클래스 안 1곳만 수정하면 전체 시스템에
반영돼요.

=======================================================
실무형 Azure OpenAI Client 클래스 코드
======================================================

import os
import time
from azure.openai import AzureOpenAI
from azure.core.exceptions import AzureError

class CustomAzureOpenAIClient:

   # 실무용 Azure OpenAI 클라이언트 래퍼 클래스  보안 키 설정, 예외 처리, 재시도 로직을 한 곳에서 관리합니다.    
    def __init__(self):
        # [복잡한 설정 숨기기] API 키나 엔드포인트 같은 민감한 정보는 내부에서 처리합니다.
        self.api_key = os.getenv("AZURE_OPENAI_API_KEY", "your-secret-api-key")
        self.endpoint = os.getenv("AZURE_OPENAI_ENDPOINT", "https://your-endpoint.openai.azure.com/")
        self.api_version = "2024-02-15-preview"
        self.model_name = "gpt-4o"  # 나중에 모델을 바꿀 때 여기만 수정하면 끝!

   # 클라이언트 초기화
        self.client = AzureOpenAI(
            azure_endpoint=self.endpoint,
            api_key=self.api_key,
            api_version=self.api_version
        )

    def get_completion(self, prompt: str, max_retries: int = 3) -> str:
   # 보안/예외처리/재시도가 포함된 안전한 답변 생성 메서드
   # 실무에 필수적인 에러 핸들링 및 재시도(Retry) 로직을 클래스 내부에 격리
        for attempt in range(max_retries):
            try:
                response = self.client.chat.completions.create(
                    model=self.model_name,
                    messages=[
                        {"role": "system", "content": "당신은 친절한 AI 비서입니다."},
                        {"role": "user", "content": prompt}
                    ]
                )
   # 복잡한 응답 파싱도 클래스 내부에서 깔끔하게 처리해서 문자열만 반환
                return response.choices[0].message.content

            except AzureError as e:
                print(f"[경고] Azure API 오류 발생 (시도 {attempt + 1}/{max_retries}): {e}")
                if attempt < max_retries - 1:
                    time.sleep(2)  # 2초 쉬고 다시 시도
                else:
                    return "죄송합니다. 현재 시스템 오류로 답변을 드릴 수 없습니다."


# =====================================================================
#   실무에서 다른 개발자들이 사용하는 모습 (호출부는 딱 한 줄!)
# =====================================================================

# 1. 프로그램 시작할 때 객체 딱 한 번 생성
ai_client = CustomAzureOpenAIClient()

# 2. 10곳이든 30곳이든 필요한 곳에서는 내부 복잡성을 모른 채 '한 줄'만 사용합니다.
user_prompt = "인공지능의 정의에 대해 짧게 설명해줘."
result = ai_client.get_completion(user_prompt)

