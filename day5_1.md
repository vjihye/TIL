## 유튜브 랭킹데이타 시각화


- day5에서 만든 youtube_rank_class.xlsx 로 파이차트 만들기



```python
from matplotlib import rc, font_manager

# 한글 표기하기 위해 글꼴 설정
path='c:/Windows/Fonts/malgun.ttf'
font_name = font_manager.FontProperties(fname = path).get_name()
rc('font', family = font_name)

df = pd.read_excel('./files/youtube_ranking_data.xlsx')

# '만' 을 0000으로 바꾸고 새로울 열 추가,여전히 데이터타입은 object임
df['구독자수 변경'] = df['구독자수'].str.replace('만','0000') 

df['구독자수 변경'] = df['구독자수 변경'].astype('int')  # 데이터타입 object에서 int로 변경

# 피봇테이블 생성, sum과 count값을 같이 가져오기 위해 리스트로 묶어준다.
pv_table = df.pivot_table(values = '구독자수 변경', index = '분류', aggfunc=['sum','count']) 

pv_table.columns=['구독자수', '채널개수']  # 컬럼명 변경, 개수 꼭 맞춰줘야함
pv_table = pv_table.reset_index() # 인덱스 초기화

# 구독자수를 기준으로 내림차순
pv_table = pv_table.sort_values(ascending = False, by ='구독자수') 
pv_table.reset_index(drop=True, inplace=True) # 0부터 시작하도록 인덱스 재설정
```



### 파이차트 만들기

- '분류'를 기준으로 구독자수 시각화하기

```python
import matplotlib.pyplot as plt

plt.rcParams['font.size'] = 12
plt.figure(figsize = (6,6))

plt.pie(pv_table['구독자수'], labels = pv_table['분류'], autopct='%.1f%%')

plt.show()
```

