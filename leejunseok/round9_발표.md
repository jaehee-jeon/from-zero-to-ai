# 뉴스 사이트에서 기사 제목을 수집하려고 합니다. 아래 코드는 requests로 HTML을 가져온 후 BeautifulSoup으로 파싱한 상태입니다.

# from bs4 import BeautifulSoup

# # 제목이 <h2 class='article-title'> 태그 안에 있습니다

from bs4 import BeautifulSoup
soup = BeautifulSoup(html_content, 'html.parser')

          #tag.get_text(strip=True) -> Text 내용만 추출 , strip : 앞뒤 공백 제거
titles = [tag.get_text(strip=True)
          for tag in soup.find_all('h2', class_='article-title')]
                    #soup.find_all : class_가 article-title 이며 거기에 h2의 태그를 가진 모든 것들을 찾아라