# 네이버에 접속 후 검색 하기
# 웹 크롤링
import selenium
import time
from selenium.webdriver import chrome
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.options import Options



##웹드라이버 객체 생성

# 1. 크롬 브라우저 옵션 설정 (맥 환경에서 안정적으로 돌리기 위한 옵션들)
chrome_options = Options()
chrome_options.add_experimental_option("detach", True)  # 코드가 끝나도 브라우저가 바로 닫히지 않게 설정

# 2. 드라이버 실행 (따라서 크롬드라이버 경로를 지정할 필요 없음!)
driver = webdriver.Chrome(options=chrome_options)
    
try :
     driver.get("https://www.naver.com")
     print("인터넷 연결 성공")
     time.sleep(3)
     driver.maximize_window()

     search = driver.find_element(By.ID, "query")
     search.click()
     search.send_keys("누가 내 머리에 똥 쌌어")
     print("검색 키워드 입력 완료")
     time.sleep(3)
     
     #엔터키 입력
     search.send_keys(Keys.ENTER)
     print("성공적으로 검색을 완료했습니다!")
     time.sleep(10)

finally : 
     for i in range(10, 0,-1):
          print(f"\r종료 {i}초 전...", end="", flush=True)
     
     time.sleep(1)
     print("이제 종료합니다.")
     driver.quit()
   