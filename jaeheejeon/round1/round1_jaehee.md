### **[개념 차이]**

if문은 '조건이 맞으면 실행'하고, while문은 '조건이 맞는 동안 반복'합니다. 그렇다면 while문 안에 조건을 True로 두고 if문으로 break를 거는 것과, while문의 조건을 직접 쓰는 것은 어떤 차이가 있나요?

***while True + if break***

```python
x = 0

while True: 
    if x >= 5:
        
        break

    print('while+if문',x)
    x += 1
```

**결과 :** 

(while문이 true인 경우 일단 무조건 실행됨)

while+if문 0
while+if문 1
while+if문 2
while+if문 3
while+if문 4

***while 조건문***

```python
x = 0

while x >= 5:  

    print('while문',x)
    x += 1
```

**결과 :**

(while 문 조건이 false라 루프문에 진입하지 못)

### **[실수 포인트]**

API 응답을 최대 5번까지 재시도하는 코드에서 `for i in range(5):`를 썼는데 4번만 실행됩니다. 초보자들이 range(5)를 '5번'이라고 생각하는데, 왜 이런 실수가 생기는지 설명해주세요.

```python
for i in range(5):
  print(i)
```

**결과:**

0
1
2
3
4

range(5)는 5번 반복이 맞다. 파이썬이 0부터 세므로 값이 0~4로 끝날 뿐 '실행 횟수 5'와 '마지막 값 4'는 다른 개념이다.

### [응용]

Azure OpenAI API 호출 시 rate limit 에러가 날 때, while문으로 무한 재시도하는 것과 for문으로 최대 횟수를 정하는 것 중 어느 것을 선택하시겠습니까? 각 방식이 엔터프라이즈 환경에서 어떤 리스크를 가지는지 설명해주세요.

- for문으로 최대 횟수를 정하는 것이 리소스를 아낄수있다
- 상기에서 보았듯이 while이 true로 지정되면 무한 반복되는 것을 알 수 있음 그러므로 무한 재시도하면 리소스가 낭비 됩니다.

rate limit이 풀리지 않으면 영영 빠져나오지 못하고(무한 루프), 그 사이 API를 계속 두드려 오히려 차단을 악화시키고, 호출당 과금이라면 비용이 무한정 증가하며, 다른 작업이 이 스레드에 막혀 전체 시스템이 멈출 수 있습니다