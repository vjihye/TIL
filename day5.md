## 웹크롤링 유튜브 랭킹정보 가져오기

- 유튜브 랭킹 데이터 수집
  - 100개 채널의 카테고리명, 크리에이터명, 구독자수,  view수,  동영상 수 

```python
import pandas as pd
import numpy as np

from selenium import webdriver
from selenium.webdriver.chrome.service import Service

from bs4 import BeautifulSoup

service = Service('../chromedriver/chromedriver.exe')
driver = webdriver.Chrome(service = service)

# 유튜브 랭킹 사이트주소
url = 'http://youtube-rank.com/board/bbs/board.php?bo_table=youtube' 
driver.get(url)

html = driver.page_source
soup = BeautifulSoup(html, 'html.parser')

#유튜브랭킹 사이트에서 f12를 누르면 개발자 모드의 창을 볼 수 있다.
#원하는 데이터100개를 가져올 수 있도록 len()으로 확인하며 를 가져올 수 있는 태그의 위치를 찾는다. 
# 태그 위치를 정확하게 찾지 않으면 나중에 결과값에서 list index out of range 오류가 발생할 수 있다. channels = soup.select('tbody>tr')로 했을때 오류났음

channels = soup.select('form>table>tbody>tr') 

for channel in channels:
    category = channel.select('p.category')[0].text.strip()
    singer = channel.select('h1>a')[0].text.strip()
    sub = channel.select('td.subscriber_cnt')[0].text
    view = channel.select('td.view_cnt')[0].text
    video = channel.select('td.video_cnt')[0].text
     
    print(category, singer, sub, view, video)

```

[음악/댄스/가수] BLACKPINK 7130만 220억1380만 395개
[음악/댄스/가수] HYBE LABELS 6360만 209억9925만 786개
[음악/댄스/가수] BANGTANTV 6290만 146억5398만 1,665개
[음악/댄스/가수] SMTOWN 2970만 234억2536만 3,840개 

...

이런식으로 결과 출력된다.



- 수집한 데이터를 데이터프레임으로 만들어서 엑셀파일로 저장하기.

```python
yout_data = []

for channel in channels:
    category = channel.select('p.category')[0].text.strip()
    singer = channel.select('h1>a')[0].text.strip()
    sub = channel.select('td.subscriber_cnt')[0].text
    view = channel.select('td.view_cnt')[0].text
    video = channel.select('td.video_cnt')[0].text
    
    
    list = [category, singer, sub, view, video]
    yout_data.append(list)
    
    # 데이터프레임으로 만들기
    df = pd.DataFrame(yout_data, columns = ['음악/댄스', '가수명', '구독자수', 'view수', '동양상수']) 

```



```python
# 엑셀파일로 저장
df.to_excel('./files/youtube_rank_class.xlsx', index = False, sheet_name = 'youtube_chart_100')
```

