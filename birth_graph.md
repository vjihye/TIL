### 생일일자 기온 변화를 그래프로 그리기



```

import csv
import matplotlib.pyplot as plt

f = open('./data/seoul.csv')
data = csv.reader(f)
next(data)  << 가장 윗줄 헤더 제거
high = []
low = []

for row in data :
    if row[-1] != '' and row[-2] != '':
        date = row[0].split('-')        
         if date[1] == '00' and date[2] == '00':  << 자신의 생일 월일 입력
            high.append(float(row[-1]))
            low.append(float(row[-]))
                
plt.rc('font', family='Malgun Gothic')
plt.rcParams['axes.unicode_minus'] = False  << 마이너스 기호 깨짐 방지
plt.title('내 생일 기온 변화 그래프')

plt.plot(high, 'hotpink', label = 'high')
plt.plot(low, 'skyblue', label = 'low')

plt.legend()
plt.show()
```

