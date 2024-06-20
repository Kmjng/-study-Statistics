### 문제 1. 
> 항암약 위약을 투여 받은 환자의 부작용은 감기약 위약을 투여받은 
> 환자의 부작용 분포와 차이가 있는가? 
> - 항암약(위약)을 투여받은 환자 그룹의 관찰된 부작용
#### 문제 1-1.  
> 항암약을 투여받은 환자 중 '무증상'의 비율을 0과 1 사이로 구하시오  
```python
# obj
# 항암약(위약)을 투여받은 환자들 중 관찰된 부작용 
ob = pd.DataFrame({'항암약':[4,4,3,4,1,4,1,4,1,4,4,2,1,4,2,3,2,4,4,4]})
# 1 아픔, 2 조금아픔, 3 속쓰림, 4 무증상 
ob.shape # 20 명 
ob.value_counts()

ob = [4,3,2,11]
ob 

# exp 
# 감기약(위약)을 투여받은 환자들 중 알려진 부작용 발생 비율 
# 1. 아픔 : 10% 
# 2. 조금 아픔 : 5% 
# 3. 속쓰림 15%
# 4. 무증상 70%

ex = [int(20*0.1), int(20*0.05), int(20*0.15), int(20*0.7)]
ex

11/20 # 정답 : 0.55
```

#### 문제 1-2.  
> 감기약의 예상 부작용 비율과 항암약의 부작용 관찰값이 통계적으로 유의미하게 차이가 있는지 확인하려고 한다.   
> 카이제곱검정을 사용해 검정 통계량을 구하시오
#### 문제 1-3.  
> 위의 p값을 구하시오 
```python
# 다른 집단 2개 => 카이제곱검정 중, 분포 차이를 보는 '적합도검정' 
from scipy import stats 
s, p = stats.chisquare(ob, ex)

s # 검정통계량 : 6.976190476190476 
p # 0.07266054733847573 # 유의하지 않음. 두 집단의 부작용 분포가 다르다. 
```





### 문제 2. 다중 선형 회귀 문제 
#### 문제 2-1.   
> 다중 선형회귀 모델을 구축하고, 독립변수 o3의 회귀계수를 구하시오   
> - 독립변수 : solar(태양에너지), wind(바람세기), o3(오존농도)  
> - 종속변수 : temperature(온도)  
```python
file1 = r"C:\ITWILL\빅분기_실기_기출_자료\data6-3-2.csv"
df2 = pd.read_csv(file1)

df2.info()

# 회귀계수를 구해야 한다. statsmodels로 모델링하기 
# 종속변수가 수치형 (온도) ; 회귀모델 
from statsmodels.formula.api import ols 

model = ols('temperature ~ solar + wind + o3', data = df2 )
lr = model.fit()

pred = lr.predict()
pred

lr.summary()

#  o3의 회귀계수
lr.params['o3'] # 0.07493854378136539

```
```
"""
                            OLS Regression Results                            
==============================================================================
Dep. Variable:            temperature   R-squared:                       0.044
Model:                            OLS   Adj. R-squared:                  0.014
Method:                 Least Squares   F-statistic:                     1.464
Date:                Thu, 20 Jun 2024   Prob (F-statistic):              0.229
Time:                        09:57:23   Log-Likelihood:                -195.45
No. Observations:                 100   AIC:                             398.9
Df Residuals:                      96   BIC:                             409.3
Df Model:                           3                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
Intercept     19.0507      1.994      9.555      0.000      15.093      23.008
solar          0.0039      0.015      0.251      0.802      -0.027       0.035
wind          -0.0252      0.090     -0.280      0.780      -0.204       0.153
o3             0.0749      0.036      2.079      0.040       0.003       0.146
==============================================================================
Omnibus:                        0.654   Durbin-Watson:                   2.328
Prob(Omnibus):                  0.721   Jarque-Bera (JB):                0.672
Skew:                           0.187   Prob(JB):                        0.715
Kurtosis:                       2.855   Cond. No.                     1.20e+03
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 1.2e+03. This might indicate that there are
strong multicollinearity or other numerical problems.
"""
```

#### 문제 2-2.  
> 데이터에서 solar와 o3값을 고정한 상태에서, 'wind' 의 세기가 증가함에 따라 temp가 감소하는 지를 검증하기 위해 다중선형회귀분석을 
> 수행하고, wind의 회귀계수에 대한 pvalue 값을 구하시오(유의수준 0.05)
```python
lr.pvalues.loc['wind']  # 0.7797177202071623 # 유의하지 않음 
```

#### 문제 2-3.  
> solar : 100, wind:5, o3:30 일때, 예측값과 그에 대한 95% 신뢰구간을 구하시오.
```python
# new data 셋을 만든다. 
new_data = pd.DataFrame({'solar': [100], 'wind':[5], 'o3':[30]})

# 예측값 구하기 
pred = lr.get_prediction(new_data)
# get_prediction()은 예측값의 신뢰구간, 관측구간도 알려준다. 
pred
pred.summary_frame(alpha = 0.05)

'''
ci: Confidence Interval
       mean   mean_se  mean_ci_lower  mean_ci_upper  obs_ci_lower  obs_ci_upper
0  21.56163  0.175263      21.213737      21.909524     18.082985     25.040276
'''

# 예측값 : 21.56163 
# 신뢰구간 (mean_ci_lower ~) :21.213737 ~ 21.909524  
```
