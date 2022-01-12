## 웹 크롤링 기초

- 크롬드라이버 설치
  1. 크롬브라우저의 우측상단 도움말 -> Chrome정보 에서 사용중인 버전 확인
  2. https://chromedriver.chromium.org/downloads 에서 사용중인 버전과 일치하는 크롬드라이버 파일 다운로드
  3. chromedriver.exe파일 저장

```python
! pip install selenium  # selenium 라이브러리 설치

from selenium import webdriver # 라이브러리 블러오기
from selenium.webdriver.chrome.service import Service #크롬웹드라이버를 사용하기위해 불러오기
from bs4 import BeautifulSoup #html 문자열을 알아보기 쉽게 하기 위해 불러오기

service = Service('../files/chromedriver/chromedriver.exe') # 다운받은 크롬드라이버 변수 저장
driver = webdriver.Chrome(service = service) # 크롬드라이버 실행 -> 새로운 크롬창 열린다
url = 'https://www.naver.com/' # 네이버 접속하기 위해 url로 저장
driver.get(url) # 접속

html = driver.page_source # html정보 불러온다
soup = BeautifulSoup(html,'html.parser') # html 형식에 맞게 알아보기 쉽게 준비
soup

```

