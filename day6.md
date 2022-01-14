



## [한국관광 데이터랩 자료 활용] 중국인 관광객수 변화



- ##### 한국관광데이터랩 자료 출처

 https://datalab.visitkorea.or.kr/datalab/portal/tst/getEntcnyFrgnCustForm.do

- ##### 방한 외래객 자료를 월별로 다운로드



##### 여러개의 엑셀파일(월별자료)을 기준년월을 기준으로 검토할 수 있도록  함수만들기

​		-  ''기준년월'' 컬럼 추가

```python
import pandas as pd
import numpy as np

def create_data(y,m): 
    file = './files/방한외래객_{}{}.xlsx'.format(y,m)    
    df = pd.read_excel(file, header = 1, usecols = 'A:G', skipfooter = 4)      

    df['기준년월'] = '{}-{}'.format(y,m)   
        
    return df

```

​       - 함수를 사용해 기준년월을 입력하면 자료를 읽어올 수 있다.

```python
tour = create_data(2018,'02')        # 앞자리 0 인식을 위해 '02'처럼 문자처리해주어야 한다.
```





##### 모든 데이터(월별자료)를 통합해서 하나의 엑셀파일로 만들기

```python
# for문 이용

df = pd.DataFrame()

for y in range(2010,2021):
    for m in range(1,13):
        m_str = str(m).zfill(2) 
        # 파일명과 같이 1월,2월과 같은 한자리수가 두자리로 인식되도록 zfill()함수 이용
        
        try:
            ym = create_data(str(y),m_str)
            df = df.append(ym, ignore_index = True)
            
        except:                  
            pass
            
     # 2020년 05월이 마지막 자료이므로 그 이후날짜로 넘어가면 에러발생, 이를 방지하기 위해 
     # try구문에서 에러가 발생하면 pass하도록 설정



# 파일 저장
df.to_excel('./files/total_data.xlsx', index = False, sheet_name = '201001~202005')
```




```python
df = pd.read_excel('./files/total_data.xlsx') # 저장한 파일 읽어오기

condition = df['국적'] == '중국'
df_ft = df[condition]
```



```python
# 선그래프로 시각화
import matplotlib.pyplot as plt
from matplotlib import rc, font_manager
import seaborn as sns

path = 'c:/Windows/Fonts/malgun.ttf'
font_name = font_manager.FontProperties(fname = path).get_name()
rc('font', family = font_name)

plt.figure(figsize = (12,5))

plt.plot(df_ft['기준년월'], df_ft['관광'], 'skyblue')
plt.xlabel('기준년월')
plt.ylabel('관광객수')

plt.xticks(['2010-02', '2011-02', '2012-02', '2013-02', '2014-02', '2015-02', '2016-02', '2017-02', '2018-02', '2019-02', '2020-02'])
plt.yticks(np.arange(0,900000,100000))
plt.show()
```

