## 엑셀파일 읽기, 데이터 합치기, 저장하기, 피벗테이블

```python
import pandas as pd 
import numpy as np

# 엑셀파일 읽기 (sample_1파일)
sample_1 = pd.read_excel('./files/sample_1.xlsx', header = 1, usecols='A:C', skipfooter=2, dtype = {'입국객수':np.int64})

# header = 첫째행을 두번째행으로 
# usercols = A부터C열 까지만 사용
# skipfooter = 2 밑에서부터 2줄 제거

## 엑셀파일 읽기(codemaster파일)
code_master = pd.read_excel('./files/sample_codemaster.xlsx', header = 0, usecols = 'A:B', skipfooter = 0)  


# 2개의 파일 합치기옆으로 통합
# 1. 옆으로 통합: how = left
sample_1_code = pd.merge(left=sample_1,  << 왼쪽에 위치할 파일
                         right=code_master,  << 오른쪽에 위치할 파일
                         how='left',          << 왼쪽테이블 기준으로 합친다
                         left_on='국적코드',    << 왼쪽테이블 기준 컬럼명
                         right_on='국적코드')   << 오른쪽테이블 기준 컬럼명

# 2. 옆으로 통합: how = inner
#  - inner조건으로 옆으로 통합: inner조건은 두테이블의 기준 칼럼의 값이 일치하는 값만 보여줌 
sample_1_code_inner = pd.merge(left=sample_1, right = code_master, how='inner', left_on = '국적코드', right_on = '국적코드')

# 3. 아래로 통합: append
sample = sample_1_code.append(sample_2_code, ignore_index = True) 
sample  # ignore_index = True로 지정하지 않으면 index가 바뀌지 않은체로 통합된다.

# 데이터 합치기1
# index = False 하면 index는 제외된다
# na_rep = 'no data'  nan값은 no data로 표시
sample.to_excel('./files/sample_class.xlsx', index = False, na_rep = 'no data', sheet_name = 'mysheet') 

# 데이터 합치기2
with pd.ExcelWriter('./files/multiple_sheet.xlsx') as writer:
    sample.to_excel(writer, sheet_name = 'my_sheet1')
    sample.to_excel(writer, sheet_name = 'my_sheet2', index = False, na_rep = 'no data')
    

## 피벗테이블 

sample_pivot = sample.pivot_table(values = '입국객수', index = '국적명', columns = '기준년월', aggfunc='mean')

```