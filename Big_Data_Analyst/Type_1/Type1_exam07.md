### 문제 1. 
> 결측치가 있는 행을 제거한 후, 학생이 제일 많이 수강한 과목(id_assessment)을 찾고,
> 해당 과목 점수(score)를 표준화(Standard scaling)한 뒤에 표준화된 가장 큰 값을 구하시오
> (반올림하여 소수 셋째자리까지 계산)

```python
import pandas as pd 
df_1 = pd.read_csv(file1)

df_1.info()
print(df_1.isnull().sum())
```




### 문제 2. 
> DE1~DE77 칼럼 중 주가지수의 종가"close"와 가장 상관관계가 높은 변수를 찾아, 해당 변수의 평균값을 구하시오
> (반올림 넷째자리)





### 문제 3. 
> IQR을 이용해 CO2 이상치 수를 찾으시오 
