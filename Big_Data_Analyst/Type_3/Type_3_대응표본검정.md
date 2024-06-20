문제 1.
-- 
> 주어진 데이터는 고혈압 환자 치료 전후의 혈압이다. 해당 치료가 효과가 있는지 대응(쌍체)표본 t-검정을 진행하시오  
* 귀무가설(H0): μ >= 0  
* 대립가설(H1): μ < 0  
* μ = (치료 후 혈압 - 치료 전 혈압)의 평균  
* 유의수준: 0.05

1-1. μ 의 표본평균은?(소수 둘째자리까지 반올림)  
1-2. 검정통계량 값은?(소수 넷째자리까지 반올림)  
1-3. p-값은?(소수 넷째자리까지 반올림)  
1-4. 가설검정의 결과는? (유의수준 5%)    

### Tip   
* 전/후 비교 -> 대응표본 t검정   
* t-test relative  
* stats.ttest_rel(<mark>df['Post'], df['Prev']</mark>, alternaive = 'less')  
```python
import pandas as pd
df = pd.read_csv("/kaggle/input/bigdatacertificationkr/high_blood_pressure.csv")

from scipy import stats 

# 표본 평균 (사후-사전 의 평균)
df['diff'] = df['bp_post']-df['bp_pre']
mean = df['diff'].mean().round(2)
statistic, p_val = stats.ttest_rel(df['bp_post'], df['bp_pre'], alternative = 'less')
print(f'''A1. 표본 평균:{mean}\n
A2. 검정통계량: {statistic.round(4)},\n
A3. p-value:{p_val.round(4)}''')
```
```
A1. 표본 평균:-6.12

A2. 검정통계량: -3.0002,

A3. p-value:0.0016
```



> 출처 : https://www.kaggle.com/datasets/agileteam/bigdatacertificationkr (빅데이터 분석기사 실기 준비를 위한 캐글 놀이터)

