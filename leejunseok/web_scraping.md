## 대량 크롤링

# import openpyxl
import time
import requests
from bs4 import BeautifulSoup

# 1. 기사 목록이 나오는 페이지 URL (예시: 네이버 뉴스 특정 섹션이나 언론사 홈)
list_url = "https://news.naver.com/breakingnews/section/103/237"  # IT/과학 섹션 예시
headers = {
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"
}

print("1단계: 기사 링크 수집을 시작합니다...")
response = requests.get(list_url, headers=headers)
soup = BeautifulSoup(response.text, "html.parser")

# 기사 링크들을 담을 세트 (중복 제거를 위해 set 사용)
article_urls = set()

# 네이버 뉴스 목록의 기사 링크 태그 패턴 찾기 (a 태그 중 href에 article이 포함된 것)
for a_tag in soup.find_all("a", href=True):
    href = a_tag["href"]
    if "article" in href:
        # 주소가 완성형이 아니라 상대 경로로 되어 있다면 앞에 도메인을 붙여줍니다.
        if href.startswith("/"):
            href = "https://n.news.naver.com" + href
        elif href.startswith("http") and "news.naver.com" in href:
            pass
        else:
            continue
        article_urls.add(href)

print(f"총 {len(article_urls)}개의 기사 링크를 찾았습니다!\n")
print("2단계: 각 기사 내용 크롤링을 시작합니다...")
print("-" * 50)

# 2. 수집한 링크들을 돌면서 제목과 본문 긁어오기
for idx, url in enumerate(article_urls, 1):
    try:
        # 과도한 요청으로 서버에서 차단당하는 것을 막기 위해 1초씩 쉬어줍니다. (매우 중요!)
        time.sleep(1)

        res = requests.get(url, headers=headers)
        article_soup = BeautifulSoup(res.text, "html.parser")

        title = article_soup.find("h2", id="title_area")
        content = article_soup.find("div", id="newsct_article")

        print(f"[{idx}]번 기사 크롤링 중...")
        if title and content:
            print(f"🔗 링크: {url}")
            print(f"📌 제목: {title.text.strip()}")
            # 본문이 너무 길면 보기 힘드니 앞부분 100글자만 잘라서 출력해봅니다.
            print(f"📝 본문(일부): {content.text.strip()[:300]}...")
        else:
            print(f"❌ 기사 데이터를 찾지 못했습니다. (링크: {url})")
        print("-" * 50)

    except Exception as e:
        print(f"⚠️ {idx}번 기사 긁어오기 실패: {e}")