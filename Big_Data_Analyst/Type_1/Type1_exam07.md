### 문제 1. 
> 결측치가 있는 행을 제거한 후, 학생이 제일 많이 수강한 과목(id_assessment)을 찾고,
> 해당 과목 점수(score)를 표준화(Standard scaling)한 뒤에 표준화된 가장 큰 값을 구하시오
> (반올림하여 소수 셋째자리까지 계산)

```python
import pandas as pd 
df_1 = pd.read_csv(file1)

df_1.info()
df_1.isnull().sum()
```
```
id_assessment         0
student_id            0
study_period_days     0
score                21
dtype: int64
```
* 결측치 제거
```python
# 결측치 제거 
df_1 = df_1.dropna(subset ='score')
df_1.isnull().sum()
```
```
id_assessment        0
student_id           0
study_period_days    0
score                0
dtype: int64
```
* 학생이 가장 많이 수강한 과목
```python
df_1.columns
df_1['id_assessment'].value_counts().idxmax() # 과목 12
df_1 = df_1[df_1['id_assessment']==12]

# 표준화
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
df_1['score'] = scaler.fit_transform(df_1[['score']])
round(df_1['score'].max(),3)
# 답 : 2.183
```
* *스케일링 시 주의사항: fit에 DataFrame이 들어가야 함*
> ```df_1['score'] = scaler.fit_transform(df_1['score']) ```
> ```
> ValueError: Expected a 2-dimensional container 
> but got <class 'pandas.core.series.Series'> instead. 
> Pass a DataFrame containing a single row (i.e. single sample) 
> or a single column (i.e. single feature) instead.
> ```

### 문제 2. 
> DE1~DE77 칼럼 중 주가지수의 종가"close"와 가장 상관관계가 높은 변수를 찾아, 해당 변수의 평균값을 구하시오
> (반올림 넷째자리)
```python
# 주의: 절대값으로 확인한다. 
df_2 = pd.read_csv(file2)
df_2.corr()['close'].abs().sort_values(ascending =False)
# DE14    0.0624004109

# DE19의 평균 
df_2.DE14.mean().round(4)
# 답 : -0.0004
```




### 문제 3. 
> IQR을 이용해 CO2 이상치 수를 찾으시오 
```python
df_3 = pd.read_csv(file3)
df_3.info()

df_3['CO2']

Q3 = df_3.CO2.quantile(0.75) # 상위 25% (하위 75%) 
Q1 = df_3.CO2.quantile(0.25)
IQR = Q3 - Q1

out = IQR*1.5
# 이상치의 수 
df_3[(df_3['CO2']< Q1-out)|(df_3['CO2']> Q3 + out)]
# 답 : 304 개 
```
