### 문제 1. 
> 주어진 조개 데이터 300개 중 앞에서부터 210개는 train, 나머지 90개는 test set으로 한다. 
> 모델을 학습할 때는train데이터를 사용하고 예측할 때는 test set을 사용한다. 
> 모델은 로지스틱 회귀를 사용하여 성별(gender)을 예측하되, 패널티는 부과하지 않는다.

```python
# 데이터 불러오기 
file1 =r"C:\ITWILL\빅분기_실기_기출_7회_자료\p7_type3\clam.csv"
file2 = r"C:\ITWILL\빅분기_실기_기출_7회_자료\p7_type3\system.csv"
import pandas as pd 
data = pd.read_csv(file1)
train = data.iloc[:210, :].reset_index(drop=True)
test = data.iloc[210:,:].reset_index(drop= True)
train.info()
```

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 210 entries, 0 to 209
Data columns (total 6 columns):
 #   Column    Non-Null Count  Dtype  
---  ------    --------------  -----  
 0   age       210 non-null    int64  
 1   length    210 non-null    float64
 2   diameter  210 non-null    float64
 3   height    210 non-null    float64
 4   weight    210 non-null    float64
 5   gender    210 non-null    int64  
dtypes: float64(4), int64(2)
memory usage: 10.0 KB
```

```python
# 전체 변수에 대해 모델링 해봄 
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import roc_auc_score 

y_train = train.pop('gender')
y_test = test.pop('gender')

model = LogisticRegression() 

lr = model.fit(train, y_train)

y_pred = lr.predict_proba(test) # array type
y_pred[:,1] # 1일 확률 

print('roc_auc_score:', roc_auc_score(y_test, y_pred[:,1]))
# roc_auc_score: 0.5106382978723405
```


### 문제 1-1. 
> weight을 독립변수로, gender를 종속변수로 사용하여 로지스틱 회귀 모형을 만들고, 
> weight변수가 한 단위 증가할 때 수컷일 오즈비 값은 ? 
> (반올림하여 넷째자리 까지 계산)

```python
import pandas as pd 
data = pd.read_csv(file1)
train = data.iloc[:210, :].reset_index(drop=True)
test = data.iloc[210:,:].reset_index(drop= True)

from statsmodels.formula.api import logit 
model = logit('gender ~ weight', data = train)

lr = model.fit()

lr.summary()
a = lr.params['weight'] # weight의 회귀 계수 

# 오즈 
import numpy as np 

odds_ratio = np.exp(a)
odds_ratio # 1.0047172874702013 
# 답 : 
# weight 한 단위 증가 시, 수컷일 확률이 
# 1.0047172874702013 배 증가 
```

### 문제 1-2.  
> gender를 종속변수로 하고 나머지 변수들(age, length, diameter, height, weight)을 독립변수로 사용하는
> 로지스틱 회귀 모델을 적합시킨 후, 잔차 이탈도(Residual Deviance)를 계산하시오 
> (반올림 소수 둘째 자리)
> > **Tip**  
> > glm(또는 sm.GML) 모델을 사용하면 summary()에서 deviance 파라미터 제공해줌  
> > 아니면, summary()에서 Log-Likelihood에서 (-2)를 곱하여 구할 수 있음  

```python
import pandas as pd 
data = pd.read_csv(file1)
train = data.iloc[:210, :].reset_index(drop=True)
test = data.iloc[210:,:].reset_index(drop= True)
train.columns
y_train = train.gender
X_train = train.drop('gender', axis = 1)

import statsmodels.api  as sm 
model = sm.GLM(y_train, X_train, family= sm.families.Binomial())
lr = model.fit() 

lr.summary()
lr.deviance.round(2) 
# 답 : 288.0
```
```
                 Generalized Linear Model Regression Results                  
==============================================================================
Dep. Variable:                 gender   No. Observations:                  210
Model:                            GLM   Df Residuals:                      205
Model Family:                Binomial   Df Model:                            4
Link Function:                  Logit   Scale:                          1.0000
Method:                          IRLS   Log-Likelihood:                -144.00
Date:                Wed, 19 Jun 2024   Deviance:                       288.00
Time:                        11:14:56   Pearson chi2:                     210.
No. Iterations:                     4   Pseudo R-squ. (CS):            0.01332
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------
age           -0.0214      0.048     -0.445      0.656      -0.115       0.073
length        -0.2920      0.875     -0.334      0.739      -2.008       1.424
diameter      -0.4725      1.218     -0.388      0.698      -2.859       1.914
height        -1.3317      2.462     -0.541      0.589      -6.157       3.494
weight         0.0069      0.005      1.481      0.138      -0.002       0.016
==============================================================================

```

** 잔차이탈도 ? 
> 일반선형모델에서 주로 쓰이는 평가지표  
D=2×(log likelihood of saturated model−log likelihood of fitted model)  
;
log likelihood of saturated model:
> 모든 관측치에 대해 예측이 완벽한 모델의 로그 우도.  
> 즉, 모든 관측치의 실제 값을 100% 정확하게 맞추는 모델  
log likelihood of fitted model:
> 실제로 피팅한 모델의 로그 우도  



### 문제 1-3. 
> 독립변수 weight만을 사용하여 학습한 모델에서 test set에서의 gender를 예측하고,   
> error rate(오류율)을 구하시오   
> (반올림 소수 둘째)  
* 오류율? ``` error rate = 1 - accuary_score ```
```python
from statsmodels.formula.api import logit 
lr = logit('gender ~ weight', data = train).fit() 

lr.summary()
```
```
                           Logit Regression Results                           
==============================================================================
Dep. Variable:                 gender   No. Observations:                  210
Model:                          Logit   Df Residuals:                      208
Method:                           MLE   Df Model:                            1
Date:                Tue, 18 Jun 2024   Pseudo R-squ.:                0.003431
Time:                        20:08:26   Log-Likelihood:                -144.91
converged:                       True   LL-Null:                       -145.41
Covariance Type:            nonrobust   LLR p-value:                    0.3178
==============================================================================
                 coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------
Intercept     -0.3140      0.276     -1.137      0.256      -0.855       0.227
weight         0.0047      0.005      0.997      0.319      -0.005       0.014
==============================================================================
```
```python
from sklearn.metrics import accuracy_score 

pred = lr.predict(test) # formula 모듈의 모델은 predict(독립종속 포함된 데이터셋) 
# pred : 양성(1)일 확률 
pred = (pred> 0.5).astype('int')
error_rate = 1 - accuracy_score(test['gender'], pred)

error_rate.round(2)
# 답 : 0.48
```

### 문제 2-1. 
> ERP와 가장 상관관계가 높은 것을 구하시오 
> (반올림 소수 셋째)

```python
df = pd.read_csv(file2)
df.info()
```
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 100 entries, 0 to 99
Data columns (total 5 columns):
 #   Column    Non-Null Count  Dtype  
---  ------    --------------  -----  
 0   ERP       100 non-null    float64
 1   Feature1  100 non-null    float64
 2   Feature2  100 non-null    float64
 3   Feature3  100 non-null    float64
 4   CPU       100 non-null    float64
dtypes: float64(5)
memory usage: 4.0 KB
```
```python
corr_ERD = df.corr(method = 'pearson')['ERP'].abs().sort_values(ascending = False)
corr_ERD
```
```
ERP        1.0000000000
Feature1   0.4344424975
CPU        0.2010272378
Feature2   0.0761159518
Feature3   0.0598878888
Name: ERP, dtype: float64
```
```python
corr_ERD.loc['Feature1'].round(3)
# 답 : 0.434 (Feature1)
```
### 문제 2-2. 
> CPU 칼럼이 100 미만인 것만 찾아 ERP를 종속변수로, 나머지 변수들을 독립변수로 
> 설정해, 선형회귀 모델을 만들고 적합한 결정계수를 구하시오 
> (반올림 소수 셋째)
### 문제 2-3.
> 문제 2-2에서 만든 모델에서 독립변수 중 pvalue가 가장 높은 값을 구하시오.  
> (반올림 소수 셋째)
```python
df = df[df['CPU']<100]

# ERP : 수치형 데이터 => 선형회귀, rmse, r2score... 
df.ERP.value_counts() # 수치형 데이터 확신 

##########
# 방법 1.
##########
import statsmodels.api as sm 
y = df['ERP']
X = df.drop('ERP', axis = 1)
X = sm.add_constant(X) # 1로 이루어진 칼럼 추가 -> 절편항 추정
X

model = sm.OLS(y,X).fit() 
model.summary()

# 결정계수 (r2_score) 구하기
# 방법 (1) 
from sklearn.metrics import r2_score 
r2_score(y,pred).round(3) # 실제값 , 예측값
# 0.226

# 방법 (2)
model.rsquared.round(3)
# 0.226
model.rsquared_adj.round(3)
# 0.193

pd.set_option('display.float_format','{:.10f}'.format)
model.pvalues.sort_values(ascending = False)
# 2-3. 답 : # Feature2     0.456708

#########################
# 방법 2.formula 모듈 사용
#########################
from statsmodels.formula.api import ols 
model = ols('ERP ~ Feature1 + Feature2 + Feature3 + CPU ', data = df).fit()

model.summary()
model.rsquared.round(3)
# 0.226
model.rsquared_adj.round(3)
# 0.193

model.pvalues

model.pvalues.sort_values(ascending = False)
'''
Feature2    0.4567075120
Feature3    0.2982063796
CPU         0.0679677409
Feature1    0.0000139351
Intercept   0.0000034610
dtype: float64
'''
# 2-3. 답 : Feature2     0.456708

```

